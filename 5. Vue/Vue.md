---
date created: 2022-05-28 23:50
date updated: 2022-05-30 02:07
---

### MVVM 的理解

1. MCV：用户操作请求服务端路由，路由调用相应控制器，它获取数据再返给前端渲染
2. MVVM：用户不用手动操作DOM，而是绑定到 ViewModel 层，视图变化则更新数据

### 响应式数据的原理

使用 Object.defineProperty 对对象中每一个 key 值进行了属性拦截操作，在 get 时会收集依赖，每个属性值内部都会有一个 dep，存储的是与其相关的所有 watcher，当 set 时会触发依赖，执行 dep.notify 操作，最终执行的是 watcher.get()，也就是传入的更新函数

### 如何检测数组变化

当我们数组发生增加或删除的时候，我们也应该触发 watcher，但是实际上仅靠 define
