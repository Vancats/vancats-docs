---
date created: 2022-05-06 14:10
date updated: 2022-05-06 14:45
---

#### 编译环节

很多代码的截取没有太大的必要，具体还是实现的路线，我会贴路径，可以自行顺着逻辑查看
编译阶段当然在 `compiler` 模块进行

1. 首先进入到 `parser` 中 `src\compiler\parser\index.js`
2. 从 `parse` 函数开始，进行 `parseHTML` 的操作
3. 调用 `closeElement` 后，实现 `processElement`
4. 可以看到实现了 `processSlotContent` 和 `processSlotOutlet`，前者主要是设置 slotScope 和 slotTarget，后者是设置 slotName，这些属性都是后面需要使用的内容

parse 大概就这样，细节可以自己看

1. 进入到 codegen 中 `src\compiler\codegen\index.js`
2. genarate
3. new 一个 CodegenState 类
