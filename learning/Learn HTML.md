###### src 和 href
- src 资源引用，下载资源并嵌入当前标签位置，如请求 js。解析时会暂停其他资源的下载和处理，所以一般 js 会在页面底部
- href 超文本引用，建立当前文档与该网络资源的链接关系。识别时会并行下载，常用于 a，link

###### 语义化
- 对开发者友好，增加可读性，结构清晰，利于维护
- 对机器友好，适合搜索引擎爬虫爬取信息，利于SEO
- 利于设备解析，自动生成目录，例如盲人解析

###### DOCTYPE 的作用
告诉浏览器以什么样的文档类型来解析文档，会影响CSS甚至JS的解析，通过document.compatMode获取
 - CSS1Compat：标准模式，页面以最高标准呈现
 - BackCompat：怪异模式，页面以向后兼容方式显示呃呃

###### defer和async的区别
没有这俩，浏览器立即执行，阻塞后续文档加载
- 执行时间：async立即加载并执行，defer立即加载，文档解析完成后，DOMContentLoader触发前执行
- 执行顺序：async不能保证加载的顺序，defer按顺序加载

###### meta标签
- charset：描述文档的编码类型 `<meta charset="UTF-8">`
- keyword：页面关键词 `<meta name="keyword" content="关键词搜索">`
- description：页面描述 `<meta name="discription" content="页面描述内容">`
- refresh：重定向和刷新
`<meta http-equiv="refresh" content="1;url='http://www.baidu.com'"`  页面1s后跳转到百度
- viewport：移动端适配
`<meta name="viewport" content="width=device-width,height=device-height,initial-scale=1,maximum-scale=1,minimum-scale=1,user-scalable=no"`
- robots：搜索引擎索引方式 `<meta name="robots" content="index,follow">`
	- all：文件将被检索，页面上的链接可以被查询
	- none：文件不被检索，页面上的链接不可以被查询
	- index：文件将被检索
	- follow：页面上的链接可以被查询
	- noindex：文件将不被检索
	- nofollow：页面上的链接不可以被查询
