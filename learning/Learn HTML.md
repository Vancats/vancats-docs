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
- BackCompat：怪异模式，页面以向后兼容方式显示
`<!Doctype html>` 标准模式，使用HTML5解析渲染（HTML5没有严格和混杂之分）

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



###### img 的 srcset属性的作用
根据屏幕的密度设置不同的图片
`<img src="image-128.png" srcset="image-128.png 128w, image-256.png 256w, image-512.png 512w" sizes="(max-width: 360px) 340px, 128px" />
- w可以理解成图片质量
- sizes设置图片临界尺寸，如上默认128px，大于360px则设置成340px

###### 行内，块，空元素
- 行内：`a b span img input select strong`
- 块：`div ul ol li dl dt dd p h1~6`
- 空（无闭合标签）：`br hr img input link meta   area col colgroup base command embed keygen param source track wbr`

###### iframe的优缺点
iframe会创建一个包含另外一个文档的内联框架（行内框架）
**优点**
- 加载速度较慢的内容，如广告
- 使脚本并行下载
- 实现跨子域通信
**缺点**
- 阻塞主页面的onload事件
- 无法被搜索引擎识别
- 产生很多页面，不易管理

###### Canvas 和 SVG
**SVG**：矢量图，基于XML描述的2D图形的语言，因此每个DOM都可用
- 不依赖分辨率
- 支持事件处理器
- 适合大型渲染区域的应用程序，如谷歌地图
- 复杂度高减慢渲染速度（任何过度使用DOM的应用都不快
- 不适合游戏应用

**Canvas**：画布，通过JS绘制2D图形，位置发生改变重新绘制
- 依赖分辨率
- 不支持事件处理器
- 弱的文本渲染能力
- 能够以 .jpg 或 .png 格式保存图像
- 适合图像密集型的游戏，许多对象可以频繁重绘

###### head标签的作用
- 定义文档头部
- 引用资源，样式表
- 描述文档信息：元信息，文档标题，在Web中的位置以及和其他文档的关系
`base link meta script style title(必需)`

###### web worker（可扩展）
在正常的HTML页面中，执行js脚本时不可响应。web worker是运行在后台的js，不影响页面性能，通过postMessage传回结果。

###### HTML5 离线储存
**原理**

###### HTML5 有哪些更新 drag

###### 渐进增强和优雅降级
- 优雅降级针对最高级最完善的浏览器构建页面，开发周期的最后阶段测试过时浏览器。旧版浏览器只提供“简陋且无妨”的浏览，只修复较大错误
- 渐进增强关注内容本身，被 Yahoo 所采纳并用以构建其“分级式浏览器支持 (Graded Browser Support)”策略
###### title/h1 strong/b i/em区别
- h1层次明确的标题，title只是表示标题
- strong加强语气，b只是加粗
- em为强调的文本，i只是斜体
###### label的作用
定义表单与控件的关系，选择标签时焦点转移到相应控件
`<label for="mobile">Number:</label> <input id="mobile type="text">" 
`<label>Date:<input type="text"></label>`