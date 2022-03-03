---
date created: 2022-03-03 12:28
date updated: 2022-03-03 16:20
---

React 16.0 版本之前使用的虚拟DOM的更新采用循环和递归

- 任务一旦开始，无法结束，直到任务结束，主线程一直被占用
- 导致大量组件实例存在时，执行效率变低
- 用户交互的动画效果，容易出现卡顿

**Fiber（链表）**

- 利用浏览器的空闲时间执行，不会长时间占用主线程
- 将对比更新DOM的操作碎片化，在 diff 时生成DOM，diff 完成DOM也生成结束
- 碎片化的任务，可以根据需要被暂停

RequestIdleCallback(function (deadline) { deadline.timeRemaining() // 获取浏览器的空闲时间 })

- 浏览器提供的 API，利用浏览器空闲时间处理任务，当前任务可被中止，优先执行更高级的任务

React 虚拟DOM了解嘛
React Fiber 了解嘛
Fiber 的优势是什么
Fiber 怎么做到比之前的渲染快的
你了解 React 虚拟DOM渲染机制嘛

**React 生命周期**
React 生命周期函数以及各个函数应用
React 16.0 前后生命周期函数的变化
对于 React 生命周期函数的变化你有什么理解
React 性能优化（关于虚拟DOM渲染）

**React Diff 算法策略**

- 16.0 之前
	- 针对树结构：忽略跨层级的操作
	- 针对组件结构：拥有相同类的两个组件形成相似树形结构，不同类的生成不同属性结构
	- 针对元素结构：对于同一层级的节点，通过 id 和 tag 区分

- 16.0 之后
	- 通过 state 计算出新的 fiber 节点
	- 通过对比节点的 key 和 tag 确定节点操作（新增，删除，移动，修改）
	- effectTag 属性标记 fiber 对象
	- 收集所有标记的 fiber 对象，形成 effetList
	- commit 阶段一次性的处理所有变化的节点

**flux**
发布订阅模式

- UI 产生动作信息，将动作传递给分发器
- 分发器广播给所有 store
- 订阅的 store 做出反应，传递新的 state 给 UI

**redux**
不可变数据集

- 单一数据源，整个应用的 state 存储在一个单一的 store 中
- state 只读，通过 action 触发修改
- 使用纯函数进行状态修改，需要开发者书写 reducer 纯函数处理，redecer 通过当前状态树和 action 进行计算，返回新 state

**mobx**

- 定义状态使其可观察
- 创建视图以响应状态变化
- 更改状态（自动响应 UI 变化）
