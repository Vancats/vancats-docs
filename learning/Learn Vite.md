---
date created: 2022-03-03 17:03
date updated: 2022-03-08 20:39
---

#### Vite 按照以下顺序调用钩子

config: 修改 Vite 配置

configResolved: Vite 配置确认

configureServer: 配置 devServer

transformIndexHtml: 用于转换宿主页

resolveId: 创建自定义确认函数，常用于定位第三方依赖

load: 创建自定义加载函数，可用于返回自定义内容

transform: 可用于转换已加载的模块内容

handleHotUpdate: 自定义 HMR 更新时调用

#### 优势

- ESM，只加载首页需要用到的文件
- 基于 ESBuild，速度非常快
- 兼容 rollup 插件，生态完整


#### Vue3
- 初始化 `npm init @vitejs/app`
- JSX `@vitejs/plugin-vue-jsx -D` 直接导入并使用 vueJsx函数即可 