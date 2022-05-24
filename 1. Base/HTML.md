---
date created: 2022-05-23 18:48
date updated: 2022-05-24 13:40
---

# HTML

#### 标签语义化

1. 设备解析，盲人阅读
2. 利于爬虫，SEO搜索

#### DOCTYPE

1. CSS1Compat 标准模式 HTML5 只有标准
2. BackCompat 怪异模式
3. document.compatMode 获取

#### meta

1. charset utf-8
2. viewport
3. robot index 文件可检索 / follow 链接可查询
4. http-equiv refresh SCP

#### link

1. preload 优先级
2. prefetch 空闲下载
3. dns-prefetch 提前 dns 解析
4. preconnect 预连接 dns + tcp + ssl

#### src href

1. 资源引用，资源嵌入标签，阻塞加载
2. 超文本引用，链接关系，并行下载

#### defer async

1. 元档解析后，DOMContentLoaded 触发前执行，顺序
2. 立即加载，无序

#### img srcset

根据界面密度来实现加载不同的图片

> img src="image-128.png" srcset="image-128.png 128w, image-256.png 256w, image-512.png 512w" sizes="(max-width: 360px) 340px, 128px"  **默认 128，大于360 则设置成 340**

#### iframe

**优点**

1. 并行加载 -- 广告
2. 跨域通信

**缺点**

1. 阻塞主页面 onload
2. 隔绝搜索引擎
3. 难以管理

#### canvas SVG

**canvas**

1. 依赖分辨率
2. 无事件处理
3. 弱文本渲染
4. 可以用 jpg png 保存
5. 适用于图像密集型游戏，可以频繁重绘

**svg**

1. 矢量图
2. DOM可用  事件处理
3. 适合大型渲染区域类型，谷歌地图
4. 渲染速度不快（DOM）

#### HTML 离线缓存

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

#### web worker(扩展)

新建一个在后台的 js 线程，通过 postMessage 传输结果
