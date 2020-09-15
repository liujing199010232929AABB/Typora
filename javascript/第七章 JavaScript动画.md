# 第七章 JavaScript动画

## 简单运动

如何让物体运动起来

css代码如下：

```css
<style type="text/css">
    *{
        padding: 0;
        margin: 0;
    }
    button{
        width: 80px;
        height: 40px;
    }
    #box{
        width: 200px;
        height: 200px;
        background-color: pink;
        position: absolute;
        top: 50px;
    }
</style>
```

html代码：

```html
<button id="btn">动画</button>
<div id="box"></div>
```

javascript代码：

```javascript
<script type="text/javascript">
    // 简单的动画存在的问题: 1.处理边界  2.定时器的管理

    // 1.获取事件源
    var btn = document.getElementById('btn');
    var box = document.getElementById('box');
    var timer = null;
    // 2.给按钮绑定事件
    btn.onclick = function (){
        // 先关闭定时器 再开启定时器 防止定时器累加 导致物体加快
        clearInterval(timer);
        // 3.让物体运动起来(定时器)   如果没有边界，速度会越来越快
        timer = setInterval(function (){
            if(box.offsetLeft === 500){
            	clearInterval(timer);
            }else{
                box.style.left = box.offsetLeft + 10 + 'px';
            }
    	},30);

    }
</script>
```

## 侧边栏动画效果

css代码如下：

```css
<style type="text/css">
	*{margin:0px;padding:0px;}
	#box{width:200px;height:200px;background-color:red;position:relative;margin-top:20px;left:-200px;}
	span{width: 40px;height: 60px;background-color: 	pink;color:#fff;position:absolute;right:-40px;top:50%;margin-top:-30px;text-align:center;line-		  			height:60px;border-top-right-radius:10px;border-bottom-right-radius:10px;}
</style>
```

html代码：

```html
<div id="box">
	<span>拉开</span>
</div>
```

javascript代码：

```javascript
<script type="text/javascript">
    window.onload = function(){
        var box = document.getElementById('box');
        var sp = document.getElementsByTagName('span');
        box.onmouseover = function(){
        	this.style.left = 0 + 'px';
        }
        box.onmouseout = function (){
            this.style.left = -box.offsetWidth + 'px';
            // this.style.left = -200 + 'px';
        }	
    }
</script>
```

[^注意：如果offsetWidth的对象有padding或者border值那么获取的obj.offsetWidth与定义的width不一致]: 

## 侧边栏动画效果(匀速运动)

css代码如下：

```css
<style type="text/css">
    *{
        padding: 0;
        margin: 0;
    }
    #box{
        width: 200px;
        height: 200px;
        background-color: red;
        position: relative;
        left: -200px;
    }
    #box span{
        position: absolute;
        width: 40px;
        height: 60px;
        color: #fff;
        background-color: #000000;
        right: -40px;
        top: 50%;
        margin-top: -30px;
        line-height: 60px;
        text-align: center;
    }
</style>
```

html代码如下：

```html
<div id="box">
	<span>拉开</span>
</div>
```

javascript代码如下：

```javascript
<script type="text/javascript">
	window.onload = function (){
    var box = document.getElementById('box');
    var timer = null;
    box.onmouseover = function(){
        //先关闭定时器
        clearInterval(timer);
        timer = setInterval(function(){
            if(box.offsetLeft === 0){
                clearInterval(timer);
                return;
            }else{
            	box.style.left = box.offsetLeft + 5 +'px';
            }					
         },100)
	  }			
      box.onmouseout = function(){
          clearInterval(timer);
          timer = setInterval(function(){
              if(box.offsetLeft === -200){
                  	clearInterval(timer);
                  	return;
                  }else{
                  	box.style.left = box.offsetLeft  -5 +'px';
             	 }
              })
           }
	   }	
</script>
```

## 侧边栏封装

javascript代码：

```javascript
<script type="text/javascript">
	window.onload = function (){
        var box = document.getElementById('box');
        var timer = null,speed = 0;
        box.onmouseover = function(){
            //先关闭定时器
            startAnimation(this,0);
        }
        box.onmouseout = function(){
        	startAnimation(this,-200);
        }
				
        function startAnimation(obj,end){
            clearInterval(timer);
        	speed = end > obj.offsetLeft ? 5 : -5;
        	timer = setInterval(function(){
        		if(obj.offsetLeft === end){
                    clearInterval(timer);
                    return;
                }else{
                	obj.style.left = obj.offsetLeft + speed +'px';
                }
            })		
         }	
     }
</script>
```

## 缓动运动

javascript代码：

```javascript
<script type="text/javascript">
    //0 —— 200
    //缓慢运动公式：加速度 = (结束值 - 起始值)/时间(缓动系数) 加速度是由快到慢
    window.onload = function (){
        var box = document.getElementById('box');
        var timer = null;end = 0;end2 = -200;
        box.onmouseover = function(){
            clearInterval(timer);
            timer = setInterval(function(){
                speed = Math.ceil((end - box.offsetLeft)/20);//0.45 数学Math.ceil()向上取整
                //处理边界问题
                if(box.offsetLeft === end){
                    clearInterval(timer);
                    return;
                }else{
                    // console.log(box.offsetLeft,speed);//-9 0.45
                    box.style.left = box.offsetLeft + speed +'px';//left: -8.55px;
                }				
       		 },30);
    	}								
        box.onmouseout = function(){
        	clearInterval(timer);
        	timer = setInterval(function(){
        		speed = Math.floor((end2 - box.offsetLeft)/20);//0.45 数学Math.ceil()向上取整
            	//处理边界问题
            	if(box.offsetLeft === end2){
            		clearInterval(timer);
            		return;
            	}else{
            		console.log(box.offsetLeft,speed);//-199 -1
            		box.style.left = box.offsetLeft + speed +'px';//left: -8.55px;
            	}				
            },30);
        }	
	}
</script>
```

## 缓动运动封装

javascript代码：

```javascript
<script type="text/javascript">
    //缓慢运动公式：加速度 = (结束值 - 起始值)/时间(缓动系数) 加速度是由快到慢
    window.onload = function (){
        var box = document.getElementById('box');
        var timer = null;
        box.onmouseover = function(){			
       		slowAnimation(this,0);
    	}

        box.onmouseout = function(){
        	slowAnimation(this,-200);
        }

        function slowAnimation(obj,end){
            clearInterval(timer);
            timer = setInterval(function(){
                speed = (end - obj.offsetLeft) / 20;
                //如果速度大于0，证明物体往右走，速度小于0，证明物体往左走
                speed = speed > 0 ? Math.ceil(speed) : Math.floor(speed);
                //处理边界问题
                if(obj.offsetLeft === end){
                    clearInterval();
                    return;
                }else{
                    //console.log(obj.offsetLeft,speed)
                    obj.style.left = obj.offsetLeft + speed + 'px';
                }
        	},30)
    	}
	}	
</script>
```

## 透明度动画

css代码：

```css
<style type="text/css">
	#box{
		width:200px;
		height:200px;
		background-color:pink;
		opacity:0.3;//全浏览器支持
		filter:alpha(opacity:30);
		//支持ie8以下浏览器  filter:alpha(opacity:30)
    }
</style>
```

javascript代码：

```javascript
<script type="text/javascript">
    window.onload = function(){
        //1.获取事件源
        var box = document.getElementById('box');
        //2.监听事件
        box.onmouseover = function(){
        	opacityAnimation(this,100);
        }
        box.onmouseout = function(){
        	opacityAnimation(this,30);
        }
        var timer = null;alpha = 30;speed = 0;
        function opacityAnimation(obj,endAlpha){
            clearInterval(timer);
            timer = setInterval(function(){
                //求透明度变化的速度
                speed = endAlpha > alpha ? 1 :-1;
                //边界处理
                if(alpha === endAlpha){
                    clearInterval(timer);
                    return;
                }
            	alpha += speed;
                obj.style.opacity = alpha/100;
                obj.style.filter = `alpha(opacity:${alpha})`;
            },30)
        }
    }
</script>
```

## 多物体缓动运动

css代码：

```css
<style type="text/css">
    *{
        padding:0px;
        margin:0px;
    }
    div{
        width:300px;
        height:150px;
        background-color:#3081BF;
        margin:20px 0px;
    }
</style>
```

html代码：

```html
<div></div>
<div></div>
<div></div>
```

javascript代码

```javascript
<script type="text/javascript">
    window.onload = function(){
        var allBoxs = document.getElementsByTagName('div');
        for(var i = 0;i<allBoxs.length;i++){
            allBoxs[i].onmouseover = function(){
            	startAnimation(this,600);
        	}
            allBoxs[i].onmouseout = function(){
                startAnimation(this,300);
            }
     	}
     
    	var timer = null,speed = 0;
        function startAnimation(obj,endTarget){
        	//针对于多物体运动，定时器的返回值要绑定在当前的对象上
        	clearInterval(obj.timer);
        	obj.timer = setInterval(function(){
                //1.求速度
                speed = (endTarget - obj.offsetWidth) / 10;
                speed = endTarget > obj.offsetWidth ? Math.ceil(speed) : Math.floor(speed);
                //2.设置临界值
                if(endTarget === obj.offsetWidth){
                    clearInterval(obj.timer);
                    return;
                }
                //3.运动
                obj.style.width = obj.offsetWidth + speed +'px';
            },30)
        }
    }
</script>
```

[^针对于多物体运动，定时器的返回值要绑定在当前的对象上]: 

## 正确获取元素样式属性

css代码

```css
<style type="text/css">
	#box{
        width:200px;
        height:200px;
        background-color:red;
        border:1px solid #000;
	}
</style>
```

javascript代码

```javascript
<script type="text/javascript">
        window.onload = function(){
            var box = document.getElementById('box');
            move(box);
            function move(obj){
                setInterval(function(){
                    // alert(obj.offsetWidth);   obj.style.width为获取行内宽度
                    obj.style.width = parseInt(getStyle(obj,'width')) - 1 + 'px';
                    // console.log(getStyle(obj,'width'));//string类型
                },30);
            }
            //获取元素属性的函数
            // @param {object} obj当前元素对象
            // @param {object} attr 当前元素对象的属性
            function getStyle(obj,attr){
                if(obj.currentStyle){
                    //判断是否在ie浏览器上  ie11
                    return obj.currentStyle[attr];
                }else{
                    //兼容主流浏览器  ie11也不支持
                    return getComputedStyle(obj,null)[attr];
                }	
            }
    	}
</script>
```

[^obj.currentStyle[attr\]：获取ie浏览器上的的样式属性，包括ie11]: 
[^getComputedStyle(obj,null)[attr\]:获取兼容主流浏览器上的样式属性，ie11不支持]: 

## 多物体缓动运动完整版

css代码

```css
<style type="text/css">
    *{
        padding:0px;
        margin:0px;
    }
    div{
        width:300px;
        height:150px;
        background-color:red;
        margin:20px 0px;
        border:4px solid #000;
    }
</style>
```

html代码

```html
<div></div>
<div></div>
<div></div>
```

javascript代码

```javascript
<script type="text/javascript">
    window.onload = function(){
        var boxs = document.getElementsByTagName('div');
        for(var i = 0;i<boxs.length;i++){
            boxs[i].onmouseover = function(){
            	startAnimation(this,600);
            }
            boxs[i].onmouseout = function(){
            	startAnimation(this,300);
            }
        }
        var speed = 0,timer = null;
        function startAnimation(obj,endTarget){
        	clearInterval(obj.timer);
        	obj.timer = setInterval(function(){
                //0.获取样式属性
                var cur = parseInt(getStyle(obj,'width'));//string
            	// 1.求速度
                speed = (endTarget - cur) / 20;
                speed = (endTarget > cur) ? Math.ceil(speed) : Math.floor(speed);
                //2.临界处理
                if(endTarget === cur){
                    clearInterval(obj.timer);	
                    return;
                 }
                //3.运动起来
                obj.style.width = cur + speed + 'px';
            },30)
        }

        //获取元素属性的函数
        // @param {object} obj当前元素对象
        // @param {object} attr 当前元素对象的属性
        function getStyle(obj,attr){
            if(obj.currentStyle){
                //判断是否在ie浏览器上  ie11
                return obj.currentStyle[attr];
            }else{
                //兼容主流浏览器  ie11也不支持
                return getComputedStyle(obj,null)[attr];
        	}	
    	}
	}
</script>
```

## 多值运动

javascript代码

```javascript
<script type="text/javascript">
    window.onload = function(){
    	var boxs = document.getElementsByTagName('div');
        // for(var i = 0;i<boxs.length;i++){
        // 	boxs[i].onmouseover = function(){
        // 		startAnimation(this,600);
        // 	}
        // 	boxs[i].onmouseout = function(){
        // 		startAnimation(this,300);
        // 	}
        // }
        boxs[0].onmouseover = function(){
        	startAnimation(this,'width',600);
        }
        boxs[0].onmouseout = function(){
        	startAnimation(this,'width',300);
        }
        boxs[1].onmouseover = function(){
        	startAnimation(this,'height',300);
        }
        boxs[1].onmouseout = function(){
        	startAnimation(this,'height',150);
        }
        boxs[2].onmouseover = function(){
        	startAnimation(this,'width',600);
        }
        boxs[2].onmouseout = function(){
        	startAnimation(this,'width',300);
        }


        var speed = 0,timer = null;
        function startAnimation(obj,attr,endTarget){
            clearInterval(obj.timer);
            obj.timer = setInterval(function(){
                //0.获取样式属性
                var cur = parseInt(getStyle(obj,attr));//string
                // 1.求速度
                speed = (endTarget - cur) / 20;
                speed = (endTarget > cur) ? Math.ceil(speed) : Math.floor(speed);
                //2.临界处理
                if(endTarget === cur){
                    clearInterval(obj.timer);
                    return;
            	}
                //3.运动起来
                //obj.style.width = cur + speed + 'px';
                obj.style[attr] = cur + speed + 'px';
        	},30)
        }

        //获取元素属性的函数
        // @param {object} obj当前元素对象
        // @param {object} attr 当前元素对象的属性
        function getStyle(obj,attr){
            if(obj.currentStyle){
                //判断是否在ie浏览器上  ie11
                return obj.currentStyle[attr];
            }else{
                //兼容主流浏览器  ie11也不支持
                return getComputedStyle(obj,null)[attr];
            }	
        }
	}
</script>
```

## 多值运动-处理透明度

css代码如下

```css
<style type="text/css">
    *{
        padding:0px;
        margin:0px;
    }
    div{
        width:300px;
        height:150px;
        background-color:pink;
        margin:20px 0px;
        border:4px solid #000;
        opacity:0.3;
        filter:alpha(opacity:30);
    }
</style>
```

javascript代码如下

```javascript
<script type="text/javascript">
    window.onload = function(){
        var boxs = document.getElementsByTagName('div');
        boxs[0].onmouseover = function(){
        	startAnimation(this,'opacity',100);
        }
        boxs[0].onmouseout = function(){
        	startAnimation(this,'opacity',30);
        }
        boxs[1].onmouseover = function(){
        	startAnimation(this,'height',300);
        }
        boxs[1].onmouseout = function(){
        	startAnimation(this,'height',150);
        }
        boxs[2].onmouseover = function(){
        	startAnimation(this,'width',600);
        }
        boxs[2].onmouseout = function(){
        	startAnimation(this,'width',300);
        }

        var speed = 0,timer = null;
        	// alert(Math.round(0.07*100));//四舍五入一下
        function startAnimation(obj,attr,endTarget){
        	clearInterval(obj.timer);
        	obj.timer = setInterval(function(){
                var cur =  0;
                //透明度变化处理
                //0.获取样式属性
                if(attr === 'opacity'){
                    cur = Math.round(parseFloat(getStyle(obj,attr))*100);//正常的四色五入
                }else{
                    cur = parseInt(getStyle(obj,attr));
                }


                // console.log(typeof getStyle(obj,attr));//string
                // 1.求速度
                speed = (endTarget - cur) / 20;
                speed = (endTarget > cur) ? Math.ceil(speed) : Math.floor(speed);
                //2.临界处理
                if(endTarget === cur){
                    clearInterval(obj.timer);
                    return;
                }
                //3.运动起来
                //obj.style.width = cur + speed + 'px';
                if(attr === 'opacity'){
                    obj.style[attr] = `alpha(opacity:${cur+speed})`;
                    obj.style[attr] = (cur + speed)/100;
                }else{
                	obj.style[attr] = cur + speed + 'px';
                }
            },30)
         }


    	//获取元素属性的函数
        // @param {object} obj当前元素对象
        // @param {object} attr 当前元素对象的属性
        function getStyle(obj,attr){
            if(obj.currentStyle){
                //判断是否在ie浏览器上  ie11
                return obj.currentStyle[attr];
            }else{
                //兼容主流浏览器  ie11也不支持
                return getComputedStyle(obj,null)[attr];
            }	
        }
	}
    </script>
```

## 链式运动

javascript代码

```javascript
<script type="text/javascript">
	window.onload = function(){
        	var box = document.getElementById('box');

            box.onmouseover = function(){
                startAnimation(box,'width',500,function(){
                	startAnimation(box,'height',400);
            	});
        	}
            box.onmouseout = function(){
                startAnimation(this,'width',300,function(){
                	startAnimation(box,'height',150);
            	});
        	}
    	}
</script>
```

myAnimation3.js代码

```javascript
var speed = 0,timer = null;
// 动画函数
// @param {object} obj 当前的对象
// @param {object} attr 当前元素对象的属性
// @param {object} endTarget 末尾位置

// alert(Math.round(0.07*100));//四舍五入一下
function startAnimation(obj,attr,endTarget,fn){
	//针对于多物体的运动，定时器的返回值要绑定到当前的对象中
	clearInterval(obj.timer);
	obj.timer = setInterval(function(){
		var cur =  0;
		//透明度变化处理
		//0.获取样式属性
		if(attr === 'opacity'){
			cur = Math.round(parseFloat(getStyle(obj,attr))*100);
		}else{
			cur = parseInt(getStyle(obj,attr));
		}
		// 1.求速度
			speed = (endTarget - cur) / 20;
			speed = (endTarget > cur) ? Math.ceil(speed) : Math.floor(speed);
		//2.临界处理
		if(endTarget === cur){
			clearInterval(obj.timer);
			if(fn){
				fn();
			}
			return;
		}
		//3.运动起来
		//obj.style.width = cur + speed + 'px';
		if(attr === 'opacity'){
			obj.style[attr] = `alpha(opacity:${cur+speed})`;
			obj.style[attr] = (cur + speed)/100;
		}else{
			obj.style[attr] = cur + speed + 'px';
		}
	},30)
}
				
				
//获取元素属性的函数
// @param {object} obj当前元素对象
// @param {object} attr 当前元素对象的属性
function getStyle(obj,attr){
	if(obj.currentStyle){
		//判断是否在ie浏览器上  ie11
		return obj.currentStyle[attr];
	}else{
		//兼容主流浏览器  ie11也不支持
		return getComputedStyle(obj,null)[attr];
	}	
}	
```

## 同时运动

javascript代码

```javascript
<script type="text/javascript">
    window.onload = function(){
        var box = document.getElementById('box');
        // json文件里面没有函数
        box.onmouseover = function(){
        	startAnimation(box,{"width":500,"opacity":30});
        }
    }
</script>
```

myAnimation4.js代码

```javascript
var speed = 0,timer = null;
// 动画函数
// @param {object} obj 当前的对象
// @param {object} attr 当前元素对象的属性
// @param {object} endTarget 末尾位置

// alert(Math.round(0.07*100));//四舍五入一下
function startAnimation(obj,json,fn){
	//针对于多物体的运动，定时器的返回值要绑定到当前的对象中
	clearInterval(obj.timer);
	obj.timer = setInterval(function(){
		var cur =  0;
		var flag = true;//标杆 如果为true，证明所有属性都达到终点值
		for(var attr in json){
			switch(attr){
				case 'opacity':
					cur = Math.round(parseFloat(getStyle(obj,attr))*100);
					break;
				case 'scrollTop':
					cur = obj[attr];//cur = obj.scrollTop
					break;
				default:
					cur = parseInt(getStyle(obj,attr));
					break;
			}
			// 1.求速度
				speed = (json[attr] - cur) / 20;
				speed = (json[attr] > cur) ? Math.ceil(speed) : Math.floor(speed);
			//2.临界处理
			if(json[attr] !== cur){
				flag = false;
			}
			//3.运动起来
			switch(attr){
				case 'opacity':
                    obj.style[attr] = `alpha(opacity:${cur+speed})`;
                    obj.style[attr] = (cur + speed)/100;
                    break;
				case 'scrollTop':
                    obj.scrollTop = cur + speed;
                    break;
				default:
                    obj.style[attr] = cur + speed + 'px';
                    break;
			}
			
			if(flag){
				clearInterval(obj.timer);
				if(fn){
					fn();
				}
				return;
			}
			
		}	
	},30)
}
								
//获取元素属性的函数
// @param {object} obj当前元素对象
// @param {object} attr 当前元素对象的属性
function getStyle(obj,attr){
	if(obj.currentStyle){
		//判断是否在ie浏览器上  ie11
		return obj.currentStyle[attr];
	}else{
		//兼容主流浏览器  ie11也不支持
		return getComputedStyle(obj,null)[attr];
	}	
}	
```

## 完美动画框架

javascript代码

```javascript
<script type="text/javascript">
    window.onload = function(){
    	var box = document.getElementById('box');
    	// json文件里面没有函数
    	box.onmouseover = function(){
    		startAnimation(box,{"width":500,"opacity":30});
    	}
    	box.onmouseout = function(){
    		startAnimation(box,{"width":200,"opacity":100});
    	}
    }
</script>
```

## 联动效果

css代码(380*380)

```css
<style type="text/css">
    *{padding:0px;margin:0px;}
    #ad{position:fixed;float:left;right:0px;bottom:0px;}
    #close{position:absolute;right:0px;top:0px;width:25px;height:25px;z-index:10;}
</style>
```

html代码

```html
<div id="ad">
    <img src="images/ad.png" width="300">
    <span id="close"></span>
</div>
```

javascript代码(myAnimation4.js)

```javascript
<script type="text/javascript">
    var ad = document.getElementById('ad');
    var close = document.getElementById('close');
    close.onclick = function(){
        startAnimation(ad,{"height":260},function(){
        	startAnimation(ad,{"width":0},function(){
        		ad.style.display = 'none';
    		})
    	})
    }
</script>
```

## 侧边栏横幅效果

css代码

```css
<style type="text/css">
    *{padding:0px;margin:0px;}
    #aside{position:absolute;top:200px;left:0px;width:300px;}
    #aside img{width:100%;}
</style>
```

html代码

```html
<body style="height:5000px;">
    <div id="aside">
    	<img src="images/aside.png">
    </div>
</body>
```

javascript代码()

```javascript
<script type="text/javascript">
    window.onload = function(){
        //1.获取标签
        var aside = document.getElementById('aside');
        var aside_top = aside.offsetTop;
    	window.onscroll = function(){
            //3.获取页面滚动的高度
            var docScrollTop = document.documentElement.scrollTop || document.body.scrollTop;
            startAnimation(aside,{"top":docScrollTop + aside_top});
        }
    }
</script>
```

[^document.documentElement.scrollTop:chrome浏览器，火狐浏览器可以，sarfi不行]: 
[^document.body.scrollTop：chrome浏览器不行，火狐浏览器可以，sarfi可以]: 

## 滚动监听特效结构和样式实现

css代码

```css
<style type="text/css">
	*{margin:0px;padding:0px;}
	ul{list-style:none;}
	a{text-decoration:none;}
	html,body{width:100%;height:100%;}
	.container{width:1190px;height:100%;margin:0px auto;}
	.container div{width:100%;height:100%;text-align:center;color:#fff;font-size:30px;}
	.aside{position:fixed;width:40px;right:20px;top:300px;font-size:16px;font-weight:700;text-align:center;}
	.aside li{height:50px;border-bottom:1px solid #ddd;}
	.aside li a{color:peru;}
	.aside li.current{background-color:coral;}
	.aside li.current a {color: #fff;}
</style>
```

html代码：

```html
<body style="height:5000px;">
    <div class="container" id="box">
        <div class="current">爱逛好货</div>
        <div>好点主播</div>
        <div>品质特色</div>
        <div>猜你喜欢</div>
    </div>
    <ul class="aside" id="aside">
        <li class="current">
        	<a href="javascript:void(0);">爱逛好货</a>
        </li>
        <li>
        	<a href="javascript:void(0);">好点主播</a>
        </li>
        <li>
        	<a href="javascript:void(0);">品质特色</a>
        </li>
        <li>
        	<a href="javascript:void(0);">猜你喜欢</a>
        </li>
    </ul>
</body>
```

javascript代码(myAnimation4.js)

```javascript
<script type="text/javascript">
    //1.获取事件源
    window.onload = function(){
        //1.获取标签
        var box = document.getElementById('box');
        var allBoxs = box.children;
        var aside = document.getElementById('aside');
        var lis = aside.children;
        var isClick = false;//默认没有滚动

        //2.上色
        var colors = ['red','pink','purple','blue'];
        for(var i=0;i<allBoxs.length;i++){
        	allBoxs[i].style.backgroundColor = colors[i];
        }	

        //3.监听侧边栏按钮的点击
        for(var j=0;j<lis.length;j++){
            lis[j].index = j;
            lis[j].onclick = function(){
            	isClick = true;
                for(var k = 0;k < lis.length;k++){
                    lis[k].className = '';
                }
                //设置当前的类
                this.className = 'current';
    			//页面动画起来
                startAnimation(document.documentElement,{
                    "scrollTop" : this.index * (document.body.clientHeight)
                    },function(){
                    	isClick = false;
                })
            	// document.documentElement.scrollTop = this.index*(document.body.clientHeight);		
            }
        }
    //4.监听滚动
        window.onscroll = function(){
        	if(!isClick){
        		//4.1获取页面滚动的高度
        		var docScrollTop = document.documentElement.scrollTop+20 || document.body.scrollTop+20;
        		for(var i=0;i<lis.length;i++){
        			if(docScrollTop > allBoxs[i].offsetTop){
                    	for(var j =0;j<allBoxs.length;j++){
                    		lis[j].className = '';
        				}
        				lis[i].className = 'current';
        			}
        		}
        	}
        }		
    }

</script>
```

## 轮播图特效

css代码

```css
*{
	padding:0px;
	margin:0px;
}
.slider{
	width:400px;
	height:500px;
	margin:100px auto;
	position:relative;
	overflow: hidden;
}
.slider .slider_scroll{
	position:relative;
	width:400px;
	height:500px;
}
.slider_main{
	position:relative;
	width:400px;
	height:500px;
}
.slider_main .item{
	width:400px;
	height:500px;
	position:absolute;
}
.slider_main .item img{
	width:400px;
	height:500px;
}
.slider_index{
	width:400px;
	height:40px;
	text-align:center;
	color:#fff;
	font-weight:700;
	z-index:20;
	position:absolute;
	bottom:0;
	background-color:rgb(0,0,0,0.5);
	cursor:pointer;
}
.slider_index .slider_index_icon{
	display: inline-block;
	line-height: 40px;
	margin: 0 10px;
}
.slider_index .slider_index_icon.current{
	color: red;
}
.slider_scroll span{
	position: absolute;
	width:30px;
	height:68px;
	background:url(../images/icon-slides.png) no-repeat;
	top:50%;
	margin-top:-34px;
	cursor:pointer;
	z-index:30;
}
.slider_scroll span.next{
	right: 0;
	background-position: -46px 0;
}
.slider_scroll span.prev{
	left: 0;
	background-position: 0 0;
}
```

html代码

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>p_20 轮播图特效</title>
		<link rel="stylesheet" href="css/index.css" type="text/css">
	</head>
	<body>
		<div id="slider" class="slider">
			<div class="slider_scroll" id="slider_scroll">
				<div class="slider_main" id="slider_main">
					<div class="item">
						<a href="#">
							<img src="images/img1.png">
						</a>
					</div>
					<div class="item">
						<a href="#">
							<img src="images/img2.png">
						</a>
					</div>
					<div class="item">
						<a href="#">
							<img src="images/img3.png">
						</a>
					</div>
					<div class="item">
						<a href="#">
							<img src="images/img4.png">
						</a>
					</div>
				</div>
				
				<span class="next" id="next"></span>
				<span class="prev" id="prev"></span>
			</div>
			<div class="slider_index" id="slider_index">
				<!-- 通过js脚本动态生成 -->
			</div>
		</div>
		<script src="../js/myAnimation4.js" type="text/javascript" charset="utf-8"></script>
		<script src="js/index.js" type="text/javascript" charset="utf-8"></script>
	</body>
</html>

```

index.js代码

```javascript
window.onload = function(){
	//1.获取标签
	var slider = document.getElementById('slider');
	var slider_main = document.getElementById('slider_main');
	var allBoxs = slider_main.children;
	var next = document.getElementById('next');
	var prev = document.getElementById('prev');
	var slider_index = document.getElementById('slider_index');
	var iNow = 0;//当前可视元素的索引
	var timer =null;
	
	//2.动态创建索引器
	for(var i=0;i < allBoxs.length;i++){
		var span = document.createElement('span');
		if(i === 0){
			span.className = 'slider_index_icon current';	
		}else{
			span.className = 'slider_index_icon';	
		}
		span.innerText = i + 1;
		slider_index.appendChild(span);
	}
	//3.让滚动的元素归位
	var scroll_w = parseInt(getStyle(slider,'width'));
	// console.log(scroll_w);
	for(var j=1;j < allBoxs.length;j++){
		allBoxs[j].style.left = scroll_w +'px';
	}
	//4.监听下一张按钮的事件
	next.onclick = function(){
		// 1.让当前可视元素动画左移
		// 2.让下一张图片快速的显示到右边
		// 3.让这个元素动画进入
		startAnimation(allBoxs[iNow],{
			"left":-scroll_w
		})
		//让iNow更新
		iNow ++;
		if(iNow >= allBoxs.length){
			iNow = 0;
		}
		allBoxs[iNow].style.left = scroll_w +'px';
		startAnimation(allBoxs[iNow],{
			"left":0
		})
		//改变索引
		changeIndex();
	}
	//5.监听上一张按钮的事件
	prev.onclick = function(){
		// 1.让当前元素快速右移
		// 2.让上一个元素快速到左边
		// 3.让这个元素动画进入
		startAnimation(allBoxs[iNow],{
			"left":scroll_w
		})
		iNow--;
		if(iNow < 0){
			iNow =allBoxs.length-1;
		}
		allBoxs[iNow].style.left = -scroll_w + 'px';
		startAnimation(allBoxs[iNow],{
			"left":0
		})
		//改变索引器
		changeIndex();
	}


	var slider_index_icons = slider_index.children;
	//6.遍历索引器，添加监听事件
	for(var i=0;i<slider_index_icons.length;i++){
		slider_index_icons[i].onmousedown = function(){
			//6.1获取当前点击的索引
			var index = parseInt(this.innerText) - 1;
			//点击的索引与当前的索引iNow并与之对比
			if(index > iNow){
				//下一个操作
				startAnimation(allBoxs[iNow],{
					'left':-scroll_w
				})
				allBoxs[index].style.left = scroll_w + 'px';
			}else if(index < iNow){
				//上一个操作
				startAnimation(allBoxs[iNow],{
					'left':scroll_w
				})
				allBoxs[index].style.left = -scroll_w + 'px';
			}
			iNow = index;
			startAnimation(allBoxs[iNow],{
				"left":0
			})
			changeIndex();
		}
	}

	//7自动轮播图
	timer = setInterval(autoPlay,1000);
	slider.onmouseover = function(){
		clearInterval(timer);
	}
	slider.onmouseout = function(){
		timer = setInterval(autoPlay,1000);
	}

	function autoPlay(){
		// 1.让当前可视元素动画左移
		// 2.让下一张图片快速的显示到右边
		// 3.让这个元素动画进入
		startAnimation(allBoxs[iNow],{
			"left":-scroll_w
		})
		//让iNow更新
		iNow ++;
		if(iNow >= allBoxs.length){
			iNow = 0;
		}
		allBoxs[iNow].style.left = scroll_w +'px';
		startAnimation(allBoxs[iNow],{
			"left":0
		})
		//改变索引
		changeIndex();
	}

	//控制当前的索引
	function changeIndex(){
		for(var i=0;i<slider_index_icons.length;i++){
			slider_index_icons[i].className = 'slider_index_icon';
		}
		slider_index_icons[iNow].className = 'slider_index_icon current';
	}	
}
```

