# 第八章深入理解函数

## 函数声明的三种方式

1.函数的声明语句

```javascript
function fn(a,b,c){
    console.log(a);
    console.log(b);
    console.log(c);
}
fn(2,3);
```

2.函数的表达式:将一个匿名函数赋值给一个新的变量

```javascript
var hello = function(x,y){
	return x+y;
}
console.log(hello(3,5));
```

```javascript
var hello = function hel(x,y){
    console.log(hel === hello);
    return x+y;
}
console.log(x);
hello(3,4); 
console.log(hello(3,5));
console.log(hel(3,5));
```

[^个人理解: hello是变量名称,相当于函数的实参,既可以在函数内部也可以在函数外部使用]: 
[^hel 是函数名称,相当于函数的形参,只能在函数的内部使用]: 

3.Function 构造函数

```javascript
var fn = new Function('x', 'y', 'return x + y;');
console.log(fn(3, 7));
function fn(x,y){
	return x+y;
}
```

## 深入理解函数返回值

应用在函数中

```javascript
function fn() {
	return {
		name: "MJJ",
		age: 18
	};
	alert(1);
}
// console.log(fn());//如果没有返回值，调用表达式的结果是undefined
```

并不是所有的函数中的return之后的代码都不执行(捕获现代代码异常)

```javascript
function testFinally(){
    try{
        // var a = 2;
        // var b = 3;
        // console.log(a+b);
        console.log(x);
        return 2;
    }catch(e){
        alert(e);
        //TODO handle the exception
    	return 1;
    }finally{
    	return 0;
    }
}
console.log(testFinally()); 
```

[^注意：不管调用成功与否，都会执行finally{return 0;}代码]: 

一个函数可以有多个return语句

```javascript
function compare(x,y){
    if(x > y){
    	return x - y;
    }else{
   	 	return y - x;
    }
} 
```

如果函数调用时在前面加上new前缀,且返回值不是一个对象或者没有返回值,则返回该对象 (this)

```javascript
function fn(){
    console.log(this);
    this.a = 2;
    return 1;
}
var test = new fn();
console.log(test);//类型 obj

// 如果返回值是一个对象,则返回该对象
function fn(){
    this.a = 2;
    return {a:1};
}
var test = new fn(); 
console.log(test.constructor);
```

## 函数调用模式和方法调用模式

函数调用4种方式: 函数调用模式,方法调用模式,构造调用模式,间接调用模式

1.函数调用模式

```javascript
function add(x,y){
	'use strict';
	// 在严格模式下,当前函数中this指向了undefined
	console.log(this);//在非严格模式，window对象
	return x + y;
}

var sum = add(3,4);
console.log(sum); 

// 重写
function fn(){
    this.a = 1;
    console.log(this);
}
fn();
console.log(this);
this.a = 5;
console.log(this); 
```

2.方法调用模式

[^注意:小心避免全局的属性重写带来的问题]: 

```javascript
var obj = {
     a:1,
     // 这个fn称为obj对象的方法
     fn:function(){
         // console.log(this);
         // console.log('1');
     	this.fn2();
        console.log(this);
     },
     fn2:function(){
    	this.a = 2;
        console.log(this);
 	}
 }
 obj.fn();
 // obj.fn2();
 console.log(obj.a); 
```

3.构造函数调用模式

```javascript
function fn(x,y){
 	this.a = x + y;
 }
 // this指向问题: 当做普通函数调用,this指向了window,当做构造函数调用,this指向当前的函数,当做对象的方法,这个this一般情况指向了当前的对象
 var obj = new fn(2,3);
 console.log(obj); 
 var obj = {a:5};
 function fn(){
     console.log(this);
     this.a = 10;
     return obj;
 }
 var a = new fn;//等价于var a = new fn();
 console.log(a); 
```

[^this指向问题: 当做普通函数调用,this指向了window,当做构造函数调用,this指向当前的函数,当做对象的方法,这个this一般指向了当前的对象]: 

4.间接调用模式call({},1,2,3) apply({},[1,2,3])

```javascript
var obj = {
    a: 10
};
function sum(x,y){
    console.log(this);
    return x + y + this.a;
}
console.log(sum.call(obj,1,2));//call可以传一个对象  改变函数中this的问题
console.log(sum.apply(obj,[1,2]));
```

[^call可以传一个对象  改变函数中this的问题]: 

## 函数参数

函数不介意传递进来多少个参数,也不在乎传进来的参数是什么数据类型,甚至可以不传参数

```javascript
function add(x){
	return x + 1;
}
console.log(add(1));//2
console.log(add('1'));//string类型的11
console.log(add());//NaN
console.log(add(1,2,3));//2

// 同名形参
function add(x,x,x){
    'use strict';//严格定义函数
    return x;
}
console.log(add(1,2,3));

// 参数个数
// 实参比形参个数少,剩下的形参都将设置为undefined
function add(x,y){
	console.log(x,y);
}
add(1,2,3,4);
```

实参比形参个数多,考虑arguments

```javascript
function add(x,y){
    console.log(arguments);
    // arguments它不是真正的数组,它是类数组,它能通过[]来访问它的每一个元素
    console.log(arguments[0],arguments[1],arguments[2]);
    console.log('实参的个数：'+ arguments.length);
}
add(1,2,3);
```

形参的个数

```javascript
console.dir(add.length);//console.dir 打开目录
```

## 函数重载现象

重载 :定义相同的函数名,传入的不同参数,实现不同的功能

[^在js中函数不存在重载现象  后面会覆盖前面的]: 

```javascript
function add(a){
	return a + 100;
}
function add(a,b){
 	return a + b + 100;
}
 alert(add(10));
 // alert(add(10,20));
```

模拟重载现象

```javascript
function doAdd() {
第一种：if...else...写法
    if(arguments.length == 0){
    	return 100;
    }else if(arguments.length == 1){
    	return arguments[0] + 100;
    }else if(arguments.length == 2){
    	return arguments[0] + arguments[1] + 100;
    }
第二种：switch写法
    switch (arguments.length) {
        case 0:
            return 100;
            break;
        case 1:
            return arguments[0] + 100;;
            break;
    	case 2:
            return arguments[0] + arguments[1] + 100;;
            break;
        default:
        	break;
    }
}
alert(doAdd());
alert(doAdd(10));
alert(doAdd(10, 20));
```

## 函数参数传递

1.基本数据类型

在向参数传递基本数据类型的值时,被传递的值会被复制到一个局部变量

```javascript
function add(num){
    num = num + 10;
    return num;
}
var count = 20;
var result = add(count);
console.log(result);
console.log(count); 
```

2.引用数据类型

在向参数传递引用数据类型的值时，会把这个值在内存中的地址复制给局部变量

```javascript
function setName(obj){
	obj.name = 'mjj';
}
var person = new Object();
setName(person);
console.log(person.name); 
```

```javascript
function setName(obj){
    obj.name = 'mjj';
    console.log(person.name);
    obj = new Object();
    obj.name = 'test';
    console.log(person.name);//mjj
}
var person = new Object();
setName(person);
```

## 函数属性

length属性

[^arguments对象中的length属性表示实参,而函数参数length属性表示的是形参的个数]: 

```javascript
function add(x,y){
    console.log(arguments.length);//实参个数
    console.log(add.length);//形参个数
}
add(2,3,4,5); 
```

name指的是当前函数的名字

```javascript
function fn(){};
console.dir(fn.name);//fn
var fn2 = function (){};
console.log(fn2.name);//fn2
var fn3 = function abc(){};
console.log(fn3.name);//abc 
```

prototype 属性

[^每一个函数都有一个prototype属性]: 

```javascript
function fn(){};
console.log(fn.prototype);//对象
fn.prototype.a = 1;
console.log(fn.prototype);
```

## 函数中apply和callf方法的使用

```javascript
function fn(){};
var obj = new fn();
fn.prototype.a = 1;//fn.prototype是父类，obj是子类，继承方法
console.log(obj); 

// apply() call()
// call({},1,2,3)
// apply({},[])
// 每个函数都包含两个非继承而来的方法
```

```javascript
window.color = 'red';
var obj = {color:'blue'};

function sayColor(){
	console.log(this.color);
}
sayColor();//red 因为当前的this指向了window
sayColor.call(this);//red
sayColor.call(window);//red
sayColor.call(obj);//blue
```

在非严格模式下,如果我们使用call()或者是apply()方法,传入一个null或者undefined会被转换成一个全局window对象

[^在严格模式下,函数的指向始终是指定的值]: 

```javascript
var color = 'red';
function displayColor() {
    'use strict';
    console.log(this);
    console.log(this.color);
}
displayColor.call(undefined);
```

## call和apply方法的应用

 1.找出数组的最大元素 Math.max() apply(null,)

```javascript
var max = Math.max(2,3,4,5,10);
var arr = [22222,1,10,30,88];
var arrMax = Math.max.apply(null,arr);//null或undefined会指向全局对象window
console.log(arrMax);
```

2.将类数组转换成真正的数组

```javascript
function add(){
	console.log(arguments);
	// arguments.push(5);
	// apply() slice()
	var arr = Array.prototype.slice.apply(arguments);//复制数组 浅复制  继承Array.prototype   智能复制元素和	length,其他方法复制不了
	console.log(arr);
}
add(1,2,3);
```

3.数组追加

```javascript
 var arr = [];
 Array.prototype.push.apply(arr,[1,2,3,4]);
 console.log(arr);
 Array.prototype.push.apply(arr,[8,9,0]);
 console.log(arr);
```

4.利用call和apply做继承

动物类 构造函数

```javascript
function Animal(name,age){
    this.name = name;
    this.age = age;
    this.sayAge = function(){
    	console.log(this.age);
	}
}
// Cat
function Cat(name,age){
    // 继承了Animal
    Animal.call(this,name,age);//把this指向了“var c = new Cat('小花',20)”的c实例对象
}
var c = new Cat('小花',20)
c.sayAge();
console.log(c.name);

function Dog(){
	Animal.apply(this,arguments)
}
var d = new Dog('阿黄',18);
console.log(d);
```

5.使用log代理console.log()

```javascript
function log(){
	console.log.apply(console,arguments);
}
log(d);
```

## bind方法的使用

bind es5新增的方法,主要作用:将函数绑定到某个对象中,并且有返回值(一个函数)

```javascript
function fn(y){
	return this.x + y;
}
var obj = {
	x: 1
}
var gn = fn.bind(obj);//返回fn函数
console.log(gn);
console.log(gn(3));//4
```

常见的函数式编程技术- 函数柯里化

```javascript
function getConfig(colors,size,otherOptions){
	console.log(colors,size,otherOptions);
}
var defaultConfig = getConfig.bind(null,'#ff6700',1024*768);
defaultConfig('123');
defaultConfig('456');
```

