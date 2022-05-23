---
date created: 2022-05-23 18:48
date updated: 2022-05-23 20:34
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

#### web worker(扩展)

新建一个在后台的 js 线程，通过 postMessage 传输结果
