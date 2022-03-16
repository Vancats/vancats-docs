---
date created: 2022-03-03 17:03
date updated: 2022-03-16 23:06
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
	extends: "standard",
	globals: {
		postMessage: true
	}
}


// .prettierrc
{
	"semi": false,
	"singleQuote": false
}


// package.json
"lint": "eslint --ext js src/,
"build": "npm run lint && ..."


// 安装 husky
yarn add husky -D
npx husky install
npx husky add .husky/pre-commit "npm run lint"
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

### HMR 热更新

```js
// 实际使用的是 import.meta.hot.accept
function render() {
  document.querySelector('#app').innerHTML = `
    <h1>Hello Vite!</h1>
    <a href="https://vitejs.dev/guide/features.html" target="_blank">Documentation12312123</a>
    <div>hell123o</div>
  `
}
render()
// 生产中不需要这个
if (import.meta.hot) {
  // 接收模块并调用该函数
  import.meta.hot.accept((newModule) => {
    newModule.render()
  })
}

// 当改变了会发送请求 main.js?import&t=1647353326355
// websorket 中的更新内容：
{
	type: "update"
	updates: [{type: "js-update", timestamp: 1647353326355, path: "/main.js", acceptedPath: "/main.js"}]
 }
```

### glob

### 预编译

1. 会生成一个 `.vite` 文件，里面存放是各个依赖，之后需要加载文件时，可以直接从这里读取
2. 会自动处理 `Common JS`，缓存一个处理过的文件，再进行导入
3. 文件打包在一起，方便后续的使用，如 `lodash` 依赖中有上百文件，如果不进行处理，每次浏览器刷新时都会重新加载
4. 第三方的依赖会进行缓存，可以看到 `cache-control` 是开启的，而 `main.js、client` 等文件则是 `no-cache`
5. 可以在 `vite.config.js` 中加入 `optimizeDeps` 对象属性，里面有 `include，exclude`，如果有第三方依赖加载进了 `cjs`，需要单独引入

### Vite 配置项

```js
"root": 项目根路径
"base": 请求前缀
"mode"
"define": 定义全局常量替换方式
"plugins"
"publicDir": 静态资源服务的文件夹
"cacheDir": 默认 node_modules/.vite
"resolve.alias"
"resolve.dedupe": 在应用程序中有不同依赖的副本，强制 Vite 解析指定副本
"resolve.conditions": 对于不同的包有不同的导出方式时
	"exprts": {
		".": {
			"import": "./index/esm.js",
			"require": "./index.cjs.js"
		}
	}
"resolve.mainFields": 指定关键字读取文件 -- main ｜ module
"resolve.extensions": ['.mjs', '.js', '.ts', '.jsx', '.tsx', '.json'] 从左向右读取文件
"css.modules": 选项将被传递给 postcss-modules
"css.postcss": 格式同 postcss.config.js
"css.preprocessorOptions": 与处理器选项
"json.namedExports": 支持 .json 按名导入
"json.stringify": 导入的 json 将被转换为 export default JSON.parse('...')
"esbuild"
"assetsInclude": 指定其他的文件类型为静态资源
"logLevel": 打印日志级别
"clearScreen": 设置为 false 可以避免清屏而错过信息
"envDir": 更改 env 文件存放路径
"envPrefix": 以其开头的环境变量，通过 import.meta.env 暴露在客户端源码中，不能置为空字符串

server 相关...

构建相关...
```

## rollup

1. 以 ESM 标准为目标的构建工具，不支持 `require/cjs`，需要 `resolveNode` 工具转换
2. Tree Shaking，只打包使用的代码

### 基本命令

```js
--input(-i) + 入口文件，多入口就写多个-i
	rollup -i index.js
	
--file + 单入口的出口文件名称
	rollup -i index.js --file dist.js
--dir + 多入口的出口文件夹
	rollup -i index.js -i a.js --dir dist

--format(-f) 后面跟 `cjs/iife/es/umd`；如果是 `umd` 格式，需要加上 `--name` 添加全局变量名称
	rollup -i index.js -i a.js --dir dist -f cjs/lift/es/umd --name 'Index'

--watch 监听文件变化
--config(-c) 确定配置项
	rollup --config rollup.config.js
	
--environment TEST:123 配置环境变量 TEST，值为 123
	rollup --config rollup.config.js --environment TEST:123
	
--plugin json 使用 `@rollup/plugin-json`
	rollup --config rollup.config.js --plugin json 全局命令
	./node_modules/.bin/rollup --config rollup.config.js --plugin json 项目命令
```

### 配置文件

```json
// 使用后可导入 json
import json from '@rollup/plugin-json'
// 如果没有 resolve，第三方依赖无法一起打包
import resolve from '@rollup/plugin-node-resolve'
// React 使用的是 commonjs 的 buddle，需要解析
import commonjs from '@rollup/plugin-commonjs'
// 压缩文件
import { terser } from 'rollup-plugin-terser'

// 默认是esm，如果使用 module.export = {} 需要更改名称为 rollup.config.cjs
export default [
  {
    input: 'index.js',
    // external 中的依赖不会被一起打包
    // external: { 'react': 'React' }, // 如果是对象需要自己加上最后的名称
    external: ['react'],
    // 注意前后顺序，前面的先加载
    plugins: [resolve(), commonjs(), json()],
    // 也可以是数组形式，但是建议使用外面的大数组
    output: [
      {
        file: 'dist/index.es.js',
        format: 'es',
        // 这里也可以加载少部分插件，如压缩
        // plugins: [terser()],
        banner: '/** Hello World **/'
      },
      {
        file: 'dist/index.umd.js',
        format: 'umd',
        name: 'Index'
      }
    ]
  },
  // {
  //   input: 'index.js',
  //   plugins: [resolve(), commonjs(), json()],
  //   output: {
  //     file: 'dist/index.umd.js',
  //     format: 'umd',
  //     name: 'Index'
  //   }
  // },
]
```
