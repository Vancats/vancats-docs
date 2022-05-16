---
date created: 2022-05-06 14:10
date updated: 2022-05-06 15:49
---

很多代码的截取没有太大的必要，具体还是实现的路线，我会贴路径，可以自行顺着逻辑查看

#### 编译环节

##### 编译阶段当然在 compiler 模块进行

1. 首先进入到 parser 中 `src\compiler\parser\index.js`
2. 从 parse 函数开始，进行 parseHTML 的操作
3. 调用 closeElement 后，实现 processElement
4. 可以看到实现了 processSlotContent 和 processSlotOutlet，前者主要是设置 slotScope 和 slotTarget，后者是设置 slotName，这些属性都是后面需要使用的内容

##### 然后是 codegen

1. 进入到 codegen 中 `src\compiler\codegen\index.js`
2. genarate
3. new 一个 CodegenState 类，获得 state
4. 调用 genElement 方法
5. 如果 tag 就是 slot，调用 genSlot，可以看到会生成一个 `_t` 开头的字符串，传入 slotName 和 children

```js
function genSlot (el: ASTElement, state: CodegenState): string {
  const slotName = el.slotName || '"default"'
  const children = genChildren(el, state)
  let res = `_t(${slotName}${children ? `,function(){return ${children}}` : ''}`
  const attrs = el.attrs || el.dynamicAttrs
    ? genProps((el.attrs || []).concat(el.dynamicAttrs || []).map(attr => ({
        // slot props are camelized
        name: camelize(attr.name),
        value: attr.value,
        dynamic: attr.dynamic
      })))
    : null
  const bind = el.attrsMap['v-bind']
  if ((attrs || bind) && !children) {
    res += `,null`
  }
  if (attrs) {
    res += `,${attrs}`
  }
  if (bind) {
    res += `${attrs ? '' : ',null'},${bind}`
  }
  return res + ')'
}
```

6. 如果是其他形式的 slot，会走 getData 逻辑中，关键代码如下

```js
if (el.slotTarget && !el.slotScope) {
  data += `slot:${el.slotTarget},`
}
if (el.scopedSlots) {
  data += `${genScopedSlots(el, el.scopedSlots, state)},`
}

function genScopedSlots(
  ...
  const generatedSlots = Object.keys(slots)
    .map(key => genScopedSlot(slots[key], state))
    .join(',')
    
  return `scopedSlots:_u([${generatedSlots}]${needsForceUpdate ? `,null,true` : ``
    }${!needsForceUpdate && needsKey ? `,null,false,${hash(generatedSlots)}` : ``
    })`
}

function genScopedSlot(
  el: ASTElement,
  state: CodegenState
): string {
  ... 
  const slotScope = el.slotScope === emptySlotScopeToken
    ? ``
    : String(el.slotScope)
  const fn = `function(${slotScope}){` +
    `return ${el.tag === 'template'
      ? el.if && isLegacySyntax
        ? `(${el.if})?${genChildren(el, state) || 'undefined'}:undefined`
        : genChildren(el, state) || 'undefined'
      : genElement(el, state)
    }}`
  const reverseProxy = slotScope ? `` : `,proxy:true`
  return `{key:${el.slotTarget || `"default"`},fn:${fn}${reverseProxy}}`
}

```

观察以上代码，首先会把所有节点进行 `genScopedSlot`，其中的 fn 中传入了一个 `$slotScope`，这个就是我们作用域插槽值传递的关键，然后我们会生成一个 `_u` 开头的字符串，并传入了相关内容，其他逻辑可以自行查看

#### 运行时

前面的 `_t`, `_u`，甚至熟悉的 `_c` 都是代表的哪些内容，可以在这里查看 `src\core\instance\render-helpers\index.js`， 其中
`target._t = renderSlot`
`target._u = resolveScopedSlots`

renderSlot `src\core\instance\render-helpers\render-slot.js`

1. 取出相应的节点    赋值涉及值的判断，可以自行查看其优先级
2. 创建节点 `this.$createElement('template', { slot: target }, nodes)`

resolveScopedSlots `src\core\instance\render-helpers\resolve-scoped-slots.js`
主要是执行了其中的所有 fn 函数，其返回的值可以通过上方的 genScopedSlot 得知会是一个节点，我们将所有的值放入一个对象并返回即可
