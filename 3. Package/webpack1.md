---
date created: 2022-05-31 23:46
date updated: 2022-06-19 20:53
---

1. webpack的环境变量都需要怎么设置，最优方案
2. 解析 css 都有哪些loader
3. css-loader 的 importLoaders
4. css 分离出文件
5. babel-loader polyfill的几种实现方式
6. source-map 以及最佳实现
7. 如何引入第三方库，五种方式
8. devServer mock以及其中间件形式
9. 有几种 hash
10. html css js 图片压缩方式
11. 如何实现css移动端适配

##

3. chunk assets file

4. html-webpack-plugin

5. 资源模块 asset / source - resource - inline
16. preset-env 只转换语法，不转换方法和API

17. babel-polyfill useBuiltIns false /  entry 全局 core-js 2 / 3   /     usage 局部

18. babel-runtime 需要手动引入

19. @babel/plugin-transform-runtime 自动分析并局部引入

20. @babel/runtime-corejs3 corejs3 helpers regenerator

21. 最佳配置
    1. babel-runtime 适合组件和库，局部引入
    2. babel-polyfill 业务使用，不怕污染全局
    3. 局部引入 不污染全局，增加文件体积
    4. 全局引入 污染全局，减少文件体积

22. 万能配置：polyfill.io 接受对一组浏览器功能的请求并仅返回请求浏览器所需的 polyfill 的服务，可以在Chrome和IE直接打开查看 <https://polyfill.io/v3/polyfill.min.js>
