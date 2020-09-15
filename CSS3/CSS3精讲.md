# CSS3精讲

CSS3是层叠样式表语言的最新版本。它带来许多期待已久的新特性，另外CSS3带来的新特性被分为”模块“。例如新的选择器、圆角、阴影、渐变、过渡、动画。以及新的布局方式

### 新特性之样式

#### css3之背景

##### 1.background-image

通过此属性添加背景图片。不同的背景图像和图像用逗号隔开，第一个设置的永远显示在最顶端

```css
body{
    background-image: url(images/hello2.jpeg),url(images/timg.jpeg);
    background-repeat: no-repeat,repeat;
    background-position: center top,center top;  
}
```

可以给不同的图片设置多个不同的属性

```css
body{
    background: url(images/hello2.jpeg) no-repeat center top,url(images/timg.jpeg) repeat center top;
}
```

##### 2.background-size

CSS3中可以指定背景图片的大小，指定的大小是父元素的宽度和高度的百分比的大小

```css
background-size: cover|contain;
```

- cover 将背景图片按照原来的缩放比，将整个容器铺满
- contain 将背景图片按照原来的缩放比，完整的显示到容器中

##### 3.background-origin

该属性指定背景图像的位置区域

`content-box`,`padding-box`和`border-box`区域内可以放置背景图像。

```css
div{
    width: 400px;
    height: 400px;
    padding: 20px;
    border: 10px solid #ff0000;
    background: url(images/hello2.jpeg) no-repeat;
    background-size: 100% 100%;
    background-origin: content-box|padding-box|border-box;
}
```

<img src="D:\web\Typora\CSS3\static\images\css_01.png" style="zoom:50%;" />

##### 4.background-clip

指定绘图区域的背景

```css
div{
    width: 400px;
    height: 400px;
    padding: 20px;
    border: 10px solid #ff0000;
    background-clip: content-box|padding-box|border-box;
    background-color: yellow;
}
```

效果显示：会发现只有内容区域有背景色。

<img src="D:\web\Typora\CSS3\static\images\css_02.png" style="zoom:50%;" />

#### css3之边框

##### box-shadow 阴影

为元素添加阴影

语法

```css
box-shodow: 水平阴影的位置 垂直阴影的位置 模糊程度 阴影大小 颜色 内阴影|外阴影
```

```css
.box{
    width: 200px;
    height: 200px;
    background-color: red;
    margin: 100px auto;
    /* 边框阴影 */
    box-shadow: -5px -5px 10px 10px #008B8B;
    border-radius: 10px 20px 30px 40px;
}
```

![](D:\web\Typora\CSS3\static\images\css_03.png)

##### border-radius 圆角

给元素添加圆角的边框

```css
border-radius: 10px 20px 30px 40px;
```

##### border-image 边框图片

```css
border-image-source:url('');//用于指定要勇于绘制边框的图像的位置
border-image-slice:10; //图像边界向内偏移
border-image-repeat: repeat(重复)|round(铺满)
```

```css
/* 边框图片设置 */
border-image-source: url(images/border-img.jpeg);
/* 图像边界向内偏移 */
border-image-slice: 20;
/* 铺满 */
border-image-repeat: round;
/* 重复 */
border-image-repeat: repeat;
```

#### css3之文本效果

##### text-shadow 文本阴影

语法：

```css
text-shadow: h-shadow v-shadow blur(可选) color(可选);
```



```css
h1{    
	text-shadow: 2px 2px #ff0000;
}
```

##### text-overflow 如何显示溢出内容

语法:

```css
text-overflow:clip(修剪文本)|ellipsis(超出显示省略符号)
```

```css
p{
    width: 100px;
    height: 40px;
    /* font-size: 40px; */
    line-height: 40px;
    border: 1px solid red; 
    /* text-shadow: 10px 20px 5px #f00 ; */
    overflow: hidden;
    white-space: nowrap;
    text-overflow: ellipsis;
}
```

### 新特性之选择器

#### 属性选择器

选中所有带有class属性的元素设置样式

```css
[class]{
    color: red;
}
```

选中`class=“active“`的所有元素设置样式

```css
[class=active]{
    border: 1px solid #000;
}
```

尤其是input，我们可以很方便的通过属性选择器选中

```css
input[type='submit']{
}
input[type='file']{
}
....
```

#### 伪类和伪元素选择器

```css
1.a标签的爱恨准则
a:link{} 没有被访问过
a:visited{} 访问过后
a:hover{} 鼠标悬停
a:active{} 鼠标摁住
2.伪元素选择器
::after{
    content:'hello world'
}
::before{
    content:"hello"
}
```

伪元素：作用于元素的一部分

```css
p::first-line
 {color:#555}
p::first-letter
 {color:#666}
a::before
 {content : "hello
 world";}
```

#### 其它选择器

```css
:first-child{} 第一个元素
:last-child{} 最后一个元素
:nth-child(n){} 当前指定的元素
:nth-child(2n){} 偶数
:nth-child(2n-1){} 奇数
:nth-child(xn+1){} 隔x-1行选中元素
```

### 新特性之颜色渐变

css3渐变(gradients)可以让两个或多个指定颜色之间显示平稳的过渡。之前显示颜色渐变是通过图片。但是,通过使用css3渐变，你可以减少下载的时间和宽带的使用。此外，渐变效果的元素在放大时看起来效果更好。因为渐变是由浏览器产生的。

为了让各大浏览器上都识别对应的属性，要加上对应的浏览器引擎前缀：
-ms- 兼容IE浏览器
-moz- 兼容firefox
-o- 兼容opera
-webkit- 兼容chrome 和 safari

#### 线性渐变

语法:

```css
background: liner-gradient(方向,颜色1,颜色2,....);
```

**线性渐变-从上往下(默认)**

**线性渐变-从左往右**

```css
background: -webkit-linear-gradient(left, red , blue); /* Safari 5.1 - 6.0 */
background: -o-linear-gradient(right, red, blue); /* Opera 11.1 - 12.0 */
background: -moz-linear-gradient(right, red, blue); /* Firefox 3.6 - 15 */
background: linear-gradient(to right, red , blue); /* 标准的语法 */
```

**线性渐变-对角**

```css
background: -webkit-linear-gradient(left top, red , blue); /* Safari 5.1 - 6.0 */
background: -o-linear-gradient(bottom right, red, blue); /* Opera 11.1 - 12.0 */
background: -moz-linear-gradient(bottom right, red, blue); /* Firefox 3.6 - 15 */
background: linear-gradient(to bottom right, red , blue); /* 标准的语法 */
```

##### 使用角度

在渐变的方向上做更多的操作，你可以定义一个角度

角度是指水平线和渐变线之间的角度，逆时针方向计算。换句话说，0deg 将创建一个从下到上的渐变，90deg 将创建一个从左到右的渐变。

```css
background: -webkit-linear-gradient(180deg, red, blue); /* Safari 5.1 - 6.0 */
background: -o-linear-gradient(180deg, red, blue); /* Opera 11.1 - 12.0 */
background: -moz-linear-gradient(180deg, red, blue); /* Firefox 3.6 - 15 */
background: linear-gradient(180deg, red, blue); /* 标准的语法 */
```

##### 重复的线性渐变

```css
/* Safari 5.1 - 6.0 */
background: -webkit-repeating-linear-gradient(red, yellow 10%, green 20%);
/* Opera 11.1 - 12.0 */
background: -o-repeating-linear-gradient(red, yellow 10%, green 20%);
/* Firefox 3.6 - 15 */
background: -moz-repeating-linear-gradient(red, yellow 10%, green 20%);
/* 标准的语法 */
background: repeating-linear-gradient(red, yellow 10%, green 20%);
```

![](D:\web\Typora\CSS3\static\images\css_04.png)

#### 径向渐变

径向渐变由它的中心定义。

为了创建一个径向渐变，你也必须至少定义两种颜色结点。颜色结点即你想要呈现平稳过渡的颜色。同时，你也可以指定渐变的中心、形状（圆形或椭圆形）、大小。默认情况下，渐变的中心是 `center`（表示在中心点），渐变的形状是 `ellipse`（表示椭圆形），渐变的大小是 `farthest-corner`（表示到最远的角落）。

```css
background:radial-gradient(center,形状 大小,开始的颜色,....,最后的颜色);
```

##### 颜色均匀分布(默认)

```css
background: -webkit-radial-gradient(red, green, blue); /* Safari 5.1 - 6.0 */
background: -o-radial-gradient(red, green, blue); /* Opera 11.6 - 12.0 */
background: -moz-radial-gradient(red, green, blue); /* Firefox 3.6 - 15 */
background: radial-gradient(red, green, blue); /* 标准的语法 */
```

![](D:\web\Typora\CSS3\static\images\css_05.png)

##### 不均匀分布

```css
background: -webkit-radial-gradient(red 5%, green 15%, blue 60%); /* Safari 5.1 - 6.0 */
background: -o-radial-gradient(red 5%, green 15%, blue 60%); /* Opera 11.6 - 12.0 */
background: -moz-radial-gradient(red 5%, green 15%, blue 60%); /* Firefox 3.6 - 15 */
background: radial-gradient(red 5%, green 15%, blue 60%); /* 标准的语法 */
```

![](D:\web\Typora\CSS3\static\images\css_06.png)

##### 设置形状

可以通过第一个参数是`circle`或`ellipse`来定义当前的形状。其中`ellipse`是默认值。

```css
background: -webkit-radial-gradient(circle, red, yellow, black); /* Safari 5.1 - 6.0 */
background: -o-radial-gradient(circle, red, yellow, black); /* Opera 11.6 - 12.0 */
background: -moz-radial-gradient(circle, red, yellow, black); /* Firefox 3.6 - 15 */
background: radial-gradient(circle, red, yellow, black 50%); /* 标准的语法 */
```

![](D:\web\Typora\CSS3\static\images\css_07.png)

##### 重复的径向渐变

```css
/* Safari 5.1 - 6.0 */
background: -webkit-repeating-radial-gradient(red, yellow 10%, green 15%);
/* Opera 11.6 - 12.0 */
background: -o-repeating-radial-gradient(red, yellow 10%, green 15%);
/* Firefox 3.6 - 15 */
background: -moz-repeating-radial-gradient(red, yellow 10%, green 15%);
/* 标准的语法 */
background: repeating-radial-gradient(red, yellow 10%, green 15%);
```

![](D:\web\Typora\CSS3\static\images\css_08.png)

### 2D转换

CSS3 准换可以对元素进行**移动**、**缩放**、**转动**、**拉长或拉伸**

首先我们需要给元素设置transform属性

语法:

```css
transform: 移动|缩放|转动|拉伸
```

#### translate()方法

该方法，根据x轴和y轴位置给定的参数，从当前元素**移动**。

```css
transform:translate(30px,20px);
-ms-transform: translate(30px,20px);
-webkit-transform: translate(30px,20px);
//水平反向移动30px,垂直方向移动20px,如果是负值表示反方向
```

#### rotate()方法

rotate()方法，在一个给定度数顺时针旋转的元素。负值是允许的，这样是元素逆时针旋转。

```css
transform: rotate(30deg);
-ms-transform: rotate(30deg); /* IE 9 */
-webkit-transform: rotate(30deg); /* Safari and Chrome */
//表示顺时针旋转30度
```

#### scale()方法

使元素增加或减小。取决于此方法的第一个参数(宽度)和第二个参数(高度)

语法:

```css
transform:scale(2,3);
```

`scale（2,3）`转变宽度为原来的大小的2倍，和其原始大小3倍的高度。

#### skew()方法

语法:

```css
transform: skew(x轴角度,y轴角度);
```

```css
transform: skew(30deg,20deg);
-ms-transform: skew(30deg,20deg); /* IE 9 */
-webkit-transform: skew(30deg,20deg); /* Safari and Chrome */
```

### 3D转换

css3允许使用3D转换对元素进行格式化。

##### perspective

在设置3D效果之前，要给父元素设置`perspective`属性，此值通常在600到1000之间。才能看出3D效果。通常该属性与perspective-origin属性一同使用，这样就能改变3D元素的地底部位置。

```css
perspective:1000px;
```

##### perspective-origin

定义3D元素所基于的x轴和y轴。该属性允许改变3D元素的底部位置。

```css
perspective-origin:50% 50%;
```

##### transform-style

指定嵌套元素是怎么样在三维空间中呈现。

> 注意：使用该属性必须先使用transform属性

```css
transform-style: flat|preserve-3d
//flat: 表示所有子元素在2D平面呈现
//preserve-3d: 表示所有子元素在3D空间中呈现
```

#### 3D转换方法

**translateX(x)**;scale3d(*x*,*y*,*z*);scaleX(*x*);rotate3d(*x*,*y*,*z*,*angle*)

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>P8_transform3D转换</title>
		<style>
			.father{
				width: 500px;
				height: 500px;
				margin: 100px auto;
				/* 设置透视 实现3D效果必须的属性 距离越小 越明显 反之亦然 */
				perspective: 800px;
				perspective-origin: center center; 
				/* 设置3D的效果 */
				transform-style: preserve-3d;
			}
			.box{
				width: 200px;
				height: 200px;
				background-color: red;
				margin-bottom: 20px;
				
			}
			.box:nth-child(1):hover{
				  transform: rotateX(40deg) translateZ(100px); 
				 /* transform: translateZ(100px);; */
			}
			.box:nth-child(2):hover{
				 /* transform: rotateY(40deg); */
				 transform: translateZ(-100px);;

			}
			.box:nth-child(3):hover{
				 /* transform: rotateZ(40deg); */
				 transform: translateZ(100px);;

			}
		</style>
	</head>
	<body>
		<div class="father">
			<div class="box"></div>
			<div class="box"></div>
			<div class="box"></div>
		</div>
	</body>
</html>

```

### 新特性之过渡

在css3中，为了添加某个效果可以从一种样式转变到另一个样式，无需使用flash动画或JavaScript。

#### 过渡属性 transition

| 属性                       | 描述                                                         |
| -------------------------- | ------------------------------------------------------------ |
| transition                 | 简写属性，用于一个属性中设置四个过渡属性                     |
| transition-property        | 规定应用过渡的css属性的名称。                                |
| transiton-duration         | 过渡的时间，默认是0                                          |
| transition-timing-function | 规定过渡效果的时间曲线。默认是ease:慢->快->慢，ease-in:慢速开始，ease-out：慢速结束，ease-in-out:慢->快->慢，linear:匀速 |
| transition-delay           | 延迟几秒之后执行。默认是0                                    |

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>P9_transition过度属性</title>
		<style>
			.box{
				width: 200px;
				height: 200px;
				background-color: #000;
				transition: all 2s linear 2s;
				/* 设置过渡的属性 */
				/* transition-property: all; */
				/* 过渡的时间 */
				/* transition-duration: 3s; */
				/* 过渡的函数 ease 慢 =》快=》慢  linear：匀速 */
				/* transition-timing-function: linear; */
				/* transition-delay: 2s; */
			}
			.box:hover{
				/* background-color: red; */
				margin-left: 500px;
			}
		</style>
	</head>
	<body>
		<div class="box"></div>
	</body>
</html>

```

### 新特性之动画

在css3中，我们可以创建动画，它可以取代许多网页动画图像，比如Flash图像和js

#### [@keyframes](https://github.com/keyframes)

要创建css3的动画，你必须知道`@keyframes`规则。它的规则是创建动画。规则内指定一个css样式和动画开始以及结束的样式

```css
@keyframes change{
    from{
        background-color: red;
    }
    to{
        background-color: green;
    }
}
```

定义好规则之后，把它绑定到一个选择器，否则动画不会有任何效果。

#### animation

```css
.box{
    width: 200px;
    height: 200px;
    animation: change 3s;
    background-color: green;
}
```

你可以用百分比规定变化发生的事件。from和to，等同于0%和100%。0%是动画的开始，100%是动画的完成。

```css
@keyframes change
{
    0%   {background: red;}
    25%  {background: yellow;}
    50%  {background: blue;}
    100% {background: green;}
}
```

#### css3的动画属性

| 属性                                       | 描述                                                     |
| ------------------------------------------ | -------------------------------------------------------- |
| [@keyframes](https://github.com/keyframes) | 规定动画                                                 |
| animation                                  | 所有动画属性的简写属性                                   |
| animation-name                             | 规定[@keyframes](https://github.com/keyframes)动画的名称 |
| animation-duration                         | 规定动画执行的时间。默认是0                              |
| animation-timeing-function                 | 规定动画的速度曲线。跟transition-timing-function值一样   |
| animation-delay                            | 延迟几秒执行动画                                         |
| animation-iteration-count                  | 规定动画被播放的次数。默认是1。通常取值infinite:永远     |
| animation-direction                        | 规定动画是否在下一周期逆向地播放。normal\reverse         |

```css
		<style>
			.box {
				width: 200px;
				height: 200px;
				background-color: green;
				/* animation-name: change;
				animation-duration: 2s;
				animation-timing-function: linear;

				animation-delay: 2s;
				规定动画播放的次数，默认是一次 infinite：表示无限次 
				animation-iteration-count: infinite; */
				animation: move 2s linear infinite;

			}
			.box:hover{
                animation-play-state: paused;
                /* running,一直运动 */
			}

			@keyframes change {

				/* from{
					background-color: red;
				}
				to{
					background-color: green;
				} */
				0% {
					background-color: red;
				}

				25% {
					background-color: yellow;
				}

				75% {
					background-color: blue;
				}

				100% {
					background-color: green;
				}
			}
			
			@keyframes move{
				from{
					transform: rotate(0deg);
				}
				to{
					transform: rotate(360deg);
				}
			}
		</style>
```

**CSS3d实现动画相册**

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title>css3实现相册</title>
        <style>
            .father {
                width: 200px;
                height: 200px;
                /* -webkit-perspective: 1000px; */
                position: relative;
                margin: 300px auto;
                -webkit-transform-style: preserve-3d;
                -webkit-transform: rotateX(13deg);
                -webkit-animation: move 5s linear infinite;
            }
            .father .box {
                width: 200px;
                height: 200px;
                /* background-color: #000000; */
                border: 1px solid #666;
                position: absolute;
                top: 50%;
                left: 50%;
                margin-top: -100px;
                margin-left: -100px;
                opacity: 0.5;
                -webkit-transition: all 3s ease;
            }
            /* 前 */
            .box:nth-child(1) {
                -webkit-transform: translateZ(100px);
            }
            /* 后 */
            .box:nth-child(2) {
                -webkit-transform: rotateX(180deg) translateZ(100px);
            }
            /* 下 */
            .box:nth-child(3) {
                -webkit-transform: rotateX(-90deg) translateZ(100px);
            }
            /* 上 */
            .box:nth-child(4) {
                -webkit-transform: rotateX(90deg) translateZ(100px);
            }
            /* 左 */
            .box:nth-child(5) {
                -webkit-transform: rotateY(-90deg) translateZ(100px);
            }
            /* 右 */
            .box:nth-child(6) {
                -webkit-transform: rotateY(90deg) translateZ(100px);
            }
            .box:nth-child(1) {
                background: url(images/1.png) no-repeat 0 0;
                /* -webkit-transform: translateZ(100px); */
            }
            .box:nth-child(2) {
                background: url(images/2.png) no-repeat 0 0;
            }
            .box:nth-child(3) {
                background: url(images/3.png) no-repeat 0 0;
            }
            .box:nth-child(4) {
                background: url(images/4.png) no-repeat 0 0;
            }
            .box:nth-child(5) {
                background: url(images/5.png) no-repeat 0 0;
            }
            .box:nth-child(6) {
                background: url(images/6.png) no-repeat 0 0;
            }
            .father:hover .box:nth-child(1) {
                -webkit-transform: translateZ(300px);
                width: 400px;
                height: 400px;
                opacity: 0.8;
                left: -100px;
                top: -50px;
            }
            .father:hover .box:nth-child(2) {
                -webkit-transform: rotateX(180deg) translateZ(300px);
                width: 400px;
                height: 400px;
                opacity: 0.8;
                left: -100px;
                top: -50px;
            }
            .father:hover .box:nth-child(3) {
                -webkit-transform: rotateX(-90deg) translateZ(300px);
                width: 400px;
                height: 400px;
                opacity: 0.8;
                left: -100px;
                top: -50px;
            }
            .father:hover .box:nth-child(4) {
                -webkit-transform: rotateX(90deg) translateZ(300px);
                width: 400px;
                height: 400px;
                opacity: 0.8;
                left: -100px;
                top: -50px;
            }
            .father:hover .box:nth-child(5) {
                -webkit-transform: rotateY(-90deg) translateZ(300px);
                width: 400px;
                height: 400px;
                opacity: 0.8;
                left: -100px;
                top: -50px;
            }
            .father:hover .box:nth-child(6) {
                -webkit-transform: rotateY(90deg) translateZ(300px);
                width: 400px;
                height: 400px;
                opacity: 0.8;
                left: -100px;
                top: -50px;
            }
            @keyframes move {
                from {
                    transform: rotateX(13deg) rotateY(0deg);
                }
                to {
                    transform: rotateX(13deg) rotateY(360deg);
                }
            }
        </style>
    </head>
    <body>
        <div class="father">
            <div class="box"></div>
            <div class="box"></div>
            <div class="box"></div>
            <div class="box"></div>
            <div class="box"></div>
            <div class="box"></div>
        </div>
    </body>
</html>
```

![](D:\web\Typora\CSS3\static\images\css_01.gif)

### 新特性之网页布局

#### 多列布局

css可将文本内容涉及成像报纸一样的多列布局。

##### 多列属性

- `column-count` 指定元素应该被分割的列数
- `column-gap` 指定列和列之间的间隙
- `column-rule-style` 列边框的样式
- `column-rule-width`列边框的宽度
- `column-rule-color` 列边框的颜色
- `column-rule` 列边框的简写
- `column-span` 跨域多少列
- `column-width` 指定列的宽度

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>P11_多列布局</title>
		<style>
			.box{
				/* 指定需要分割的列数 */
				column-count: 3;
				/* 指定列和列之间的间隙 */
				column-gap: 50px;
				/* 列边框的样式 */
				column-rule: 1px solid #bbb;
				column-span: all;
				/* column-width: 500px; */
			}
		</style>
	</head>
	<body>
		<div class="box">
			<div>
				学完此课程，你编写的代码就可以移动设备上完美兼容啦。HTML5是移动端开发最常用的技术，熟悉HTML5新标签和Api以及CSS3的新的Api，可以使你的网页更加的绚丽多彩，并且开发出你意想不到的网页效果。
			</div>
			<div>
				学完此课程，你编写的代码就可以移动设备上完美兼容啦。HTML5是移动端开发最常用的技术，熟悉HTML5新标签和Api以及CSS3的新的Api，可以使你的网页更加的绚丽多彩，并且开发出你意想不到的网页效果。
			</div>
			<div>
					学完此课程，你编写的代码就可以移动设备上完美兼容啦。HTML5是移动端开发最常用的技术，熟悉HTML5新标签和Api以及CSS3的新的Api，可以使你的网页更加的绚丽多彩，并且开发出你意想不到的网页效果。
					学完此课程，你编写的代码就可以移动设备上完美兼容啦。HTML5是移动端开发最常用的技术，熟悉HTML5新标签和Api以及CSS3的新的Api，可以使你的网页更加的绚丽多彩，并且开发出你意想不到的网页效果。
			</div>

		</div>
	</body>
</html>

```

#### 弹性盒布局

布局的传统解决方案，基于盒子模型，依赖display属性+position属性+float属性好。它对于那么特殊的布局非常不方便。比如垂直居中就不容易实现。

2009年，W3C 提出了一种新的方案——Flex 布局，可以简便、完整、响应式地实现各种页面布局。目前，它已经得到了所有浏览器的支持，这意味着，现在就能很安全地使用这项功能。

##### 1.Flex布局是什么

Flex是Flexible Box的缩写，也叫”弹性布局”，用来为盒状模型提供最大的灵活性。

任何一个容器都可以指定为Flex布局

```css
.box{
    display:flex;
}
```

*注意：设置Flex布局以后，子元素的float、clear和vertical-align属性将失效*

一旦给父元素设置了`display:flex;`，这个父元素称为Flex容器。它的所有子元素自动成为该容器成员，称为Flex项目，简称项目

![](D:\web\Typora\CSS3\static\images\css_09.png)

容器默认存在两根轴：水平的主轴（main axis）和垂直的交叉轴（cross axis）。主轴的开始位置（与边框的交叉点）叫做`main start`，结束位置叫做`main end`；交叉轴的开始位置叫做`cross start`，结束位置叫做`cross end`。**项目默认沿主轴排列。**

##### 2.容器的常用属性

1. `flex-direction : 定义主轴的方向`
2. `flex-wrap : 项目在主轴上是否换行`
3. `flex-flow : flex-direction和flex-wrap的简写方式`
4. `justify-content :定义项目在主轴上的排列方式`
5. `align-items: 定义项目在垂直交叉轴的排列方式`
6. `align-content:定义多根轴线的对齐方式`

**2.1 flex-direction**

`flex-direction`属性决定主轴的方向(既项目的排列方向)

```css
.box{
    flex-direction: row | row-reverse | column | column-reverse
}
```

分别对应着项目排列方式

![](D:\web\Typora\CSS3\static\images\css_10.png)

###### **2.2 flex-wrap属性**

默认情况下，子项目都排在一条轴线上。`flex-wrap`属性定义如果一条轴线拍不下，如何换行。

```css
.box{
    flex-wrap:nowrap | wrap | wrap-reverse;
}
```

【1】`nowrap`（默认）：不换行

![](D:\web\Typora\CSS3\static\images\css_11.png)

【2】`wrap`：换行，第一行在上方

![](D:\web\Typora\CSS3\static\images\css_12.png)

【3】`wrap-reversse`: 换行，第一行在下方

![](D:\web\Typora\CSS3\static\images\css_13.png)

###### **2.3 flex-flow属性**

flex-flow属性是flex-direction属性和flex-wrap属性的简写形式，默认值为row nowrap

```css
.box{
    flex-flow: <flex-direction> <flex-wrap>;
}
```

**2.4 justify-content属性**

`justify-content`属性定义了项目在主轴上的对齐方式

```css
.box {
    justify-content: flex-start | flex-end | center | space-between | space-around;
}
```

【1】flex-start

![](D:\web\Typora\CSS3\static\images\css_14.png)

【2】flex-end

![](D:\web\Typora\CSS3\static\images\css_15.png)

【3】center

![](D:\web\Typora\CSS3\static\images\css_16.png)

【4】space-between

![](D:\web\Typora\CSS3\static\images\css_17.png)

【5】space-around

![](D:\web\Typora\CSS3\static\images\css_18.png)

具体对齐方式与轴的方向有关。假设主轴从左到右

- `flex-start`（默认值）：左对齐
- `flex-end`：右对齐
- `center`： 居中
- `space-between`：两端对齐，项目之间的间隔都相等。
- `space-around`：每个项目两侧的间隔相等。所以，项目之间的间隔比项目与边框的间隔大一倍。

**2.5 align-items属性**

`align-items`属性定位项目在垂直交叉轴上如何对齐

```css
.box {
  align-items: flex-start | flex-end | center | baseline | stretch;
}
```

- `flex-start`：交叉轴的起点对齐。
- `flex-end`：交叉轴的终点对齐。
- `center`：交叉轴的中点对齐。 
- `baseline`: 项目的第一行文字的基线对齐。
- `stretch`（默认值）：如果项目未设置高度或设为auto，将占满整个容器的高度。  stretch 不设置子盒子的高度观察

baseline

![](D:\web\Typora\CSS3\static\images\css_19.png)

**2.6 align-content属性**

`align-content`属性定义了多根轴线的对齐方式。该属性配合`flex-wrap:wrap`一起使用。

```css
.box {
    align-content: flex-start | flex-end | center | space-between | space-around | stretch;
}
```

- `flex-start`：与交叉轴的起点对齐。
- `flex-end`：与交叉轴的终点对齐。
- `center`：与交叉轴的中点对齐。
- `space-between`：与交叉轴两端对齐，轴线之间的间隔平均分布。
- `space-around`：每根轴线两侧的间隔都相等。所以，轴线之间的间隔比轴线与边框的间隔大一倍。
- `stretch`（默认值）：轴线占满整个交叉轴。

stretch 不设置子盒子的高度，查看效果

![](D:\web\Typora\CSS3\static\images\css_20.png)

##### 3.项目属性

```css
order
flex-grow
flex-shrink
flex-basis
flex
align-self
```

**3.1 order属性**

`order`属性定义项目的排列方式。数值越小，排列越靠前。默认为0

```css
.item{
    oreder: <integer>
}
```

###### 3.2 flex-grow属性

`flex-grow`属性定义项目的放大比例，默认为0，既如果存在剩下空间，也不放大

```css
.item{
    flex-grow:<numer>; /*默认为0*/
}
```

###### 3.3 flex-shrink 属性

`flex-shrink`属性定义了项目的缩小比例，默认是1，既如果空间不足，该项目将缩小

如果所有的项目`flex-shrink`属性都为1，当空间不足时，都将等比例缩小。如果一个项目的`flex-shrink`属性为0，其它项目都为1，则空间不足时，前者不缩小

![](D:\web\Typora\CSS3\static\images\css_21.png)

###### 3.4 flex-basis属性

`flex-basis`属性定义了在分配多余空间之间，项目占据的主轴空间。浏览器根据这个属性，计算主轴是否有多余空间。它的默认值为auto,即项目的本来大小

![](D:\web\Typora\CSS3\static\images\css_22.png)

###### 3.5 flex属性

flex属性是`flex-grow`,`flex-shrink`,`flex-basis`的简写，默认值为0 1 auto,后两个属性可选

该属性有两个快捷键：`auto(1 1 auto)`和`none (0 0 auto)`

常用属性案例：

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>P12_Flex布局容器常用属性一</title>
		<style>
			.father{
				width: 800px;
				height: 800px;
				border: 1px solid #000;
				
				/* 弹性盒 */
				display: flex;
				justify-content: center;
				align-items: center;
				/* 决定主轴的方向 */
				/*flex-direction: row;*/
				/* flex-direction: row-reverse; */
				/* flex-direction: column-reverse; */
				
				/* 决定了子项目的换行不换行 */
				/* flex-wrap: nowrap; */
				/* flex-wrap: wrap-reverse; */
				/* 它是 flex-direction和flex-wrap的简写方式*/
				/* flex-flow: column wrap; */
				
				/* 定义当前的项目在主轴上的排列方式 */
				/* justify-content: flex-start;
				justify-content: flex-end;
				justify-content: center;
				justify-content: space-between;
				justify-content: space-around; */
				/* 交叉轴的起点对齐 */
				/* align-items: flex-start; */
				/* 交叉轴的末点对齐 */
				/* align-items: flex-end; */
				/* 默认值 给项目不设置高度时观察 */
				/* align-items: stretch; */
				/* 交叉轴的中点对齐 */
				/* align-items: center; */
				/* align-items: baseline; */
				/*flex-wrap: wrap;*/
				/* align-content一定要结合flex-wrap：wrap一起使用 */
				/*align-content: center;*/
				
				/*justify-content: center;*/

			}
			.father div{
				width: 300px;
				height: 300px;	
				line-height: 200px;
				font-size: 40px;
				font-weight: 700;
				color: #fff;
				text-align: center;
			}
			.father .item:nth-child(1){
				width: 100px;
				line-height: 400px;
				background-color: red;
			}
			.father .item:nth-child(2){
				width: 200px;
				line-height: 100px;
				background-color: green;
			}
			.father .item:nth-child(3){
				width: 100px;
				background-color: yellow;
			}
		</style>
	</head>
	<body>
		<div class="father">
			<div class="item">1</div>
			<div class="item">2</div>
			<div class="item">1</div>
			<div class="item">1</div>
			<!-- <div class="item">2</div>
			<div class="item">3</div> -->
			
		</div>
	</body>
</html>
```

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>P14_Flex布局项目常用属性</title>
		<meta name="viewport" content="width=device-width, initial-scale=1.0">
		<style>
			.box {
				height: 800px;
				width: 1000px;
				border: 1px solid #000;
				display: flex;
			}

			.box .item {
				width: 200px;
				height: 100px;
				border: 2px solid darkgoldenrod;
				background-color: darkcyan;
				margin-right: 10px;
				margin-bottom: 10px;
				font-size: 30px;
				color: #fff;
				/* 定义项目的排列方式，数值越小，排列越往前 */
				/* order: 2; */
				/* 定义子元素的放大比例，默认值是0 ，存在剩余的空间，子元素也不放大*/
				/* 如果给子元素设置flex-grow为1，则将等分剩余的空间 */
				/* flex-grow: 1; */
				
				/* 定义子元素的缩小比例，默认值是1，如果flex-shrink设置为0，则不缩小比例，则当前元素不缩小 */
				/* flex-shrink: 1; */
				/* flex-basis:500px; 0% //设置尺寸大小 */
				
				/* 简写 */
				/* flex: auto; */
				/* flex: 1 1 auto; */
				/* flex-grow: 1  flex-shrink: 1 flex-basis: auto */
				/* flex: none; */
				/* flex: 0 0 auto */
				flex: 1;
				/* flex: 1 1 0%; 0%：按照自己尺寸大小 */
				
			}

			.box .item:nth-child(2) {
				/* order: 1; */
				/*  flex-shrink设置为0，则不缩小比例*/
				/* flex-shrink: 0; */
				
				/* 定义项目在主轴上的占据的空间大小 */
				/* flex-basis: 500px; */
				
			}
		</style>
	</head>
	<body>
		<div class="box">
			<div class="item">1</div>
			<div class="item">2</div>
			<div class="item">3</div>
			<div class="item">4</div>
			<div class="item">5</div>
			<div class="item">6</div>
			<div class="item">7</div>
			<div class="item">8</div>
			<div class="item">9</div>
		</div>
	</body>
</html>

```

