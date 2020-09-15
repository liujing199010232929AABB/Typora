# HTML5精讲

#### 音频audio

HTML5提供了播放音频文件的标准。直到现在，仍然不存在在网页上播放音频的标准。今天，大多数音频都是通过插件(比如Flash)来播放的。然而，并非所有浏览器都拥有同样的插件。

```html
<audio controls="controls">
    <source src="static/music/轻音乐.mp3"  type="audio/mp3">
    您的浏览器不支持audio元素
    </audio>
<br>
<audio src="static/music/一剪梅.mp3" controls autoplay>

</audio>
```

- controls属性添加音频的控件，播放、暂停和音量控件
- autoplay:使音频自动播放
- loop:使音频自动重复播放

在`<audio>`与`</audio>`之间插入浏览器不支持的提示文本。`audio`元素允许使用多个`source`标签，`source`标签可以链接不同的音频文件，浏览器将使用第一个支持的音频文件。

##### 浏览器支持

目前，此标签支持三种音视频格式文件：MP3/Wav和Ogg;

| 浏览器               | MP3  | Wav  | Ogg  |
| -------------------- | ---- | ---- | ---- |
| Internet Explorer 9+ | YES  | NO   | NO   |
| Chrome 6+            | YES  | YES  | YES  |
| Firefox 3.6+         | YES  | YES  | YES  |
| Safari 5+            | YES  | YES  | NO   |
| Opera 10+            | YES  | YES  | YES  |

同样，audio可以配合JS来实现自己的音乐播放器

大家可以参考MDN`video`和`audio`标签的相关事件：[媒体对象相关事件](https://developer.mozilla.org/zh-CN/docs/Web/Guide/Events/Media_events),[DOM相关事件](http://www.w3school.com.cn/tags/html_ref_audio_video_dom.asp)

##### 纯js实现古风音乐播放器案例：

html代码：

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>古风播放器</title>
    <link type="text/css" href="css/style.css" rel="stylesheet">
</head>
<body>
    <div class="btns-bg">
        <div class="PlayEy"></div>
        <div class="Btn"></div>
        <div class="Play">
            <!-- <audio id="audios" src="http://music.163.com/song/media/outer/url?id=504924216.mp3"></audio> -->
            <audio id="audios" src="music/一剪梅.mp3"></audio>
        </div>
    </div>
    <script type="text/javascript" src="js/script.js">
        // var i = 0;
        // var oPlayEy = document.getElementsByClassName("PlayEy")[0];
        // var oPlay = document.getElementsByClassName("Play")[0];
        // var audios = document.getElementById('audios');
        // oPlay.onclick = function () {
        //     var seii = setInterval(function () {
        //         (i == 360) ? i = 0: i++;
        //         oPlayEy.style.transform = "rotate(" + i + "deg)";
        //         if (audios.paused) {
        //             clearInterval(seii)
        //         }
        //     }, 30);
        //     if (audios.paused) {
        //         audios.play();
        //         oPlay.style.backgroundImage = "url(../pic/play.png)";
        //         oPlay.style.width = 32 + "px";
        //         oPlay.style.height = 32 + "px";
        //     } else {
        //         audios.pause();
        //         oPlay.style.backgroundImage = "url(../pic/pause.png)";
        //         oPlay.style.width = 29 + "px";
        //         oPlay.style.height = 36 + "px";
        //     }
        // }
    </script>
</body>
</html>
```

style.css代码：

```css
body {
    margin:0;
    background-repeat:no-repeat;
    background-position:50%;
    background-image:url(../pic/page-bg.png);
    background-size:100% auto;
    background-color:#efebcb;
}
.PlayEy {
    display:flex;
    justify-content:center;
    align-items:center;
    width:653px;
    height:653px;
    background:url(../pic/bg_circle.png), url(../pic/bg_center.png) no-repeat center;
    /* background-size:100%  auto; */
}
.Btn {
    position:absolute;
    display:flex;
    justify-content:center;
    align-items:center;
    width:95px;
    height:95px;
    background-color:#ff0;
    background:url(../pic/btn-bg.png) no-repeat;
    animation:change 3s linear infinite;
}
@keyframes change{
	from{
		transform: rotate(0deg);
	}
	to{
		transform: rotate(360deg);

	}
}
.Play {
    position:absolute;
    width:29px;
    height:36px;
    background:red;
    background:url(../pic/pause.png) no-repeat;
    transition:.5s;
    /* 切换时的过度 */
}
.btns-bg {
    display:flex;
    justify-content:center;
    align-items:center;
    margin:30px auto;
    width:653px;
    height:653px;
}

```

script.js代码：

```javascript
window.onload = function(){
	// 1.获取标签
	var PlayEy = document.querySelector('.PlayEy');
	var Play = document.querySelector('.Play');
	var audio = document.querySelector('audio');
	var i = 0;//默认的角度
	// 2.监听事件
	Play.onclick = function(){
		
		var timer = setInterval(function(){
			i++;
			if(i == 360){
				i = 0;
			}
			PlayEy.style.transform = `rotate(${i}deg)`
			if(audio.paused){
				clearInterval(timer);
			}
		},30);
		
		
		if(audio.paused){
			audios.play();
			Play.style.backgroundImage = `url('pic/play.png')`;
		}else{
			audios.pause();
			Play.style.backgroundImage = `url('pic/pause.png')`;
		}
	}
}
```

#### 视频video

##### 基本使用

```html
<video width="800" height="" controls="">
    <source src="Hero.mp4" type="video/mp4"></source>
	<source src="Hero.ogv" type="video/ogg"></source>
	<source src="Hero.webm" type="video/webm"></source>
	当前浏览器不支持 video直接播放
</video>
```

video元素提供了 播放、暂停和音量控件来控制视频。

同时`<video>` 元素也提供了 width 和 height 属性控制视频的尺寸.如果设置的高度和宽度，所需的视频空间会在页面加载时保留。如果没有设置这些属性，浏览器不知道大小的视频，浏览器就不能再加载时保留特定的空间，页面就会根据原始视频的大小而改变。

##### 简单视频的DOM操作

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title>01_简单视频的DOM操作</title>
    </head>
    <body>
        <div class="box">
            <button id="playOrStop">播放/暂停</button>
        </div>
        <video width="800" height="">
            <source src="static/music/短视频.mp4" type="video/mp4"></source>
            <!-- <source src="static/music一剪梅.mp3" type="video/mp3"></source> -->
            <!-- <source src="static/music/一剪梅.mp3" type="video/webm"></source> -->
            当前浏览器不支持 video直接播放
        </video>
        <audio src="static/music/一剪梅.mp3" controls="controls" autoplay>

        </audio>
        <script type="text/javascript">
            var playOrStop  = document.getElementById('playOrStop');
            var video  = document.querySelector('video');
            var audio  = document.querySelector('audio');
            console.dir(video);//html:25
            console.dir(audio);//html:26
            console.dir(playOrStop);//html:27
            playOrStop.onclick = function(){
                console.log(video.paused);
                if(video.paused){
                    video.play();
                }else{
                    video.pause();
                }
                if(audio.paused){
                    audio.play();
                }else{
                    audio.pause();
                }

            }
        </script>
    </body>
</html>
```

![](D:\web\Typora\HTML5\static\images\html5\h5_3.png)

#### HTML5实现调用摄像头

```html
<!DOCTYPE html>
<html lang="zh">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <meta http-equiv="X-UA-Compatible" content="ie=edge">
        <title>02_HTML5实现调用摄像头</title>
    </head>
    <body>
        <!-- 要想实现调用摄像头，使用了html5的getUserMedia()API -->
        <video id="video" autoplay style="width: 480px;height: 320px;"></video>
        <div>
            <button id="capture">拍照</button>
        </div>
        <!-- 展示拍摄的照片 -->
        <canvas id="canvas" width="480" height="320"></canvas>
        <script type="text/javascript">
            window.onload = function() {
                // 1.获取标签
                var video = document.getElementById('video');
                var capture = document.getElementById('capture');
                var ctx = document.getElementById('canvas').getContext('2d');
                // 调用媒体对象
                // 参数为constraints 一个约束对象  是video还是audio
                navigator.mediaDevices.getUserMedia({
                    video: {
                        width: 480,
                        height: 320
                    }
                }).then(function(stream) {
                    // 获取到window.URL对象
                    var URL = window.URL || window.webkitURL;
                    // 创建一个video的url字符串
                    try {
                        video.src = URL.createObjectURL(stream);
                    } catch (e) {
                        video.srcObject = stream;
                    }
                    // 视频播放
                    video.play();
                }).catch(function(err) {
                    console.log(err);
                })
                // 点击拍照按钮事件
                capture.onclick = function() {
                    ctx.drawImage(video,0,0,480,320);
                }
            }
        </script>
    </body>
</html>
```

#### 音视频的常用属性

##### 	常用属性

​    1.autoplay 自动播放 布尔值 默认是false

​    2.loop 是否自动循环播放歌曲 布尔值 默认是false

​    3.muted 是否静音

​    4.volume 音量

​    5.paused 是否停止播放

​    6.duration 返回当前音频的总时间

​    7.currentTime 当前的播放时间

#####     常用事件

​    1.loadstart ： 当浏览器开始查找音视频触发

​    2.progress: 当浏览器正在下载音频/视频时触发。

​    3.durationchange : 当音视频的时长已更改时触发

​    4.loadedmetadata: 当浏览器已加载音视频的元数据触发

​    5.loadeddata: 当浏览器已加载音频/视频的当前帧时触发。

​    6.canplay: 当浏览器可以开始播放音频/视频时触发。

​    7.canplaythrough: 当浏览器可在不因缓冲而停顿的情况下进行播放时触发。

```javascript
var audio = document.querySelector('audio');
// audio.autoplay = true;
// audio.loop = true;
// audio.muted = true;
// audio.volume = 0.5;//控制音量

audio.onloadstart = function() {
    console.log('1开始查找音视频');
}
audio.onprogress = function() {
    console.log('2当浏览器正在下载音频/视频时触发');
}
audio.ondurationchange = function() {
    console.log('3当音视频的时长已更改时触发');
}
audio.onloadedmetadata = function() {
    console.log('4当浏览器已加载音视频的元数据触发');
}
audio.onloadeddata = function() {
    console.log('5当浏览器已加载音频/视频的当前帧时触发');
}
audio.oncanplay = function() {
    console.log('6当浏览器可以开始播放音频/视频时触发');
}
audio.oncanplaythrough = function() {
    console.log('7当浏览器可在不因缓冲而停顿的情况下进行播放时触发');
}

// 当目前的播放位置发生改变时
audio.ontimeupdate = function(){
    console.log('音频一直在播放');
}
audio.onended = function(){
    console.log('音频已播放结束');
}
// audio.play();
// audio.pause();
audio.onvolumechange = function(){
    console.log('音量变化了');
}
```

### Canvas画布

#### 基本用法

```html
<canvas id='canvas' width="150" height="150"></canvas>
```

`<canvas>`看起来跟`img`标签有点像，唯一不同的是它没有src属性和alt属性。实际上，canvas标签只有两个属性:`width`和`height`。

如果没有设置宽度和高度，默认的canvas会初始化`width：300px,height:150px`

##### 渲染上下文对象

`canvas`标签创造了一个固定大小的画布，它有一个或者多个**渲染上下文对象**，用它可以绘制和处理要展示的内容。接下来我们把注意力放在2D渲染上下文中。

canvas起初是空白的。为了展示，首先脚本需要找到渲染上下文，然后在它的上面绘制。`canvas`元素有一个叫做 `getContext()`的方法，这个方法是用来获得渲染上下文和它的绘画功能。`getContext()`只有一个参数，上下文的格式。

```javascript
var canvas = document.querySelector('canvas');
//获取绘画上下文对象
var ctx = canvas.getContext('2d');
console.log(canvas.__proto__);//html:15
console.log(ctx);//html:16
```

![](D:\web\Typora\HTML5\static\images\html5\h5_1.png)

#### 绘制形状

在这里你将学会如何绘制矩形、三角形、直线、圆弧和曲线。如果想绘制比较复杂的图形，我们需要掌握路径。

##### 栅格(了解)

![](D:\web\Typora\HTML5\static\images\html5\h5_2.png)

##### 绘制矩形常用API

绘制一个填充的矩形

```javascript
fillRect(x,y,width,height);
```

绘制一个矩形的边框

```javascript
strokeRect(x, y, width, height)
```

清除指定矩形区域，让清除部分完全透明。

```javascript
clearRect(x, y, width, height)
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>P5_canvas标签的基本使用</title>
</head>
<body>
    <!-- 默认是300px*300px -->
    <canvas id="" width="300px" height="300px"></canvas>
    <!-- 注意力 2D渲染的上下文 -->
    <script type="text/javascript">
        var canvas = document.querySelector('canvas');
        var ctx = canvas.getContext('2d');
        console.log(canvas.__proto__);//html:15
        console.log(ctx);//html:16

        //填充颜色
        ctx.fillStyle = 'red';
        //边框颜色
        ctx.strokeStyle = '#FDDD9B';
        //填充矩形
        ctx.fillRect(25,25,100,100);//25,25:坐标系的值，100,100:100px*100px大小 默认颜色:黑色
        ctx.clearRect(45,45,60,60);
        //填充没有背景颜色的矩形
        ctx.strokeRect(50,50,50,50);
    </script>
</body>
</html>
```

#### 使用路径绘制图形

图形的基本元素是路径。路径是通过不同颜色和宽度的线段或曲线相连形成的不同形状的点的集合。一个路径，甚至一个子路径，都是闭合的。使用路径绘制图形我们需要做以下几步。

1. **创建路径起始点**
2. **使用画布的各种方法划出路径**
3. **然后把路径封闭**
4. **一旦路径生成，你就能通过描边或填充路径区域来渲染图形**

##### 绘制路径常用api

###### beginPath()

```javascript
新建一条路径，生成之后，图形绘制api被指向到路径上生成路径。无参数
```

###### closePath()

```javascript
闭合路径之后图形绘制api又重新指向了上下文中
```

###### stroke()

```javascript
 通过线条来绘制图形轮廓
```

###### fill()

```javascript
通过填充路径的内容区域生成实心的图形
```

###### moveTo(x,y)

```javascript
将画笔移动到指定的坐标x以及y上
```

当canvas初始化或者`beginPath()`调用后，你通常会使用`moveTo()`函数设置起点。我们也能够使用`moveTo()`绘制一些不连续的路径

###### lineTo(x,y)

```javascript
绘制直线，绘制一条从当前位置到指定x以及y位置的直线
```

该方法有两个参数：x以及y ，代表坐标系中直线结束的点。开始点和之前的绘制路径有关，之前路径的结束点就是接下来的开始点，等等。。。开始点也可以通过`moveTo()`函数改变。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>P6_绘制填充和描边的三角形</title>
</head>
<body>
    <!-- 默认是300px*300px -->
    <canvas id="" width="300px" height="300px" style="border:1px solid red;"></canvas>
    <!-- 注意力 2D渲染的上下文 -->
    <script type="text/javascript">
        // 1.创建路径起始点
        // 2.使用画布的各种方法画出路径
        // 3.把路径封闭
        // 4.一旦路径生成，你就能通过描边或填充来渲染图像
            // beginPath() 无参数
			 
			// closePath() 关闭路径
			 
			// moveTo(x,y) 将画笔移动到指定的坐标上
			 
			// 绘制一条线
			// lineTo(x,y)
			 
			// 描边或填充的方法
			 
			// stroke() 通过线条
			// fill()  通过填充
			 
			 
			
			// 1.画填充的三角形
			var ctx = document.querySelector('canvas').getContext('2d');
			// (1) 开始路径
			ctx.beginPath();
			// (2) 落点
			ctx.moveTo(25,25);
			// (3) 画路径
			ctx.lineTo(105,25);
			ctx.lineTo(25,105);
			// (4)关闭
			ctx.closePath();
			// (5) 填充
			ctx.fill();
			
				// (1) 开始路径
			ctx.beginPath();
			// (2) 落点
			ctx.moveTo(125,125);
			// (3) 画路径
			ctx.lineTo(125,45);
			ctx.lineTo(45,125);
			ctx.closePath();
			// (4) 描边
			ctx.stroke();

    </script>
</body>
</html>

```

###### arc()

绘制圆弧或者圆

```javascript
arc(x,y,radius,startAngle,endAngle,anticlockwise);
```

圆心在 (x, y) 位置，半径为 radius ，根据anticlockwise （默认为顺时针）指定的方向从 startAngle 开始绘制，到 endAngle 结束。

anticlockwise:可选的，布尔值，如果为true，逆时针绘制圆弧，反之，顺时针绘制。

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>P8_绘制圆或者圆弧并制作笑脸图像</title>
	</head>
	<body>
		<canvas id="" width="300px" height="300px" style="border: 1px solid red;"></canvas>
		<script type="text/javascript">
			// arc(x,y,半径,开始的角度，结束的角度，true表示逆时针)

			var ctx = document.querySelector('canvas').getContext('2d');

			ctx.beginPath();
			ctx.arc(75, 75, 50, 0, Math.PI * 2, true);

			ctx.moveTo(110, 75);
			ctx.arc(75, 75, 35, 0, Math.PI, false);

			ctx.moveTo(65, 65);
			ctx.arc(60, 65, 5, 0, Math.PI * 2, true);
			
			ctx.moveTo(95, 65);
			ctx.arc(90, 65, 5, 0, Math.PI * 2, true);
			ctx.stroke();
		</script>
	</body>
</html>

```

![](D:\web\Typora\HTML5\static\images\html5\h5_4.png)

###### quadraticCurveTo(cp1x,cp1y,x,y)

```
绘制二次贝塞尔曲线,cp1x,cp1y为一个控制点，x,y为结束点
```

###### bezierCurveTo(cp1x, cp1y, cp2x, cp2y, x, y)

```
绘制三次贝塞尔曲线，cp1x,cp1y为控制点一，cp2x,cp2y为控制点二，x,y为结束点。
```

![](D:\web\Typora\HTML5\static\images\html5\h5_5.png)

参数x、y在两个方法中都是结束点坐标。cp1x,cp1y为坐标的第一个控制点(上图中的红色点)，cp2x,cp2y为坐标中的第二个控制点

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>P7_贝塞尔曲线绘制对话气泡和心心</title>
	</head>
	<body>
		<canvas id="" width="300px" height="300px" style="border: 1px solid red;"></canvas>
		<script type="text/javascript">
			var ctx = document.querySelector('canvas').getContext('2d');
            //对话气泡
 			ctx.beginPath();
 			ctx.moveTo(75, 25);
 			ctx.quadraticCurveTo(25, 25, 25, 62.5);
 			ctx.quadraticCurveTo(25, 100, 50, 100);
 			ctx.quadraticCurveTo(50, 120, 30, 125);
 			ctx.quadraticCurveTo(60, 120, 65, 100);
			ctx.quadraticCurveTo(125, 100, 125, 62.5);
 			ctx.quadraticCurveTo(125, 25, 75, 25);
 			ctx.strokeStyle = '#008000';
 			ctx.stroke();


            // 心心
			ctx.beginPath();
			ctx.moveTo(75, 40);
			ctx.bezierCurveTo(75, 37, 70, 25, 50, 25);
			ctx.bezierCurveTo(20, 25, 20, 62.5, 20, 62.5);
			ctx.bezierCurveTo(20, 80, 40, 102, 75, 120);
			ctx.bezierCurveTo(110, 102, 130, 80, 130, 62.5);
			ctx.bezierCurveTo(130, 62.5, 130, 25, 100, 25);
			ctx.bezierCurveTo(85, 25, 75, 37, 75, 40);
			ctx.fillStyle = 'red';
			
			ctx.fill();
		</script>
	</body>
</html>

```

#### 样式和颜色

- fillStyle = color:设置图形的填充颜色
- strokeStyle = color: 设置图形边框的颜色
- globalAlpha :设置透明度值，取值范围为0~1之间的数值
- lineWidth = value:设置线条宽度，数值无单位
- lineCap = type 设置线段末端的样式
  - type:butt 默认值，方形
  - type:round 圆形
  - type:square 以方形结束，但是增加一个宽度和线段相同，宽度是线段宽度一半的矩形区域
- lineJoin = type:设定线条和线条连接的样式
  - type:round 通过填充一个额外的，圆心在相连部分末端的扇形，绘制拐角的形状。 圆角的半径是线段的宽度。
  - type:bevel 在相连部分的末端填充一个额外的以三角形为底的区域， 每个部分都有各自独立的矩形拐角。
  - type: miter 通过延伸相连部分的外边缘，使其相交于一点，形成一个额外的菱形区域

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
	</head>
	<body>
		<!DOCTYPE html>
		<html>
			<head>
				<meta charset="utf-8">
				<title>P9_常用样式属性设置</title>
			</head>
			<body>
				<canvas id="" width="300px" height="300px" style="border: 1px solid red;"></canvas>
				<script type="text/javascript">
					var ctx = document.querySelector('canvas').getContext('2d');
					// (1) 开始路径
					ctx.beginPath();
					// (2) 落点
					ctx.moveTo(25, 25);
					// (3) 画路径
					ctx.lineTo(105, 25);
					ctx.lineTo(25, 105);
					// (4)关闭
					ctx.closePath();
					// 填充颜色
					ctx.fillStyle = '#008000';
					// 描边颜色
					ctx.strokeStyle = '#FF0000';
					// 线宽  无单位
					ctx.lineWidth = 10;
					// 透明度 0~1之间的数
					// ctx.globalAlpha = 0.5;
					// 线条的形状
					ctx.lineJoin = 'bevel';//miter 默认三角形 round:圆角 bevel:斜面
					ctx.fill();
					ctx.stroke();
				
				</script>
			</body>
		</html>

	</body>
</html>

```

#### 绘制文本

canvas提供了两种方法来渲染文本

- `filleText(text,x,y,[,maxWidth])`

在指定的(x,y)位置填充指定的文本，绘制的最大宽度是可选的

- `strokeText(text,x,y,[,maxWidth])`

在指定的(x,y)位置绘制文本边框，绘制的最大宽度是可选的

##### 有样式的文本

- `font = value`

当前我们用来绘制文本的样式. 这个字符串使用和 `CSS font`属性相同的语法. 默认的字体是 `10px sans-serif`。

- `textAlign = value`

文本对齐选项. 可选的值包括：`start`, `end`, `left`, `right` or `center`. 默认值是 `start`。

- `textBaseline = value`

基线对齐选项. 可选的值包括：`top`, `hanging`, `middle`, `alphabetic`, `ideographic`, `bottom`。默认值是 `alphabetic。`

- `direction = value`

文本方向。可能的值包括：`ltr`, `rtl`, `inherit`。默认值是 `inherit。`

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>P10_Canvas绘制文本</title>
	</head>
	<body>
		<canvas id="" width="300" height="300" style="border: 1px solid #0CB6D4"></canvas>
		<script type="text/javascript">
			var ctx = document.querySelector('canvas').getContext('2d');
			// fillText('绘制的文本',x,y)
			ctx.font = '30px 微软雅黑';
			ctx.fillStyle = 'red';
			ctx.fillText('hello world',50,50);//默认字体大小：10px
			ctx.strokeText('hello world',50,100);
		</script>
	</body>
</html>

```

#### 使用图片

canvas更有意思的一项特性就是图像操作能力。可以用于动态的图像合成或者作为图形的背景，以及游戏界面（Sprites）等等。浏览器支持的任意格式的外部图片都可以使用，比如PNG、GIF或者JPEG。 你甚至可以将同一个页面中其他canvas元素生成的图片作为图片源。

引入图像到canvas里需要以下两步基本操作：

1. 获得一个指向`HTMLImageElement`的对象或者另一个canvas元素的引用作为源，也可以通过提供一个URL的方式来使用图片
2. 使用`drawImage()`函数将图片绘制到画布上

##### 核心方法

```
drawImage(imgObj,x,y,width,height,dx,dy,dWith,dHeight)
```

imgObj:	image或者canvas对象

x和y:	在canvas里的起始坐标

width,height:	两个参数是可选的，默认是当前画布设置的大小,这两个参数用来控制当前canvas缩放的大小

如果是8个参数，用来控制做切片显示，前四个参数是定义图像源后的切片位置和大小，后四个参数是定义切片的目标显示的位置和大小

![](D:\web\Typora\HTML5\static\images\html5\h5_6.png)

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>P11_Canvas使用图片绘制折线图</title>
	</head>
	<body>
		<canvas id="" width="1000" height="1000" style="border: 1px solid #f00"></canvas>
		<!-- 1. 指定一个img对象或者是一个canvas元素的引用资源
				 2. dragImage(img0bj,x,y)//一共有9个参数
		 -->
		<script type="text/javascript">
			var ctx = document.querySelector('canvas').getContext('2d');
			// 1.创建img对象
			var imgObj = new Image();
			// 3.监听图像加载
			imgObj.onload = function(){
				// 3.1将图像设置到canvas中
				ctx.drawImage(imgObj,0,0);
				
				// 3.2 绘制折线
				ctx.beginPath();
				ctx.moveTo(123,400);
				ctx.lineTo(198,350);
				ctx.lineTo(300,200);
				ctx.lineTo(500,150);
				ctx.stroke();
			}
			// 2.设置img的src属性
			imgObj.src = 'static/pic/broken.png'
		</script>
	</body>
</html>

```

效果如图：

![](D:\web\Typora\HTML5\static\images\html5\h5_7.png)

切图功能实现代码：

```html
![h5_8](D:\web\Typora\HTML5\static\images\html5\h5_8.png)<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>P12_Canvas切图功能</title>
	</head>
	<body>
		<canvas id="canvas" width="348" height="267"></canvas>
		<img src="ima" alt="">
		<script type="text/javascript">
			var ctx = document.getElementById('canvas').getContext('2d');
			// drawImag(imgObj,x,y,width,height,dx,dy,dwidth,dheight)

			function draw() {
				var imgObj = new Image();
				imgObj.onload = function(){
					ctx.drawImage(imgObj,20,20,50,50,0,0,50,50);
				}
				imgObj.src = 'static/pic/splice.jpg';
			}
			draw()
		</script>
	</body>
</html>

```

![](D:\web\Typora\HTML5\static\images\html5\h5_8.png)

![](D:\web\Typora\HTML5\static\images\html5\h5_9.png)

#### 状态的保存和恢复

```
save()
```

保存画布的所有状态

```
restore()
```

save和restore方法是用来保存和恢复canvas状态的。都没有参数。

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
        <title>P13_save和restore方法</title>
        <style type="text/css">
            *{
                padding:0px;
                margin:0px;
            }
        </style>
	</head>
	<body>
		<canvas id="canvas" width="200" height="200"></canvas>
		<script type="text/javascript">
			function draw() {
				var ctx = document.getElementById('canvas').getContext('2d');
				ctx.save();
				ctx.fillStyle = '#0CB6D4';
				ctx.fillRect(0,0,150,150);
				ctx.save();//保存第一次的状态
				
				ctx.fillStyle = 'red';
				ctx.fillRect(15,15,120,120);
				ctx.save();//保存第二次状态
				
				
				ctx.fillStyle = 'greenyellow';
				ctx.fillRect(30,30,90,90);
				
				ctx.restore();//恢复之前的颜色状态(红色)
				ctx.fillRect(45,45,60,60);

				ctx.restore();//恢复到#0CB6D4颜色状态
				ctx.fillRect(60,60,30,30);
				
				ctx.restore();//恢复到默认的颜色状态
				// ctx.fillRect(67.5,67.5,15,15);
				

			}
			draw();
		</script>
	</body>
</html>

```

![](D:\web\Typora\HTML5\static\images\html5\h5_10.png)

##### 移动translate

```
translate(x,y)
```

translate方法接收两个参数。x是左右偏移量，y是上下偏移量。**在做变形之前先保存状态是良好的一个习惯**

##### 旋转

```
rotate(angle)
```

只接受一个参数：选装的角度。顺时针方向

#### 基本动画

如何通过canvas来制作动画呢？

步骤：

1. **清空canvas**

   通过`clearReact()`来清空canvas，保证自己的画布是干净的

2. **保存canvas状态**

3. **绘制动画图形**

4. **恢复canvas**状态

##### 操控动画的方法

```
setInterval(functuon,delay)
```

在设定好间隔时间后，function会定期执行

```
setTimeout(function,delay)
```

在设定好的时间之后执行函数

```
requestAnimationFrame(callback)
```

此方法一般每秒钟回到函数执行60次。告诉浏览器你希望执行一个动画，并在重绘之前，请求浏览器执行一个特定的函数来更新动画。

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>P15_requestAnimationFrame动画方法</title>
	</head>
	<body>
		<canvas id="" width="500" height="500" style="border: 1px solid red"></canvas>
		<script type="text/javascript">
			var ctx = document.querySelector('canvas').getContext('2d');
			// ctx.translate(50,10);
			var num = 0;
			

			function draw() {
				// 1.清空canvas
				ctx.clearRect(0, 0, 500, 500);
				num++;
				// 2.保存状态
				ctx.save()

				// 3.绘制动画图形
				ctx.rotate(num * 0.1 * Math.PI / 180); //顺时针
				ctx.fillRect(200, 200, 100, 100);
				// 4.恢复canvas状态
				ctx.restore();
				//此方法一般每秒钟回到函数执行60次。告诉浏览器你希望执行一个动画，并在重绘之前，请求浏览器执行一个特定的函数来更新动画。
				window.requestAnimationFrame(draw);
			}
			window.requestAnimationFrame(draw);
		</script>
	</body>
</html>

```

P16_案例_太阳系运动动画案例

html代码：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>P16_案例_太阳系运动动画案例</title>
</head>
<body>
    <canvas id="" width="500" height="500"></canvas>
    <script src="static/js/sunSys.js" type="text/javascript" charset="utf-8">
        
    </script>
</body>
</html>
```

sunSys.js代码：

```javascript
window.onload = function(){
    function SunSys(){
        this.sun = new Image();
        this.moon = new Image();
        this.earth = new Image();
        this.ctx = document.querySelector('canvas').getContext('2d');
        this.ctx.globalCompositeOperation = 'destination-over';//把圆覆盖到图片上面
        this.init();
    }
    SunSys.prototype.init = function(){//初始化
        this.sun.src = './static/pic/sun.png';
        this.moon.src = './static/pic/moon.png';
        this.earth.src = './static/pic/earth.png';
        var _this = this;

        window.requestAnimationFrame(function(){
            console.log(this);//类似于自执行函数,IIFE函数  指向window
            _this.draw();//this指向SunSys
        })
    }
    SunSys.prototype.draw = function(){
        //1.清空canvas
        this.ctx.clearRect(0,0,500,500);
        //2.设置轨迹的背景填充色和边框色
        this.ctx.fillStyle = "rgba(0,0,0,0.4)";
        this.ctx.strokeStyle = "rgba(0,153,255,0.4)";
        //3.保存当前的状态
        this.ctx.save();

        //画地球
        //4.改变地球的位置
        this.ctx.translate(150,150);

        var time = new Date();
        //5.让地球旋转起来
        this.ctx.rotate(((2*Math.PI)/60*time.getSeconds()) + ((2*Math.PI)/60000)*time.getMilliseconds());
        this.ctx.translate(105,0);
        this.ctx.drawImage(this.earth,-12,-12);


        //画月球
        this.ctx.save();
        this.ctx.rotate(((2*Math.PI)/6*time.getSeconds()) + ((2*Math.PI)/6000)*time.getMilliseconds());
        this.ctx.translate(0,30);
        this.ctx.drawImage(this.moon,0,0);
        this.ctx.restore();


        //恢复原始状态
        this.ctx.restore();

        // 画地球轨迹圆环
        // 开始路径
        this.ctx.beginPath();
        //画圆   arc(x,y,半径,开始的角度，结束的角度，true表示逆时针)
        this.ctx.arc(150,150,105,0,Math.PI*2,false);
        //描边
        this.ctx.stroke();




        //把太阳绘制到canvas中
        this.ctx.drawImage(this.sun,0,0,300,300);
        //实时执行动画
        var _this = this;
        window.requestAnimationFrame(function(){
            _this.draw();
        })

    }
    new SunSys();
}
```

![](D:\web\Typora\HTML5\static\images\html5\H5_001.gif)

P17_案例_桌面动画台球

html代码：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>P17_案例_桌面动画台球</title>
    <script src="static/js/Ball.js" type="text/javascript" charset="uft-8"></script>
</head>
<body>
    <canvas id="" width="600" height="300" style="border:1px solid #f00;"></canvas>
    <script type="text/javascript">

    </script>
</body>
</html>
```

Ball.js代码：

```javascript
window.onload = function(){
    function Ball(){
        this.x = 100;
        this.y = 100;
        this.vx = 5;
        this.vy = 2;
        this.radius = 25;
        this.color = 'black';
        this.canvas = document.querySelector('canvas');
        this.ctx = this.canvas.getContext('2d');
        //还可以添加摩擦力参数
        // this.init();
    }
    Ball.prototype.init = function(){
        //1.画小球
        this.drawBall();
        var _this = this;
        //2.监听canvas的点击事件  默认是冒泡的
        this.canvas.addEventListener('click',function(){
            window.requestAnimationFrame(function(){
                _this.draw();
            })
        })
    }

    Ball.prototype.draw = function(){
        this.ctx.clearRect(0,0,this.canvas.width,this.canvas.height);
        this.drawBall();
        this.x += this.vx;
        this.y += this.vy;
        //处理边界
        if(this.y+this.vy > this.canvas.height || this.y+this.vy < 0){
            this.vy = -this.vy;

        }
        if(this.x+this.vx > this.canvas.width || this.x+this.vx < 0){
            this.vx = -this.vx;
            
        }

        var _this = this;
        window.requestAnimationFrame(function(){
            _this.draw();
        })
    }

    Ball.prototype.drawBall = function(){
        //(1)开始路径
        this.ctx.beginPath();
        //(2)画图
        this.ctx.arc(this.x,this.y,this.radius,0,Math.PI*2,true);
        //(3)关闭路径
        this.ctx.closePath();
        //(4)设置填充的颜色
        this.ctx.fillStyle = this.color;
        //(5)填充
        this.ctx.fill();
    }

    var ball = new Ball();
    ball.init();
}
```

![](D:\web\Typora\HTML5\static\images\html5\H5_002.gif)

### SVG

SVG 是一种基于 XML 语法的图像格式，全称是可缩放矢量图（Scalable Vector Graphics）。其他图像格式都是基于像素处理的，SVG 则是属于对图像的形状描述，所以它本质上是文本文件，体积较小，且不管放大多少倍都不会失真。

#### 语法以及常用标签

##### 1.svg标签

SVG代码都放在顶层标签`SVG`之中

```html
<svg width="100%" height="100%">
	<circle id="mycircle" cx="50" cy="50" r="50" />
</svg>
```

`svg`的`width`属性和`height`属性，指定SVG图像在HTML元素中所占据的宽度和高度。如果不指定，默认大小是300px宽，150px高

##### 2.circle标签

`circle`标签代表圆形

```html
<svg width="300" height="180">  
    <circle cx="30"  cy="50" r="25" />  
    <circle cx="90"  cy="50" r="25" class="red" />  
    <circle cx="150" cy="50" r="25" class="fancy" />
</svg>
```

上面代码定义了三个圆。`circle`标签`cx`,`cy`,`r`属性分别为横坐标、纵坐标和半径，单位为像素。坐标都是相对于`svg`画布的左上角原点。

`class`属性用来指定对应的css类

```css
.red {
    fill: red;
}
.fancy {
    fill: none;
    stroke: black;
    stroke-width: 3pt;
}
```

SVG 的 CSS 属性与网页元素有所不同。

- fill：填充色
- stroke：描边色
- stroke-width：边框宽度

![](D:\web\Typora\HTML5\static\images\html5\h5_11.png)

##### 3.line标签

`line`标签用来绘制直线

```svg
<line x1="0" y1="0" x2="200" y2="0" style="stroke:rgb(0,0,0);stroke-width:5" />
```

- x1：线段起点的横坐标
- y1：起点纵坐标
- x2： 终点的横坐标
- y2：终点的纵坐标
- style：线段的样式

![](D:\web\Typora\HTML5\static\images\html5\h5_12.png)

##### 4.polyline标签

绘制一根折线

```html
<polyline points="3,3 30,28 3,53" fill="none" stroke="black" />
```

`<polyline>`的`points`属性指定了每个端点的坐标，横坐标与纵坐标之间与逗号分隔，点与点之间用空格分隔。

![](D:\web\Typora\HTML5\static\images\html5\h5_13.png)

##### 5.rect标签

绘制矩形

```html
 <rect x="0" y="0" height="100" width="200" style="stroke: #70d5dd; fill: #dd524b" />
```

`<rect>`的`x`属性和`y`属性，指定了矩形左上角端点的横坐标和纵坐标；`width`属性和`height`属性指定了矩形的宽度和高度（单位像素）。

![](D:\web\Typora\HTML5\static\images\html5\h5_14.png)

##### 6.ellipse标签

绘制椭圆

```html
<ellipse cx="60" cy="60" ry="20" rx="40" stroke="black" stroke-width="5" fill="silver"/>
```

`<ellipse>`的`cx`属性和`cy`属性，指定了椭圆中心的横坐标和纵坐标（单位像素）；`rx`属性和`ry`属性，指定了椭圆横向轴和纵向轴的半径（单位像素）。

![](D:\web\Typora\HTML5\static\images\html5\h5_15.png)

##### 7.polygon标签

绘制多边形

```html
<polygon fill="green" stroke="orange" stroke-width="1" points="0,0 100,0 100,100 0"/>
```

`<polygon>`的`points`属性指定了每个端点的坐标，横坐标与纵坐标之间与逗号分隔，点与点之间用空格分隔。

![](D:\web\Typora\HTML5\static\images\html5\h5_16.png)

##### 8.path标签

制作路径

```html
<path d="
  M 18,3
  L 46,3
  L 46,40
  L 61,40
  L 32,68
  L 3,40
  L 18,40
  Z
"></path>
```

`<path>`的`d`属性表示绘制顺序，它的值是一个长字符串，每个字母表示一个绘制动作，后面跟着坐标

- M：移动到（moveto）
- L：画直线到（lineto）
- Z：闭合路径

![](D:\web\Typora\HTML5\static\images\html5\h5_17.png)

##### 9.text标签

绘制文本

```html
<text x="50" y="25">Hello World</text>
```

`<text>`的`x`属性和`y`属性，表示文本区块基线（baseline）起点的横坐标和纵坐标。文字的样式可以用`class`或`style`属性指定。

##### 10.use标签

复制一个形状

```html
<circle id="myCircle" cx="5" cy="5" r="4"/>
<use href="#myCircle" x="10" y="0" fill="blue" />
<use href="#myCircle" x="20" y="0" fill="white" stroke="blue" />
```

![](D:\web\Typora\HTML5\static\images\html5\h5_18.png)

##### 11.g标签

用于将多个形状组成一个组(group),方便复用

```html
<g id="myCircle">
    <text x="25" y="20">圆形</text>
    <circle cx="50" cy="50" r="20" />
</g>
<use href="#myCircle" x="100" y="0" fill="blue" />
<use href="#myCircle" x="200" y="0" fill="white" stroke="blue" />
```

![](D:\web\Typora\HTML5\static\images\html5\h5_19.png)

##### 12.defs标签

用于自定义形状。它内部的代码不会显示，仅供引用

##### 13.image标签

用于插入图片文件

```html
<image xlink:href = "img_submit.gif"  width="20%" height="20%"/>
```

##### 14.animate标签

产生动画效果

```html
<rect x="0" y="0" width="100" height="100" fill="#feac5e">
    <animate attributeName="x" from="0" to="500" dur="2s" repeatCount="indefinite" />
</rect>
```

矩形会在水平反向上不断移动，产生动画效果。

- attributeName：发生动画效果的属性名。
- from：单次动画的初始值。
- to：单次动画的结束值。
- dur：单次动画的持续时间。
- repeatCount：动画的循环模式。

![](D:\web\Typora\HTML5\static\images\html5\H5_003.gif)

定制多个animate

```html
<animate attributeName="x" from="0" to="500" dur="2s" repeatCount="indefinite" />
<animate attributeName="height" to="500" dur="2s" repeatCount="indefinite" />
```

![](D:\web\Typora\HTML5\static\images\html5\H5_004.gif)

##### 15.animateTransform标签

`<animateTransform>`的效果为旋转（`rotate`），这时`from`和`to`属性值有三个数字，第一个数字是角度值，第二个值和第三个值是旋转中心的坐标。`from="0 200 200"`表示开始时，角度为0，围绕`(200, 200)`开始旋转；`to="360 400 400"`表示结束时，角度为360，围绕`(400, 400)`旋转。

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>P19_SVG制作动画效果</title>
	</head>
	<body>
		<!-- animate -->
		<svg width="1000" height='500'>
			<!-- <rect x='0' y='0' width="100" height="100" fill='#ff6700'>
				<animate attributeName='x' from='0' to='500' dur='2s' repeatCount="indefinite" />
				<animate attributeName='height' from='0' to='500' dur='2s' repeatCount="indefinite" />
			</rect> -->
			<rect x='250' y='250' width ='100' height = '100' fill = '#4bc0c8'>
				<animateTransform attributeName = 'transform' type='rotate' begin = '0s' dur = '10s' from = '0 200 200' to = '360 400 400'></animateTransform>
			</rect>
		</svg>

	</body>
</html>

```

关于SVG其实跟canvas差不了多少，都是可以做一下复杂的图形和动画效果。SVG的入门教学先写到这里。如果有同学在公司用到了SVG的高级特效，可以参考MDN[SVG](https://developer.mozilla.org/zh-CN/docs/Web/SVG)。

#### SVG文件的引入方式

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>P20_SVG文件的引入方式</title>
	</head>
	<body>
		<!-- <embed src="static/pic/method-draw-image.svg" type="image/svg+xml"> -->
		<!-- <object data="static/pic/method-draw-image.svg" type="image/svg+xml"></object> -->
		<iframe src="static/pic/method-draw-image2.svg" width="500" height="500"></iframe>
	</body>
</html>

```

### 拖拽/拖放

拖放(drap&drop)在我们平时的工作中，经常遇到。它表示：抓取对象以后拖放到另一个位置。目前，它是HTML5标准的一部分。

##### 拖动

每个可拖动的元素，在拖动过程中，都会经历三个过程：

```
拖动开始=>拖动过程中=>拖动结束
```

| 对象             | 事件名称  | 描述                     |
| ---------------- | --------- | ------------------------ |
| 被拖动的元素对象 | dragstart | 在元素开始被拖动时候触发 |
|                  | drag      | 在元素被拖动时反复触发   |
|                  | dragend   | 在拖动操作完成时触发     |

| 对象     | 事件名称  | 描述                                         |
| -------- | --------- | -------------------------------------------- |
| 目标对象 | dragenter | 当被拖动元素进入目标元素占据的屏幕空间时触发 |
|          | dragover  | 当被拖动元素在目标元素内时触发               |
|          | dragleave | 当被拖动元素没有放下就离开目标元素时触发     |

> 注意:dragenter和dragover事件的默认行为是拒绝接受任何被拖放的元素。因此，我们必须阻止浏览器这种默认行为。e.prevenDefault();

拖拽的事件

```html
<!DOCTYPE HTML>
<html>
	<head>
		<title>22 拖拽的事件02</title>
	</head>
	<style>
		#current {
			border: 1px solid #000;
			padding: 20px;
			margin-bottom: 20px;
		}

		#target {
			border: 1px solid #f00;
			width: 400px;
			height: 400px;
		}

		.box {
			width: 100px;
			height: 100px;
			border: 1px solid #FF6700;
		}
	</style>
	<body>
		<p>不能被拖拽的文字</p>
		<div id="current">
			<div class="text" id="text">
				<p draggable="true">可拖拽的文字</p>
				<img src="static/pic/play.png" alt="">
				<div class="box" draggable="true"></div>
			</div>
		</div>
		<div id="target"></div>
		<script>
			var text = document.getElementById('text');
			var target = document.getElementById('target');
			text.ondragstart = function(event) {
				console.log('元素开始被拖动1');
			}
			text.ondrag = function(e) {
				// console.log('一直被拖拽着2');
			}
			text.ondragend = function(e) {
				console.log('拖拽操作完成时触发3');
			}
			target.ondragenter = function(e) {
				console.log('进入了所要的目标对象中4');
				e.preventDefault()//阻止默认事件
			}
			target.ondragover = function(e) {
				console.log('在目标元素内触发5');
				e.preventDefault();
			}
			target.ondragleave = function(e) {
				console.log('3拖拽着元素没有放下，并离开目标对象时6');
				// e.preventDefault();
			}
			target.ondrop = function(event) {
				console.log('4被拖拽元素在目标元素里放下是触发7');
				event.preventDefault();
			}
		</script>
	</body>
</html>

```

效果展示:

![](D:\web\Typora\HTML5\static\images\html5\H5_005.gif)

由上个例子我们可以看出我们确实实现了拖放的功能，猜想：能否让我们拖拽的元素放到指定的目标对象上呢？答案是可以的，如果想实现该功能，就要学一下`DataTransfer`对象了

#### DataTransfer对象

在进行拖放操作时，`DataTransfer`对象用来保存，通过拖放操作，拖动到浏览器的数据。它可以保存一项或多项数据、一种或者多种数据类型。

```javascript
event.DataTransfer
```

##### 方法

【1】`DataTransfer.setData()`

该方法用来设置拖动操作的当前数据

语法：

```javascript
DataTransfer.setData(format,data);
```

- format 拖动数据的MIME类型，通常`text/plain`和`text/uri-list`
- data 要添加的数据

【2】`DataTransfer.getData()`

接收指定类型的拖放数据。如果拖放行为没有操作任何数据，则返回一个空字符串。返回值是字符串类型

语法:

```javascript
dataTransfer.getData(format);
```

- format 拖动数据的MIME类型，通常`text/plain`和`text/uri-list`

【3】`DataTransfer.clearData()`

删除给定类型拖动操作的数据。

【4】`DataTransfer.setDragImage()`

可以使用该方法来拖拽图片

语法：

```javascript
DataTransfer.setDragImage(img,xOffset,yOffset)
```

- img:拖拽图像的当前元素
- xOffset :图片的横向偏移量
- yOffset: 图片的纵向偏移量

#### 定义拖动效果

`dropEffect`属性可以定义完成具体的效果

我们可以定义三种效果：

1. `copy` 表示拖动的数据将从其当前位置复制到放置位置。
2. `move` 表示拖动的数据将从其当前位置移动到放置位置。
3. `link` 表示将在源位置和放置位置之间创建某种形式的关系或连接。

```javascript
<script type="text/javascript">
    DataTransfer对象:在进行拖放操作时，DataTransfer对象用来保存，通过拖放操作，拖动到浏览器的数据。它可以保存一项或多项数据、一种或者多种数据类型。
    1.event.DataTransfer
    2.DataTransfer.setData(format,data):该方法用来设置拖动操作的当前数据
    3.dataTransfer.getData(format):format 拖动数据的MIME类型，通常text/plain和text/uri-list
    4.DataTransfer.clearData()
    5.DataTransfer.setDragImage(img,xOffset,yOffset):可以使用该方法来拖拽图片
    定义拖动效果
    6.dropEffect属性可以定义完成具体的效果
        6.1copy 表示拖动的数据将从其当前位置复制到放置位置
        6.2move 表示拖动的数据将从其当前位置移动到放置位置
        6.3link 表示将在源位置和放置位置之间创建某种形式的关系或连接
</script>
```

#### 例子:实现复制和移动元素

```html
<!DOCTYPE html>
<html lang="zh">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <meta http-equiv="X-UA-Compatible" content="ie=edge">
        <title>P23_案例_实现复制和移动操作</title>
        <style type="text/css">
            #copy,#move{
                border: 1px solid #000;
                width: 300px;
                height: 200px;
            }
            #copyTarget,#moveTarget{
                width: 300px;
                height: 200px;
                border: 1px solid #FF0000;
            }
            #newId{
                width: 200px;
                height: 50px;
                border: 1px solid darkcyan;
            }
        </style>
    </head>
    <body>
        <h2>使用拖拽实现移动和复制功能</h2>
        <div draggable="true" id="copy">要复制的元素</div>
        <div id="copyTarget"></div>
        <div draggable="true" id="move">要移动的元素</div>
        <div id="moveTarget"></div>
        <script type="text/javascript">
            window.onload = function() {
                function $(ele) {
                    return document.getElementById(ele);
                }
                // 复制
                $('copy').ondragstart = handler_dragstart;
                $('copy').ondragend = handler_dragend;
                $('copyTarget').ondragover = handler_dragover;
                $('copyTarget').ondragleave = handler_dragLeave;
                $('copyTarget').ondrop = handler_drop;
                // 移动
                $('move').ondragstart = handler_dragstart;
                $('move').ondragend = handler_dragend;
                $('moveTarget').ondragover = handler_dragover;
                $('moveTarget').ondragleave = handler_dragLeave;
                $('moveTarget').ondrop = handler_drop;
                function handler_dragstart(event) {
                    console.log(event.dataTransfer);
                    console.log('拖拽开始');
                    // 设置数据
                    event.dataTransfer.setData('text/plain', event.target.id);
                    // 设置拖动效果 设置既复制又移动
                    event.effectAllowed = 'copyMove';
                }
                function handler_dragend(event) {
                    // 拖动操作完成时 清空设置的数据
                    // event.target.style.borderColor = 'black';
                    event.dataTransfer.clearData();
                }
                // 当被拖动元素在目标元素内时触发  一直反复触发
                function handler_dragover(event) {
                    event.target.style.background = 'lightblue';
                    event.preventDefault();
                    // 注意:dragenter和dragover事件和dragLeave的默认行为是拒绝接受任何被拖放的元素。因此，我们必须阻止浏览器这种默认行为。e.preventDefault();
                }
                function handler_drop(event) {
                    event.preventDefault();
                    // 获取设置的数据
                    var id = event.dataTransfer.getData('text/plain');
                    if (id === 'copy' && event.target.id === 'copyTarget') {
                        var nodeCopy = document.getElementById(id).cloneNode(true);
                        nodeCopy.id = 'newId';
                        event.target.appendChild(nodeCopy);
                    }
                    if(id === 'move' && event.target.id === 'moveTarget'){
                        event.target.appendChild(document.getElementById(id));
                        console.log(event.target.children);
                    }
                }
                function handler_dragLeave(event) {
                    event.target.style.background = 'white';
                    event.preventDefault();
                }
                
            }
        </script>
    </body>
</html>
```

![](D:\web\Typora\HTML5\static\images\html5\h5_20.png)

#### 例子:实现简单拖拽购物车功能

```html
<html>
    <head>
        <meta charset="utf-8" />
        <title>使用拖放API将商品拖入购物车</title>
        <style>
            body {
                font-size: 12px
            }
            .liT {
                border-bottom: solid 1px #ccc;
                background-color: #eee;
                font-weight: bold
            }
            .liF {
                float: left;
                margin-right: 5px;
            }
            ul {
                list-style-type: none;
                padding: 0px;
                height: 106px;
                width: 330px
            }
            ul li {
                overflow: hidden;
            }
            ul li img {
                width: 68px;
                height: 96px;
                border: solid 1px #ccc;
                padding: 3px
            }
            ul li span {
                float: left;
                width: 70px;
                padding: 5px;
            }
        </style>
    </head>
    <body>
        <ul>
            <li class="liF">
                <img src="static/pic/image01.png" id="img02" alt="42" title="2006作品" draggable="true">
            </li>
            <li class="liF">
                <img src="static/pic/image02.png" id="img03" alt="56" title="2008作品" draggable="true">
            </li>
            <li class="liF">
                <img src="static/pic/image03.png" id="img04" alt="52" title="2010作品" draggable="true">
            </li>
        </ul>
        <ul id="ulCart">
            <li class="liT">
                <span>书名</span>
                <span>定价</span>
                <span>数量</span>
                <span>总价</span>
            </li>
        </ul>
        <script type="text/javascript">
            pageload();
            function $$(id) {
               return document.getElementById(id);
            }
            //自定义页面加载时调用的函数
            function pageload() {
                //获取全部的图书商品
                var Drag = document.getElementsByTagName("img");
                //遍历每一个图书商品
                for (var intI = 0; intI < Drag.length; intI++) {
                    console.log(Drag[intI]);
                    //为每一个商品添加被拖放元素的dragstart事件
                    Drag[intI].addEventListener("dragstart", function(e) {
                        e.dataTransfer.clearData();
                        var objDtf = e.dataTransfer;
                        console.log(objDtf);
                        objDtf.setData("text/plain", addCart(this.title, this.alt, 1));
                    },true);
                }
                var Cart = $$("ulCart");
                //添加目标元素的drop事件
               Cart.addEventListener("drop",function(e) {
                    var objDtf = e.dataTransfer;
                    var strHTML = objDtf.getData("text/plain");
                    var num = top_();
                    Cart.innerHTML += strHTML;
                    var lists = document.getElementsByClassName('liC');
                    for(var i = 0; i < lists.length; i++){
                        var spans = lists[i].children;
                        console.log(spans);
                        spans[2].innerHTML = num;
                        spans[3].innerHTML = num * spans[1].innerHTML;
                    }
                    e.preventDefault();//阻止默认事件
                    e.stopPropagation();
                },false);
            }
            //添加页面的dragover事件
            document.ondragover = function(e) {
                //阻止默认方法,取消拒绝被拖放
                e.preventDefault();
            }
            //添加页面drop事件
            document.ondrop = function(e) {
                //阻止默认方法,取消拒绝被拖放
                e.preventDefault();
            }
            //自定义向购物车中添加记录的函数
            function addCart(a, b, c) {
                var strHTML = `<li class = 'liC'>
                <span>${a}</span>
                <span class="price">${b}</span>
                <span class="num">${c}</span>
                <span class="sum">${b*c}</span>
            </li>`
                return strHTML;
            }
            //提示输入框
            function top_() {
                var str = prompt("请输入要购买的数量", 1);
                return str;
            }
        </script>
    </body>
</html>
```

![](D:\web\Typora\HTML5\static\images\html5\h5_21.png)