# 第六章 JS事件

## 事件流介绍

1. js操作css: 脚本号的css,js与html交互通过事件完成
2. 事件: 文档或浏览器窗口中发生一些特定的交互性瞬间
3. 事件流(事件传播):  描述从页面中接受事件的顺序

[^IE事件流是事件冒泡流；Netscape事件流式事件捕获流]: 

## 事件冒泡的概念

事件冒泡: 事件开始时由最具体的元素接收,然后逐级向上传播到较为不具体的节点(文档)

<img src="D:\Typora\program\javascript\pic\image-20190507142910045.png" alt="image-20190507142910045" style="zoom:80%;" />

## 事件捕获的概念

1. 事件捕获：有不太具体的节点更早的接收事件,而最具体的节点应该最后接收到事件

2. 方法:addEventListener()  参数：类型,回调函数,默认false(冒泡阶段)   事件监听者

3. 事件流：事件捕获阶段;处于目标阶段;事件冒泡阶段

   ```javascript
   var box = document.getElementById('box');
   box.addEventListener('click',function(){
       box.innerHTML += 'box\n';
   },true)//捕获阶段
   document.body.addEventListener('click',function(){
   	box.innerHTML += 'body\n';
   },true)//捕获阶段
   document.documentElement.addEventListener('click',function(){
   	box.innerHTML += 'html\n';
   },true)//捕获阶段
   document.addEventListener('click',function(){
   	box.innerHTML += 'document\n';
   },true)//捕获阶段
   window.addEventListener('click',function(){
   	box.innerHTML += 'window\n';
   },true)//捕获阶段
   			
   //冒泡阶段
   box.addEventListener('click',function(){
   	box.innerHTML += 'box\n';
   },false)
   document.body.addEventListener('click',function(){
   	box.innerHTML += 'body\n';
   },false)
   document.documentElement.addEventListener('click',function(){
   	box.innerHTML += 'html\n';
   },false)
   document.addEventListener('click',function(){
   	box.innerHTML += 'document\n';
   },false)
   window.addEventListener('click',function(){
   	box.innerHTML += 'window\n';
   },false)
   ```

   得到的结果如下：

   ![6_1](D:\Typora\program\javascript\pic\6_1.png)

## HTML事件处理程序

缺点: html+js无分离,后期不易维护

特别，注意不论在函数内还是函数外，this都是指向window对象

```javascript
console.log(this);//指向window对象
function test(){
    // console.log(this); 指向window对象
    document.getElementById('box').innerHTML += '1';
}
```

## DOM_0级事件处理程序

优点：1.简单 2.跨浏览器

缺点：不能给同一个元素来绑定相同的事件处理程序，如果绑定了，会有覆盖现象

```javascript
var box = document.getElementById('box');
box.onclick = function(){
	this.innerHTML += 1;
}
//删除事件处理程序
box.onclick = null;
// 垃圾回收机制
//缺点：不能给同一个元素来绑定相同的事件处理程序，如果绑定了，会有覆盖现象
box.onclick = function(){
	this.innerHTML += 2;
}
```

## DOM_2级事件处理程序

1. addEventListener(事件名，处理程序的函数，布尔值) 
2. removeEventListener()  
3. 优点：没有事件覆盖现象

[^布尔值默认false是处于冒泡阶段，如果是true,是处于捕获阶段]: 

IE8浏览器不支持DOM2处理程序

```javascript
//没有事件覆盖现象
setTimeout(function(){
	box.addEventListener('click',function(){
		this.innerHTML += 1;
	},false);
},10)
// //监听函数传参，可以用匿名函数包装一个监听函数
box.addEventListener('click',function(){
	//监听匿名函数
	test(2);
},false);
// box.addEventListener('click',test(3),false);
function test(x){
	alert(x);
}
```

无效的移除事件的方式	

```javascript
//添加事件监听者
box.addEventListener('click',function(){
	this.innerHTML += 1;
},false);
			
//移除事件
box.removeEventListener('click',function(){
	this.innerHTML += 1;
},false);
```

正确移除事件方式

```javascript
function handler(){
	this.innerHTML += 1;
}
box.addEventListener('click',handler,false);
box.removeEventListener('click',handler,false);
```

## IE事件处理程序

IE: attachEvent()  detachEvent()

[^IE10以下浏览器可以使用  默认冒泡]: 

```javascript
var box = document.getElementById('box');
box.attachEvent('onclick',function (){
	this.innerHTML += '1';
})
```

## 事件绑定兼容写法

1.DOM2级事件处理程序 addEventListener() IE8不支持,  ie attachEvent()

[^IE8浏览器不支持DOM2处理程序]: 

2.attachEvent()内部的this指向了window,我们要对this的指向也做兼容

ie8的写法如下：

```javascript
var btn = document.getElementById('btn');
function handler(){
	console.log(this.innerHTML);
}
btn.attachEvent('onclick',function(){
	handler.call(btn);//将方法绑定到对象上，这时候this指向btn对象
})

btn.addEventListener('click',fn,false); 默认为false,冒泡  this指向当前btn对象
btn.attachEvent('onclick',fn) this指向window
```

全浏览器事件处理程序的兼容性代码:

```javascript
addEvent(btn,'click',function(){
	console.log(this.innerHTML);
})

function addEvent(target,eventType,handler){
	if(target.addEventListener){
		//chrome ff safari都可以
		target.addEventListener(eventType,handler,false);//事件冒泡
	}else{
			target.attachEvent('on' + eventType,function(){
				handler.call(target);//将方法绑定到对象上，这时候this指向对象
		});
	}
}
```

call方法可以改变this指向问题

```javascript
console.log(this);//chrome ff 指向window对象
var obj = {
	innerHTML : 'liujing'
}
function fn(){
	console.log(this.innerHTML);
}
						
//call方法可以改变this指向问题
fn.call(obj);//chrome ff this指向obj对象  
fn.call();//chrome ff 指向window对象
```

## 事件调用顺序总结

1. 相同点：如果同时出现HTML事件处理程序和DOM0级事件处理程序,DOM0级会覆盖HTML事件处理程序

2. 不同点：

    2.1 chrome,safari,FF浏览器以及IE11结果： DOM0级 DOM2级
    2.2 IE9、10结果为：DOM0级 DOM2级 IE 
    2.3 ie8结果： DOM0级 IE

DOM0级事件处理程序

```javascript
box.onclick = function(){
	this.innerHTML += 'DOM0级\n';
}
```

DOM2级事件处理程序

```javascript
if(box.addEventListener){
    box.addEventListener('click',function(){
    	this.innerHTML += 'DOM2级\n';
    })
}
```

IE级事件处理程序

```javascript
if(box.attachEvent){
        box.attachEvent('onclick',function(){
        box.innerHTML += 'IE\n';
	})
}
```

## 如何获取事件对象

1.如何获取事件对象
2.事件目标
3.事件代理
4.事件冒泡
5.事件流阶段 eventPhase
6.取消默认事件  //javascript:void(0);

css代码：

```css
<style type="text/css">
    #box {
        width: 300px;
        height: 100px;
        background-color: orange;
    }
</style>
```

html代码：

```html
<a href="javascript:void(0);"></a>
<div id='box'></div>
```

JavaScript代码：

```javascript
// 兼容性
window.onload = function() {
	var box = document.getElementById('box');
	// 1.event对象是事件处理程序的第一个参数 ie8浏览器不兼容
	// ie8浏览器得到的结果是undefined,其它浏览器[object MouseEvent]
    box.onclick = function (e){
        console.log(e);
        this.innerHTML = e;
    }

	// 2.直接可以使用event变量 火狐浏览器低版本获取出来的是undefined
    box.onclick = function() {
        this.innerHTML = event;
    }

	// 兼容性代码
	box.onclick = function(e){
		console.log(e);
		// 兼容性写法
		e = e || window.event;//[object MouseEvent]
		box.innerHTML = e;  //[object Event]
	}
}
```

## 事件目标

1.currentTarget属性：返回事件当前所在的节点,正在执行的监听函数所绑定的节点

[^target和srcElement功能一样]: 

css代码：

```css
<style type="text/css">
    .item {
        width: 100px;
        height: 80px;
        background-color: lightblue;
        margin: 50px 10px;
    }
</style>
```

html代码：

```html
<ul id="box">
    <li class="item">1</li>
    <li class="item">2</li>
</ul>
```



```javascript
box.onclick = function (e){
	e =  e || window.event;
	// console.log(e);//MouseEvent对象中的cancelBubble,cancelable;clientX;clientY属性
	//e,currentTarget 值是null //ie8低版本
	//e.srcElement值是li.item
	console.log(e.currentTarget);//整个ul对象
	var items = document.getElementsByTagName('li');
	items[0].innerHTML = e.currentTarget;//[object HTMLListElement]   [object HTMLUListElement]
}
//监听的是li的父级节点ul
```

2.target属性：返回的是事件的实际目标对象

[^注意：target属性 ie8不支持]: 

```javascript
 box.onclick = function (e){
	e =  e || window.event;
    console.log(e.target);//<li class="item">1</li>
    console.log(e.target === this);//false
				
    // this对象跟e.currentTarget属性是一致的
	console.log(e.currentTarget === this);//true

}
```

 3.srcElement跟target功能是一致的,FF低版本(firefox)的浏览器不支持

```javascript
box.onclick = function(e) {
    e = e || window.event;
    // console.log(e.srcElement);//<li class="item">1</li>

    var target = e.target || e.srcElement;
    target.style.backgroundColor = 'red';

}
box.onmouseout = function(e) {
    e = e || window.event;
    var target = e.target || e.srcElement;
    target.style.backgroundColor = 'lightblue';

}
```

## 事件代理

css代码如下：

```css
<style type="text/css">
    * {
        padding: 0;
        margin: 0;
    }

    ul {
        list-style: none;
        overflow: hidden;
        margin-top: 80px;
    }

    ul li {
        float: left;
        width: 100px;
        height: 30px;
        text-align: center;
        line-height: 30px;
        color: #fff;
        background-color: #000;
        margin: 0 10px;
    }
</style>
```

html代码：

```html
<ul id="box">
    <li>1</li>
    <li>2</li>
    <li>3</li>
    <li>4</li>
    <li>5</li>
</ul>
```

javascript代码：

```javascript
window.onload = function() {
    // 常规方法实现
    // 1.获取标签
    var lis = document.getElementsByTagName('li');
    for(var i = 0; i < lis.length; i++){
        lis[i].onmouseover = function (){
        	this.style.backgroundColor = 'blue';
    	}
        lis[i].onmouseout = function (){
        	this.style.backgroundColor = 'black';
        }
    }

    // 事件代理的方式实现 事件实际目标对象来实现
    // 事件代理应用: 事件实际目标对象target和srcElement属性完成
    // 优点: 提高性能以及降低代码的复杂度
    //ul 也被触发了
    var box = document.getElementById('box');
    box.onmouseover = function(e) {
        e = e || window.event;
        var target = e.target || e.srcElement;
        target.style.backgroundColor = 'blue';
    }
    box.onmouseout = function(e) {
        e = e || window.event;
        var target = e.target || e.srcElement;
        target.style.backgroundColor = 'black';
    }

}
```

[^ul 也被触发了]: 

## 事件代理的应用

```javascript
//模拟未来的某个事件来添加对应的数据
var box = document.getElementById('box');
setTimeout(function(){
    var item = document.createElement('li');
    item.innerHTML = '6';
    box.appendChild(item);
},100);
```

target本身就会因为事件捕获而指向具体的节点

```javascript
// target本身就会因为事件捕获而指向具体的节点
var box = document.getElementById('box');
// 给未来的元素绑定事件,使用事件代理
box.onmouseover = function (e){
    e = e || window.event;
    var target = e.target || e.srcElement;
    target.style.backgroundColor = 'blue';
}
box.onmouseout = function (e){
    e = e || window.event;
    var target = e.target || e.srcElement;
    target.style.backgroundColor = 'black';
}
				
```

## 事件冒泡的用法

属性： bubbles; cancelBubble 

方法：stopPropagation() ;stopImmediatePropagation()

1.bubbles 返回一个布尔值 表示当前事件是否会冒泡,只读 true:冒泡;false:非冒泡

html代码：

```html
<button id="btn" style="height: 30px; width: 200px;">按钮</button>
<input type="text" id="test">
```

[^大部分事件都会冒泡,但是focus blur scroll事件不会冒泡]: 

```javascript
var btn = document.getElementById('btn');
var test = document.getElementById('test');
btn.onclick = function(e) {
    e = e || window.event;
    console.log(e.bubbles);//true
}
test.onfocus = function (e){
    e = e || window.event;
    console.log(e.bubbles);//false
}
```

2.stopPropagation()表示取消事件的进一步冒泡 无返回值,但是无法阻止同一事件的其它监听函数被调用

[^ie8浏览器不支持]: 

```javascript
btn.onclick = function(e){
    e = e || window.event;
    //阻止冒泡
    e.stopPropagation();
    this.innerHTML = '阻止冒泡';
}

document.body.onclick = function(e){
    e = e || window.event;
	console.log('body');
}

btn.addEventListener('click',function(e){
    e = e || window.event;
    e.stopPropagation();
    this.innerHTML = '修改了';
},false);
btn.addEventListener('click',function(e){
    e = e || window.event;
    this.style.backgroundColor = 'blue';//上面没有阻止这个操作
},false);
document.body.onclick = function (e){
    e = e || window.event;
    console.log('body');//阻止这个操作
}
```

3.stopImmediatePropagation()既可以阻止冒泡,也可以阻止同一事件的其它监听函数被调用

```javascript
btn.addEventListener('click', function(e) {
    e = e || window.event;
    e.stopImmediatePropagation();
    this.innerHTML = '修改了';
}, false);
btn.addEventListener('click', function(e) {
    e = e || window.event;
    this.style.backgroundColor = 'blue';
}, false);
document.body.onclick = function(e) {
    e = e || window.event;
    console.log('body');
}
```

4.cancelBubble 属性用于阻止冒泡 可读写 默认值为false,当设置为true,cancelBubble可以取消事件冒泡

```javascript
btn.onclick = function(e) {
    e = e || window.event;
    if(e.stopPropagation){
        e.stopPropagation();
    }else{
        e.cancelBubble = true;//全局浏览器支持 ie8支持
	}
	this.innerHTML = '修改了';
}
document.body.onclick = function(e) {
    e = e || window.event;
    console.log('body');
}
```

兼容stopPropagation() stopImmediatePropagation()ie8不支持

[^e.cancelBubble = true;全浏览器都支持 不是标准写法]: 

```javascript
var handler = function (e){
    e = e || window.event;
    if(e.stopPropagation){
        e.stopPropagation();
    }else{
        e.cancelBubble = ture;
    }
}
```

## 事件流阶段属性

e.eventPhase  0 表示事件没有发生  1 表示捕获阶段 2目标阶段  3冒泡阶段

```javascript
var btn = document.getElementById('btn');
// 2 目标阶段
btn.onclick = function (e){
    e = e || window.event;
    this.innerHTML = e.eventPhase + '阶段';
    console.log(e.eventPhase);
}
```

```javascript
// 1 捕获阶段
document.body.addEventListener('click',function (e){
    e = e || window.event;				
    console.log(e.eventPhase);
},true);
```

```javascript
// 3 冒泡阶段
document.body.addEventListener('click',function (e){
    e = e || window.event;				
    console.log(e.eventPhase);
},false);
```

## 取消默认事件

事件对象中两个方法 阻止默认事件: 方法：preventDefault()  属性：returnValue兼容ie8以下浏览器   return fasle

```html
<a href="#" id="apeland">小猿圈</a>
<a href="javascript:;">小猿圈2</a>
```

```javascript
var apeland = document.getElementById('apeland');
	// 1.preventDefault() ie8不支持
	apeland.onclick  = function (e){
		e = e || window.event;
		// 阻止冒泡
		// e.stopPropagation();

		// 阻止默认事件 ie8以下不支持
		// e.preventDefault();
		
		
		// 阻止默认事件 FF ie8以上不支持  ie9不支持
		// e.returnValue = false;

		// 兼容性写法
		if(e.preventDefault){
			e.preventDefault();
		}else{
			//兼容IE8以下的浏览器
			e.returnValue = false;
		}

		// 阻止默认事件
		return false;
	}
```

## 事件对象中的坐标位置

事件对象中提供了:clientX/Y,x/y,offsetX/Y,screenX/Y,pageX/pageY

clientX/Y和x/y: 相对于浏览器（浏览器的有效区域）的X轴和Y轴的距离

```javascript
this.innerHTML = `clientX:${e.clientX};clientY:${e.clientY};X:${e.x};Y:${e.y}`;
//clientX:8;clientY:8;X:8;Y:8
```

screenX/Y:相对于显示器屏幕的X轴和Y轴的距离

```javascript
this.innerHTML = `screenX:${e.screenX};screenY:${e.screenY};`;
//screenX:8;screenY:110;
```

pageX/Y: 相对于页面的X轴和Y轴的距离   滚动和clientY不同

```javascript
this.innerHTML = `pageX:${e.pageX};pageY:${e.pageY};`;
//pageX:8;pageY:1316;
```

offsetX/Y:相对于事件源的X轴和Y轴的距离

```javascript
this.innerHTML = `offsetX:${e.offsetX};offsetY:${e.offsetY};`;
//offsetX:0;offsetY:0;
```

## 事件总结

事件
	1.事件流
		描述的是从页面接收事件的顺序
	    IE事件流是事件冒泡流，Netscape的事件流是事件捕获流
    2.事件流阶段
		（1）事件捕获阶段 （2）处于目标阶段  (3)事件冒泡阶段
		事件捕获阶段：从最不具体的节点（window/document）接收事件 往具体的节点进行传播
		事件冒泡阶段：从具体的节点开始接收事件，逐级往上传递到最不具体的节点。
	3.事件对象
		3.1 e.eventPhase 描述事件发生的阶段
		事件捕获阶段  1
		处于目标阶段  2
		事件冒泡阶段  3
		3.2 事件目标
			e.currentTarget === this

​			e.target
​			e.srcElement(FF浏览器兼容)

​			//兼容性代码

```javascript
var target = e.target || e.srcElement;
```

​		3.3 事件代理 也叫事件委托
​		3.4 取消默认行为

```javascript
e.preventDefault();//ie8不兼容
e.returnValue = false;//兼容
return false;
```

​		3.5 事件冒泡

```javascript
e.bubbles
blur  focus scroll三个事件返回值为false
e.stopPropagation(); //常用
e.stopImmediatePropagation();
e.cancelBubble = true; 

var handler = function(e){
    e = e || window.event;
    if(e.stopPropagation){
    	e.stopPropagation();
    }else{
    	e.cancelBubble = true; 
    }
}
```

​	4.事件处理程序

​		1) HTML事件处理程序
​		2) DOM0级事件处理程序

```javascript
btn.onclick = function(e){
	e = e || window.event;
}
//有事件覆盖现象
btn.onclick = function(e){
	e = e || window.event;
}
```

​		3) DOM2级事件处理程序

```javascript
btn.addEventListener('click',function (){

},false);
btn.addEventListener('click',function (){

},false);
var handler = function(){
....
}
btn.addEventListener('click',handler,false);
btn.removeEventListener('click',handler)
```

​		4) IE事件处理程序

```javascript
btn.attachEvent('onclick',function(){
	//在ie中小心this 这个this指向了window
})
btn.detachEvent('onclick',function(){

})

处理this的指向问题，函数中的call(target);
```

5. 事件对象中的属性 坐标位置
	5.1 clientX/Y x/y
		相对于当前的浏览器（有效的浏览器区域）的x轴和y轴的距离，与滚动条无关
	5.2 screenX/Y
		相对于显示器屏幕的x轴和y轴的距离
	5.3 pageX/Y
		相对于页面的x轴和y轴的距离，如果有滚动条，包含整个页面，与滚动条有关
	5.4 offsetX/Y
		相对于事件源x轴和y轴的距离

## 放大镜效果结构样式搭建

css代码

```css
<style type="text/css">
    *{margin:0px;padding:0px;}
    #box{width:430px;height:430px;border:1px solid #DDD;position:relative;margin:50px;}
    #small_box{width:430px;height:430px;position:relative;}
    #small_box #mask{position:absolute;width:210px;height:210px;background:url(images/dotted.png) repeat;top:0px;left:0px;display:none;}
    #big_box{position:absolute;left:440px;top:0px;width:430px;height:430px;border:1px solid #ddd;overflow:hidden;display:none;}
    #big_box img{position:absolute;z-index:5;}
</style>
```

html代码

```html
<div id="box">
    <div id="small_box">
        <img src="images/photo.jpg">
        <span id="mask"></span>
    </div>
    <div id="big_box">
    	<img src="images/photo01.jpg">
    </div>
</div>
```

javascript代码

```javascript
window.onload = function(){
	//1.获取需要的标签
    var box = document.getElementById('box');
    var small_box = box.children[0];
    var big_box = box.children[1];

    var small_img = small_box.children[0];
    var mask = small_box.children[1];
    var big_img = big_box.children[0];

	//2.监听鼠标移动
	small_box.onmouseover = function(){
		//2.1让遮罩层和大盒子显示出来
        mask.style.display = 'block';
        big_box.style.display = 'block';
		//2.2监听鼠标移动
        small_box.onmousemove = function(e){
            e = e || window.event;
            //2.3求出小盒子移动的水平和垂直的距离
            var moveX = e.clientX - small_box.offsetLeft - box.offsetLeft - mask.offsetWidth*0.5;
            //small_box.offsetLeft 为0
            var moveY = e.clientY - small_box.offsetTop - box.offsetTop - mask.offsetHeight*0.5;
            //2.4移动边界处理
            if(moveX < 0){
                moveX = 0;
            }else if(moveX >= small_box.offsetWidth - mask.offsetWidth){
                moveX = small_box.offsetWidth - mask.offsetWidth;
            }
            
            if(moveY < 0){
                moveY = 0;
            }else if(moveY >= small_box.offsetWidth - mask.offsetWidth){
                moveY = small_box.offsetHeight - mask.offsetHeight;
            }

            //2.5让小盒子移动起来
            mask.style.left = moveX + 'px';
            mask.style.top = moveY + 'px';

            //2.6大图移动起来
            //公式：moveX/大图移动的距离 = (small_box宽度-mask宽度)/(big_img宽度-big_box宽度);
            // var x = moveX / (small_box.offsetWidth - mask.offsetWidth);
            // var y = moveY / (big_img.offsetWidth - big_box.offsetWidth);

			var scale = (big_img.offsetWidth - big_box.offsetWidth)/(small_box.offsetWidth - mask.offsetWidth);

            // console.log(small_box.offsetWidth);//430
            // console.log(mask.offsetWidth);//210
            // console.log(big_img.offsetWidth);//800
            // console.log(big_box.offsetWidth);//432

            // big_img.style.left = - x * (big_img.offsetWidth - big_box.offsetWidth) +'px';
            // big_img.style.top = - y * (big_img.offsetHeight - big_box.offsetHeight) +'px';

            big_img.style.left = - moveX * scale +'px';
            big_img.style.top = - moveY * scale +'px';


            // var lw = mask.offsetLeft;
            // var lh = mask.offsetTop;
            // var rate = big_img.offsetWidth/big_box.offsetWidth;
            // // var rate = small_box.offsetWidth/mask.offsetWidth;
            // var newX = lw * 1.68;
            // var newY = lh * 1.68;
            // big_img.style.left = -newX +'px';
            // big_img.style.top = -newY +'px';

		}

	}
    small_box.onmouseout = function(e){
        mask.style.display = 'none';
        big_box.style.display = 'none';
	}
}
```

## 进度条特效

css代码：

```css
<style type="text/css">
    * {
        padding: 0;
        margin: 0;
    }

    #progress {
        width: 600px;
        height: 35px;
        line-height: 35px;
        margin: 100px auto;
        position: relative;
    }

    #progress_bar {
        position: relative;
        width: 500px;
        height: 100%;
        background-color: #CCCCCC;
        border-radius: 8px;
    }

    #progress_bar_current {
        width: 0;
        height: 100%;
        background-color: orange;
        border-top-left-radius: 8px;
        border-bottom-left-radius: 8px;
    }

    #bar {
        position: absolute;
        width: 25px;
        height: 50px;
        background-color: orange;
        top: -5px;
        left: 0;
        border-radius: 8px;
        cursor: pointer;
    }

    #progress_value {
        position: absolute;
        right: 30px;
        top: 0;
    }
</style>
```

html代码

```html
<div id="progress">
    <div id="progress_bar">
    	<div id="progress_bar_current"></div>
    	<span id="bar"></span>
    </div>
    <div id="progress_value">0%</div>
</div>
```

javascript代码：

```javascript
<script type="text/javascript">
    window.onload = function() {
        // 1.需要的标签
        var progress = document.getElementById('progress');
        var progress_bar = progress.children[0];
        var progress_value = progress.children[1];
        var progress_bar_current = progress_bar.children[0];
        var bar = progress_bar.children[1];
        // 2.监听鼠标摁下
        bar.onmousedown = function(e) {
            e = e || window.event;
            // 2.1 获取初始距离
            var offsetLeft = progress.offsetLeft;

            // 2.2 监听鼠标移动
            document.onmousemove = function(e) {
            	e = e || window.event;
            	//2.3 获取移动的距离
            	var x = e.clientX - offsetLeft;

                // 2.4 边界处理
                if (x < 0) {
                	x = 0;
                } else if (x >= progress_bar.offsetWidth - bar.offsetWidth) {
                	x = progress_bar.offsetWidth - bar.offsetWidth;
                }

                // 2.5 移动
                bar.style.left = x + 'px';
                progress_bar_current.style.width = x + 'px';
                progress_value.innerHTML = parseInt((x / (progress_bar.offsetWidth - bar.offsetWidth)) * 100) + '%';
            }

            document.onmouseup = function() {
                console.log('鼠标弹起了');
                document.onmousemove = null;
    		}

   	 	}
    }
</script>
```

