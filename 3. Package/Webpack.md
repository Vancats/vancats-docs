---
date created: 2022-05-31 23:46
date updated: 2022-06-25 00:30
---

@babel/core 核心代码，包括 transform parse
babylon babel 解析器
babel-types 用于 AST 节点的 Lodash 式工具库，包括构造，验证以及变换 AST 节点方法
babel-traverse 遍历 AST，负责替换，移除和添加节点

1. 读取配置对象，读取 shell 配置对象并合并参数
2. 创建 Compiler
   1. 保存 options
   2. 创建 this.hooks
   3. 初始化内部对象
3. 挂载所有插件
4. 执行 Compiler run 方法
5. 编译入口模块
6. 读取模块内容，找到与模块对应的所有 loaders
7. 调用所有 loader 转换文件内容
8. 创建入口模块对象
	1. 模块 ID：相对于根路径的相对路径

---

1. webpack的环境变量都需要怎么设置，最优方案
2. 解析 css 都有哪些loader
3. css-loader 的 importLoaders
4. css 怎么分离文件
5. babel-loader polyfill的几种实现方式
6. source-map 以及最佳实现
7. 如何引入第三方库，五种方式
8. devServer mock以及其中间件形式
9. 有几种 hash
10. html css js 图片压缩方式
11. 如何实现css移动端适配
12. 资源模块是什么
13. 有哪些常用插件
