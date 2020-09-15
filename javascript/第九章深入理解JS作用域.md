# 第九章深入理解JS作用域

## 作用域内部原理的过程介绍

全局作用域，函数作用域

分为五个阶段：内部原理分成- 编译,执行,查询,嵌套,异常

编译阶段 ：边解释边执行

```javascript
var a = 2;
console.log(a);//2
function add(){
    var b = 3;
    console.log(a);
}
console.log(b);// b is not defined
```

## 编译阶段

1.1 分词

词法单元: var, a, =, 2,;

```javascript
{
    "var":"keyword",//关键字
    "a" : "indentifier",//标识符
    "=" : "assignment",//分配
    "2" :"interger",//整数
    ";" :'eos',//(end of statement)//结束语句
}
```

1.2 解析

抽象语法树(AST Abstract Snatax Tree)

1.3 代码生成

将AST准换成可执行的代码的过程,转换成一组机器指令

## 执行阶段

简言之,编译过程就是编译器把程序分解成词法单元,将词法单元解析成AST,再把AST转换成机器指令等待执行得过程

```javascript
var a = 2;
console.log(a);
console.log(b);
```

1.引擎运行代码时首先查找当前的作用域,看a变量是否在当前的作用域下,如果是,引擎就会直接使用这个变量;如果否,引擎会继续查找该变量

2.如果找到了变量,就会将2赋值给当前的变量,否则引擎就会抛出异常

## 查询

查询可分为：LHS查询 和RHS查询

[^有等号赋值的称为叫LHS查询,反之叫RHS查询]: 

```javascript
function foo(a){
	console.log(a);
}
foo(2); 
```

查询过程如下：

1.foo()对foo函数对象进行RHS引用

2.函数传参a=2对a进行了LHS引用

3.console.log(a);对console对象进行RHS引用,并检查其是否有log()方法

4.console.log(a);对a进行RHS引用,并把得到的值传给了console.log(..)

[^LHS:Left Hand Side; RHS:Right Hand Side]: 

## 作用域变量的查找机制(important)

在当前作用域中无法找到某个变量时,引擎就会在外层嵌套的作用域中继续查找,直到找到该变量,或者是抵达最外层的作用域(全局作用域)为止

```javascript
function foo(a){
    function fo(){
    	console.log(a + b);
    }
	fo();
}
var b = 2;
foo(4);
```

## 异常

```javascript
// RHS
function fn(a){
	a = b;//b is not defined
}
fn(2); 

function fn2(){
    var b = 0;
    b();//b is not a function
}
fn2(); 

function fn(){
    'use strict';
    a = 1;
}
fn();//a is not defined
console.log(window);
console.log(a);
```

```javascript
function fn(a){
    // console内置对象
    console.log(a);
}
fn(2);
```

[^作用域内部原理过程]: 

## 词法作用域

```javascript
function foo(a){
    var b = a * 2;
    function bar(c){
    	console.log(a,b,c);// 2 4 12
	}
	bar( b * 3);
}
foo(2);
```

## 遮蔽效应

作用域查找从运行时所处的最内部作用域开始,逐级向上进行,直到遇到第一个匹配的标识符为止

在多层的嵌套作用域可以定义同名的标识符,这叫做 "遮蔽效应"

```javascript
var a = 0;
function test(){
    var a = 1;
    console.log(a);
}
test();
```

## 变量的声明提升

声明从他们在代码中出现的位置被移动到最上面,这个过程叫做变量提升,预解释

```javascript
// 预解释
a = 2;
var a;
console.log(a);
var a;
console.log(a);
a = 2; 
// 声明从他们在代码中出现的位置被移动到最上面,这个过程叫做变量提升,预解释
var a;
console.log(a);//undefined
a = 0;
function fn(){
	var b;
	console.log(b);//undefined
	b = 1;
	function test(){
		var c;
		console.log(c);//undefined
		c = 2;
	}
	test();
}
fn();
```

## 函数的声明提升

函数调用在函数声明前，也可以调用到函数

[^函数的声明可以提升，但是函数表达式不能提升]: 

```javascript
var foo;
foo();
foo = function bar(){
	console.log(1);
}
```

## 声明时的注意事项

1.声明提升: 变量声明提升和函数声明提升 变量的声明优先于函数的声明.但是 函数的声明会覆盖未定义的同名的变量

```javascript
var a;
function a(){};
console.log(a);
			
			
var a;
function a(){};
a = 10;
console.log(a); 
```

2.变量的重复声明是无用的,但是函数的重复声明会覆盖前面的声明(无论是变量还是函数声明)

```javascript
var a;
var a;
a = 1;
a = 10;
console.log(a);
```

3.函数声明提升优先级高于变量的声明提升

```javascript
var a;
function a(){
	console.log('hello wolrd');
}
a();
```

4.后面的函数声明会覆盖前面的函数声明

```javascript
fn();
function fn(){
	console.log('fn');
}
function fn(){
	console.log('fn2');
}
```

[^应该避免在同一作用域中重复声明]: 

## 作用域链

 bar => fn => 全局

查找机制:在当前作用域中发现没有该变量,然后沿着作用域链往上级查找,直到查到对应的变量为止,如果查找不到,直接报错

自由变量:在当前作用域中存在但未在当前作用域中声明的变量

一旦出现自由变量,就肯定会有作用域链,再根据作用域链查找机制,查找到对应的变量

```javascript
var a = 1;
var b = 2;
// fn=>全局
function fn(x) {
    var a = 10;
    // bar => fn => 全局
	function bar(x) {
        // 自由变量:在当前作用域中存在但未在当前作用域中声明的变量
        // 一旦出现自由变量,就肯定会有作用域链,再根据作用域链查找机制,查找到对应的变量
        // 查找机制:在当前作用域中发现没有该变量,然后沿着作用域链往上级查找,直到查到对应的变量为止,如果查找不到,直接报错
        var a = 100;
        b = x + a;
        return b;
	}
    bar(20);
    bar(200);
}
fn(0);
```

## 执行上下文环境和执行流

每个执行环境都有一个与之关联的变量对象,环境中定义的函数和变量都保存在这个对象

```javascript
var a = 1;
var b = 2;
function fn(x) {
    // arguments
    // this
    var a = 10;
    function bar(x) {
        var a = 100;
        b = x + a;
        return b;
	}
    bar(20);
    bar(200);
}
fn(0);
```



## 执行环境栈

执行环境栈:其实就是一个出栈和压栈的过程

## 总结

执行环境 相当于作用域链一样

总结：

1.在js中，除了全局作用域，每个函数都会创建自己的作用域。

2.作用域在函数定义的时候已经确定了，与函数调用无关。

3.通过作用域，可以查找作用域范围内的变量和函数有哪些，却不知道变量的值是什么。所以作用域是静态

4.对于函数来说，执行环境在函数调用时确定的。执行环境包含作用域内的所有的变量和函数的值。在同一个作用域下，不同的调用会产生不同的执行环境，从而产生不同的变量和值。所以执行环境是动态.