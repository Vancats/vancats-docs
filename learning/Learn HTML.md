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
基于一个 .appcache 文件的缓存机制（不是存储技术），通过这个文件的解析清单离线存储资源。当离线时可以正常访问站点或应用，联网后更新缓存文件

**使用方法**
1. 创建和 html 同名的 manifest，在页面头部加入 manifest 时 `<html lang="en" manifest="index.manifest">`
2. 在 cache.manifest 文件中编写需要离线存储的资源
```
CACHE MANIFEST
    #v0.11
    CACHE:
    js/app.js
    css/style.css
    NETWORK:
    resourse/logo.png
    FALLBACK:
    / /offline.html
```
- CACHE：离线存储的资源列表，包含 manifest 文件的页面自动离线存储，不需要列出页面自身
- NETWORK：在线资源，不被缓存。CACHE存在该资源，则缓存，它的优先级更高
- FALLBACK：访问第一个资源失败，使用下面的资源来替换，上文表示访问根目录下任何一个资源失败，去访问 offline.html
3. 离线状态时，操作 window.applicationCache 进行离线存储的操作

**如何更新缓存**
1. 更新 manifest 文件
2. 通过 js 缓存
3. 清除浏览器缓存

**注意事项**
1. 浏览器对缓存数据的容易不同，如每个站点5MB
2. 如果 manifest 文件获取内部任意文件下载失败，整个更新过程就失败，浏览器继续使用原缓存
3. 引用 manifest 的 html 必须和 manifest 文件同源，同一个域
4. FALLBACK 中的资源必须和 manifest 文件同源
5. 资源缓存后，浏览器直接请求其绝对路径也会从缓存中访问
6. 站点其他页面的资源，即使没有 manifest 属性，请求的资源在缓存中也会从缓存读取
7. manifest 文件改变，资源请求本身也会触发更新

**离线资源管理**
- 在线时，如果 html 有 manifest 属性，请求 manifest 文件。第一次访问浏览器会根据上文情况离线缓存。已经进行存储，浏览器使用离线资源加载，并对比新旧 manifest 文件，如果改变重新缓存
- 离线时，直接使用离线资源缓存

###### HTML5 有哪些更新
1. 语义化标签
2. 媒体标签
	- audio `<audio src='' controls autoplay loop='true'></audio>`
	- video `<video src='' poster='imgs/aa.jpg' controls width="x" height="x"></video>` poster 封面（默认第一帧）
	- source 浏览器对视频格式支持程度不一样，为了能够兼容不同的浏览器，可以通过 source 来指定视频源
`<video><source src='a.flv' type='video/flv'></source> <source src='b.mp4' type='video/mp4'></source></video>`
3. 表单
	**表单类型**
	- email：验证邮箱合法性
	- url：验证URL
	- number
	- search：自带删除
	- range
	- color
	- time
	- date
	- datetime
	- datetime-local
	- week
	- mouth

	**表单属性**
	- placeholder ：提示信息
	- autofocus ：自动获取焦点
	- autocomplete=“on” 或者 autocomplete=“off” 使用这个属性需要有两个前提：
		- 表单必须提交过
		- 必须有 name 属性。
	- required：要求输入框不能为空，必须有值才能够提交。
	- pattern=" " 里面写入想要的正则模式，例如手机号 patte="^(+86)?\d{10}$"
	- multiple：可以选择多个文件或者多个邮箱
	- form=" form 表单的 ID"

	**表单事件：**
	- oninput 每当 input 里的输入框内容发生变化都会触发此事件
	- oninvalid 当验证不通过时触发此事件

4. 进度条，度量器
	- progress（任务进度）：max/value
	- meter（显示剩余容量）：high/low 高/低的范围 max/min 最大/小值 value 当前值
5. DOM查询 querySelector(All)
6. Web存储 localStorage sessionStorage
7. drag 拖放
	- dragstart：事件主体是被拖放元素，在开始拖放被拖放元素时触发。
	- darg：事件主体是被拖放元素，在正在拖放被拖放元素时触发。
	- dragenter：事件主体是目标元素，在被拖放元素进入某元素时触发。
	- dragover：事件主体是目标元素，在被拖放在某元素内移动时触发。
	- dragleave：事件主体是目标元素，在被拖放元素移出目标元素是触发。
	- drop：事件主体是目标元素，在目标元素完全接受被拖放元素时触发。
	- dragend：事件主体是被拖放元素，在整个拖放操作结束时触发。
8. Geolocation
9. SVG/Canvas

**移除的元素有**
- 纯表现的元素：basefont，big，center，font, s，strike，tt，u;
- 对可用性产生负面影响的元素：frame，frameset，noframes；

**总结：**
- 新增语义化标签：nav、header、footer、aside、section、article
- 音频、视频标签：audio、video
- 数据存储：localStorage、sessionStorage
- canvas（画布）、Geolocation（地理定位）、websocket（通信协议）
- input 标签新增属性：placeholder、autocomplete、autofocus、required
- history API：go、forward、back、pushstate

###### 渐进增强和优雅降级
- 优雅降级针对最高级最完善的浏览器构建页面，开发周期的最后阶段测试过时浏览器。旧版浏览器只提供“简陋且无妨”的浏览，只修复较大错误
- 渐进增强关注内容本身，被 Yahoo 所采纳并用以构建其“分级式浏览器支持 (Graded Browser Support)”策略
###### title/h1 strong/b i/em区别
- h1层次明确的标题，title只是表示标题
- strong加强语气，b只是加粗
- em为强调的文本，i只是斜体
###### label的作用
定义表单与控件的关系，选择标签时焦点转移到相应控件
`<label for="mobile">Number:</label> <input id="mobile" type="text">`
`<label>Date: <input type="text"></label>`