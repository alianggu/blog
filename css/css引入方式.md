&emsp;&emsp;引入 css，我把它分为在 html 文件中引入和在 css 文件中引入（网上大多称这种方式为导入）两种不同的方式。

### 一、html文件中引入css
在 html 文件中引入 css，有三种方式：内联样式、内嵌样式表、外部样式表
1. 内联样式，也称为行内样式
```css
<p style="color: #ff0000">行内样式</p>
```
2. 内嵌样式表: 在HTML 头部中 `<head>` 部分用 `<style>` 标签包起来
```css
<style>
  p {
    color: #ff0000;
  }
</style>
```
3. 外部样式表：通过 `<link>` 标签引入外部样式表
```
<link rel="stylesheet" href="./css/style.css">
```

### 二、在 css 文件中引入其他 css 文件（ css 导入）：使用 `@import`
```css
@import './var.css';  /* 这样就引入成功了 */
/* 或者 */ 
@import url('./var.css');  /* 这样就引入成功了 */
```  
兼容性？看下图：
![@import浏览器兼容性](https://upload-images.jianshu.io/upload_images/10570770-34a8b89031eb236e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![@import移动浏览器](https://upload-images.jianshu.io/upload_images/10570770-395f9db6d19b25ca.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

IE5.5 以上都兼容，所以不太需要担心兼容性问题
### 三、`<link>` vs `@import`
`<link>` 标签和 `@import` 都是引入外部样式表，那有什么区别呢？
- `<link>` 标签在html中引入，`@import` 在 css 中引入
- 用 `<link>` 引入的 css 会在页面被加载的时候就同时加载，而用`@import` 引入的 css 文件会等待页面加载完再加载，故用 `@import` 引入的样式有可能会在那么一小小短时间内无法呈现

*小结：能使用 `<link>` 方式就不要用或者少用 `@import` 方式*

### 四、优先级
**越靠近元素的优先级最高**，所以：
行内样式 > 内嵌样式表 > 外部样式表 > @import 

-----------
参考：
1.  [http://www.stevesouders.com/blog/2009/04/09/dont-use-import/](http://www.stevesouders.com/blog/2009/04/09/dont-use-import/)
2. [https://stackoverflow.com/questions/1022695/difference-between-import-and-link-in-css](https://stackoverflow.com/questions/1022695/difference-between-import-and-link-in-css)

