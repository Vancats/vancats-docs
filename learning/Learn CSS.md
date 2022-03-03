---
date created: 2022-03-03 00:34
date updated: 2022-03-03 00:52
---

#### CSS 高度坍塌

**浮动**
定义：非 IE 下，元素未设宽且自元素浮动导致内容溢出影响布局
工作原理：浮动元素脱离文档流，碰到包含它的边框或者其他浮动元素边框停留

**方案**

1. 设置`overflow:hidden`：子元素内容不能超父元素范围
2. 父元素结尾添加空子元素并设置`clear:both`：影响选择器和查找元素
3. 父元素浮动：产生新的浮动影响 ---> 父元素平级元素设置`clear:both`
4. 末尾伪元素设置`clear:both`，clear 只对块元素生效，个别浏览器再加上`display:table`，保险起见，再加`height: 0`，IE6-7 不支持:after，添加 zoom:1 触发 hasLayout

```css
.clearfix:after{
    content: "";
    display: table;
    height: 0;
    clear: both;
}
.clearfix{
	*zoom: 1;
}
```

**产生 BFC**

1. `float` 不是 `none`
2. `position` 不是 `static` 或 `relative`
3. `overflow` 不是 `visible`
4. `display` 是 `inline-block、inline-flex、flex、table-caption、table-cell、grid`

**margin 重叠**

1. 父元素`overflow:hidden`：子元素内容不能超过父元素
2. 父元素加上边框，设置颜色透明：增大父元素实际大小
3. 使用父元素`padding-top`代替：需要加上 `box-sizing: border-box;`
4. 父元素的第一个元素前添加`<table></table>`，形成 BFC，阻断 `margin`：干扰查找元素
5. 前置伪元素添加 `::before{ content: ''; display: table; }`
6. 子元素：`display: inline-block`

###### 水平居中（premise：父级块级并且宽度已设置）

1. 子元素是行内元素：父元素`text-align`
2. 子元素块级未设宽：默认撑满父级宽度
3. 子元素块级并设宽：
		1. 子元素`margin: 0 auto`
		2. `父margin/子padding + border-box`
		3. `left: 50%` + `margin-left || transform: translateX`
		4. `flex`

**水平垂直居中**：`display:table-cell;vertical:middle;text-align:center`+`display:inline-block`

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

**水平垂直居中**：`display:table-cell;vertical:middle;text-align:center`+`display:inline-block`

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
		2. `<link rel="stylesheet" href="base.css" media="screen" / "(max-width: 800px)">` 未设置即为全局
		3. `@import url(base.css) print`
		4. `@media`
3. 多设备支持：`import url(base.css) print, screen`
4. 设备方向：`orientation: landscape横 | portrait竖`
5. `eg: @media not screen and (orientation: landscape) and (max-width:600px) {...}`

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
5. 生成内容属性：content、_counter-reset、counter-increment_
6. 轮廓样式属性：outline-style、outline-width、outline-color、outline
7. 页面样式属性：size、_page-break-before、page-break-after_
8. 声音样式属性：_pause-before、pause、pause-after、cur-before、cur、cur-after、play-during_

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

1. 选择器
2. 边框：&-radius、&-shadow、&-image
3. 背景
		1. &-clip：border-box(no-clip) padding-box content-box 背景从 border padding content 开始
		2. &-origin：border-box padding-box(默认) content-box 以 ... 的左上角对齐
		3. &-size：content(缩小图片) --- cover(扩展元素) --- 100px 100px  --- 100% 50%
		4. &-break：控制背景如何在不同的盒子中显示
4. 文字
		1. word-wrap：normal 默认；break-all：允许单词内换行
		2. text-shadow：clip 直接修剪多余文本；ellipsis：省略号
		3. text-overflow、text-decoration
5. transition: 属性 时间 效果曲线 延迟时间
6. transform：translete、scale、rotate、skew、perspective
7. animation
8. 渐变：linear-gradient、radial-gradient，新增RGBA、HSLA模式
9. flex，grid

###### 图片格式

1. BMP：无损，不压缩 很大
2. JPEG：有损，直接色的点阵图，适合存储照片，不储存 logo，线框图。图片模糊，文件比 GIF 大
3. GIF：无损，索引色，文件小，支持透明和动画，仅支持 8bit 的索引色，适合色彩要求不高，文件小场景
4. GIF-8：无损，索引色，GIF 的优秀替代者，支持透明度，只是不支持动画，文件比 GIF 小
5. GIF-24：无损，索引色，压缩过，比 BMP 小
6. SVG：无损矢量图，放大不失真，适合绘制 Logo，Icon
7. WebP：同时支持有损和无损，直接色；无损压缩下，比 GIF 小 26%了；有损压缩，比 JPEG  小 25%-34%；要支持图片透明度，只需 22% 额外空间

###### CSS Sprites

- 优点：减少 HTTP 请求次数，减少图片总字节
- 缺点：开发麻烦需要借助 PS，后期维护麻烦

###### 单行、多行文本

- 字符超出部分换行： `overflow-wrap: break-word`
- 字符超出部分连字符：`hyphens:auto`
- 单行 `overflow: hidden; text-overflow: ellipsis; white-space: no-wrap`
- 多行

```css
overflow: hidden;
text-overflow: ellipsis;
display: -webkit-box;  // 设置为弹性伸缩盒子
-webkit-box-orient: vertical; // 从上到下排列
-webkit-line-clamp: 3; // 行数
```

###### z-index 失效

1. 该元素 position -> relative absolute fixed
2. 元素设置了 float，删除并用 display: inline-block
3. 父元素 position -> static absolute (relative 失效)

###### 判断元素在显示区域

1. window.innerHeight 浏览器可视高度
2. document.body.scrollTop || document.documentElement.scrollTop 滚动高度
3. ele.offsetTop 元素距离顶部的高度

`docuemnt.body.scrollTop + ele.height < ele.offsetTop < window.innerHeight + document.body.scrollTop(...)`

###### 层叠上下文

1. z-index > 0
2. z-index = 0
3. 行内盒
4. 浮动盒
5. 块级盒
6. z-index < 0
7. 背景和边框

z-index: auto 生成盒在当前层叠上下文中层级为 0，不建立新的层叠上下文，根元素除外

###### 命名冲突

- 团队命名约定
- CSS in JS
- CSS Modules
- webpack--css-loader 和 postcss--postcss-modules

```javascript
module: {
loaders: [
  {
	test: /\.css$/,
	loader: "style-loader!css-loader?modules&localIdentName=[path][name]---[local]---[hash:base64:5]"
  },
]
}
```

- HTML5 的 style scoped 解决部分，缺陷很多

###### 两栏布局

1. 左边元素浮动或 absolute，右边元素 margin-left: 左边元素宽度，宽度 auto
2. 左边元素浮动，右边 BFC overflow: hidden
3. 左边元素固定，右边元素 absolute，left 为 左边宽度，其他三个为 0
4. flex
5. grid

###### 三栏布局

1. 左右浮动或 absolute，中间 margin-left/right 相应宽度，如果是浮动，则中间元素必须在最后面
2. flex
3. grid

```css
display: grid;
grid-template-columns:200px auto 200px;
```

4. 圣杯：浮动与负边距，中间一列最前浮动，将其他两列挤到第二行

```html
<div class="outer">
	<div class="center"></div>
	<div class="left"></div>
	<div class="right"></div>
</div>

.outer {
	height: 100px;
	padding-left: 100px;
	padding-right: 200px;
}
.center {
	float: left;
	width: 100%;
	height: 100px;
	background: green;
}

.left {
	// 浮动
	float: left;
	// 向上移动
	margin-left: -100%;

	// 移动，向左移动
	position: relative;
	left: -100px;

	width: 100px;
	height: 100%;
	background: red;
}
.right {
	float: left;
	margin-left: -200px;

	position: relative;
	left: 200px;

	width: 200px;
	height: 100%;
	background: grey;
}
```

5. 双飞翼：左右位置保留通过中间列的 margin 值实现

```html
<div class="outer">
	<div class="center">
		<div class="inner"></div>
	</div>
	<div class="left"></div>
	<div class="right"></div>
</div>

.outer {
	height: 100px;
}

.center {
	float: left;
	width: 100%;
	height: 100px;
	background: green;
}
.inner {
	margin-left: 100px;
	margin-right: 200px;
	height: 100px;
}
.left {
	float: left;
	margin-left: -100%;

	width: 100px;
	height: 100px;
	background: red;
}
.right {
	float: left;
	margin-left: -200px;

	width: 200px;
	height: 100px;
	background: grey;
}
```

###### 选择器

链接样式：link, visited, hover, active

- 属性选择器
	- `[attr]`：指定属性的元素；
	- `[attr=val]`：属性等于指定值的元素；
	- `[attr*=val]`：属性包含指定值的元素；
	- `[attr^=val]` ：属性以指定值开头的元素；
	- `[attr$=val]`：属性以指定值结尾的元素；
- 组合选择器
	- 相邻兄弟：A + B
	- 普通兄弟：A ~ B
	- 子选择器：A > B
	- 后代选择器：A  B
- 伪类（全加一个：）
	- 行为伪类
		- `:active`：鼠标激活的元素；
		- `:hover`： 鼠标悬浮的元素；
		- `::selection`：鼠标选中的元素；
	- 状态伪类
		- `:target`：当前锚点的元素；`:link`：未访问的链接元素；
		- `:visited`：已访问的链接元素；`:focus`：输入聚焦的表单元素；
		- `:required`：输入必填的表单元素；`:valid`：输入合法的表单元素；
		- `:invalid`：输入非法的表单元素；`:in-range`：输入范围以内的表单元素；
		- `:out-of-range`：输入范围以外的表单元素；`:checked`：选项选中的表单元素；
		- `:optional`：选项可选的表单元素；`:enabled`：事件启用的表单元素；
		- `:disabled`：事件禁用的表单元素；`:read-only`：只读的表单元素；
		- `:read-write`：可读可写的表单元素；`:blank`：输入为空的表单元素；
		- `:current()`：浏览中的元素；`:past()`：已浏览的元素；
		- `:future()`：未浏览的元素；
	- 结构伪类
		- `:root`：文档的根元素；`:empty`：无子元素的元素；
		- `:first-letter`：元素的首字母；`:first-line`：元素的首行；
		- `:nth-child(n)`：元素中指定顺序索引的元素；`:nth-last-child(n)`：元素中指定逆序索引的元素；；
		- `:first-child`：元素中为首的元素；`:last-child` ：元素中为尾的元素；
		- `:only-child`：父元素仅有该元素的元素；`:nth-of-type(n)`：标签中指定顺序索引的标签；
		- `:nth-last-of-type(n)`：标签中指定逆序索引的标签；`:first-of-type` ：标签中为首的标签；
		- `:last-of-type`：标签中为尾标签；`:only-of-type`：父元素仅有该标签的标签；
	- 条件伪类
		- `:lang()`：基于元素语言来匹配页面元素；
		- `:dir()`：匹配特定文字书写方向的元素；
		- `:has()`：匹配包含指定元素的元素；
		- `:is()`：匹配指定选择器列表里的元素；
		- `:not()`：用来匹配不符合一组选择器的元素；
- 伪元素`::before  ::after`

###### CSS 工程化

**预处理器**：代码嵌套、变量、计算函数、extends、mixins、循环语法、CSS 模块化
**PostCss**：处理 CSS 代码。1. 可以做类似预处理器的事情 2. Autoprefixer 3. 能够帮助我们编译 CSS next
**Webpack**：css-loader：导出 CSS 模块，进行编译处理 style-loader：创建 style 标签，写入 CSS 内容

###### CSS 提高性能方法

**加载性能**

1. css 压缩
2. css 单一样式：border: 0 1px 1px 0; ---> border-right: 1px; border-bottom: 1px
3. 使用 link 代替 @import

**选择器性能**

1. 使用 class 替换标签
2. 尽量不使用通配符
3. 使用 id 选择器就别添加标签了
4. 关键选择器准确：尽量降低选择器的深度
5. 不要对可继承属性重复指定规则

**渲染性能**

1. 减少页面重排重绘
2. 去除空规则 {}
3. 属性为 0 时不加单位
4. 属性值为 0.xx 省略0
5. 标准化浏览器前缀时，标准属性在后
6. 不使用 @import
7. 选择器嵌套优化
8. css sprites
9. 不滥用 web 字体，有些很大
10. display 属性可能导致其他样式属性不可用，要正确使用
11. 谨慎使用高性能：浮动，定位

**可维护，健壮**

1. 抽离可复用的属性，统一使用
2. 样式和内容分离，css 定义在外部

###### 画图形

**三角形**
`border-left: 50px solid transparent; border-bottom: 100px solid green`

**扇形**
`border-top: 100px solid transparent; border-top-color: red; border-radius: 100px`

**宽高自适应的正方形**

```css
// 1. vw
.square {
	width: 10%;
	height: 10vw;
	background: red;
}

// 2. padding-top
.square {
	width: 20%;
	padding-top: 20%;
	height: 0;
	background: red;
}

// 3. 子元素的 margin-top
.square {
	width: 20%;
	overflow: hidden;
	background: red;
}
.square::after {
	content: '';
	display: block;
	margin-top: 100%;
}
```

**0.5px的线**

```js
// transform + scale：使用 scale 需要设置好 transform
.main {
	height: 1px;
	transform: scaleY(0.5);
	transform-origin: 50% 100%; // 防止线模糊
}

// viewport: 所有内容都会缩放
<meta name="viewport" content="width=device-width, initial-scale=0.5, minimum-scale=0.5, maximum-scale=0.5"/>
```

###### 小于12px的字

```css
-webkit-text-size-adjust: none; // Chrome 27 以上不支持

display: inline-block;  /* 缩放只能用于块或行内块 */
-webkit-transform: scale(0.5); /* 定义缩放大小 */
-webkit-transform-origin: left top; /* 必须指定缩放源点，防止padding/margin缩放的影响 */

内容固定可以转换成图片
```

###### 1px 问题

在一些 Retina 屏幕上，1px 显示出大于 1px 的效果 `window.devicePixelRatio = 设备物理像素 / CSS 像素`

1. 直接写入 0.5 px；大于 IOS8 支持，安卓不支持

```js
<div id="app" data-device={{ window.devicePixelRatio }}></div>

#app[data-device="2"] { border: 0.5px solid #333; }
```

2. 伪元素先放大再缩小

```js
<div id="app" data-device={{ window.devicePixelRatio }}></div>

#app[data-device="2"] { postion: relative; }
#app[data-device="2"] {
	content: '';
	position: absolute; left: 0; top: 0
	width: 200%;
	height: 200%
	transform: scale(0.5);
	transform-origin: left top;
	box-sizing: border-box;
	border: 1px solid #333;
}
```

3. viewport：处理了 1px 的情况，但是整个页面都将被缩放

`<mata name="viewport" content="initial-scale=0.5,maximum-scale=0.5,minimum-scale=0.5,user-scalable=no"`

```js
const scale = 1 / window.devicePixelRatio

metaEL.setAttribute(
'content',`width=device-width,user-scalable=no,initial-scale=${scale},maximum-scale=${scale},minimum-scale=${scale}`)
```

4. border-image
5. border-shadow
