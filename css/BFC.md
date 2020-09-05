## BFC是什么？
BFC，块状格式化上下文，其实是一个隔离的独立盒子（容器），他有以下特性：
- 容器里面的子元素不会影响到外面的元素，容器外的元素也不会影响到里面
- BFC 容器会一个挨着一个排列
- 计算 BFC 的高度时，浮动元素也参与计算
- BFC的区域不会与 float box 重叠

## 怎样触发（创建）一个 BFC？
- boby的根节点
- 浮动的元素：float 除了 none 以外
- 绝对定位的元素：position（`absolute`、`fixed`）
- display为 `inline-block`、`table-cells`、`flex`、`inline-flex`、`flow-root`（*没有副作用的方案，但需注意兼容性*）、`grid`、`inline-grid` 等
- overflow 除了 visible 之外
 
## BFC 有什么用？
#### 1. 清除浮动：因为BFC可以包含浮动的元素，故把浮动元素的父元素设为 BFC 容器即可
```html
<!-- html -->
<div class="box">
  <div class="floatDiv">
    我是一个浮动的div，现在我想让它高一点，高过另外一行文字吧，对吧，这样才能看到效果
  </div>
  这是一段很长很长很长很长很长很长很长很长（有多长随便啦）的文字
</div>
```
```css
/* CSS */
.box {
  border: 5px dotted #faa509;
  padding: 10px;
  width: 300px;
}
.floatDiv {
  float: left;
  border: 2px solid #00d0ff;
  width: 100px;
}
```
现在的效果如图：
![未消除浮动](https://upload-images.jianshu.io/upload_images/10570770-4f48847552f9027c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

按照以前的方案，一般会在后面加一个空的div标签来清除浮动（或者有其他各种方案）
```html
<!-- html -->
<div class="box">
  <div class="floatDiv">
    我是一个浮动的div，现在我想让它高一点，高过另外一行文字吧，对吧，这样才能看到效果
  </div>
  这是一段很长很长很长很长很长很长很长很长（有多长随便啦）的文字
  <div class="clearfix"></div>
</div>
```
```css
/* 添加CSS */
.clearfix{
  clear: both;
}
```
那么使用BFC特性怎么操作呢？
很简单，只需要给`.box`加多一个样式使其变成BFC就可以了
```css
/* 添加CSS */
.box{
  display: flow-root;
}
```
效果图如下：
![BFC消除浮动](https://upload-images.jianshu.io/upload_images/10570770-0c045ebf2ac11ebd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 2. 解决元素间上下边距发生折叠的问题（外边距塌陷）：把元素*放入（注意：是放入，不是设置成）*不同的 BFC 容器中
```html
<!-- html -->
<div class="container">
  <p class="text">这是一堆文字</p>
  <p class="text">随便写，开心就好</p>
</div>
```
```css
/* CSS */
.container {
  border: 5px dotted #faa509;
  width: 300px;
  background: #ccc;
}
.text{
  border: 1px solid #faa509;
  margin: 20px;
  text-align: center;
}
```
现在的效果图为：
![上下边距塌陷](https://upload-images.jianshu.io/upload_images/10570770-145b1081653b75f0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
- 发现问题：认真看一眼，会发现，其实第一段文字和第二段文字中间的 margin 加起来应该是 40px，但是现在效果图看来只有 20px。这就是上下边距折叠，也就是外边距塌陷。
- 解决问题：那么怎么解决这个问题呢？就是上面所说的，把元素***放入***不同的 BFC 容器中
为什么这里要强调是***放入***而不是设置成，因为本人一开始只是把两段文本设置成不同的BFC容器，然后就陷入了短暂的懵比。
```css
/* 添加CSS */
.text{
  display: flow-root;
}
```
添加上面 css 后，会发现，一点改变都没有，如图：
![设置成不同的BFC](https://upload-images.jianshu.io/upload_images/10570770-5ef8ad3d3ef9b82b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
正确的姿势应该是：
```html
<!-- 修改html -->
<div class="container">
  <p class="text">这是一堆文字</p>
  <div class="textBox"> <!-- 用textBox把其中一段文本包起来 -->
    <p class="text">随便写，开心就好</p>
  </div>
</div>
```
```css
/* 添加CSS */
.textBox{
  display: flow-root;
}
```
首先用一个 div 把文本包起来，再把这个 div 设置为 BFC，这样，上下边距折叠的问题就解决了，如图：
![放入不同的BFC](https://upload-images.jianshu.io/upload_images/10570770-32e63694b3651582.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 3. 防止元素间的重叠覆盖，可实现自适应两列布局：左边元素左浮动，右边元素设置为 BFC 容器
先看一下元素间重叠是怎样的
```html
<!-- html -->
 <div class="container">
  <div class="left"></div>
  <div class="right"></div>
</div>
```
```css
/* CSS */
.container {
  border: 5px dotted #faa509;
  width: 300px;
}
.left{
  background: #fadc09;
  height: 100px;
  width: 100px;
  float: left;
}
.right{
  height: 300px;
  background: #faa509;
}
```
![元素间重叠](https://upload-images.jianshu.io/upload_images/10570770-135cf95f709c8831.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
你会发现，浮动的元素和不浮动的元素重叠在一起了，利用 BFC 的区域不会与 float box 重叠的特性，我们就可以简单的实现两列布局了。只需要把右边元素设置为一个BFC就行
```css
/* 添加CSS */
.right{
  display: flow-root;
}
```
![两栏布局](https://upload-images.jianshu.io/upload_images/10570770-5dbd0667a48c9623.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
#### 竟然说到了两栏布局，那就顺便介绍一种黑魔法实现等高两栏布局，直接上代码
```html
<!-- html -->
 <div class="container">
  <div class="left"></div>
  <div class="right"></div>
</div>
```
```css
/* CSS */
.container {
  border: 5px dotted #faa509;
  width: 300px;
  overflow: hidden;
}
.container>div{
  /* 注意这里 */
  padding-bottom: 100000px;
  margin-bottom:  -100000px;
}
.left{
  background: #fadc09;
  height: 100px;
  width: 100px;
  float: left;
}
.right{
  height: 300px;
  background: #faa509;
  display: flow-root;
}
```
![两栏等高布局](https://upload-images.jianshu.io/upload_images/10570770-8f56b93bb3c284fa.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
它根据子元素的最大高度来决定整体高度，可以随意改变 `.left` 或者 `.right` 的高度来测试一下是否符合等高，也可以往里面填充内容测试

## 顺便提一下 `display:flow-root`？
前面说到 `display:flow-root` 这种方式创建一个BFC是最没有副作用的方式，那么这里就介绍一下 `flow-root` 这个 display 属性的新取值
1. W3C 中怎么描述 `flow-root`
> The element generates a block container box, and lays out its contents using flow layout. It always establishes a new block formatting context for its contents. [CSS2]

直译过来就是：该元素生成一个块容器框，并使用流布局布置其内容。它始终为其内容建立新的块格式化上下文
*说明这是 SS2 新增的专为建立块格式化上下文的一个属性值*

2. 兼容性

兼容性问题，一般都是直接 [Can I use](https://caniuse.com/#search=display%3A%20flow-root) 一把梭，会发现，其实除了firefox和chrome（当然还有Opera）对这个属性比较友好外，其他浏览器还不太支持
![can i use display: flow-root](https://upload-images.jianshu.io/upload_images/10570770-91eba0ed694d9ec7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

--------
*参考：*
1. [块格式化上下文 in MDN](https://developer.mozilla.org/zh-CN/docs/Web/Guide/CSS/Block_formatting_context)
2. [Understanding CSS Layout And The Block Formatting Context](https://www.smashingmagazine.com/2017/12/understanding-css-layout-block-formatting-context/)
3. [Airen的博客 — flow-root](https://www.w3cplus.com/css3/display-flow-root.html)
