---
date created: 2022-05-31 23:46
date updated: 2022-06-03 19:56
---

1. chunk assets file
2. html-webpack-plugin
3. 环境变量
   1. mode 直接设置传递给模块
   2. --mode 传值给模块
   3. --env 传值给配置函数的第一个参数
   4. Webpack.DefinePlugin 里面的内容提供给模块使用，只是单纯的常量，与 process.env 无关
   5. cross-env 完美的解决方案
4. raw-loader style-loader css-loader less-loader file-loader url-loader
5. 资源模块 asset / source - resource - inline
6. babel-loader @babel/core 转换代码的引擎 有 transform 方法 @babel/preset-env 具体转换规则
7. source-map
   1. inline base64行内嵌套
   2. hidden 没有关联关系，可调试
   3. eval 每个模块都用 eval 包括实现，利于缓存
   4. nosources 有source-map，也能找到位置，但是是空的
   5. cheap 只有行映射，不包含 loader
   6. cheap-module 只有行映射
