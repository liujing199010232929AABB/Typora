# 第十二章面向对象编程

## 本章内容介绍

面向对象编程

1.对象是什么？

2.构造函数

  new

  constructor

3.原型对象

4.创建对象的5中模式

  对象字面量 

  工厂模式

  构造函数模式

  原型模式

  组合模式

5.实现继承的5种方式

6.选项卡（面向对象）

## 对象的定义

面向对象编程(OOP):具有灵活,代码可复用性,高度模块化等特点

[^OOP：object oriented programming]: 

1.对象是单个实物的抽象

2.对象是一个容器,封装了对应属性和方法

属性是对象的状态,方法是对象的行为(完成的任务)

## 构造函数实例化对象

需要一个模板(表示一类实物的共同特征),让对象生成

类 (class) 就是对象的模板

[^js不是基于类的,而是基于构造函数（constructor）和原型链（prototype)]: 

###  特点:

1.函数体内使用this关键字,代表了所要生成的对象实例

 2.生成对象,必须使用new关键字实例化

```javascript
function Dog(name,age){
    'use strict';
    // name和age就是当前实例化对象的属性
    this.name = name;
    this.age = age;
}
var dog1 = new Dog('阿黄',10); 
var d = Dog('阿黄',10);
console.log(d);//undefined
```

[^Dog是构造函数,为了与普通函数区别,构造函数的名字的第一个字母通常都大写]: 

```javascript
function Person(name){
    if(this instanceof Person){
    	// this指向了当前的实例,外部使用了关键字new
    	this.name = name;
    }else{
    	// this指向了window,外部没有使用了关键字new
    	return new Person(name);
    }
}
var p1 = new Person('mjj');
var p2 = Person('mjj');
console.log(p1);
console.log(p2);
```

[^a instanceof A   a是否是A的一个实例]: 

## new命令内部原理

new命令的原理

1.创建一个空对象,作为将要返回的对象实例

2.将这个空的对象原型对象,指向了构造函数的prototype属性对象

3.将这个实例对象的值赋值给函数内部的this关键字

4.执行构造函数体内的代码

```javascript
function Person(name){
	this.name = name;
}
var p1 = new Person('mjj');
// console.log(Person.prototype);
console.log(p1.__proto__ === Person.prototype);
console.log(Person.prototype);

var p1 = function Person(name){
	this.name = name;
};
Person.prototype = p1.__proto__;
```

## constructor属性

每个对象在创建时都会自动拥有一个构造函数属性constructor

```javascript
function Person(name){
	this.name = name;
}
var p1 = new Person('mjj');	
console.log(p1);
// constructor继承自原型对象,指向了构造函数的引用
console.log(p1.constructor === Person);
console.log(Person.prototype);
```

## 使用构造函数创建对象的利与弊

```javascript
<script type="text/javascript">
function Person(name) {
	this.name = name;
    this.sayName = function() {
    	console.log(this.name);
    }
}
var p1 = new Person('Tom');
var p2 = new Person('Jack');
console.log(p1,p2);
</script>
```

![01](D:\Typora\program\javascript\pic\12\01.png)

生成两个Person空间，造成资源浪费

## 原型对象介绍

原型对象:Foo.prototype

实例对象: f1就是实例对象,每一个原型对象中都有一个__proto__,每个实例对象都有一个constructor属性,这个constructor通过继承关系继承来的,它指向了当前的构造函数Foo

构造函数:用来初始化新创建对象的函数,Foo是构造函数,自动给构造函数赋予一个属性prototype,该属性指向了实例对象的原型对象

```javascript
function Foo(){};//Foo是构造函数；自动赋予属性Foo.prototype
Foo.prototype.showName = function(){
    console.log('mjj');
};
var f1 = new Foo();//f1.__proto__(f1的原型对象);f1.constructor
var f2 = new Foo();
console.log(f1.showName());
console.log(f2.showName());
```

## 理清原型对象实例对象构造函数之间的关系![理清原型对象_实例对象_构造函数之间的关系](D:\Typora\program\javascript\pic\12\理清原型对象_实例对象_构造函数之间的关系.png)

```javascript
<script type="text/javascript">
    function Foo(){};
    var f1 = new Foo();
    console.log(Foo.prototype === f1.__proto__)//true
    console.log(f1.__proto__.__proto__)//object实例
    console.log(f1.__proto__.__proto__.__proto__)//null
</script>
```

## prototype属性的作用

js继承机制:通过原型对象实现继承

[^原型对象的作用,就是定义所有实例对象共享的属性和方法]: 

```javascript
function Foo(){};
Foo.prototype.name = 'mjj';
var f1 = new Foo();
var f2 = new Foo();
console.log(f1.name);//mjj
console.log(f2.name);//mjj
Foo.prototype.name = 'alex';
console.log(f1.name);//alex
console.log(f2.name);//alex
```

## 原型链挖掘

js规定,所有的对象都有自己的原型对象

[^原型链:对象的原型=&gt;原型的原型=&gt;原型的原型的原型======null]: 
[^根据原型链查找,如果一层一层往上查找,所有对象的原型最终都可以寻找到Object.prototype,Object构造函数的prototype]: 

所有的对象都继承了Object.prototype上的属性和方法,例如：toString()

### 读取属性和方法的规则:

js引擎会先寻找对象本身的属性和方法,如果找不到.就到它的原型对象去找,如果还是找不到,就到原型的原型去找,如果直到最顶层的Object.prototype还是找不到,就会返回undefined

[^如果对象和它的原型,都定制了同名的属性,那么优先读取对象自身的属性,这也叫覆盖]: 

```javascript
function Person(name){
    this.name = name;
    this.showName = function(){
    	console.log('小猿');
    }
}
Person.prototype.showName = function(){
	console.log(this.name);
}
Object.prototype.showAge = function(){
	console.log('24');
}

var p1 = new Person('mjj');
var p2 = new Person('alex');
p1.showName();
p2.showName();
console.log(p1.age); 
p1.showAge();
var arr = [12,3,4];
arr.showAge()
```

## 修改原型对象后constructor属性的注意点

一旦我们修改构造函数的原型对象,为防止引用出现问题,同时也要修改原型对象的constructor属性

[^constructor属性表示原型对象和构造函数之间的关联关系，Foo.prototype.constructor = Foo]: 

```javascript
function MyArray(){};
MyArray.prototype = Array.prototype;
// console.log(MyArray.prototype.constructor);
MyArray.prototype.constructor = MyArray;
var arr = new MyArray();
console.log(arr);//MyArray{}
arr.push(1,2,3);
console.log(arr);//MyArray(3){1,2,3}
console.log(arr.constructor);//MyArray(){}
console.log(arr.__proto__ === MyArray.prototype);//true
```

## 原型对象构造函数实例对象总结

1.构造函数:Foo

2.实例对象:f1

3.原型对象:Foo.prototype

```javascript
function Foo(){};
var f1 = new Foo();
```

1.原型对象和实例对象的关系

```javascript
console.log(Foo.prototype === f1.__proto__);
```

 2.原型对象和构造函数的关系

```javascript
console.log(Foo.prototype.constructor === Foo);
```

3.实例对象和构造函数

[^间接关系是实例对象可以继承原型对象的constructor属性]: 

```javascript
console.log(f1.constructor === Foo);
Foo.prototype = {};//重置原型对象
console.log(Foo.prototype);//{} constructor: ƒ Object()
console.log(Foo.prototype === f1.__proto__);//false
console.log(Foo.prototype.constructor === Foo);//false
// 所以,代码的顺序很重要
```

## 构造函数创建方式

### 1.对象字面量方式

#### 1.1new构造函数

```javascript
var obj = new Object();
obj.name = '小猿';
console.log(obj); 
```

#### 1.2对象字面量 (语法糖)

```javascript
var person1 = {
    name:"MJJ",
    age:29
}
var person2 = {
    name:"MJJ",
    age:29
}
var person3 = {
    name:"MJJ",
    age:29
}
var person4 = {
    name:"MJJ",
    age:29
}
console.log(person);
console.log(person.name);
```

#### 1.3Object.create(对象)

需求：从一个实例对象生成另一个实例对象

[^create()中的参数a作为返回实例对象b的的原型对象,在a中定义的属性方法,都能被b实例对象继承下来]: 

```javascript
var a = {
    getX:function(){
    	console.log('X');
    }
}
var b = Object.create(a);//a作为b的原型对象
b.getX();
```

### 2.工厂模式

特点：能创建多个类似的对象

缺点：没有解决对象识别的问题,使用构造函数模式解决

```javascript
<script type="text/javascript">	
    function createPerson(name,age){
        var o = new Object();
        o.name = name;
        o.age = age;
        o.sayName = function (){
            console.log(this.name);
        }
        return o;
    }
    var p1 = createPerson('mjj',18);
    var p2 = createPerson('alex',38);
    p1.sayName();
    console.log(p1 instanceof Object);
    console.log(p2 instanceof Object);
</script>
```

### 3.构造函数模式

### 一般构造函数模式

缺点：浪费了内存空间

```javascript
<script type="text/javascript">
    function Person(name,age){
        this.name = name;//this指向实例对象(man/woman)
        this.age = age;
        this.sayName = function(){
        	console.log(this.name);
        }
    }
    var man = new Person('mjj',18);
    var woman = new Person('alex',38);
    // 具有相同的sayName方法在man和woman实例中占用了不同的内存空间
    console.log(man.sayName === woman.sayName);
</script>
```

#### 3.1构造函数拓展模式

缺点：定义多个全局函数,严重污染全局空间

```javascript
function Person(name,age){	
    this.name = name;
    this.age = age;
    this.sayName = sayName;
}
function sayName(){
	console.log(this.name);
}
var man = new Person('mjj',18);
var woman = new Person('alex',38);
console.log(man.sayName === woman.sayName);//false,代表创建了两个sayName函数对象
```

#### 3.2寄生构造函数模式(工厂模式和构造函数模式)

创建一个函数,函数体内部实例化一个对象,并且将对象返回,在外部的使用使用new来实例化对象

缺点：1.定义了相同的方法,浪费内存空间；2.instanceof运算符和prototype属性都没有意义

[^该模式尽量避免使用]: 

```javascript
function Person(name,age){
    var o = new Object();
    o.name = name;
    o.age = age;
    o.sayName = function(){
    	console.log(this.name);
    }
    return o;
}
var man = new Person('mjj',18);
var woman = new Person('alex',38);
console.log(man.sayName === woman.sayName);//false,代表创建了两个sayName函数对象
console.log(man.__proto__ === Person.prototype);//false
console.log(man instanceof Person);//false
```

会造成空间浪费

#### 3.3稳妥构造函数模式

稳妥模式:没有公共属性,并且它的方法也不引用this对象

[^instancof prototype属性都不能适用]: 

```javascript
function Person(name){
    var a = 10;
    var o = new Object();
    // name就属于私有属性 结合闭包 让数据安全
    o.sayName = function(){
        console.log(a);//10
        console.log(name);//mjj
    }
    return o;
}
// p1对象叫稳妥对象
var p1 = Person('mjj');
var p2 = Person('mjj2');
p1.sayName();//false
console.log(p1.sayName === p2.sayName);//没有结果
```

### 4.原型模式

缺点：在于引用类型值属性会被所有的实例对象共享并且修改,很少适用原型模式的原因

```javascript
function Person(){};
```

```javascript
 第一种写法：
 Person.prototype.name = 'mjj';
 Person.prototype.age = 28;
 Person.prototype.sayName = function(){
 		console.log(this.name);
 } 
```

```javascript
 第二种写法：
Person.prototype = {
        constructor:Person,
        name:'mjj',
        age:28,
        friends:['alex','阿黄'],
        sayName:function(){
        	console.log(this.name);
	}
}
```

```javascript
//Person.prototype.constructor = Person;
var me = new Person();
me.sayName();
var you = new Person();
you.sayName();

me.friends.push('小红');
console.log(me.friends);
console.log(you.friends);//其他实例也会修改
//console.log(me.constructor === Person);
```

### 5.组合模式

特点：该模式是目前使用最广泛,认同度最高的一种创建自定义对象的模式

```javascript
function Person(name,age){
    // 定制当前对象自己的属性
    this.name = name;
    this.age = age;
    this.friends = ['alex','阿黄'];
};
// 定制各个实例对象的共享属性
Person.prototype = {
    // 改变原型对象的同时要改变该原型对象的constructor属性,让它指向了当前的构造函数Person
    constructor:Person,
    sayName:function(){
    	console.log(this.name);//21
    }
}
var wo = new Person('wo',18);
var you = new Person('you',28);
console.log(wo,you);//26
wo.friends.push('小红');
console.log(wo.friends);//28
console.log(you.friends);//29
wo.sayName();//21
you.sayName();//21
```

![03](D:\Typora\program\javascript\pic\12\03.png)

### 6.动态原型模式

特点：该模式是目前使用最广泛,认同度最高的一种创建自定义对象的模式

```javascript
function Person(name, age) {
    // 定制当前对象自己的属性
    this.name = name;
    this.age = age;
    this.friends = ['alex', '阿黄'];
    if(typeof this.sayName != 'function'){
    	// 初始化原型对象上的属性
    	Person.prototype.sayName = function(){
     	   	console.log(this.name);
        }
    }
};

console.log(Person.prototype);//object
var wo = new Person('mjj',18);
console.log(Person.prototype);
console.log(wo instanceof Person);
```

## 基于面向过程的选项卡样式实现

css代码：

```css
<style type="text/css">
    *{
        padding:0px;
        margin:0px;
    }
    ul{
        list-style:none;
        overflow: hidden;
        border:1px solid #3081BF;
        width:300px;
        height:40px;
        line-height:40px;
    }
    a{
    	text-decoration:none;
    }
    body{
   	 background:#B49B8B;
    }
    #wrap{
        width:302px;
        height:402px;
        margin:100px auto;
    }
    #wrap ul li{
        float:left;
        width:100px;
        height:100%;
    }
    #wrap ul li.active{
        background-color:#3081BF;
        font-weight:bold;
    }
    #wrap ul li a{
        display:block;
        width:100%;
        height:100%;
        color:#262626;
        font-size:18px;
        text-align:center;
    }
    div.content{
        width:300px;
        height:360px;
        border:1px solid #3081BF;
        display:none;
    }
</style>
```

html代码

```html
<div id="wrap">
    <ul>
        <li class="active">
        	<a href="javascript:void(0);">推荐</a>
        </li>
        <li>
        	<a href="javascript:void(0);">小说</a>
        </li>
        <li>
        	<a href="javascript:void(0);">导航</a>
        </li>
    </ul>
    <div class="content" style="display:block;">推荐</div>
    <div class="content">小说</div>
    <div class="content">导航</div>
</div>
```

javascript代码

一般写法：

```javascript
<script type="text/javascript">
    window.onload = function(){
        //1.获取事件源
        // var tablis = document.getElementsByTagName('li');
        var wrap = document.getElementById('wrap');//事件委托
        var tablis = wrap.children[0].getElementsByTagName('li');
        // var contentDivs = document.getElementsByClassName('content');
        var contentDivs = wrap.getElementsByTagName('div');
        //2.绑定事件
        //一般写法
        for(var i =0;i<tablis.length;i++){
             //绑定i的索引
             tablis[i].index = i;
             tablis[i].onclick = function(){
                 //去掉所有
                 for(var j=0;j<tablis.length;j++){
                     tablis[j].className = '';
                     contentDivs[j].style.display = 'none';
                 }
                 this.className='active';
                 contentDivs[this.index].style.display = 'block';
             }
         }
    }
</script>
```

闭包写法

```javascript
<script type="text/javascript">
    window.onload = function(){
        //1.获取事件源
        // var tablis = document.getElementsByTagName('li');
        var wrap = document.getElementById('wrap');//事件委托
        var tablis = wrap.children[0].getElementsByTagName('li');
        // var contentDivs = document.getElementsByClassName('content');
        var contentDivs = wrap.getElementsByTagName('div');
        //2.绑定事件
        for(var i =0;i<tablis.length;i++){
            tablis[i].onclick = (function(n){
                return  function(){
                    //去掉所有
                    for(var j=0;j<tablis.length;j++){
                        tablis[j].className = '';
                        contentDivs[j].style.display = 'none';
                    }
                    this.className='active';
                    contentDivs[n].style.display = 'block';
                }
            })(i);
        }
    }
</script>
```

封装函数写法

```javascript
<script type="text/javascript">
    window.onload = function(){
        //1.获取事件源
        // var tablis = document.getElementsByTagName('li');
        var wrap = document.getElementById('wrap');//事件委托
        var tablis = wrap.children[0].getElementsByTagName('li');
        // var contentDivs = document.getElementsByClassName('content');
        var contentDivs = wrap.getElementsByTagName('div');
        //2.绑定事件
        for(var i =0;i<tablis.length;i++){
            //绑定i的索引
            tablis[i].index = i;
            tablis[i].onclick = clickFun;
        }
    	
    	function clickFun(){
            //去掉所有
            for(var j=0;j<tablis.length;j++){
                tablis[j].className = '';
                contentDivs[j].style.display = 'none';
            }
            this.className='active';
            contentDivs[this.index].style.display = 'block';
        }
    }
</script>
```

## 基于面向对象实现选项卡

基于面向对象实现选项卡写法流程如下：

1.构造函数

2.属性属于实例的属性

3.添加到原型上prototype

javascript代码

```javascript
<script src="js/TabSwitch.js"></script>
<script type="text/javascript">
    // 1.构造函数
    // 2.属性属于实例的属性
    // 3.添加到原型上prototype
    //对象：属性和方法
    window.onload = function(){
        var wrap = document.getElementById('wrap');//事件委托
        var tab = new TabSwitch(wrap);
	}
</script>
```

TabSwitch.js封装代码：

```javascript
//1.构造选项卡的函数
function TabSwitch(fatherObj){
    //保存this
    // console.log(this);//指向函数对象TabSwitch
    var _this = this;
    // console.log(_this);//指向函数对象TabSwitch
    //2.绑定实例属性
    this.tablis = fatherObj.children[0].getElementsByTagName('li');
    this.contentDivs = fatherObj.getElementsByTagName('div');
    for(var i=0;i<this.tablis.length;i++){
        //绑定i的索引
        this.tablis[i].index = i;
        this.tablis[i].onclick = function(){
            // console.log(this);//指向每个li对象
            _this.clickFun(this.index);
        }   
    }
}
//3.绑定共享的方法
TabSwitch.prototype.clickFun = function(index){
    console.log(this);//指向函数对象TabSwitch   object,HTMLElement
    //去掉所有
    for(var j=0;j<this.tablis.length;j++){
        this.tablis[j].className = '';
        this.contentDivs[j].style.display = 'none';
    }
    this.tablis[index].className='active';
    this.contentDivs[index].style.display = 'block';
}
```

## 对象创建总结

### 重点

1.字面量方式

  问题：创建多个对象会造成冗余的代码

2.工厂模式

  解决对象字面量方式创建对象的问题

  问题：对象识别的问题

3.构造函数模式

  解决工厂模式的问题

  问题：方法重复被创建

4.原型模式

  解决构造函数模式创建对象的问题

  特点：在于方法可以被共享

  问题：给当前实例定制的引用类型的属性会被所有的实例所共享

5.组合模式（构造函数和原型模式）

  构造函数模式：定义实例属性

  原型模式：用于定义方法和共享的属性，还支持向构造函数中传递参数。

  该模式是使用最广泛，应用度最高的一种模式

###   了解部分

  构造函数扩展模式

  寄生构造函数模式

  稳妥构造函数模式

  动态原型模式

## 继承方式

### 1.原型链继承

继承:在原型对象的所有属性和方法,都能被实例所共享

定制Animal

```javascript
function Animal(){
    this.name = 'alex';
    this.colors = ['red','green','blue'];
}
Animal.prototype.getName = function(){
	return this.name;
}
```

Dog类

```javascript
function Dog(){};
```

Dog继承了Animal

本质:重写原型对象,将一个父对象的属性和方法作为一个子对象的原型对象的属性和方法

```javascript
Dog.prototype = new Animal();
Dog.prototype.constructor = Dog;
var d1 = new Dog();
var d2 = new Dog();
console.log(d1.colors);
console.log(d2.colors);
d1.colors.push('purple');
console.log(d1.colors);
console.log(d2.colors);
```

![04](D:\Typora\program\javascript\pic\12\04.png)

缺点：

1.父类中的实例属性一旦赋值给子类的原型属性,此时这些属性都属于子类的共享属性

2.实例化子类型的时候,不能向父类型的构造函数传参

### 2.借用构造函数继承

经典继承:在子类的构造函数内部调用父类的构造函数

缺点:父类定义的共享方法不能被子类所继承下来

父类：

```javascript
function Animal(name){
    this.name = name;
    this.colors = ['red','green','blue'];
}
Animal.prototype.getName = function(){
	return this.name;
}
```

子类：

```javascript
function Dog(name){
    // 继承了Animal
    // 当new实例的时候,内部构造函数中this指向了d1,然后在当前构造函数内部再去通过call再去调用构造函数,那么父类中的构造函数中的this指向了d1,但是方法不能被继承下来
    Animal.call(this,name);
}
var d1 = new Dog('阿黄');
var d2 = new Dog('小红');
d1.colors.push('purple');
console.log(d1.name);//阿黄
console.log(d1.colors);//Array(4)
// console.log(d1.getName());
```

### 3.组合继承

父类

```javascript
function Animal(name) {
    this.name = name;
    this.colors = ['red', 'green', 'blue'];
}
Animal.prototype.getName = function() {
	return this.name;
}
```

子类：

```javascript
function Dog(name) {
    // 继承了Animal
    // 让父类的实例属性继承下来,实例修改引用类型的值,另一个实例的引用类型的值不会发生变化
    Animal.call(this, name);
}
// 重写原型对象:把父类的共享方法继承下来
Dog.prototype = new Animal();
Dog.prototype.constructor = Dog;
var d1 = new Dog('阿黄');
var d2 = new Dog('阿红');
```

缺点：无论在什么情况下,都会调用父类构造函数两次(1.一个是我们初始化子类的原型对象的时候; 2.在子类构造函数内部调用父类的构造函数)

### 4.寄生组合式继承

父类

```javascript
function Animal(name) {
    this.name = name;
    this.colors = ['red', 'green', 'blue'];
}
Animal.prototype.getName = function() {
	return this.name;
}
```

子类

```javascript
function Dog(name) {
    // 继承了Animal
    // 让父类的实例属性继承下来,实例修改引用类型的值,另一个实例的引用类型的值不会发生变化
    Animal.call(this, name);
}
// 重写原型对象:把父类的共享方法继承下来
Dog.prototype = Object.create(Animal.prototype)
Dog.prototype.constructor = Dog;
var d1 = new Dog('阿黄');
var d2 = new Dog('阿红');
```

### 5.继承总结

#### 5.1原型链继承

  特点：重写子类的原型对象，父类原型对象上的属性和方法都会被子类继承

  问题：在父类中定义的实例引用类型的属性，一旦被修改，其它的实例也会被修改

​     当实例化子类的时候，不能传递参数到父类

#### 5.2借用构造函数模式

  特点：在子类构造函数内部间接调用（call(),apply(),bind()）父类的构造函数

​     原理：改变父类中的this指向

​     优点：仅仅的是把父类中的实例属性当做子类的实例属性，并且还能传参

​     缺点：父类中公有的方法不能被继承下来

#### 5.3组合继承

  特点:结合了原型链继承和借用构造函数继承的优点

​    原型链继承：公有的方法能被继承下来

​    借用构造函数：实例属性能被子类继承下来

  缺点：调用了两次两次父类的构造函数

​     1.实例化子类对象

​     2.子类的构造函数内部（好）

#### 5.4寄生组合式继承

```javascript
 var b = Object.create(a);
```

  将a对象作为b实例的原型对象

  把子类的原型对象指向了 父类的原型对象

```javascript
 Dog.prototype = Object.create(Animal.prototype)
```

[^开发过程中使用最广泛的一种继承模式]: 

### 6.多重继承

多重继承:一个对象同时继承多个对象

定制Person对象

```javascript
function Person(){
	this.name = 'Person';
}
Person.prototype.sayName = function(){
	console.log(this.name);
}
```

定制Parent

```javascript
function Parent(){
	this.age = 30;
}
Parent.prototype.sayAge = function(){
	console.log(this.age);
}
```

继承Person的属性

```javascript
function Me(){
    Person.call(this);
    Parent.call(this);
}
```

继承Person的方法

```javascript
Me.prototype = Object.create(Person.prototype);
// 不能重写原型对象来实现 另一个对象的继承
// Me.prototype = Object.create(Parent.prototype);
// Object.assign(targetObj,copyObj)
// 混入技术  Mixin
Object.assign(Me.prototype,Parent.prototype);
// 指定构造函数
Me.prototype.constructor = Me;
var me = new Me();
```

## Object对象中相关方法介绍

### 1.Object对象本身的方法

Object的静态方法

#### 1.1Object.keys(对象)

用法：参数是一个对象,返回是一个数组

```javascript
// Object.keys(对象)
var arr = ['a','b','c'];
console.log(Object.keys(arr));
var obj = {
    0: 'e',
    1: 'f',
    2 : 'j'
}
console.log(Object.keys(obj)); 
```

#### 1.2getOwnPropertyNames(对象)

用法：返回不可枚举和枚举的属性名都能获取

Object.keys()返回可枚举的属性

接收一个对象作为参数,返回一个数组,包含了该对象自身的所有属性名

```javascript
 var arr = ['a','b','c'];
 for(var key in arr){
 	console.log(key);
 }
//length不能够被遍历
 console.log(Object.getOwnPropertyNames(arr).length);
var obj = {
    0: 'e',
    1: 'f',
    2 : 'j'
}
 console.log(Object.getOwnPropertyNames(obj).length); 
```

![05](D:\Typora\program\javascript\pic\12\05.png)

#### 1.3Object.getPrototypeOf()

用法：参数是该对象，返回该对象的原型,也是获取原型对象的标准方法

```javascript
function Fn(){};
var f1 = new Fn();
console.log(Object.getPrototypeOf(f1) === Fn.prototype);//true
```

介绍几种特殊对象的原型

空对象的原型是Object.prototype

```javascript
console.log(Object.getPrototypeOf({}) === Object.prototype);//true
```

Object.prototype 的原型是null

```javascript
console.log(Object.getPrototypeOf(Object.prototype) === null);//true
```

函数的原型是Function.prototype

```javascript
function f(){};
console.log(Object.getPrototypeOf(f) === Function.prototype); 
```

#### 1.4Object.setPrototypeOf() 设置原型；Object.getPrototypeOf()获取原型

用法：接收两个参数,第一个参数是现有对象,第二个是原型对象

```javascript
var a = {};
var b = {x : 1};
Object.setPrototypeOf(a,b);
console.log(a.x);//1
console.log(Object.getPrototypeOf(a)); //a继承对象b,object
```

```javascript
function F(){
	this.foo = 'foo';
}
var f = Object.setPrototypeOf({},F.prototype); //var f = new F();与之等价
F.call(f);//f.prototype.constructor = F;
console.log(f);//输出原型对象F
```

#### 1.5Object.create()

用法：一个实例对象生成另一个实例对象

接受一个对象作为参数,然后以它为原型,返回一个实例对象

```javascript
var  A = {
    hello:function(){
        console.log('hello');
    }
}
var B = Object.create(A);
console.log(Object.getPrototypeOf(B));
B.hello();
```

### 2.Object实例方法介绍

#### 2.1Object.prototype.valueOf()

用法：返回当前对象的值,默认情况返回对象本身

```javascript
var obj = new Object();
//用自定义的Object.valueOf 覆盖 Object.prototype.valueOf
obj.valueOf = function(){
	return 2;
}
console.log(obj.valueOf() === obj);//false
console.log(1 + obj);//1[object Object]
```

#### 2.2Object.prototype.toString()

用法：返回一个对象的字符串形式,默认返回类型字符串

```javascript
ar obj2 = {a: 1};
obj2.toString = function(){
	return 'hello';
}
// 如果没有定制toString方法得到如下结果
console.log(obj2.toString());//hello
// 自定义了toString()方法
console.log(obj2.toString() + ' world');//"hello world"

var arr = [1,2,3];
console.log(arr.toString());//1,2,3
console.log('123'.toString());//123

console.log((function(){
	return 123
}).toString());//返回function(){return 123}
console.log((new Date()).toString());//Fri Aug 28 2020 22:38:14 GMT+0800 (中国标准时间)
```

#### 2.3Object.prototype.toLocaleString()

用法：当地地区标准

```javascript
console.log((new Date()).toLocaleString());//2020/8/28 下午10:42:01
```

#### 2.4Object.prototype.isPrototypeOf()

用法：用来判断该对象是否是另一个对象的原型

```javascript
var o1 = {};
var o2 = Object.create(o1);
var o3 = Object.create(o2);
console.log(o2.isPrototypeOf(o3));//true
console.log(o1.isPrototypeOf(o3));//true
console.log(o3);//{}
console.log(Object.prototype.isPrototypeOf({}));//true
console.log(Object.prototype.isPrototypeOf([]));//true
console.log(Object.prototype.isPrototypeOf(Object.create(null)));//false
console.log(Object.prototype.__proto__);//null
```

#### 2.5hasOwnProperty()

用法：接收一个字符串作为参数,返回一个布尔值,表示该实例对象自身是否具有该属性,继承来的属性返回为false

```javascript
var obj = {
a: 123
}
// obj.toString()
console.log(obj.hasOwnProperty('a'));//true
console.log(obj.hasOwnProperty('b'));//false
console.log(obj.hasOwnProperty('toString'));//false
```

#### 2.6Object.prototype.propertyIsEnumerable()

用法：判断某个属性是否可以遍历,只能判断实例对象本身的属性,对于继承来的属性和设置不可枚举的属性一律返回false

可枚举:可以遍历的属性

```javascript
var obj = {};
obj.a = 123;
console.log(obj.propertyIsEnumerable('a'));//true
console.log(obj.propertyIsEnumerable('toString'));//false
for(var key in obj){
console.log(obj[key]);//123
}
var arr = [1,2,3];
console.log(arr.propertyIsEnumerable('length'));//false
```

#### 2.7getOwnPropertyDescriptor()

用法：属性描述对象:一个对象内部的数据结构

```javascript
var obj = {};
obj.name = 'mjj';
obj.name = 'alex';
// console.log(delete obj.name);//与configurable，可配置属性相关
// console.log(obj.propertyIsEnumerable('name'));//enumerable
console.log(Object.getOwnPropertyDescriptor(obj,'name'));
// console.log(obj.name);
输出的 name: 
    {
        value: alex,
        writable:true,
        enumerable:true,
        configurable:true,
        set:undefined,
        get:undefined
    }
```

用法：可以获取属性描述对象.它的第一个是目标对象,第二个参数是一个字符串(目标对象的属性名)

```javascript
var arr = [1,2,3];
arr.length = 5;
console.log(arr.length);
console.log(delete arr.length);
// console.log(arr['length']);
console.log(Object.getOwnPropertyDescriptor(arr,'0'));
console.log(Object.getOwnPropertyDescriptor(arr,'length'));
console.log(Object.getOwnPropertyDescriptor(arr,'toString'));//undefined 不能用于继承的属性 只能用于对象自身的属性
```

#### 2.8Object.defineProperty()方法

用法：Object.defineProperty(属性所在的对象,属性名,属性描述对象)，定义单个属性

```javascript
var obj = Object.defineProperty({},'name',{
    value:'mjj',
    writable:false, //可写性
    enumrable: true, //可遍历
    configurable: false //是否能被删除
})
console.log(obj.name);
obj.name = 'alex';
console.log(obj.name);
/* for(var key in obj){
console.log(obj[key]);
} */
delete obj.name;
console.log(obj);
```

#### 2.9Object.defineProperties()方法

用法：定义多个属性

```
var obj = Object.defineProperties({},{
    p1:{
        value:123,
        enumerable:true
    },
    p2:{
        value:"mjj",
        enumerable:false
    },
    p3:{
        get:function(){
        	return this.p1 + this.p2;
    	},
        enumerable:true,
        configurable:true
    }
})
console.log(obj);
console.log(obj.p1);

// 一旦定义了取值函数get,就不能同时定义value属性,否则会报错
/* var obj2 = Object.defineProperty({},'p',{
    value:'123',
    get:function(){
    	return 456;
    }
})
console.log(obj2.p); */

// 一旦定义了取值函数get,就不能同时定义writable
var obj3 = Object.defineProperty({},'a',{
    get:function(){
    	return 456;
    }
})
console.log(obj3.a);
var obj4 = Object.defineProperty({},'foo',{});
console.log(Object.getOwnPropertyDescriptor(obj4,'foo'));
/* for(var key in obj4){
	console.log(obj4[key]);
} */
```

#### 2.10属性描述对象中的value和writable属性

```javascript
<script type="text/javascript">
    // "use strict";
    var obj = {};
    obj.p = 123;
    console.log(Object.getOwnPropertyDescriptor(obj,'p').value);
    Object.defineProperty(obj,'p',{value: 456});
    console.log(obj.p);
    // writable
    Object.defineProperty(obj,'p',{
        value: 890,
        writable:false
    });
    console.log(obj.p);
    obj.p = 100;
    console.log(obj.p); 
    var pro = Object.defineProperty({},'foo',{
        value:'a',
        writable:false
    })
    var obj2 = Object.create(pro);
        Object.defineProperty(obj2,'foo',{
        value:'b'
    })
    console.log(obj2.foo);	
</script>
```

#### 2.11enumrable属性用法

用法：如果一旦enumrable设置false,通常以下三个操作不会取到该属性

1.for...in；2.Object.keys()；3.JSON.stringify()

```javascript
var obj = {};
Object.defineProperty(obj, 'foo', {
    value: 123,
    enumrable:false
})
console.log(obj.foo);
    for(var key in obj){
    console.log(key);//不起任何作用
}
console.log(Object.keys(obj));//[]
console.log(JSON.stringify(obj));//"{}"

function A(){
	this.name = 'mjj';
}

function B(){
	A.call(this);
}
var b = new B();
Object.defineProperty(b,'age',{
    value: 18,
    enumrable:false
})
console.log(b);
```

[^注意: for...in循环可以遍历继承来的属性]: 

```javascript
for(var key in b){
	console.log(key);
}
// 可以获取继承来的属性
// console.log(Object.keys(b));
// getOwnPropertyNames 可以获取继承和不可遍历的属性
console.log(Object.getOwnPropertyNames(b));
```

#### 2.12configurable属性用法

用法：configurable 可配置性,决定是否我们可以修改属性描述对象

1.一旦设置configurable为false,属性描述对象 value,writable,enumrable,configurable都不能被修改

```javascript
var obj = Object.defineProperty({},'a',{
    value:1,
    writable:false,
    enumrable:false,
    configurable:false
})
// Cannot redefine property: a
Object.defineProperty(obj,'a',{
	configurable: true
})
console.log(Object.getOwnPropertyDescriptor(obj,'a')); 
```

2.注意:writable只有在false改为true的时候会静态失败,true改为false会允许的

```javascript
var obj2 = Object.defineProperty({},'p',{
    writable:true,
    configurable:false
});
Object.defineProperty(obj2,'p',{
    writable:false,
    configurable:false
})
obj2.p = 10;
console.log(obj2);
console.log(Object.getOwnPropertyDescriptor(obj2,'p'));
```

3.value属性 只要writable和 configurable有一个为true,就允许被修改

```javascript
var obj2 = Object.defineProperty({},'p',{
    value: 1,
    writable:false,
    configurable:true
});
Object.defineProperty(obj2,'p',{
	value:10
})
console.log(obj2);
console.log(Object.getOwnPropertyDescriptor(obj2,'p')); 
```

4.configurable一旦被配置了true,可以被删除,否则就不能被删除

```javascript
var obj = Object.defineProperties({},{
    a:{
        value: 1,
        configurable: true
    },
    b:{
        value: 2,
        configurable: false
    }
});
delete obj.a;
delete obj.b;
console.log(obj);
```

## 存取器

Object.defineProperty() get和set

```javascript
Object.defineProperty() //get和set
var obj = Object.defineProperty({},'p',{
    get:function(){
    	return 'getter';
	},
	set:function (value){
        // console.log('setter:' + value);
        return;
    }
})
console.log(obj.p);
obj.p = 123;
console.log(obj); 

var obj = {
    get p(){
    	return 'getter';
    },
    set p(value){
    	console.log('setter:' + value);
    }
}
console.log(obj.p);
obj.p = 123; 

var obj = {
    n: 5,
    get a(){
    	return this.n++;
	},
	set a(newValue){
        if(newValue > this.n){
            this.n = newValue;
        }else{
            throw new Error('新的值必须大于当前的值');
        }
	}
}
console.log(obj.a);
console.log(obj.a);
obj.a = 10;
console.log(obj.a);
obj.a = 3;
```

## 浅拷贝

```javascript
// 基本数据类型:按值传递
var a = 1;
var b = a;
b = 200;
console.log(b);
console.log(a);
// 引用数据类型:按引用地址传递
var arr = [1,3,4];
var newArr = arr;
newArr.push(10);
console.log(newArr);
console.log(arr);
// 拷贝: 浅拷贝 和 深拷贝
// 操作拷贝之后的对象的某个属性不会影响原始对象中的属性,这种拷贝就称为叫深拷贝
// 反之,有影响叫浅拷贝
var obj = {
    name:'小马哥',
    age: 20,
    hobby:'eat',
    friend:{
        name:'alex',
        age:38
    }
}
function shadowCopy(toObj,fromObj){
    for(var key in fromObj){
    	toObj[key] = fromObj[key];
    }
    // 来实现浅拷贝
    return toObj;
}
// 浅拷贝不是直接赋值,浅拷贝新建了一个对象,将原来对象的属性都一一的复制过来,复制的是值,而不是引用.浅拷贝的复制只复制了第一层的属性,并没有递归所有的值复制归来
var newObj = shadowCopy({},obj);
newObj.age = 30;
newObj.friend.name = '阿黄';
```

## 深拷贝

深拷贝对目标对象的完全拷贝,不像浅拷贝那样只是复制了一层引用,就连值也复制过来

[^只要进行了深拷贝,它们老死不相往来,谁也不影响谁]: 

```javascript
var obj = {
    name: '小马哥',
    age: 20,
    hobby: ['eat','songs'],
    friend: {
        name: 'alex',
        age: 38,
        hobby: '鸡汤',
    	friend: {
            name: '阿黄',
            age: 10,
            hobby: 'play'
        }
	}
}
function deepCopy(to,from){
// 遍历from对象的所有的属性,拷贝到to对象中
for(var key in from){
	// 不遍历原型链上的属性
    if(from.hasOwnProperty(key)){
        	/*如果值是对象并且有值，再遍历对象*/
        	if(from[key] && typeof from[key] === 'object'){
                    //区分是一般对象还是数组
                    to[key] = from[key].constructor === Array ? [] : {};
                    to[key] = deepCopy(to[key],from[key]);	  
            	}else{
                // 如果不是,直接赋值
                to[key] = from[key];
        	}
		}
	}
	return to;
}
var newObj = deepCopy({},obj);
newObj.friend.name = '小红';
console.log(newObj);
console.log(obj);
```

## ES6模块的基本实现

javascript代码

```javascript
<!-- 一个js文件就是一个模块 -->
<script src="js/module1.js" type="text/javascript" charset="utf-8"></script>
<script type="text/javascript">
    module1.m1();
    module1.m2();
    console.log(module1);
</script>
```

module1.js

```javascript
// 业务逻辑
// 1.字面量方式
/* var module1 = new Object({
	count: 0,
	m1:function() {
		// ....
	},
	m2:function() {
		//....
	}
})
 */
// IIFE 立即执行函数
 var module1 = (function() {
	// count是一个私有的变量
	var count = 0;
	var m1 = function() {
		console.log('m1');
		//....
	};
	var m2 = function() {
		console.log('m2');
		//....
	}
	return {
		m1: m1,
		m2: m2
	}
})(); 
// 放大模式
// 宽放大模式
(function(mod){
	mod.m3 = function(){
		console.log('m3');
	}
	return mod;
})(window.module1 || {});
```

## 命名空间避免全局变量污染

javascript代码

```javascript
<script src="js/namespace.js" type="text/javascript" charset="utf-8"></script>
<script src="js/namespace_sub.js" type="text/javascript" charset="utf-8"></script>
<script type="text/javascript">
    console.log(namespace);
    var p1 = new namespace.PersonInfo({
        name: 'mjj',
        gender: '男'
    });
    // console.log(p1);
    var helloStr = p1.getHello();
    console.log(helloStr);
    // namespace.personInfoUtil.show(p1);
    var c = new namespace.sub.Cat('闪电', '白色');
    c.sleep();
    console.log(c);
</script>
```

namespace.js

```javascript
// 个人信息类
// 姓名 性别
var namespace = (function(namespace){
	// 声明了一个顶层的命名空间
	// 个人信息类：构造函数构建对象
	namespace.PersonInfo = function(obj){
		obj = obj || {};
		this.name =  obj.name || '';
		this.gender = obj.gender || "?";
	}
	// 称谓方法
	namespace.PersonInfo.prototype.getAppellation = function(){
		var str = "";
		if(this.gender === '男'){
			str = '男士';
		}else{
			str = '女士';
		}
		return str;	
	}
	// 欢迎的方法
	namespace.PersonInfo.prototype.getHello = function(){
		var s = "Hello " + this.name + this.getAppellation();
		return s;
	}
	// 个人信息工具类
	namespace.personInfoUtil = function(){
		return {
			// p形参是代指是哪个对象
			show:function(p){
				alert('姓名：'+ p.name+ "，性别："+ p.gender);
			}
		}
	}()
	return namespace;
})(window.namespace || {});
```

namespace_sub.js

```javascript
// 动物园

namespace.sub = (function(sub){
	// 定义一个子的命名空间
	// 动物类
	// sub.Animal = {
	// 	createNew:function(){
	// 		var animal = {};
	// 		animal.sleep = function(){
	// 			alert('睡懒觉');
	// 		}
	// 		return animal;
	// 	}
	// }


	sub.Animal = function(name,color){
		this.name = name;
		this.color = color;
	}
	sub.Animal.prototype.sleep = function(){
		console.log('睡懒觉');
	}
	// 猫类
	sub.Cat = function(name,color){
		// 继承属性
		sub.Animal.call(this,name,color);
	}
	// 继承方法
	sub.Cat.prototype = Object.create(sub.Animal.prototype);
	sub.Cat.prototype.constructor = sub.Animal;
	return sub;
})(window.namespace.sub || {});
```

