---
date created: 2022-05-23 18:48
date updated: 2022-05-23 20:07
---

# HTML

#### 标签语义化

1. 设备解析，盲人阅读
2. 利于 SEO 搜索

#### DOCTYPE

1. CSS1Compat 标准模式 HTML5 默认
2. BackCompat 怪异模式

#### meta

1. charset utf-8
2. viewport
3. robot index 可检索 follow
4. http-equiv refresh SCP

#### link

1. preload 优先级
2. prefetch 空闲下载
3. dns-prefetch 提前建立 dns
4. preconnect dns + tcp + ssl

#### src href

1. 阻塞加载
2. 并行下载

#### defer async

1. DOMContentLoaded 后加载，顺序
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
1. 像素级别
2. 无事件处理
3. 弱文本渲染
4. 适用于大型游戏
**svg**
#### HTML5

#### HTML 离线缓存
