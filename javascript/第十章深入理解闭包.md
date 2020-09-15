# 第十章深入理解闭包

## 理解闭包

作用域:全局作用域和函数作用域

闭包就是fn2,既能够读取其它函数内部变量的函数

[^定义在一个函数内部的函数]: 

闭包最大的特点:就是它可以记住诞生的环境,比如fn2记住了它诞生的环境是fn1,所以在fn2中可以得到fn1中的内部变量

[^本质上：闭包就是函数内部和函数外部链接的一座桥梁。]: 

```javascript
var a = 123;
function fn1(){
    console.log(a);
    var b = 234;
    function fn2(){
    	console.log(b);
	}
	return fn2;
}
var result = fn1();
result();//b is not defined
```

## 闭包的用途

1.通过闭包制作计数器

[^作用:读取函数内部的变量,这些变量始终在内存中,使用闭包小心内存的泄露]: 

```javascript
function a(){
    var start = 0;
    function b(){
    	return start++;
	}
	return b;
}
var inc = a();
console.log(inc());
console.log(inc());
console.log(inc());
// 释放当前的变量
inc = null;
```

2.闭包能够封装对象的私有属性和方法

```javascript
function Person(name){
    //私有的属性
    var age;
    //私有的方法
    function setAge(n){
    	age = n;
    }
    function getAge(){
    	return age;
    }
    return {
        name:name,
        setAge:setAge,
        getAge:getAge
    }
}
var p1 = Person('mjj');
p1.setAge(18);
console.log(p1.getAge());
p1 = null;
```

## 闭包的注意点

1.使用闭包使得函数中的变量始终在内存中,内存消耗很大,所以不能滥用闭包.否则会造成页面的性能问题.在ie中可能导致内存泄露.

2.变量用完不会被销毁，函数用完会销毁，私有属性会被存储起来

3.每个父函数调用完成,都会形成新的闭包,父函数中的变量始终会在内存中,相当于缓存,小心内存的消耗问题

4.闭包需要三个条件：1.函数嵌套 ;2.访问所在的作用域 ;3.在所在作用域外被调用

## 立即执行函数(IIFE)--Imdiately Invoked Function Expression

IIFE:  ()是表达式 跟在函数后面 表示调用函数 fn()

立即执行函数:定义函数之后,立即调用该函数，这种函数叫做立即执行函数

[^注意: 如果function出现在行首 一律解释成函数声明语句]: 

简称：自执行函数

```javascript
//1.常用的两种写法
(function(){

})()
!(function(){

}());
//2.通常情况下,写自执行函数的时候
!(function(){})();
```

## 自执行函数的应用

计数器

```javascript
// 1.全局变量,会污染这个全局的变量
	var a = 0;
    function add(){
    	return ++a;
    }
    console.log(add());
    console.log(add());
    console.log(add());
    console.log(add());
    console.log(add());
// 2.自定义属性 有些代码可能会无意修改add.count
    function add(){
    	return ++add.count;
    }
    add.count = 0;
    console.log(add());
    console.log(add());
    console.log(add());
    console.log(add()); 
			
// 立即执行函数也叫闭包,可以封装私有的属性,同时可以减少对全局变量的污染
   var add = (function (){
   		// 私有属性
       var count = 0;
       return function (){
       		return ++count;
   	   }
   })();

    var add2 = (function(){
        var count = 1;
    })();
    console.log(add);
    console.log(add());
    console.log(add());
    console.log(add());
    console.log(add());
    console.log(add());
    console.log(add());
```

## 对循环和闭包的理解错误

```javascript
function foo(){
    var arr = [];
    for(var i = 0; i < 10; i++){
        arr[i] = function(){
            return i;
        }
    }
    return arr;
}
var bar = foo();
console.log(bar[0]());//10
console.log(bar[5]());//10 
```

解决方案:

1.使用闭包解决循环中变量的问题,相当于把变量保存在内存中,,每次执行的时候从内存中获取

```javascript
function foo(){
    var arr = [];
    for(var i = 0; i < 10; i++){
        arr[i] = (function(n){//第一种写法
        	return function(){
        		return n;
        	}
		})(i); 

        (function(n){//第二种写法
            arr[n] = function(){
            	return  n;
            }
        })(i);
	}
	return arr;
} 

var bar = foo();
console.log(bar);
console.log(bar[0]());
console.log(bar[1]());
console.log(bar[2]());
console.log(bar[3]());
console.log(bar[4]()); 
```

2.解决方法 let块级作用域 ES6

```javascript
function foo(){
    var arr = [];
    for(let i = 0; i < 10; i++){
    	arr[i] = function(){
    		return i;
        }
    }
	return arr;
}
var bar = foo();
console.log(bar[0]());
console.log(bar[1]());
console.log(bar[2]());
console.log(bar[3]());
```

[^MJJ名言：在编程,如果实际和预期结果不符,就按照代码执行顺序一步一步地把执行环境图示画出来,会发现很多时候我们都是在想当然]: 

## 闭包的10种表示形式

1.返回值 最常见的一种形式

```javascript
var fn = function(){
    var name = 'mjj';
    return function(){
    	return name;
	}
}
var fnc = fn();
console.log(fnc());
```

2.函数赋值 一种变形的形式是将内部函数赋值给一个外部的变量

```javascript
var fn2;
var fn = function(){
    var name = 'mjj';
    var a = function (){
    	return name;
    }
    fn2 = a;
}
fn();
console.log(fn2()); 
```

3.函数参数

```javascript
function fn2(f) {
	console.log(f());
}

function fn() {
    var name = 'mjj';
    var a = function() {
    	return name;
    }
    fn2(a);
}
fn();
```

4.IIFE

```javascript
function fn3(f) {
	console.log(f());
}

(function() {
    var name = 'alex';
    var a = function() {
    	return name;
    }
    fn3(a);
})(); 
```

5.循环赋值

```javascript
function foo() {
	var arr = [];
	for (var i = 0; i < 10; i++) {
        //第一种写法：
     	(function(i){
			arr[i] = function(n){
 				return n;
 			}
 		})(i);
        
        //第二种写法：
        arr[i] = (function(n) {
            return function() {
                return n;
            }
        })(i);
	}
	return arr;
}
var bar = foo();
console.log(bar[3]());
```

6.getter和setter函数来将要操作得变量保存在函数内部,防止暴露在外部

```javascript
	  var getValue, setValue;
	 (function() {
    	var num = 0;
        getValue = function() {
        	return num;
        }
        setValue = function(v) {
        	if (typeof v === 'number') {
            	num = v;
             }
		  }
		})();
        console.log(getValue());
        setValue(10);
        console.log(getValue()); 
```

7.迭代器(计数器)

```javascript
var add = function() {
	var num = 0;
	return function() {
		return ++num;
	}
}();
console.log(add());
console.log(add()); 

//['alex','mjj','阿黄']
function setUp(arr) {
	var i = 0;
	return function() {
		return arr[i++];
	}
}
var next = setUp(['alex', 'mjj', '阿黄']);
console.log(next());
console.log(next());
console.log(next()); 
```

8.区分首次

```javascript
var firstLoad = (function() {
	var list = [];
	return function(id) {
		if (list.indexOf(id) >= 0) {
        	//list已经有id
        	return false;
        } else {
        	// 首次调用
        	list.push(id);
        	return true
        }
    }
})();
console.log(firstLoad(10));
console.log(firstLoad(10));
console.log(firstLoad(30));
console.log(firstLoad(40));
console.log(firstLoad(40)); 
```

9.缓存机制

9.1 未加入缓存

```javascript
function mult(){
	// arguments
	var sum = 0;
	for(var i = 0; i < arguments.length; i++){
		sum = sum + arguments[i];
	}
	return sum;
}
			
console.log(mult(1,2,3,1,1,2,3)); //求和
console.log(mult(1,2,3,1,1,2,3)); //求和 
console.log(mult(1,2,3,1,1,2,3,4)); //求和 
```

 9.2 有缓存机制

```javascript
// 模拟一个对象的key 看该对象中是否有相同的key，如果有则直接获取value返回
{
    key:value
    1,2,3,1,1,2,3: 18,
    1,2,3,1,1,2,3,4: 22
} 
var mult = function(){
    // 缓存对象
    var cache = {};
    var calculate = function (){
		var sum = 0;
		for(var i = 0; i < arguments.length; i++){
			sum = sum + i;
		}
		return sum;
	}
	return function (){
		// 对cache对象进行操作
		// 1,2,3,1,1,2,3
		var args = Array.prototype.join.call(arguments,',');
        if(args in cache){
        	return cache[args];
        }
        console.log(cache);
        return cache[args] = calculate.apply(null,arguments);
    }
}();
console.log(mult(1,2,3,1,1,2,3));
console.log(mult(1,2,3,1,1,2,3));
console.log(mult(1,2,3,1,1,2,3,10,20));
console.log(mult(1,2,3,1,1,2,3,10,20,1)); 
```

10.img图片对象上报

new Image() 进行数据上报

低版本浏览器在进行数据上报会丢失30%左右的数据

```javascript
// 使用闭包来做图片上传
var report = function (src){
    var imgs = [];
    return function (src){
        var img = new Image();
        imgs.push(img);
        img.src = src;
    }
}();
report('http://xxx.com/getUserInfo');
```

