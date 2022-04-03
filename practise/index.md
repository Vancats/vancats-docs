---
date created: 2022-03-30 20:25
date updated: 2022-03-31 13:45
---

#### src 与 href

1. src 是资源引用，会下载该资源并直接嵌入到该标签位置，解析时会暂停其他资源的下载和处理，例如 js
2. href 是链接引用，会并行下载，例如 a，link

#### 语义化

1. 方便解析，例如盲人解析
2. 对机器友好，适合爬虫，利于SEO搜索引擎
3. 利于开发，增加可读性

#### DOCTYPE 的作用

1. 帮助确认使用哪种规范进行解析，如果没指定就是使用浏览器自身规范
2. CSS1Compat：标准模式；BackCompat：怪异模式，向后兼容
3. `<!Doctype html>` 标准模式，使用 HTML5 解析，HTML5 没有严格混杂的区别

#### defer 和 asyc 的区别

1. 两者都是异步下载文件的参数
2. async 是下载完直接执行，defer 下载完之后是在文档解析之后，DOMContentLoader 触发前执行
3. async 不能确保加载顺序，defer 可以

#### meta 标签

1. viewport：移动端适配
		`<meta name="viewport" content="width=device-width,initial-scale=1,maximum-scale=1;minimum-scale=1,user-scalable=no">`
2. charset：编码类型
		`<meta charset="uft-8"`
3. keyword：利于 SEO
		`<meta name="keyword" content="hello">`
4. describe：利于 SEO
		`<meta name="describe" content="你好">`
5. refresh：页面一秒后**刷新**并**重定向**到百度
		`<meta http-equiv="refresh" content="1;url=http://www.baidu.com"`
6. robots：(no)index 文件（不可）检索 (no)follow 页面链接（不）可查询，all，none
		`<meta name="robots" content="index,follow">`

#### 行内、块、空元素

1. 行内元素：**span、img、input、select、a、i、b**
2. 块元素：**div、p、h、ul、ol、li、dl、dt、dd**
3. 空元素：**meta、input、img、link、br、hr**
