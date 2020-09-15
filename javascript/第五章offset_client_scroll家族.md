# 第五章offset_client_scroll家族

## 本章内容介绍

核心：ECMAScript

1.  文档对象模型：DOM

2.  浏览器对象模型：BOM

3.  offset/client/scroll家族

4. 事件

   4.1事件流;
   4.2事件处理程序;1.HTML事件处理程序 2.DOM 0级事件处理程序 3.DOM 2级事件处理程序  4.IE事件处理程序
   4.3事件对象;1.获取事件对象 2.事件目标 3.事件代理 4.事件冒泡 5.事件流阶段 6.阻止默认行为
   4.4事件对象属性;1.坐标位置

5. 动画：基本动画;匀速动画;缓慢动画;透明度动画;动画封装

   

## offsetParent讲解

offset demension 偏移量
	1.定位父级 offsetParent 找到有定位的父级
	2.offsetLeft； offsetTop
	3.offsetHeight ；offsetWidth 

html代码如下：

```html
<div id="grandFather" style="position:relative;">
	<div id="father">
    	<div id="box"></div>
    </div>
</div>
```



javascript代码如下：

```javascript
// offsetParent定义:与当前元素最近的经过定位的父级元素
//1.元素自身有fixed定位，offsetParent是null  style="position:fixed;"  固定定位相对于的是浏览器
var box =document.getElementById('box');
console.log(box.offsetParent);
		
// 2.元素自身无fixed定位,且无定位父级元素，offsetParent是body
		
//3.元素自身无定位，父级元素存在定位，offsetParent是最近的经过定位的父级元素
		
//4.body元素offsetParent是null
console.log(document.body.offsetParent);
```



## offsetWidth和offsetHeight讲解

css代码如下：

```css
<style type="text/css">
		#box{width:200px;height:200px;background-color: red;padding:10px;margin:10px;border:1px solid #0000FF;}
</style>
```

html代码如下：

```html
<div id="box"  style="width:100px;height:100px;"></div>
```

javascript代码如下：

```javascript
//offsetWidth = width + border-left-width + border-right-width  + padding-left-width + padding-right-width
var box = document.getElementById('box');
console.log(box.offsetWidth,box.offsetHeight);

//只能获取行内的属性
console.log(box.style.width,box.style.height);
// offsetWidth,offsetHeight 是只读属性
// box.offsetWidth = 500;

box.style.height = 500 + 'px';
box.style.width = 500 +'px';
```



## offsetTop和offsetLeft讲解

css代码如下：

```css
<style type="text/css">
	*{margin:0px;padding:0px;}
    #father{width:400px;height:400px;position:relative;background-color:red;margin:40px;}
    #son{width:200px;height:100px;background-color: #0000FF;margin-left:20px;margin-top:30px;border:5px solid #ccc;}
</style>
```

html代码如下：

```html
<div id="father">
	<div id="son"></div>
</div>
```

javascript代码如下：

```javascript
//1.offsetTop  当前元素的上边框到offsetParent上边框的距离  与边框么有关系
//1.offsetLeft  当前元素的左边框到offsetParent左边框的距离
var son = document.getElementById('son');
//如果有父元素定位
console.log(son.offsetParent);
console.log(son.offsetTop,son.offsetLeft);//0,20
	
//如果没有父元素定位
// console.log(son.offsetParent);
// console.log(son.offsetTop,son.offsetLeft);//40,60
		
//总结：相对于父元素(父元素是否有定位，如果有定位，以父元素为基准，如果没有往上寻找到，则以body为基准)
```



## 如何求出当前元素在页面上的偏移量

css代码如下：

```css
<style type="text/css">
	/* *{margin:0px;margin:0px;} */
    #grandfater{
        border:5px solid red;
        padding:30px;
        position:absolute;
    }
    #father{
        padding:20px;
        border:1px solid #000;
        position:relative;
    }
    #son{
        width:100px;
        height:100px;
        margin:10px;
        background-color: red;
    }
</style>
```

html代码如下：

```html
<div id="grandfater">
    <div id="father">
    	<div id="son"></div>
    </div>
</div>
```

javascript代码如下：

```javascript
 var son = document.getElementById('son');
 console.log(getElementLeft(son));
 console.log(getElementTop(son));
		
 function getElementLeft(obj){
 	//1.获取当前元素的左边偏移量
 	var actualLeft = obj.offsetLeft;
 	//2.求出定位父级
 	var parent = obj.offsetParent;
 	//3.循环
 	while(parent != null){
 		//3.1求出当前的左方偏移量
 		actualLeft = actualLeft + parent.clientLeft + parent.offsetLeft;
 		console.log(parent);
 		//3.2更新定位父级；
 		parent = parent.offsetParent;
	}
	return actualLeft + 'px';
}
		
  function getElementTop(obj){
  	//1.获取当前元素的左边偏移量
  	var actualTop = obj.offsetTop;
 	 //2.求出定位父级
  	var parent = obj.offsetParent;
  	//3.循环
  	while(parent != null){
  		//3.1求出当前的左方偏移量
  		actualTop = actualTop + parent.clientTop + parent.offsetTop;
  		console.log(parent);
  		//3.2更新定位父级；
 		 parent = parent.offsetParent;
	}
	return actualTop + 'px';
}
```



## client客户端大小的使用

html代码如下：

```html
<div id="box" style="width: 200px;height: 200px;border: 1px solid #ccc;background-color: red;padding:20px;"></div>
```

javascript代码如下：

```javascript
//client 客户端大小:指的是元素内容到内边距占据的空间大小
// 不包含border
//1.clientWidth = width + padding-left + padding-right 
//2.clientHeight = height + padding-top + padding-bottom
//3.clientLeft  左边框的大小
//4.clientTop	上边框的大小
var box = document.getElementById('box');
console.log(box.clientWidth,box.clientHeight);
console.log(box.clientTop,box.clientLeft);

// 获取当前页面元素的大小
console.log(document.documentElement.clientWidth);
console.log(document.documentElement.clientHeight);

//注意：1.所有client是只读的 静态失败
// box.clientHeight = 400;
// cosole.log(box.clientHeight);

//2.如果给元素设置display：none ，客户端属性client属性都为 0

//3.尽量避免重复访问这些属性
console.time('time');
var b = box.clientHeight; 
	for(var i = 0;i <100000;i++){
    	// console.log(box.clientHeight);
        var a = b;    //time: 1.81201171875ms
    }
console.timeEnd('time');//time: 26.23291015625ms
```



## scrollWidth和scrollHeight讲解

css代码如下：

```css
<style type="text/css">
    #box{
        width:100px;
        height:100px;
        border:1px solid #000;
        padding:10px;
        margin:10px;	
        overflow: scroll;
    }
</style>
```

html代码如下：

```html
<div id="box">
    <p>内容</p>
    <p>内容</p>
    <p>内容</p>
    <p>内容</p>
    <p>内容</p>
    <p>内容</p>
    <p>内容</p>
    <p>内容</p>
    <p>内容</p>
    <p>内容</p>
    <p>内容</p>
    <p>内容</p>
    <p>内容</p>
    <p>内容</p>
    <p>内容</p>
    <p>内容</p>
    <p>内容</p>
    <p>内容</p>
    <p>内容</p>
    <p>内容</p>
</div>
<button id="btn1">向下滚动</button>
```

javascript代码如下：

```javascript
//1.scrollHeight 表示元素的总高度，包含由于溢出而在网页上面均不可见部分
//1.scrollWidth 表示元素的总宽度，包含由于溢出而在网页上面均不可见部分
var box = document.getElementById('box');
var btn1 = document.getElementById('btn1');
//1.无滚动条的时候，scrollHeight和clientHeight属性结果是相同的
// console.log(box.scrollHeight);
// console.log(box.scrollWidth);

//2.有滚动条的时候
// console.log(box.scrollHeight);
// console.log(box.scrollWidth);

//3.scrollTop 元素被卷起的高度
// box.onscroll = function(){
// 	console.log(box.scrollTop,box.scrollHeight);
// 	//当滚动条滚动到底部是，符合下公式：
// 	//scrollHeight = clientHeight + scrollTop;
// }

//注意：scrollTop是可读写的
btn1.onclick = function(){
    box.scrollTop += 20;
    console.log(box.scrollTop);
}

//4.scrollLeft 元素被卷起的宽度
```



## 页面滚动

css代码如下：

```css
<style type="text/css">
    #box{
        width:100px;
        height: 100px;
        border:1px solid #000;
        padding:10px;
        margin:10px;
        overflow:scroll;
    }
    #btn{
        position:fixed;
        top:200px;
        left:30px;
    }
</style>
```

html代码如下：

```html
<body style="height:2000px;">
	<div id="box">
		<p>内容</p>
		<p>内容</p>
		<p>内容</p>
		<p>内容</p>
		<p>内容</p>
		<p>内容</p>
		<p>内容</p>
		<p>内容</p>
		<p>内容</p>
		<p>内容</p>
		<p>内容</p>
		<p>内容</p>
		<p>内容</p>
		<p>内容</p>
		<p>内容</p>
		<p>内容</p>
		<p>内容</p>
		<p>内容</p>
		<p>内容</p>
		<p>内容</p>	
	</div>
	<button id="btn">回到顶部</button>
```

javascript代码如下：

```javascript
window.onscroll = function(){
    //console.log(document.documentElement.scrollTop);//chrome浏览器，火狐浏览器可以，ssrifi步行
    //console.log(document.body.scrollTop);//chrome浏览器不行，火狐浏览器可以，srifi可以

	//兼容代码
	var doScrollTop = document.documentElement.scrollTop || document.body.scrollTop;
	console.log(doScrollTop);

	var btn = document.getElementById('btn');
	btn.onclick = scrollTop;
	function scrollTop(){
        if(doScrollTop){
            // document.documentElement.scrollTop = 0;
            // document.body.scrollTop = 0;
            //简化写法
            document.documentElement.scrollTop = document.body.scrollTop = 0;
        }
	}
}
```

## 实用的页面滚动

html代码如下：

```html
<body style="height:2000px;">
    <button id="backTop" style="position: fixed;">回到顶部</button>
```

javascript代码如下：

```javascript
var backTop = document.getElementById('backTop');
backTop.onclick = function(){
	window.scrollTo(0,100);
}
```

