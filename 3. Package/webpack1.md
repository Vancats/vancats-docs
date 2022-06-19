---
date created: 2022-05-31 23:46
date updated: 2022-06-19 20:47
---

1. webpack的环境变量都需要怎么设置，最优方案
2. 解析 css 都有哪些loader
3. babel-loader polyfill的几种实现方式
4. source-map 以及最佳实现
5. 如何引入第三方库

##

3. chunk assets file
4. html-webpack-plugin
7. 资源模块 asset / source - resource - inline
8. babel-loader @babel/core 转换代码的引擎 有 transform 方法 @babel/preset-env 具体转换规则

11. 直接引入 模块内需要引入
12. ProvidePlugin 模块内不需要引入
13. expose-loader 模块内至少引用一次 会把变量挂载到 window
14. externals 不需要模块引入，直接引用CDN
15. HTMLWebpackExternalsPlugin
16. watch watchOptions
17. Webpack.BannerPlugin CopyWebpackPlugin CleanWebpackPlugin
18. proxy before after webpackdevmiddleware
19. Mini-Css-Extract-Plugin
20. css 图片路径配置
21. hash chunkhash contenthash
22. postcss-loader postcss-preset-env
23. importLoaders
24. css 压缩 OptimizeCssAssetWebpackPlugin
25. 图片 压缩 image-webpack-loader
26. js 压缩 Webpack.TerserPlugin
27. html 压缩 html-webpack-plugin
28. px2rem 750px -> 10 rem /  lib-flexible 自动计算 1rem = 多少 px  移动端自适应，在index.html配置font-size
29. preset-env 只转换语法，不转换方法和API
30. babel-polyfill useBuiltIns false /  entry 全局 core-js 2 / 3   /     usage 局部
31. babel-runtime 需要手动引入
32. @babel/plugin-transform-runtime 自动分析并局部引入
33. @babel/runtime-corejs3 corejs3 helpers regenerator
34. 最佳配置
    1. babel-runtime 适合组件和库，局部引入
    2. babel-polyfill 业务使用，不怕污染全局
    3. 局部引入 不污染全局，增加文件体积
    4. 全局引入 污染全局，减少文件体积
35. 万能配置：polyfill.io 接受对一组浏览器功能的请求并仅返回请求浏览器所需的 polyfill 的服务，可以在Chrome和IE直接打开查看 <https://polyfill.io/v3/polyfill.min.js>
