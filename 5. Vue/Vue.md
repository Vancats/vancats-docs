---
date created: 2022-05-28 23:50
date updated: 2022-05-29 22:21
---

### MVVM 的理解

1. MCV：用户操作请求服务端路由，路由调用相应控制器，它获取数据再返给前端渲染
2. MVVM：用户不用手动操作DOM，而是绑定到 ViewModel 层，视图变化则更新数据

### 响应式数据的原理
1. 使用 Object.defineProperty 对对象中每一个 key 值进行了属性拦截操作，在 get 时会收集依赖，每个属性值内部都会有一个 dep，存储的是与其相关的所有 watcher，当 set 时会触发依赖，执行 dep.no