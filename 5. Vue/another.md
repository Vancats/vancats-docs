---
date created: 2022-03-03 00:35
date updated: 2022-05-16 16:19
---

### 多次阅读

> history / hash
> 节点属性更新过程，key怎么起作用
> _c,l,t,s,v
> keep-alive(LRU), 插槽， 编译器工作原理
> compileToFunctions：template => render
> mountComponent
> initGlobalAPI：set,delete,use,mixin,extend,component,directive,filter
> mergeOptions
> _init
> 响应式
> patch：patchVnode(oldVnode, vnode) / createElm / createComponent

### 异步组件

```javascript
Vue.component('Foo', () => {
  component: delay(4000, () => ('Foo.js')),
  loading: LoadingComponent,
 	error: ErrorComponent,
  timeout: 5000
})

const delay = (time, fn) => {
  return new Promise((res, rej) => {
    setTimeout(() => {
      resolve(fn())
    }, time)
  })
}

const LoadingComponent = {
  template: `<div>Loading...</div>`
}

const ErrorComponent = {
  template: `<div>Error</div>`
}
```

### Vue Plugins

1. 添加全局函数
2. 添加全局资源 - 组件,指令
3. 混入组件选项
4. 添加 Vue 的实例方法

牛客网第七题 Object.prototype.setPrototype

图片宽度一样，高度不同，4列摆放，100张图片，怎么使得4列高度差最小

微前端

1. spa版本的iframe
2. html-entry 入口加载：怎么loader入口，解析入口文件加载的css和js地址
3. css隔离：多个应用切片，css不冲突、动态样式表、shadow-dom
4. js隔离：全局变量的隔离（window diff，proxy）
5. 生命周期
6. 路由切换
