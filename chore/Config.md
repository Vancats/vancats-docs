---
date created: 2022-03-03 00:35
date updated: 2022-03-17 02:06
---

### .browserslistrc

**可以配置在 package.json 中** "browserslist": []

> 5% 基于全球使用率统计而选择的浏览器版本 5% in US / alt-AS / in my stats

`cover 99.5%` 使用率总和为 99.5% 的浏览器版本，前提是浏览器提供使用覆盖率
`maintained node versions` 所有还被 node 基金会维护的 node 版本

`node 10 and node 10.4` node 10.x.x 或者 10.4.x 版本

`current node` 当前被 browserslistrc 使用的版本

`last 2 versions` 每个浏览器最近的两个版本

`dead` 通过`last 2 versions`筛选的浏览器版本中，全球使用率低于0.5%并且官方声明不在维护或者事实上已经两年没有再更新的版本。目前符合条件的有 `IE10`,`IE_Mob 10`,`BlackBerry 10`,`BlackBerry 7`,`OperaMobile 12.1`

`defaults`：默认配置`> 0.5%, last 2 versions, Firefox ESR, not dead`

### .eslintrc.js

**创建文件**：

`./noee_modules/.bin/eslint --init`，支持js，yaml，yal，json

在 `package.json` 创建 `eslintConfig` 属性，`.eslintrc.js` 优先

**选项说明**

`"root": true` 默认情况一直往上查找配置文件直到根，增加选项后停止在父级寻找

`"parserOptions"` 解析器选项

- "parser"："babel-eslint", // 默认使用 Espree
- "ecmaVersion": 6, // 支持 ES6，但是不代表支持新的 ES6 全局变量等
- "sourceType": "module", // 指定来源的类型，"script"(默认) 或 "module"(ECMAScript 模块)
- "ecmaFeatures" // 使用额外的语言特性
	- "jsx": true, // 启用JSX
	- "globalReturn": true, // 允许全局使用 return
	- "impliedStrict": true, // 启用全局 strict mode
	- "experimentalObjectRestSpread": true, // 启用全局性的 object rest

`"env"` 指定想要启动的环境

- es6: true,
- node: true,
- brower: true, // 浏览器全局变量
- jquery: true

`"globals"`

- template: false,
- _util: false

`"plugins"` 加载插件

`"extends": "eslint: recommended"`从基础配置中继承已启用的规则

`"rules"`

- `"off/0"` 关闭规则
- `"warn/1"` 开启规则，警告级别
- `"error/2"` 开启规则，错误级别，程序会推出

`require.context(directory{string}, useSubdirectories{boolean}, regExp(RegExp))`
`require.context('./test', false,/.test.js$/) 遍历 test 文件夹下的所有 .test.js 文件，不遍历子目录`

### package.json

```json
{
	"name": "my-package",
	"version": "0.1.0",
	"author": "leiqifan <leiqifan@163.com>",
	// 便于 npm search
	"description": "我的 package 文件",
	// 便于 npm search
	"keywords":["node.js","javascript"],
	// 为 true 时 npm 禁止发布
	"private": true,
	// bug 提交地址
	"bugs":{"url":"http://path/to/bug","email":"bug@example.com"},
	"contributors":[{"name":"李四","email":"lisi@example.com"}],
	"repository": {
		"type": "git",
		"url": "https://path/to/url"
	},
	"license":"MIT",
	// 项目包的官网
	"homepage": "http://necolas.github.io/normalize.css",
	"dependencies": {
	    "react": "^16.8.6",
	    "react-dom": "^16.8.6",
	    "react-router-dom": "^5.0.1",
	    "react-scripts": "3.0.1"
	},
	"devDependencies": {
	    "browserify": "~13.0.0",
	    "karma-browserify": "~5.0.1"
	},
	// 暴露给用户的文件
	"files": ["bin", "dist", "client.d.ts", "buildin/", "declarations/", "hot/", "web_modules/", "schemas/", "SECURITY.md"],
	// 可执行文件，表示包要对外提供的脚本，全局安装 -> 可执行文件添加到 PATH 变量（全局可执行），局部安装 -> node_modules/.bin/
	"bin": { "webpack": "./bin/webpack.js" }
	// 项目默认执行文件，require('webpack') 时，默认执行 index.js
	"main": "lib/webpack.js",
	// 
	"browser": "",
	"module": "es/index.js",
	// pre / post
	"script": {},
	// 可以将这些依赖提升到 node_modules 中，避免重复安装，定义在这里的依赖，本地开发时不会引入，可以在 devDependencies 再引入
	"peerDependencies",
	// 将依赖打包到 tgz 文件中
	"bundledDependencies": ["vue", "vue-router"],
	// 配置可选的依赖
	"optionDependencies":
	// eslint 检查文件配置，自动读取验证
	"eslintConfig": { "extends": "react-app" },
	// 在脚本中可通过 npm_package_config_port 引用
	"config": { "port": "8080" }
	// 项目运行平台
	"engines": { "node": ">=0.10.3 <0.12"},
	// 供浏览器使用时，样式文件所在的位置；样式文件打包工具parcelify，通过它知道样式文件的打包位置
	"style": ["./node_modules/tipso/src/tipso.css"]
	"browserslist": {
		"production": [
			">0.2%",
			"not dead",
			"not op_mini all"
		],
		"development": [
			"last 1 chrome version",
			"last 1 firefox version",
			"last 1 safari version"
		]
	}
}
```

![](http://api.fly63.com/vue_blog/public/Uploads/20190611/5cffb5222d1a8.jpg)

### npm CLI

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/3e9300401a4e43baa028814d0358d7d3~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)

```js
npm help

npm init 初始化

npm init xxx
	- npm init @vitejs/app 实际上是调用了 npx @vitejs/create-app
	- 所以需要提供一个 create-xxx 的包
	- npm init xxx 和 npx create-xxx 也是一般CLI工具的常用套路

npm config list/set/get/delete

npm install  --save(-S) 	--save-dev(-D)
	- 如果 NODE_DEV 是 production，或者指定了 --production，就不安装 devDependencies 的包
	- 如果别人 install 你的包，只会安装 dependencies
	- 如果项目中引入了依赖，就会进入 webpack 的 Dependency Graph，会被打包，不管是不是在 devDependencies

npm start 默认启用 node server.js

npm version
	- npm version  major/minor/patch -m "reason for upgrade"
				  主版本/此版本/补丁版本
	- "scripts": { "version": "conventional-changelog -p angular -i CHANGELOG.md -s && git add CHANGELOG.md" } 自动生成 CHANGELOG.md

npm login/adduser 登录 npm

发布 npm 前必须切回 npm 源 --- npm config set registry http://registry.npmjs.org/

npm publish 发布 npm 包，以 version 为界限，因此每次发布 version 必须改变

npm unpublish 移除已发布的包，可以移除整个的，也可以针对下架某个版本

npm pack 将 package 打包成 tgz 格式

npm link
```

### package.json

```json
[{

 // tab 大小为2个空格

 "editor.tabSize": 2,

 // 注释 I/H

 "fileheader.customMade": {

 "Author": "Lqf",

 "Date": "Do not edit",

 "LastEditors": "Lqf",

 "LastEditTime": "Do not edit",

 "Description": "我添加了修改"

 },

 "fileheader.cursorMode": {

 "Author": "Lqf",

 "description": "Annotation",

 "param ": "",

 "return": "null"

 },

 "fileheader.configObj": {

 "autoAdd": true,

 "prohibitAutoAdd": [

 "json",

 "md",

 "js",

 "ts"

 ],

 },

 // 字体，颜色，字号，主题

 "editor.fontFamily": "Fira Code",

 "editor.fontLigatures": true,

 "editor.fontSize": 13,

 "workbench.colorTheme": "Dracula At Night",

 "vsicons.dontShowNewVersionMessage": true,

 "markdown-preview-enhanced.previewTheme": "github-dark.css",

 "markdown-preview-enhanced.automaticallyShowPreviewOfMarkdownBeingEdited": true,

 "workbench.iconTheme": "vscode-icons",

 // 自动保存，滚轮缩放，自动换行，文件删除、移动确认

 "files.autoSave": "onFocusChange",

 "editor.wordWrap": "on",

 "diffEditor.wordWrap": "on",

 "explorer.confirmDelete": false,

 "explorer.confirmDragAndDrop": false,

 // 自动更新路径，删除css，js行末分号，文件引入双引号

 "javascript.updateImportsOnFileMove.enabled": "always",

 "css.completion.completePropertyWithSemicolon": false,

 "less.completion.completePropertyWithSemicolon": false,

 "scss.completion.completePropertyWithSemicolon": false,

 "javascript.format.semicolons": "remove",

 "typescript.format.semicolons": "remove",

 "autoimport.doubleQuotes": true,

 // 括号样式

 "bracket-pair-colorizer-2.depreciation-notice": false,

 "bracket-pair-colorizer-2.highlightActiveScope": false,

 "bracket-pair-colorizer-2.showBracketsInGutter": true,

 "bracket-pair-colorizer-2.showBracketsInRuler": true,

 "bracket-pair-colorizer-2.forceUniqueOpeningColor": true,

 "bracket-pair-colorizer-2.colors": [

 "Gold",

 "Orchid",

 "LightSkyBlue"

 ],

 // 终端以及工作台样式

 "terminal.integrated.fontSize": 13,

 "terminal.integrated.fontFamily": "Hack Nerd Font Mono, Fira Code",

 "workbench.colorCustomizations": {

 "terminal.background": "#212121",

 "terminal.foreground": "#b9a7e4",

 "terminalCursor.background": "#c4bfa5",

 "terminalCursor.foreground": "#81B5AC"

 },

 // vim 配置

 "vim.handleKeys": {

 "<C-a>": false,

 "<C-x>": false,

 "<C-c>": false,

 "<C-v>": false,

 "<C-f>": false,

 "<C-s>": false,

 "<C-z>": false,

 "<C-j>": false,

 "<C-d>": false,

 "<C-y>": false

 },

 // background 配置

 "update.enableWindowsBackgroundUpdates": true,

 "background.customImages": [

 "file:///C:/Users/leiqifan/Desktop/files/file/dm/dm.png" // 图片地址

 ],

 "background.style": {

 "content": "''",

 "pointer-events": "none",

 "position": "absolute", // 图片位置

 "width": "100%",

 "height": "100%",

 "z-index": "99999",

 "background.repeat": "no-repeat",

 "background-size": "20%,20%", // 图片大小

 "opacity": 0.2 // 透明度

 },

 "background.useFront": true,

 "background.useDefault": false,

 // leetcode 配置

 "leetcode.defaultLanguage": "javascript",

 "leetcode.endpoint": "leetcode-cn",

 "leetcode.workspaceFolder": "C:\\Users\\leiqifan\\.leetcode",

 "leetcode.hint.configWebviewMarkdown": false,

 "leetcode.hint.commentDescription": false,

 // 杂项

 "editor.suggestSelection": "first",

 "vsintellicode.modify.editor.suggestSelection": "automaticallyOverrodeDefaultValue",

 "minapp-vscode.disableAutoConfig": true,

 "diffEditor.maxComputationTime": 0,

 "diffEditor.ignoreTrimWhitespace": false,

 "editor.unicodeHighlight.ambiguousCharacters": false,

 // eslint 检测文件类型和工作目录

 "eslint.validate": [

 "vue",

 "html",

 "javascript",

 "typescript",

 "javascriptreact",

 "typescriptreact"

 ],

 "eslint.workingDirectories": [

 ".eslintrc.js",

 {

 "mode": "auto"

 }

 ],

 "[jsonc]": {

 "editor.defaultFormatter": "vscode.json-language-features"

 },

 "[html]": {

 "editor.defaultFormatter": "vscode.html-language-features"

 },

 "[javascript]": {

 "editor.defaultFormatter": "vscode.typescript-language-features"

 },

 "[typescript]": {

 "editor.defaultFormatter": "vscode.typescript-language-features"

 },

 // 配置错误警告

 "vetur.ignoreProjectWarning": true,

 // 保存的时候格式化

 // 保存时eslint修复

 "editor.formatOnSave": true,

 "editor.codeActionsOnSave": {

 "source.fixAll.eslint": true

 },

 "[vue]": {

 "editor.defaultFormatter": "octref.vetur"

 },

 // vutur默认格式

 // "vetur.format.defaultFormatter.js": "prettier",

 "vetur.format.defaultFormatter.js": "vscode-typescript",

 // 函数名称前 加一个空格

 // "javascript.format.insertSpaceBeforeFunctionParenthesis": true,

 "vetur.format.defaultFormatter.html": "prettyhtml",

 "vetur.format.defaultFormatterOptions": {

 "prettyhtml": {

 "printWidth": 120,

 "singleQuote": false,

 "wrapAttributes": false,

 "sortAttributes": false

 },

 "js-beautify-html": {

 "wrap_attributes": "force-expand-multiline",

 "semi": false,

 },

 "prettier": {

 "printWidth": 120, // 指定代码长度，超出换行

 "tabWidth": 2, // tab 键的宽度

 "useTabs": false, // 不使用tab

 "semi": false,

 "singleQuote": true,

 "quoteProps": "as-needed",

 "jsxSingleQuote": false, // 在jsx中使用单引号代替双引号

 "trailingComma": "none", //禁止随时添加逗号,这个很重要

 "bracketSpacing": true,

 "jsxBracketSameLine": false, // 在jsx中把'>' 是否单独放一行

 "arrowParens": "avoid", // 为单行箭头函数的参数添加圆括号

 "requirePragma": false, // 是否严格按照文件顶部的特殊注释格式化代码

 "insertPragma": false, // 是否在格式化的文件顶部插入Pragma标记，以表明该文件被prettier格式化过了

 "proseWrap": "always", // 文本样式进行折行

 "htmlWhitespaceSensitivity": "ignore", // html文件的空格敏感度，控制空格是否影响布局

 "endOfLine": "auto" // 结尾是 \n \r \n\r auto

 }

 },

 "javascript.validate.enable": false,

 "workbench.startupEditor": "none",

}](<{
  // tab 大小为2个空格
  "editor.tabSize": 2,
  // 注释 I/H
  "fileheader.customMade": {
    "Date": "Do not edit",
    "LastEditTime": "Do not edit",
    "Description": "我添加了修改"
  },
  "fileheader.cursorMode": {
    "description": "Annotation",
    "param ": "",
    "return": ""
  },
  "fileheader.configObj": {
    "autoAdd": true,
    "prohibitAutoAdd": [
      "json",
      "md",
      "js",
      "ts",
      "html",
      "css",
      "less",
      "scss",
      "jsx"
    ],
  },
  // 字体，颜色，字号，主题
  "editor.fontFamily": "Fira Code",
  "editor.fontLigatures": true,
  "editor.fontSize": 13,
  "workbench.colorTheme": "Dracula At Night",
  "vsicons.dontShowNewVersionMessage": true,
  "markdown-preview-enhanced.previewTheme": "github-dark.css",
  "markdown-preview-enhanced.automaticallyShowPreviewOfMarkdownBeingEdited": true,
  "workbench.iconTheme": "vscode-icons",
  // 自动保存，滚轮缩放，自动换行，文件删除、移动确认
  "files.autoSave": "onFocusChange",
  "editor.wordWrap": "on",
  "diffEditor.wordWrap": "on",
  "explorer.confirmDelete": false,
  "explorer.confirmDragAndDrop": false,
  // 自动更新路径，删除css，js行末分号，文件引入双引号
  "javascript.updateImportsOnFileMove.enabled": "always",
  "css.completion.completePropertyWithSemicolon": false,
  "less.completion.completePropertyWithSemicolon": false,
  "scss.completion.completePropertyWithSemicolon": false,
  "javascript.format.semicolons": "remove",
  "typescript.format.semicolons": "remove",
  "autoimport.doubleQuotes": true,
  // 括号样式
  "bracket-pair-colorizer-2.depreciation-notice": false,
  "bracket-pair-colorizer-2.highlightActiveScope": false,
  "bracket-pair-colorizer-2.showBracketsInGutter": true,
  "bracket-pair-colorizer-2.showBracketsInRuler": true,
  "bracket-pair-colorizer-2.forceUniqueOpeningColor": true,
  "bracket-pair-colorizer-2.colors": [
    "Gold",
    "Orchid",
    "LightSkyBlue"
  ],
  // 终端以及工作台样式
  "terminal.integrated.fontSize": 13,
  "terminal.integrated.fontFamily": "Hack Nerd Font Mono, Fira Code",
  "workbench.colorCustomizations": {
    "terminal.background": "#212121",
    "terminal.foreground": "#b9a7e4",
    "terminalCursor.background": "#c4bfa5",
    "terminalCursor.foreground": "#81B5AC"
  },
  // vim 配置
  "vim.handleKeys": {
    "<C-a>": false,
    "<C-x>": false,
    "<C-c>": false,
    "<C-v>": false,
    "<C-f>": false,
    "<C-s>": false,
    "<C-z>": false,
    "<C-j>": false,
    "<C-d>": false,
    "<C-y>": false
  },
  // code spell 配置
  "cSpell.userWords": [
    "mixins",
    "vite",
    "vitejs",
    "mockjs",
    "vuedx",
    "leiqifan",
    "execa",
    "nums",
    "NlogN",
    "NlgN"
  ],
  // background 配置
  "update.enableWindowsBackgroundUpdates": true,
  "background.customImages": [
    "file:///C:/Users/leiqifan/Desktop/files/file/dm/dm.png" // 图片地址
  ],
  "background.style": {
    "content": "''",
    "pointer-events": "none",
    "position": "absolute", // 图片位置
    "width": "100%",
    "height": "100%",
    "z-index": "99999",
    "background.repeat": "no-repeat",
    "background-size": "20%,20%", // 图片大小
    "opacity": 0.2 // 透明度
  },
  "background.useFront": true,
  "background.useDefault": false,
  // leetcode 配置
  "leetcode.defaultLanguage": "javascript",
  "leetcode.endpoint": "leetcode-cn",
  "leetcode.workspaceFolder": "C:\\Users\\leiqifan\\.leetcode",
  "leetcode.hint.configWebviewMarkdown": false,
  "leetcode.hint.commentDescription": false,
  // 杂项
  "editor.suggestSelection": "first",
  "vsintellicode.modify.editor.suggestSelection": "automaticallyOverrodeDefaultValue",
  "minapp-vscode.disableAutoConfig": true,
  "diffEditor.maxComputationTime": 0,
  "diffEditor.ignoreTrimWhitespace": false,
  "editor.unicodeHighlight.ambiguousCharacters": false,
  // eslint 检测文件类型和工作目录
  "eslint.validate": [
    "vue",
    "html",
    "javascript",
    "typescript",
    "javascriptreact",
    "typescriptreact"
  ],
  "eslint.workingDirectories": [
    ".eslintrc.js",
    {
      "mode": "auto"
    }
  ],
  "[jsonc]": {
    "editor.defaultFormatter": "vscode.json-language-features"
  },
  "[html]": {
    "editor.defaultFormatter": "vscode.html-language-features"
  },
  "[javascript]": {
    "editor.defaultFormatter": "vscode.typescript-language-features"
  },
  "[typescript]": {
    "editor.defaultFormatter": "vscode.typescript-language-features"
  },
  // 配置错误警告
  "vetur.ignoreProjectWarning": true,
  // 保存的时候格式化
  // 保存时eslint修复
  "editor.formatOnSave": true,
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": true
  },
  "[vue]": {
    "editor.defaultFormatter": "octref.vetur"
  },
  // vutur默认格式
  // "vetur.format.defaultFormatter.js": "prettier",
  "vetur.format.defaultFormatter.js": "vscode-typescript",
  // 函数名称前 加一个空格
  // "javascript.format.insertSpaceBeforeFunctionParenthesis": true,
  "vetur.format.defaultFormatter.html": "prettyhtml",
  "vetur.format.defaultFormatterOptions": {
    "prettyhtml": {
      "printWidth": 120,
      "singleQuote": false,
      "wrapAttributes": false,
      "sortAttributes": false
    },
    "js-beautify-html": {
      "wrap_attributes": "force-expand-multiline",
      "semi": false,
    },
    "prettier": {
      "printWidth": 120, // 指定代码长度，超出换行
      "tabWidth": 2, // tab 键的宽度
      "useTabs": false, // 不使用tab
      "semi": false,
      "singleQuote": true,
      "quoteProps": "as-needed",
      "jsxSingleQuote": false, // 在jsx中使用单引号代替双引号
      "trailingComma": "none", //禁止随时添加逗号,这个很重要
      "bracketSpacing": true,
      "jsxBracketSameLine": false, // 在jsx中把'>' 是否单独放一行
      "arrowParens": "avoid", // 为单行箭头函数的参数添加圆括号
      "requirePragma": false, // 是否严格按照文件顶部的特殊注释格式化代码
      "insertPragma": false, // 是否在格式化的文件顶部插入Pragma标记，以表明该文件被prettier格式化过了
      "proseWrap": "always", // 文本样式进行折行
      "htmlWhitespaceSensitivity": "ignore", // html文件的空格敏感度，控制空格是否影响布局
      "endOfLine": "auto" // 结尾是 \n \r \n\r auto
    }
  },
  "javascript.validate.enable": false,
  "workbench.startupEditor": "none",
  "cSpell.allowCompoundWords": true,
}>)
```
