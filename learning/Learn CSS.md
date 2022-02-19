###### CSS 高度坍塌
1. 设置`overflow:hidden`：子元素内容不能超父元素范围
2. 父元素结尾添加空子元素并设置`clear:both`：影响选择器和查找元素
3. 父元素浮动：产生新的浮动影响 ---> 父元素平级元素设置`clear:both`
4. 末尾伪元素设置`clear:both`，个别浏览器再加上`display:table`，保险起见，再加`height: 0;`

###### 产生 BFC
1. `float` 不是 `none`
2. `position` 不是 `static` 或 `relative`
3. `overflow` 不是 `visible`
4. `display` 是 `inline-block、inline-flex、flex、table-caption、table-cell`

###### 子元素margin-top覆盖父元素
1. 父元素`overflow:hidden`：子元素内容不能超过父元素
2. 父元素加上边框，设置颜色透明：增大父元素实际大小
3. 使用父元素`padding-top`代替：需要加上 `box-sizing: border-box;`
4. 父元素的第一个元素前添加`<table></table>`，形成 BFC，阻断 `margin`：干扰查找元素
5. 前置伪元素添加 `::before{ content: ''; display: table; }`

###### 水平居中（premise：父级块级并且宽度已设置）
1. 子元素是行内元素：父元素`text-align`
2. 子元素块级未设宽：默认撑满父级宽度
3. 子元素块级并设宽：
	1. 子元素`margin: 0 auto`
	2. `父margin/子padding + border-box`
	3. `left: 50%` + `margin-left || transform: translateX`
	4. `flex`

###### 垂直居中（premise：父元素是盒子容器）
1. 子元素是行内元素
	1. 单行：`line-height`
	2. 多行：`display: inline/inline-block/table-cell; vertical-align: middle`
2. 子元素块级未设宽
	1. `display: inline/inline-block/table-cell; vertical-align: middle`
	2. `flex`
3. 子元素块级并设宽
	1. `父margin/子padding + border-box`
	2. `top: 50%` + `margin-top || transform: translateY`
	3. `flex`

###### 响应式布局
1. `meta`标签
	1. `viewport`：显示可以显示内容的大小
	2. `width`：视口宽，`device-width`，物理设备宽
	3. `user-scalable`：是否允许用户缩放
	4. `initial-scale`：初始化缩放大小
	5. `maximum-scale`：最大缩放
	6. `minimum-scale`：最小缩放
2. 布局方式
	1. `flex`：中间有内容会有缩放最小值；左右侧的宽度会变小
	2. 父相子绝：代码复杂，宽度小于600时，右侧覆盖左侧，需要用media平分屏幕解决
	3. `float`
	4. `rem`：浏览器字体有最小值
	5. `grid`：

###### 媒体查询
1. 媒体类型：`all screen print 打印机 speech 发声设备`
2. 引入方法：
	1. `<style media="print">`
	2. `<link rel="stylesheet" href="base.css" media="screen">` 未设置即为全局
	3. `@import url(base.css) print`
	4. `@media`
3. 多设备支持：`import url(base.css) print, screen`
4. 设备方向：`orientation: landscape横 | portrait竖`
5. `eg: @media not screen and (orientation: landscape) and (max-width:600px) {...}`
###### CSS三角形
宽高置0，设置`border`
`border-left: 50px solid transparent; border-bottom: 100px solid green`

###### 0.5px的线
```css
.main {
	height: 1px;
	transform: scaleY(0.5);
	transform-origin: 50% 100%; // 防止线模糊
}
```

###### 小于12px的字（缩放）
```css
display: inline-block;  /* 缩放只能用于块或行内块 */
-webkit-transform: scale(0.5); /* 定义缩放大小 */
-webkit-transform-origin: left top; /* 必须指定缩放源点，防止padding/margin缩放的影响 */
```

###### Sass常用功能
1. 变量：`$primary-color: #666; div { color: $primary-color}`
2. 引入：`import 'base.scss'`
3. 嵌套：
	1. 父级选择器`$`
	2. 嵌套属性：`.demo{ font: { family: fantasy; size: 16px; } }`=== `.demo { font-family: fantasy; font-size: 16px }`
4. 混入（mixin）
5. 算术运算符：`.article[role="main"] { ... }`
6. 继承：`%common {...}   .message { @extend %commpon; }` 只有被继承过的代码才会输出到样式文件

###### CSS继承
**不可继承**
1. display
2. 文本属性：vertical-align、text-decoration、text-shadow、white-space(空白符)、unicode-bidi(文本方向)
3. 背景属性：background、&-color、&-image、&-repeat、&-position、&-attachment
4. 定位属性：float、clear、position、top、right、bottom、left、min/max-width、min/max-height、overflow、clip、z-index
5. 生成内容属性：content、*counter-reset、counter-increment*
6. 轮廓样式属性：outline-style、outline-width、outline-color、outline
7. 页面样式属性：size、*page-break-before、page-break-after*
8. 声音样式属性：*pause-before、pause、pause-after、cur-before、cur、cur-after、play-during*

**可继承**
1. 字体属性：font-family、font-size、font-weight、font-style
2. 文本属性：color、text-indent、text-align、line-height、word-spacing(单词间距)、letter-spacing(中文、字母间距)、text-transform
3. 元素可见性：visibility
4. 列表布局：list-style（list-style-type、list-style-image）
5. 光标属性：cursor

###### 隐藏元素的方法
1. display: none；渲染树中不存在，不占位置，不响应绑定的监听事件
2. opacity: 0；占位置，不响应
3. visibility: hidden；占位置，能响应
4. transform: scale(0, 0)；占位置，能响应
5. position: absolute 移出屏幕
6. z-index 负值，隐藏在其他元素下
7. clip/clip-path：元素裁剪实现隐藏，占位置，不响应

###### link @import 区别
1. 前者除了加载CSS还能定义RSS等其他，后者只加载CSS
2. 前者是XMTML标签，无兼容性问题，后者是 CSS2.1 提出，浏览器兼容
3. 前者同时加载，后者需要页面完全载入后再加载
4. 前者可以操作DOM元素，后者只是单纯的引入样式

###### 伪类和伪元素的区别
- 伪类是对于特定选择器添加类别，外部可见，不在文档中生成 
`p::before {content: 'content'} p::first-line {background: red} p::first-letter {font-size: 30px}`
- 伪元素是在元素内容前后添加元素 `p:hover {color: red}`

###### requestAnimationFrame
- 使用：window.requestAnimationFrame(callback) callback在下次重绘前更新动画帧调用
- 取消：window.cancelAnimationFrame(id)
- 优势
	- CPU 节能：在浏览器切屏后停止执行渲染，setInterval 即便在后台也会执行
	- 函数节流：固定 16.7ms 执行一次
	- 减少DOM操作：会在下次重绘前进行统一的DOM操作
- setTimeout缺点：1. 固定间隔不一定等于帧数刷新时间（16.7）2. 进入宏任务队列，不一定按固定间隔执行

###### li 之间的空白换行符
1. 全部 li 写在一行：不美观
2. ul: font-size 置 0：需要额外设置其他字符属性，且 safari 依然存在空间
3. ul: letter-spacing: -8px：设置了 li 字符间隔，需要重新设置 li: letter-spacing: normal
4. li: float: left：有些容器不能设置浮动，如左右切换的焦点图

###### CSS3 新特性

###### 图片格式
1. BMP：无损，不压缩 很大
2. JPEG：有损，直接色，适合存储照片，
