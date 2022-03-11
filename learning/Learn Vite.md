---
date created: 2022-03-03 17:03
date updated: 2022-03-11 21:23
---

### 开始

#### 优势

- ESM，只加载首页需要用到的文件
- 基于 ESBuild，速度非常快
- 兼容 rollup 插件，生态完整

#### 初始化

- Vue3: `npm init vite`

- JSX : `yarn add @vitejs/plugin-vue-jsx -D` 直接导入并使用 vueJsx函数即可

- React
	- react-hot-loader -> FastRefresh 解决了遗留问题，更快，局部更新

### CSS

#### 原生 CSS variable

```css
/* @import alias: { '@styles': '/src/styles' } */
@import url(@styles/other.css)

/* 命名空间 */
:root {
	--main-bg-color: red;
}
.root {
	color: var(--main-bg-color);
}
```

#### POSTCSS

```js
module.exports = {
	// 需要安装
	plugins: [require('autoprefixer'), require('@postcss-plugins/console')]
}

// index.css
.root {
	@console.error hello root123
	color: red;
}
```

#### CSS Modules

```js
// app.module.css
.root {
	color: 'red';
}

import app from 'app.module.css'

app.root // 使用
```

#### CSS pre-processors

```js
npm i less
```

### Plugin

**按照以下顺序调用钩子**

config: 修改 Vite 配置

configResolved: Vite 配置确认

configureServer: 配置 devServer

transformIndexHtml: 用于转换宿主页

resolveId: 创建自定义确认函数，常用于定位第三方依赖

load: 创建自定义加载函数，可用于返回自定义内容

transform: 可用于转换已加载的模块内容

handleHotUpdate: 自定义 HMR 更新时调用

### Typescript

Vite 只编译，不校验

tsc --noEmit

```json
// tsconfig.json
{
	"compilerOptions": {
		"target": "esnext",
		"module": "esnext",          // vite 模块加载必须使用 module
	    "moduleResolution": "node",  // 通过 node 方式来解析模块
		"strict": true,
	    "jsx": "preserve",           // 配置成不编译 jsx 语法；如果不配置默认遵循 React 规范，与 Vue 的不符
	    "sourceMap": true,
	    "resolveJsonModule": true,   // 是否可以直接 import JSON
	    "esModuleInterop": true,     // 方便引入 可以省略括号内容 import (* as )React form 'React'
	    "isolatedModules": true,
	    "lib": ["esnext", "DOM"]，   // 支持使用nei
	},
	"include": [
	    "src/**/*.ts",
	    "src/**/*.d.ts",
	    "src/**/*.tsx",
	    "src/**/*.vue",
	]
}


// package.json
{
	"scripts": {
		"build": "vue-tsc --noEmit && tsc --noEmit && vite build"
	}
}
```
