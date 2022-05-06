---
date created: 2022-05-06 14:10
date updated: 2022-05-06 14:40
---

#### 编译环节

很多代码的截取没有太大的必要，具体还是实现的路线，我会贴路径，可以自行顺着逻辑查看

编译阶段当然在 `compiler` 模块进行，我们直接进入到 `parser` 中，从 [`parse`](src\compiler\parser\index.js) 函数开始，进行 `parseHTML` 的操作，调用 `closeElement` 后，实现 `processElement`，其中就可以看到实现了 `processSlotContent` 和 `processSlotOutlet`

```js
```
