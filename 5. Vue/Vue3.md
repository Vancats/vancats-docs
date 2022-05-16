---
date created: 2022-04-23 20:48
date updated: 2022-05-16 16:21
---

Vue3 定义 Props

```ts
// 1. ts 专用声明，无默认值
defineProps<{
	msg: string
}>

// 2. ts 专用声明，可以有默认值
interface Props {
	msg: string
}

const props = withDefaults(defineProps<Props>(), {
	msg: string
})

// 3. 非 ts 专用声明
defineProps({
	msg: String // 大写
})
```

Vue3 源码

```js

vue.with compiler

umd global

包含编译器,运行时编译

编译发生的时刻是挂载时

vue.runtime.js

运行时不会编译

预编译:vue-loader

打包:webpack调用它执行编译,将所有vue文件,SFC,转换 template => render

好处:包体积小,执行时候速度快

ast:与vnode相似的中间产物

template =>(粗糙) ast => transform(深加工) => ast => generate(代码生成) => render

### reactive

- ref
- reactive
- computed
- effect

### runtime-core

- h
- lifecycle
- scheduler
- vnode
- teleport
- suspense
- keepAlive
- renderer
- errorHandling
- createApp

### runtime-dom

- nodeOps
- patchProps: class、style、attrs、event
- directives
- components

### compile-dom

- parserOptions
- transformStyle
- vHtml
- vText
- vOn
- vShow
- warnTransitionChildren
- stringifyStatic

### compile-core

- compile
- parse
- transform
- codegen
- ast
- errors
```

###### Vue3 新特性

1. RFC 机制：保证社区活力
2. 响应式系统
3. 自定义渲染器：拆包扩展，使用了 monorepe 管理方式
4. 使用 TS 重构
5. Composition API
6. 新的组件：Fragment、Teleport、Suspense
7. Vite
