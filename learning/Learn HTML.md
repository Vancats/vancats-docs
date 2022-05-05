---
date created: 2022-03-30 20:25
date updated: 2022-05-05 20:49
---

#### 语义化

1. 利于设备解析，自动生成目录，例如盲人解析
2. 对机器友好，适合爬虫，利于SEO搜索引擎
3. 利于开发，增加可读性

#### DOCTYPE 的作用

1. 帮助确认使用哪种规范进行解析，如果没指定就是使用浏览器自身规范
2. CSS1Compat：标准模式；BackCompat：怪异模式，向后兼容
3. `<!Doctype html>` 标准模式，使用 HTML5 解析，HTML5 没有严格混杂的区别
4. 使用 `document.compatMode` 可以获取

#### header 标签的作用

1. 定义文档头部
2. 引入资源、样式表
3. 描述文档信息：文档标题，元信息，在 web 中的位置以及和其他文档相对位置

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

#### Link 标签

1. preload：指明后续可能有该文件，可以提醒浏览器处理其优先级
2. prefetch：预加载文件，比如 html 中肯定包含的文件先行加载
3. dns-prefetch：进行提前的 dns 解析
4. preconnect：进行预连接，包括 TCP 三次握手

#### src 与 href

1. src 是资源引用，会下载该资源并直接嵌入到该标签位置，解析时会暂停其他资源的下载和处理，例如 js
2. href 是超文本引用，建立一个链接关系，并行下载，例如 a，link

#### defer 和 asyc 的区别

1. async 是下载完直接执行，defer 下载完之后是在文档解析之后，DOMContentLoader 触发前执行
2. async 不能确保加载顺序，defer 可以

#### 行内、块、空元素

1. 行内元素：**span、img、input、select、a、i、b**
2. 块元素：**div、p、h、ul、ol、li、dl、dt、dd**
3. 空元素：**meta、input、img、link、br、hr**

#### label 的作用

定义表单和控件的关系，在选择标签时转移焦点到对应控件
`<label for="mobile">Number: </label> <input id="mobile" />`

#### img 的 srcset 属性

> 根据不同屏幕的密度设置不同图片，w 是图片质量，sizes 是临界尺寸，

> img src="image-128.png" srcset="image-128.png 128w, image-256.png 256w, image-512.png 512w" sizes="(max-width: 360px) 340px, 128px"  **默认 128，大于360 则设置成 340**

#### iframe 内联框架

- 优点
  1. 加载速度较慢的内容，如广告
  2. 使用脚本并行下载
  3. 实现跨子域通信
- 缺点
  1. 会阻塞主页面的 onload 事件
  2. 无法被搜索引擎识别
  3. 产生过多页面不易管理

#### Canvas SVG

- Canvas：画布，通过 js 绘制，位置改变将重新绘制
  1. 依赖分辨率
  2. 不支持事件处理
  3. 弱的文本渲染能力
  4. 可以 jpg png 格式保存图片
  5. 适合图像密集型的游戏，可以频繁重绘
- SVG：矢量图，基于 XML 的图形语言，因此 DOM 可用
  1. 不依赖分辨率
  2. 支持事件处理
  3. 适合大型渲染区域的应用程序，如谷歌地图
  4. 复杂度高会减慢渲染速度，任何过度使用 DOM 的应用都不快
  5. 不适合游戏应用

#### HTML5 更新

1. 语义化标签
2. 媒体标签
   1. audio
   2. video：poster 封面，默认第一帧
   3. source：使用 type 属性为不同浏览器指定视频源
3. 表单类型、属性、事件
4. 进度条 progress，度量器 meter
5. Web 存储：localStorage、sessionStorage
6. Drag
7. Geolocation
8. SVG/Canvas

#### HTML 离线缓存（需扩展）

**原理**

基于 appcache 文件的缓存机制（不是存储技术），通过这个文件的解析清单离线存储资源。离线时正常访问，联网后更新缓存文件

##### 使用方法

1. 创建和 html 同名的 manifest，页面头部加入 manifest
2. 编写 cache.manifest 文件
3. 离线状态通过 `window.applicationCache` 进行离线缓存操作
4. 更新缓存：1. 更新 manifest 文件 2. js 操作 3. 清除浏览器

```js
// html 文件
<html lang="en" manifest="index.manifest">

// cache.manifest 文件
CACHE MANIFEST
	#v0.11
CACHE:     // 离线存储的资源，页面本身自动存储
	js/app.js
	css/style.css
NETWORK:   // 不被缓存的资源，优先级不如 CACHE
	resource/logo.png
FALLBACK:  // 访问第一个资源的任何文件失败，就去访问这个文件
	offline.html
```

##### 离线资源管理和加载

1. 离线时使用离线资源
2. 在线时：当发现 html 头部有 manifest 属性，就去加载 manifest 文件，第一次会离线存储，之后就是先用离线资源加载页面，然后对比新旧 mainfest 文件，有变化就更新

##### 注意事项

1. 浏览器对缓存数据的容量可能不太一样，如 5MB
2. manifest 或 CACHE 中的任何一个下载失败，使用原来的离线缓存
3. html 文件和 manifest 文件和 FALLBACK 中的文件必须同源，在同一个域下
4. manifest 文件改变，资源请求本身也会触发更新
5. 浏览器可以请求缓存的绝对路径
6. 站点的其他页面请求的资源在缓存中会从缓资中访问

#### Web Worker（需扩展）

- 运行在后台的 js，不影响页面性能，通过 postMessage 传回结果。
- 在正常的 HTML 页面执行 js 不响应，

#### 渐进增强和优雅降级

- 渐进增强：关注内容本身，成为一种更为合理的设计范例，**分级式浏览器支持**
- 优雅降级：针对最完善最高级的浏览器去设计页面，最后再去测试过时浏览器，只提供简陋且无妨的功能，修复较大的错误
