顾名思义，css 变量，就是原生 css 里面也可以像 less/sass 一样，拥有定义变量的能力
### 一、语法
定义时：变量名前面要加两根连词线（--），如：`--bg-color`,
使用时：用 `var()` 来访问变量
### 二、基本用法
随便写个简单的 html
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>css Variables</title>
  <link rel="stylesheet" href="./css/style.css">
</head>
<body>
  <div class="container">
    <p>这是container的子节点</p>
  </div>
  <div class="box">这是个盒子</div>
</body>
</html>
```
1. 定义全局变量
  ```css
  :root{
  --bg-color: #ff0000;
  --font-color: #fff;
  }
  .box{
  text-align: center;
  line-height: 100px;
  height: 100px;
  width: 100px;
  background: var(--bg-color);
  color: var(--font-color);
  }
  ```
效果图：

![全局变量生效](https://upload-images.jianshu.io/upload_images/10570770-1a93998160ff6cd5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

2. 局部变量
  ```css
  .container{
  --bg-color: #ff0000;
  --font-color: #fff;
  }
  p{
  background: var(--bg-color);
  color: var(--font-color);
  }
  .box{
  text-align: center;
  line-height: 100px;
  height: 100px;
  width: 100px;
  background: var(--bg-color);
  color: var(--font-color);
  }
  ```
效果图：
![局部变量只在子节点生效](https://upload-images.jianshu.io/upload_images/10570770-5af858e6cfbcffe3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

3. **引入不同 css 文件的自定义变量可以使用吗？答案是可以的**
动手写一写，在 css 文件夹里面新增了个 var.css 文件，整个的目录截图：
![demo目录截图](https://upload-images.jianshu.io/upload_images/10570770-a4e7a272cc5091a7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
3.1 把自定义变量移到 var.css 文件中
    
  ```css
  :root{
  --bg-color: #ff0000;
  --font-color: #fff;
  }
  ```
3.2 引入var.css并使用
  ```css
  @import './var.css';  /* 这样就引入成功了 */

  p{
  background: var(--bg-color);
  color: var(--font-color);
  }
  .box{
  text-align: center;
  line-height: 100px;
  height: 100px;
  width: 100px;
  background: var(--bg-color);
  color: var(--font-color);
  }
  ```  
3.3 效果图

 ![证明外部引入的变量样式也能生效](https://upload-images.jianshu.io/upload_images/10570770-bd7aed4ee160341e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 三、浏览器兼容性
兼容性这个东西，去 [can I use](https://caniuse.com/) 查一下，就一目了然了
![浏览器兼容性](https://upload-images.jianshu.io/upload_images/10570770-9d88d5d1752326eb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



