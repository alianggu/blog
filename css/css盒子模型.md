### 一、页面布局时，每个 html 元素都是一个矩形的盒子。每个盒子由四部分组成，由内至外为：
- content：盒子的主体内容，例如文本、图片等
- padding：盒子的内边距，也叫内收，**负责延伸内容区域的背景**，在主题内容和边界之间，所以设置背景时padding会被包含在内
- border：盒子的边框，围绕内边距一圈
- margin：盒子的外边距，用空白区域隔开相邻盒子
![元素盒子（截取于chrome调试窗口）](https://upload-images.jianshu.io/upload_images/10570770-7de72c8e97ce7bb5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 二、通常说到盒子模型，会把它分为两种：W3C 标准盒模型和IE盒模型
*下面讨论他们的区别，width 和 height 分别为我们在 css 里面写的width和 height*
1. W3C 标准盒模型（content-box）
    width = content width
    height = content height
2. IE盒模型（border-box）
    width = border + padding + content width
    height = border + padding + content height 


```html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<meta http-equiv="X-UA-Compatible" content="ie=edge">
<title>Box Model</title>
<style>
    .box {
    width: 100px;
    height: 100px;
    border: 10px solid #ccc;
    margin: 10px;
    padding: 20px;
    background: #fa0000;
    }
</style>
</head>
<body>
<div class="box"></div>
</body>
</html>
```
上面的 demo 在 ie 各版本和现代浏览器里面看一下就能一目了然地看出区别，至于如何简单地模拟ie各版本，我是简单粗暴地使用了ie11版本的调试工具来仿真模拟，如下图标记的两处
![ie11浏览器仿真其他版本](https://upload-images.jianshu.io/upload_images/10570770-714633c59873723d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
**然后你会发现，其实，在 ie7+ 及现代浏览器中，默认的盒子模型都是W3C标准盒模型**

### 三、改变元素的盒模型：使用 css 属性 `box-sizing`
`box-sizing` 这个 css 属性特别简单，就两个取值，就上面说到的两个
- content-box：使用 W3C 标准盒模型，我们写的 width 和 height 就是元素的内容宽度和高度
- border-box： 使用 IE 盒模型，我们写的 width 和 height 除了元素的内容宽度和高度外，还要加上 padding 和 border
`box-sizing`浏览器兼容性(截自 MDN web docs)：
![box-sizing浏览器兼容性](https://upload-images.jianshu.io/upload_images/10570770-81922a04becc2ae0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)