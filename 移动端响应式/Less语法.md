# Less快速入门

Less 是一门 CSS 预处理语言，它扩展了 CSS 语言，增加了变量、Mixin、函数等特性，使 CSS 更易维护和扩展。

Less 可以运行在 Node 或浏览器端。

先安装Easy LESS插件

![](D:\web\Typora\移动端响应式\static\images\less\less_01.png)

接着

![](D:\web\Typora\移动端响应式\static\images\less\less_01.gif)

vscode中设置

![](D:\web\Typora\移动端响应式\static\images\less\less_02.gif)

#### less

##### 定义变量

在less文件中，我们可以声明变量

```less
@color: #f00;
h2{
    color: @color;
}
```

##### Minxins混入

Minxins是一种将一组属性从一个规则集合混入到另一个规则集合的方法。

```less
@color: #f00;
@width: 100px;
@height: @width;
.active{
    border: 1px solid @color;
    width: @width;
    height: @height;
}
#box2{
    /*混入了.active的规则*/
    .active;
}
```

##### 嵌套规则

less能够可以使用嵌套规则来编写我们的代码

```less
<div id="box">
    <h2>
        <span>Mjj</span>
    </h2>
</div>
```

以前的css写法:

```css
#box {
  width: 100px;
  height: 100px;
  border: 1px solid #000;
}
#box h2 {
  background-color: green;
  padding: 20px;
}
#box h2 span {
  color: #f00;
}
```

使用less嵌套规则编写

```less
@color: #f00;
@width: 100px;
@height: @width;
#box{
    width: @width;
    height: @height;
    border: 1px solid #000;
    h2{
        background-color: green;
        padding: 20px;
        span{
            color: @color;
        }
    }
}
```

##### 运算

算数运算符`+`,`-`,`*`,`/`可以在less中使用

```less
@a: 20px;
@b: 30px;
@sum = @a + @b;
.active{
    width: @sum;
}
```

##### 逃离

它允许你使用任意字符串作为属性或变量值。内部的任何内容`~"anything"`按原样使用。除了插值外没有任何变化。

```less
// 插入变量 使用@min768，相当于把(min-width:768px)完整的插入进去
@min768:~"(min-width:768px)";
body{
    @media @min768 {
        background-color: red;
    }
}
//编译之后
@media (min-width:768px) {
  body {
    background-color: red;
  }
}
```

##### 可变插值

**选择器名称可以作为可变的值**

```less
//定义自己的选择器
@my-selector: banner;
//使用自己的选择器
.@{my-selector}{
    font-size: 30px;
}
//编译为
.banner{
    font-size: 30px;
}les
```

**网址插入**

```less
// Variables
@images: "../img";
// Usage
body {
  color: #444;
  background: url("@{images}/white-sand.png");
}
```

**导入语句**

```less
@import './hello.less'
```

##### 父选择器

`&`引用父选择器

```less
a {
  color: blue;
  &:hover {
    color: green;
  }
}
//编译
a {
  color: blue;
}
a:hover {
  color: green;
}
```

看看这个例子，或许你能晓得`&`符号的强大之处

```less
.button {
  &-ok {
    background-image: url("ok.png");
  }
  &-cancel {
    background-image: url("cancel.png");
  }
  &-custom {
    background-image: url("custom.png");
  }
}
//编译
.button-ok {
  background-image: url("ok.png");
}
.button-cancel {
  background-image: url("cancel.png");
}
.button-custom {
  background-image: url("custom.png");
}
```

##### 自定义函数

```less
@base:375 / 375 * 0.01;
//自定义函数
.px2rem(@name, @px) {
     @{name}: @px * @base * 1rem;
}
//使用自定义函数
.footer{
    font-size: 20px;
    .px2rem(padding,30);
}
```

案例如下：

html代码：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width,initial-scale=1,minimum-scale=1,maximum-scale=1,user-scalable=no" />
    <title>P15_预处理less语言快速入门</title>
    <link rel="stylesheet" href="css/style1.css" type="text/css">
    <script src="js/resize.js" type="text/javascript" charset="utf-8"></script>
    <style>
        
    </style>
</head>
<body>
    <div id="box">
        <h3>
            <span>mjj</span>
        </h3>
    </div>
    <div class="active">

    </div>
    <a href="">百度一下</a>
    <button type="button" class="btn-danger">按钮1</button>
    <button type="button" class="btn-info">按钮2</button>
    <button type="button" class="btn-success">按钮3</button>
    <button type="button" class="btn-warning">按钮4</button>
</body>
</html>
```

less代码：

```less
//1.定义变量
@baseColor:#ff6700;
@width:300px;
@height:300px;
@color:#666;

//自定义函数
//1rem = 50px; 300px = 6rem;
@base:100*(375/750);
.px2rem(@name,@px){
    @{name}:@px/@base*1rem;

}
// .active{
//     border:1px solid @baseColor;
//     width:@width;
//     height:@height;
// }
// h3{
//     color:@baseColor;
//     .active;
// }

#box{
    width:100%;
    padding:20px;
    h3{
        color:@baseColor;
        .active;
        span{
            font-size:20px;
            color:@color;
        }
    }
}
.active{
    border:1px solid @baseColor;
   .px2rem(width,300);
    height:@height;
}
a{
    color:darkgoldenrod;
    &:hover{
        color:#fff;
    }
}
.btn{
    &-danger{
        background-color:red;
    }
    &-success{
        background-color:green;
    }
    &-info{
        background-color:lightblue;
    }
}
@min768:~"screen and (min-width:768px) ";
body{
    @media @min768{
        background-color:blue;
    }
}
```

![](D:\web\Typora\移动端响应式\static\images\less\less_03.gif)

具体请参考：[less链接](http://lesscss.cn/)

