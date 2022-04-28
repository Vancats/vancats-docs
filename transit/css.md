---
date created: 2022-03-30 20:25
date updated: 2022-04-21 15:43
---

#### CSS 高度坍塌(loading)

**float 导致**

1. 添加 border
2. 添加 clear: both;
3. overflow: hidden;
4. display: block;
5.

#### 居中

##### 水平居中

1. text-align: center
2. margin: 0 auto
3. absolute + left50 + transform: translateX
4. flex + justify-content

##### 垂直居中

1. line-height
2. absoluye + top50 + transform: translateY
3. flex + align-items

> 多行行内元素居中：
> display: inline / inline-block / table-cell; vertical-align: middle;
> 水平垂直居中：
> display: table-cell; vertical: middle; text-align: center;
> display: inline-block; (子元素)

#### 响应式布局(loading)

1. vh vw
2. flex
3. rem em
4. grid

#### 媒体查询

1. @all @screen @print 打印机 @speech 发声设备
2. <link rel="stylesheet" href="xx.css" media="screen"> 默认全局
3. @import url(xx.css) print, screen 可以多设备
4. @media not screen and (orientation: landscape 横) and (max-width: 600px) {...}

#### Sass 常用功能

1. 变量 $ less #
2. 嵌套 父选择器 $    属性可以嵌套
3. 算术运算符 .artical[role="main"]
4. 引入 import
5. 混入 mixin
6. 继承 %

#### CSS 继承

1. 字体属性 font-family font-size font-weight font-style
2. 文本属性 color text-align text-indent line-height word-spacing letter-spacing
3. 列表属性 list-style
4. 元素可见性 visibility opacity
5. 光标属性 cursor

#### 隐藏元素的方法

1. visibility hidden  占位，能响应
2. opacity 占位，不响应
3. display 不占位置 不响应
4. transform scale(0,0) 占位置，能响应
5. absolute 界外

#### link @import 的区别

1. 内部自带的标签，兼容性好，后者 CSS2.1 提出，兼容性差
2. 可以定义 RSS 等和操作 DOM 元素，后者只是 css
3. 同时加载，后者等页面完全载入后

#### 伪类和伪元素区别

1. 伪类是将特殊效果添加到特定选择器上
2. 伪元素在内容前后添加额外元素和样式，文档中不存在，仅显示可见

#### requestAnimationFrame

1. CPU 节能 切屏后停止渲染
2. 函数节流 固定一帧一次
3. 减少 DOM 操作 在下次重绘前进行统一的 DOM 操作

#### li 之间的空白换行符

1. 所有 li 写在一行 不美观
2. ul font-size 0 需要重新设置字符属性
3. ul letter-spacing -8px + li letter-spacing normal
4. li float left 左右切换的焦点图不能浮动

#### CSS3 特性(loading)

1. border - radius shadow image
2. background -
3. word-wrap: normal 默认 break-all 单词内换行

#### 图片格式

1. BMP

#### CSS Sprites

1. 使用 background 的多个属性，对图形进行合并操作
2. 如果有任何位置上的改动，需要重新绘制
3. 可以有效减少发送请求的次数

#### 单行、多行文本省略

overflow: hidden; text-overflow: ellipsis; white-space: no-wrap;

overflow: hidden; text-overflow: ellipsis; display: -webkit-box; -webkit-box-orient: vertical; -webkit-line-clamp: 3
