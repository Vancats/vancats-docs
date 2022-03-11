---
date created: 2022-03-03 17:03
date updated: 2022-03-12 00:50
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

**Vite 只编译，不校验**

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
	    "lib": ["esnext", "DOM"]，   // 支持使用哪些内置的 module
	    "isolatedModules": true,     // 见下方
	    "types": ["vite/client"],    // import.meta
	},
	"include": ["src/**/*.ts", "src/**/*.d.ts", "src/**/*.tsx", "src/**/*.vue"]
}


// package.json
// 需要安装 typescript vue-tsc
"scripts": {
	"build": "vue-tsc --noEmit && tsc --noEmit && vite build"
}
```

> 关于 isolatedModules 配置项

```js
/* 
	vite 编译时会清除环境常量枚举，使得无法访问而报错 
	vite 编译时，interface 会被清除，在 test2 导出时会报错
	写一个没有 import 也没有 export 的文件会报错
*/

// test1.ts
export interface A { name: string, age: number }

// test2.ts
import { A } from './test1.ts'
declare const enum Num { First = 0, Second = 1 }

export const a: A = { name: 'Lqf', age: Num.First }
export { A } // => export { type A }

```

### Assets

#### 图片

- 链接引入
- import 引入

#### 普通文件

```js
// 1. url
import test from './test?url'
// 打印该文件的相对路径 /src/test.ts

// 2. raw
import test from './test?raw'
// 直接打印该文件的源文件内容

// 3. worker webworker 单开一个线程
// worker.js
var i = 0
function timeCount() {
	i = i + 1
	postMessage(i)
	setTimeout(timeCount, 500)
}
timeCount()

// main.js
import Worker from './worker?worker'
const worker from 'Worker'
worker.onmessage = function(e) {
	console.log(e)
}
```

#### JSON

```js
import pkg from './package.json' // 可以解构
pkg.version
```

#### Web assembly

```js
import init from './fib.wasm'
init().then(m => console.log(m.fib(10)))

// assembly.ts => fib.wasm
export function fib(n: i32): i32 {
	var a = 0, b = 1
	if (n > 0) {
		while (--n) {
			let t = a + b
			a = b
			b = t
		}
		return b
	}
	return a
}
```

### ESLint + Prettier

```js
// .eslintrc.js
module.exports = {
	extends: "standard"
}


// .prettierrc
{
	"semi": false,
	"singleQuote": false
}


// package.json
"lint": "eslint --ext js src/
```

### ENV

```js
import.meta.env // 打包后也是直接输出这些内容而不是变量
{ BASE_URL: '/', MODE: 'development', DEV: true, PROD: false, SSR: false }

.env
.env.develpment
.env.develpment.local
.env.productuon
新增需要加上 VITE_ 前缀，如 VITE_TITLE

可以更改位置和名称
.env.test
VITE_TITLE=TEST
"dev": "vite --mode test" // 切换成 .env.test 文件

```
