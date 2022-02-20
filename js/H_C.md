### 常见布局
​	**两栏**
- float + overflow（BFC 原理）
- float + margin
- flex
- grid
​	**三栏**
- 圣杯布局
- 双飞翼布局
- flex
- float + overflow（左右 float）
- grid
  ```javascript
  display:grid;
  grid-template-columns:200px auto 200px;
  ```

### 移动端 1px 解决方案
1. border:0.5px solid E5E5E5 仅限IOS端
2. border-image
3. box-shadow
4. 使用伪元素：绝对定位，长宽两倍，再进行 scale(0.5) 缩放
```css
::after {
   content: '';
   position:absolute;
   top: 0; left: 0;
   width: 200%; height: 200%;
   border: 1px solid #000;
   transform: scale(0.5);
}
```
5. 设置 viewport 的 scale 值，使用 DOM 修改 content 属性
   - 可用该方法画 0.5px 的线（也可以用 transform:scale(0.5,0.5)
   ```javascript
   <meta name="viewport" id="WebViewport" 
         content="initial-scale=1, maximum-scale=1, minimum-scale=1, user-scalable=no">
   ```

### @ 规则
- @import：告诉 css 引擎引入一个外部样式表
  - 从属关系：link 是 html 标签，还能导入图片等，还可以定义 RSS、rel 连接属性，后者只导入 css
  - 加载顺序：link 导入样式同时加载，后者等页面加载完加载
  - 兼容性：link 无问题，后者不兼容 ie5 以下
  - 权重：link 权重高于后者
  - DOM 可控性：可以通过 js 操作 DOM 动态引入样式表改变样式，后者不行
- @media：`@media (min-height: 680px), screen and (orientation: portrait) {}`
- @keyframes：描述 css 动画的关键帧
- @supports：查询特定的 css 是否生效，可以结合 not、and、or
- @namespace：告诉 css 引擎必须考虑 XML 命名空间
- @font-face：描述将下载的外部字体
- @document：文档样式表满足给定条件则生效
- @charset：使用的字符编码

### 值与单位
- 相关概念
  - 设备像素（Device pixels）：屏幕分辨率
  - 设备像素比（DPR）
    - 一个 css 像素等于几个物理像素
    - 通过 `window.devicePixelRatio`获取
  - 像素密度（DPI / PPI）
    - 像素密度 = 屏幕对角线像素尺寸 / 物理尺寸
  - 设备独立像素（DIP）--- 安卓
    - dip = px * 160 / dpi
- px
- em：相对于父元素
- rem：相对于根元素--多用于自适应网站或者 H5
- vh / vw：相对于视口 `100 vw = window.innerWidth`

### 选择器
> 链接样式保持：link,visited,hover,active
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
