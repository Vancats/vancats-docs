---
date created: 2022-05-28 23:50
date updated: 2022-06-14 15:53
---

### MVVM 的理解

1. MCV：用户操作请求服务端路由，路由调用相应控制器，它获取数据再返给前端渲染
2. MVVM：用户不用手动操作DOM，而是绑定到 ViewModel 层，视图变化则更新数据

### 响应式数据的原理

主要原理是使用 Object.defineProperty 来实现对 get set 属性的拦截，在 get 操作时，会为该 key 创建一个 dep，用于收集与该值相关的所有 watcher，而在修改该值的时候，会执行 dep.notify 函数，它会遍历存储的所有 watcher，执行它的 get 来更新所有相关数据

### 如何检测数组变化

### 异步渲染 nextTick

实际上是一个批量更新的操作
