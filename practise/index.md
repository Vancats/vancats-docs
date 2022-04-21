---
date created: 2022-03-30 20:25
date updated: 2022-04-21 10:51
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
1. visibility: hidden  占位，能响应事件
2. opacity 占位，不响应事件
3. display 不占位置 不xiang
4. absolute 界外