---
date created: 2022-03-03 00:34
---

React 16.0 版本之前使用的虚拟DOM的更新采用循环和递归

- 任务一旦开始，无法结束，直到任务结束，主线程一直被占用
- 导致大量组件实例存在时，执行效率变低
- 用户交互的动画效果，容易出现卡顿

Fiber（链表）

- 利用浏览器的空闲时间执行，不会长时间占用主线程
- 将对比更新DOM的操作碎片化，在 diff 时生成DOM，diff 完成DOM也生成结束
- 碎片化的任务，可以根据需要被暂停

RequestIdleCallback(function (deadline) { deadline.timeRemaining() // 获取浏览器的空闲时间 })

- 浏览器提供的 API，利用浏览器空闲时间处理任务，当前任务可被中止，优先执行更高级的任务

React 生命周期
React
