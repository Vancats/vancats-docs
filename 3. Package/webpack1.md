---
date created: 2022-05-31 23:46
date updated: 2022-06-01 00:10
---

1. chunk assets file
2. html-webpack-plugin
3. 环境变量
   1. --mode 传值给模块
   2. --env 传值给配置函数的第一个参数
   3. Webpack.DefinePlugin 里面的内容提供给模块使用，只是单纯的常量，与 process.env 无关
   4. cross-env 完美的解决方案
