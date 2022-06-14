---
date created: 2022-05-28 23:50
date updated: 2022-06-14 20:14
---

### MVVM 的理解

1. MCV：用户操作请求服务端路由，路由调用相应控制器，它获取数据再返给前端渲染
2. MVVM：用户不用手动操作DOM，而是绑定到 ViewModel 层，视图变化则更新数据

### 响应式数据的原理

主要原理是使用 Object.defineProperty 来实现对 get set 属性的拦截，在 get 操作时，会为该 key 创建一个 dep，用于收集与该值相关的所有 watcher，而在修改该值的时候，会执行 dep.notify 函数，它会遍历存储的所有 watcher，执行它的 get 来更新所有相关数据

### 如何检测数组变化

在 vue2 中，数组和对象的处理方式实际上是不同的，首先是覆盖其原型链上可以修改数组的七个方法，使其可以进行响应式通知，对有可能生成的新数据也需要进行响应式

### nextTick

这是 vue 的异步机制，我们在操作时，往往一次性会有许多的更新操作，每一步的操作都执行更新函数显然不必要，所以用异步的方式来进行数据的更新会节省很多性能。当我们需要进行异步更新的时候，我们会打开一个 waiting 的打开，进而开启 nextTick，传入 flushSchedulerQueue，这是一个执行所有 watcher 的函数（会排序），我们会把这个函数存入一个 callbacks 数组中，然后打开 pending 开关，开启 timeFunc 的异步事件，其中有异步的降级策略，Promise - MutationObserver - setImmediate - setTimeout，然后循环执行 callbacks 中的每一项，也就是每一个 flushSchedulerQueue

### 更新流程

dep.notify - watcher.update - queueWatcher - nextTick(flushSchedulerQueue) - timeFunc - flushScheduler - watcher.run


### computed