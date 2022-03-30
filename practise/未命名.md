---
date created: 2022-03-30 20:28
date updated: 2022-03-30 20:29
---

compile

```
let template = `

 <div id="app">

 <h1 @click="add" class="item" :id="count">{{count}}</h1>

 <p>今天真快乐</p>

 </div>

`

  

function parse(template) {

 // 1. 语义拆分成数组

 const tokens = tokenizer(template)

 // console.log('tokens: ', tokens)

  

 // 2. 数组 -> 树

 let cur = 0

 let ast = {

 type: 'root',

 props: [],

 children: []

 }

 while (cur < tokens.length) {

 ast.children.push(walk())

 }

  

 return ast

  

 function walk() {

 let token = tokens[cur]

 if (token.type === 'tagstart') {

 let node = {

 type: 'element',

 tag: token.val,

 props: [],

 children: []

 }

 token = tokens[++cur]

 while (token.type !== 'tagend') {

 if (token.type === 'props') {

 node.props.push(walk())

 } else {

 node.children.push(walk())

 }

 token = tokens[cur]

 }

 cur++

 return node

 }

 if (token.type === 'tagend') {

 cur++

 }

 if (token.type === 'text') {

 cur++

 return token

 }

 if (token.type === 'props') {

 cur++

 const [key, val] = token.val.split('=')

 return { key, val }

 }

 }

}

  

function tokenizer(input) {

 // 返回数组

 const tokens = []

 let type = ''

 let val = ''

  

 for (let i = 0; i < input.length; i++) {

 let ch = input[i]

 if (ch === '<') {

 push() // < 开始的字符，语义更换，之前收集的内容 push 到 token 里

 if (input[i + 1] === '/') {

 type = 'tagend' // 结束标签

 } else {

 type = 'tagstart' // 开始标签

 }

 }

  

 if (ch === '>') { // 标签结束

 push()

 type = 'text'

 continue

 } else if (/[\s]/.test(ch)) {

 push()

 type = 'props'

 continue

 }

  

 val += ch

 }

  

 return tokens

  

 function push() {

 if (val) {

 if (type === 'tagstart') val = val.slice(1)

 if (type === 'tagend') val = val.slice(2)

 tokens.push({

 type,

 val

 })

 val = ''

 }

 }

}

  

function transform(ast) {

 let context = {

 helpers: new Set(['openBlock', 'createVNode']) // 用到的工具函数

 }

 tranverse(ast, context)

 ast.helpers = context.helpers

}

  

function tranverse(ast, context) {

 switch (ast.type) {

 case 'root':

 context.helpers.add('createBlock')

 case 'element':

 // 1. 标记 vue 语法

 // 2. 静态标记

 ast.children.forEach(node => {

 tranverse(node, context)

 })

 ast.flag = { class: false, props: false, event: false }

 ast.props = ast.props.map(prop => {

 const { key, val } = prop

 if (key[0] === '@') {

 ast.flag.event = true

 return {

 key: 'on' + key[1].toUpperCase() + key.slice(2),

 val

 }

 }

 if (key[0] === ':') {

 ast.flag.props = true

 return {

 key: key.slice(1),

 val

 }

 }

 if (key.startsWith('v-')) {

 // ...

 }

 return {

 ...prop, static: true

 }

 })

 case 'text':

 let re = /\{\{(.*)\}\}/g

 if (re.test(ast.val)) {

 ast.static = false

 context.helpers.add('toDisplayString')

 ast.val = ast.val.replace(re, function (s0, s1) {

 return s1

 })

 } else {

 ast.static = true

 }

 }

}

  

function generate(ast) {

 const { helpers } = ast

 let code = `

 import {${[...helpers].map(v => v + ' as _' + v)}} from 'vue'\n

 export function render(_ctx, _cache) {

 return (_openBlock(), ${ast.children.map(node => walk(node))})

 }

 `

  

 function walk(node) {

 switch (node.type) {

 case 'element':

 let { flag } = node

 let props = '{' + node.props.reduce((ret, p) => {

 if (flag.props) {

 ret.push(p.key + ':_ctx.' + p.val.replace(/['"]/g, ''))

 } else {

 ret.push(p.key + ':' + p.val)

 }

 // TODO class

 return ret

 }, []).join(',') + '}'

 return `createVNode("${node.tag}", ${props},

 [${node.children.map(n => walk(n))}],

 ${JSON.stringify(flag)}

 )`

 case 'text':

 if (node.static) {

 return '"' + node.val + '"'

 } else {

 return `_toDisplayString(_ctx.${node.val}'`

 }

 }

 }

 return code

}

  
  
  
  

function compile() {

 let ast = parse(template)

 // ll(ast)

 transform(ast)

 // ll(ast)

 const code = generate(ast)

 console.log('code: ', code)

 return code

}

  

function ll(ast) {

 console.log(JSON.stringify(ast, null, 2))

}

  

let code = compile(template)
```
