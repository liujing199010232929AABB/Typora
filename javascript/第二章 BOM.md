# 第二章BOM

## BOM对象介绍

BOM:浏览器对象模型：

| window(全局作用域)对象 | location对象 | screen 屏幕对象 | history历史模式对象 |
| ---------------------- | ------------ | --------------- | ------------------- |
| alert();               | href         |                 | go()                |
| confirm();             | Hash         |                 |                     |
| prompt();              | URL          |                 |                     |
| setInterval();         | reload()     |                 |                     |
| setTimeout();          |              |                 |                     |



## window对象的alert|confirm|prompt方法

```javascript
 window.alert('myname');
 var a = confirm('你确定离开网站吗？');
 console.log(a);
 var str = prompt('请输入你的内容？','liujing');//liujing是默认值
 console.log(str);
```

## window对象的定时器方法

1. setInterval();延迟性的操作

2. setTimeout();周期性循环操作

3. clearInterval();清除定时器

   ```javascript
   // setInterval();
   // setTimeout();
   //延迟性的操作  等待操作
   window.setTimeout(function(){
   	console.log('liujing');
   },2000)//即使延迟时间为0;但是也是队列
   
   //周期性循环的语句
   var num = 0;
   var timer = null;
   window.setInterval(function(){
       num++;
       if(num > 5){
           clearInterval(timer);
           return;//直接跳出循环
   	}
   	console.log('num:'+ num);
   },1000)
   // clearInterval()清除定时器
   ```

## location对象的常用属性介绍

```javascript
//  http://localhost:63342/WWW/BOM/index.html?user=111&pwd=1222
console.log(location.host);//localhost:63342
console.log(location.hostname);//localhost
console.log(location.href);//http://localhost:63342/WWW/BOM/index.html?_ijt=3l2tefda44uql91v587ner77ji
console.log(location.pathname);///WWW/BOM/index.html
console.log(location.port);//63342
console.log(location.protocol);//http: https:加密的
console.log(location.search);//?user=111&pwd=1222

// 位置操作
//2秒之后跳转猿圈 https://www.apeland.cn/web
setTimeout(function(){
    // location.href='https://www.apeland.cn/web'; //有历史记录
    // location.replace('https://www.apeland.cn/web');//没有历史记录
    location.reload();
},2000)
```

[^注意：一定需要通过站点去访问页面]: 

## navigator对象：

如何检测当前浏览器上的插件(navigator对象):

```javascript
console.log(navigator);
console.log(navigator.plugins);
function hasPlugins(name){
	//如果有插件 返回true 反之亦然
	name = name.toLowerCase();
	for(var i=0;i<navigator.plugins.length;i++){
    	if(navigator.plugins[i].name.toLowerCase().indexOf(name)>-1){
            //有此插件
            return true;
        }else{
        	return false;
        }
	}
}
// alert(hasPlugins('Flash'));
alert(hasPlugins('Chrome PDF Plugin'));
```

## history对象:

```javascript
// 历史记录
// console.log(history);
var count = 0;
var a = setTimeout(function(){
	count++;
    console.log(count);
    history.go(0);//原网页刷新  1:代表前进按钮按一次；-1：代表后退按钮后退1次      
},2000)
console.log(a);
```

[^注意：go(2)代表网页前进2次；go(-2)代表网页后退2次]: 

