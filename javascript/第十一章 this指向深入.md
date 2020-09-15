# 第十一章 this指向深入

## this的绑定规则- 默认绑定

this默认指向了window

1.全局环境下的this指向了window

```javascript
console.log(this);
```

2.函数独立调用,函数内部的this也指向了window

```javascript
function fn(){
	console.log(this);
}
fn();
```

3.被嵌套的函数独立调用时,this默认指向了window

```javascript
var a = 0;
var obj = {
    a:2,
    foo:function (){
        //函数当做对象的方法来调用 this指向了obj
        var that = this;
        function test(){
        	console.log(that.a);//2，test()函数内部指向window对象，obj.foo()指向obj对象，将obj对象赋值给变量that
    	}
    	test();
    }
}
obj.foo(); 
```

4.IIFE 自执行函数中内部的this指向了window

```javascript
var a = 10;
function foo(){
    console.log(this);//{a: 2, foo: ƒ}
    (function test(that){
    	console.log(that.a);//2
    })(this);
}
var obj = {
    a:2,
    foo:foo
}
obj.foo();

(function (){
	console.log('自执行函数中内部的this:'+ this);////自执行函数中内部的this:[object Window]
})() 
```

5.闭包中this默认指向了window

```javascript
var a = 0;
var obj = {
    a:2,
    foo:function(){
    	var _this = this;
    	return function test(){
    		return _this.a;
    	}
    }
}
var fn = obj.foo();
console.log(fn());//2
```

## 隐式绑定

当函数当做方法来调用,this指向了直接对象

```javascript
function foo(){
	console.log(this.a);//1
}
var obj = {
    a:1,
    foo:foo,
    obj2:{
        a: 2,
        foo:foo
    }
}
// foo()函数的直接对象是obj,this的指向指向了直接对象
obj.foo();
// foo()函数的直接对象是obj2,this的指向指向了直接对象
obj.obj2.foo();//2
```

## 隐式丢失

隐式丢失就是指被隐式绑定的函数丢失了绑定对象 从而默认绑定到window

 1.隐式丢失 函数别名

```javascript
var a = 0;
function foo(){
	console.log(this.a);
}
var obj = {
    a: 1,
    foo:foo
}
// 把obj.foo()赋值给别名bar,造成隐式丢失的情况.因为只是把obj.foo()赋值了bar变量.而bar与obj对象毫无关系
//第一种写法
var bar = obj.foo();
bar(); 
//第二种写法
var a = 0;
var bar = function foo(){
	console.log(this.a);
}
bar(); 
```

2.参数传递

```javascript
//第一种写法
var a = 0;
fn = function foo(){
	console.log(this.a);
}
function bar(fn){
	fn();
}
var obj = {
    a: 1,
    foo:foo
}
// 把obj.foo当做参数传递到bar函数中,有隐式的函数赋值 fn = obj.foo,只是把foo函数赋值给了fn,而fn与obj对象毫无关系,所以当前foo函数内部的this指向了window
bar(obj.foo) 

//第二种写法
var a = 0;
function bar(fn){
	fn();
}
bar(function foo(){
    // 内部的this指向了window
    console.log(this.a);
}) 
```

3.内置函数 setTimeout()和setInterval()第一个参数的回调函数中的this默认指向了window,跟第二种情况是类似

```javascript
var a = 10;
function foo(){
	console.log(this.a);
}
var obj = {
    a: 1,
    foo:foo
}
setTimeout(obj.foo,1000);
```

4.间接调用

```javascript
function foo(){
	console.log(this.a);
}
var a = 2;
var obj = {
    a: 3,
    foo:foo
}
var p = {a: 4};
```

隐式绑定,函数当做对象中的方法来使用,内部的this指向了该对象     

```javascript
obj.foo();//3
```

将obj.foo函数对象赋值给p.foo函数，然后立即执行。相当于仅仅是foo()函数的立即调用,内部的this默认指向了window (p.foo = obj.foo)();

将obj.foo赋值给p.foo函数,之后p.foo()函数再执行,其实是属于p对象的方法的指向,this指向了当前的p对象

```javascript
p.foo = obj.foo;
p.foo();//4
```

5.其它情况 指向了window的特殊情况

```javascript
var a = 0;
var obj = {
    a: 1,
    foo:foo
}
function foo(){
	console.log(this.a);
}
(obj.foo = obj.foo)();//0
(false || obj.foo)();//0
(1,obj.foo)();//0
```

## 显示绑定

1.显示绑定:call() apply() bind()把对象绑定到this上

```javascript
var a = 0;
function foo(){
	console.log(this.a);
}
var obj = {
	a: 2
}
foo();//0
foo.call(obj);//2
foo.apply(obj);
var fn = foo.bind(obj)
fn(); 
```

[^注意：bind()返回的是一个函数对象]: 

2.硬绑定是显示绑定的一个变种,使得this不能再被改变

```javascript
var a = 0;
function foo(){
	console.log(this.a);
}
var obj = {
	a: 2
}
var bar = function (){
	foo.call(obj);
}
bar();
setTimeout(bar,2000);
bar.call(window); //指向obj
```

3.数组的forEach(fn,对象) map() filter() some() every()

```javascript
var id = 'window';
function fn(el){
	console.log(el,this.id);//"window"
}
var obj = {
	id: 'fn'
}

var arr = [1,2,3];
// arr.forEach(fn);
arr.forEach(function(el,index){
	console.log(el,index,this);////1 0 {id: "fn"}    2 1 {id: "fn"}   3 2 {id: "fn"}
},obj);
```

## new绑定

1.如果是new关键来执行函数 相当于构造函数来实例化对象,那么内部的this指向了当前实例化的对象

```javascript
function fn(){
    console.log(this);//fn2 {}
    return;
}
var fn = new fn();
console.log(fn);
```

```javascript
function fn2(){
    // this还是指向了当前的对象
    console.log(this);
    // 使用return关键来返回对象的时候，实例化出来的对象是当前的返回对象 
    return {
    	name:'mjj'
    } 
    return this;
}
var fn2 = new fn2();
console.log(fn2);//{name: "mjj"}

var person = {
    fav: function (){
    	return this;
    }
}
var p = new person.fav();
console.log(p,p === person);//fav {}, false
```

## 严格模式下this的指向

1.严格模式下，独立调用的函数内部的this指向了undefined

```javascript
function fn(){
    'use strict';
    console.log(this);
}
fn();
```

2.严格模式下，函数apply()和call()内部的this始终是它们的第一个参数

```javascript
var color = 'red';
function showColor(){
    'use strict';
    console.log(this);//undefined
    console.log(this.color);//Cannot read property 'color' of undefined
}
showColor.call(undefined);
```

## this总结

## 总结：

​     1.默认绑定

​     2.隐式绑定

​     3.显式绑定 

​     4.new绑定

​     分别对应函数的四种调用：

​     \- 独立调用

​     \- 方法调用

​     \- 间接调用

​      call() apply() bind()

​     \- 构造函数调用

## 隐式丢失：

​      1.函数别名

​      2.函数当做参数传递

​      3.内置函数

​      4.间接调用