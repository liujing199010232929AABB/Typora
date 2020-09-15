# 第一章 JavaScript基础入门

## ECMAScript发展：

ECMA-计算机联盟协会 欧洲计算机协会标准

ECMAScript是一种语言标准，JavaScript是对ECMAScript的一种实现

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

1. 1999.12：增加正则，更好的文字处理、新的控制语句，try/catch异常处理、更加明确的错误定义，数字输出格式等等
2. 2009.12：完善ECMAScript 3版本、增加"strict mode"(严格模式)、以及新的功能，如getter和setter、JSON库支持和完善更完整的对象属性
3. 2011.6：ES5
4. 2015.6：ES6（ES2015）
5. 2016.06：ECMAScript2016，包括两个新的功能：求幂运算符（*）和array.prototype.includes方法
6. 2017.06：增加新的功能，如并发、原子操作、Object.values/Object.entries，字符串填充、promises、await/asy等等

------



## Javascript内核：

1. 核心（ECMAscript）;[变量,测试语句,数据类型,运算符,数据类型转换,流程控制,常用内置对象（Array,String,Date,Math内置对象，Function函数）]

2. 文档对象模型（DOM）;[DOM节点,获取DOM的三种方式,值的操作,样式操作,属性操作,DOM操作]

3. 浏览器对象模型（BOM）

   

## Javascript语法

### Javascript变量：

变量初始化 

```javascript
var x =30;
```

声明变量 

```javascript
var y;
var name = 'nini';
```

变量赋值

```javascript
y=50;
```

[^规则： 1.必须使用字母、下划线(_)开始；2. 多个英文字母 驼峰 myName；3.不能使用js中的关键字和保留字来进行命名； 4.严格区分大小写]: 

### Javascript变量类型：

1.   变量类型

2.   基本的数据类型

3.   Number String Boolean underfined null

4.   引用数据类型

5.   Object Array Function

   ------

typeof 来检查当前变量的数据类型：

```javascript
var a = 3;alert(typeof a);
```

### Javascript运算符：

算数运算符：

```javascript
var x = 10;
var y = 2;
var sum = x + y;
var en = x - y;
var or = x * y;
var op = x % y;
```

比较运算符：

```javascript
=== 等同于和!==
```

逻辑运算符：

```javascript
var weather = 'sunny';
var temp = 32;
if(weather === 'sunny'){
    if(temp > 30){
    console.log('温度有点高');    
    }else{
    console.log('天气非常好');
}
等同于：
if(weather === 'sunny' && temp > 30){
    console.log('温度有点高');  

    }else if(weather === 'sunny' && temp <= 30){
    console.log('天气非常好');
}
```

赋值运算符：

1.隐式转换，数值转字符串

```javascript
var num = 234;
var myStr = num + "";
alert(typeof myStr);
```

2.toString()数值转字符串

```javascript
var myStr2 = num.toString();
alert(typeof myStr2);
```

3.字符串转数值

```javascript
var myNumber = Number(num);
alert(typeof myNumber);
```

4.数值转字符串

```javascript
var n = '2424jjj';
var a = Number(n);
alert(typeof a);//NaN Not a Number
```

## Javascript字符串和数值间相互转换：

```javascript
var str = '1212121212.0232';
//字符串转换整型
console.log(parseInt(str));//1212121212 数字
var str2 = '1212xxx.000';

console.log(parseFloat(str));//1212121212.0232 数字
console.log(typeof parseFloat(str));//number
console.log(Number(str2));//NaN not a number
var a = Number(str2);
console.log(isNaN(a));//true

//数字转换字符串
var num = 1212.789;
//强制转换
console.log(num.toString());//1212.789 字符串
console.log(typeof num.toString());//string
console.log(String(num));//1212.789 字符串

//隐式转换
var num2 = '' + 111111;
console.log(num2);//111111 字符串
console.log(''.concat(num));//1212.789 字符串

//按照位数四舍五入输出数字 默认是保留整数
console.log(Number(num.toFixed(1)));//字符串 1212.8 数字
```



### Javascript字符串：

```javascript
var name = 'would you eat an apple?'
var name2 = "would you eat an 'apple'?"
var name3 = 'would you eat an "apple"?'
alert(name);
alert(name2);
alert(name3);
var name4 = "I'm \'liujing\'"// 转义字符
alert(name4);
```

### JavaScript中强大的数组Array：

```javascript
var shopping = ['香蕉','苹果','牛奶','红牛'];
console.log(shopping);
var rand = ['tree',123,shopping];
console.log(rand);
console.log(typeof rand[1]);
var a =rand[2][2];
console.log('数组的长度是:'+rand.length);//数组的长度 for循环遍历
```

### Javascript语句：

条件判断语句：

switch语句：

```javascript
var weather = 'sunny';
switch(weather){
    case 'sunny':
    alert(1);
    break;
    case 'rainy':
    alert(2);
    break;
    case 'snowing':
    alert(3);
    break;
    default:
    alert(4);
    break;
}
```

三元运算:

```javascript
// （条件）?真的:假的
var isresult = 1<2 ? '真的' : '假的';
alert(isresult);
```

for循环语句：

```javascript
var arr = [1,2,3,4];
for(var i=0;i<arr.length;i++){
    console.log(arr[i]);
    document.write(arr[i]);
}
var sum = 0;
for(var j=0;j<10000;j++){
    sum = sum + j;
}
console.log(sum);
```

break和continue：

```javascript
//break可以跳出死循环
//continue跳出当前循环，下次循环继续
var sum = 0;
for(var i = 1;i <=10;i++){
    if(i ===7 ){
        continue;
    }
	sum = sum + i;
}
console.log(sum);
```

while循环语句：

```javascript
 // while(判断循环的结束条件)
 var sum = 0;
 var n = 99;
 while(n > 0){
     sum = sum + n;
     n = n - 2;
 }
 alert(sum);
```

do-while循环语句：

```javascript
var sum = 0;
var i = 1;
do{
    sum = sum + i;
    i++;
	// console.log(sum);
}while(i <= 10);
```

### Javascript中的函数：

函数：封装重复性的代码

```javascript
//addition(加法) subtraction(减法) multiplication(乘法) division(除法)
function addition(a,b){
	return a + b;
}
function subtraction(a,b){
	return a - b;
}
function multiplication(a,b){
	return a * b;
}
/* function division(a,b){
	return a / b;
} */
//函数表达式 匿名函数
var division = function(a,b){
	return a / b;
}
var r = addition(3,2);
console.log(r);
console.log(division(3,2));
```

函数作用域：

```javascript
/* var a = 1;
console.log(a);
function add(){
	var b = 3;
}
console.log(b); */

//全局污染的问题
//(function(){})/*匿名函数 */();/*自执行 */
//var a = 3;
//相当于window.a = 3;相当于a挂载在window对象，相当于全局
console.log(window);
first();
second();
------------------------------------------------------------------------------------------------------------first.js代码如下：
(function(){
	var name = 'mjj';
	var hello = function (){
		alert('hello ' + name);
	}
	window.first = hello;
})();

//(function(){})/*匿名函数 */();/*自执行 */
------------------------------------------------------------------------------------------------------------
second.js代码如下：
(function(){
	var name = 'jack';
	var hello = function(){
		alert('hello ' + name);
	}
	window.second = hello;
})();
```

### Javascript中的对象和数组：

对象object讲解：

```javascript
//对象 字面量创建 姓名，年龄，性别，爱好
var person = {
    name : 'liujing',
    age : 18,
    sex : '女',
    fav : function(a){
        alert('aihao');
        return 'nini'+a;
    }
}
console.log(person);
console.log(person.sex);
console.log(person.fav(18));
```

内置对象Array：

```javascript
//内置对象native object对象:属性和方法
// document.write('hhhh');
// alert(typeof arr);

// js提供构造函数
var colors = new Array();
var colors2 = [];
if(Array.isArray(colors)){
        colors[0] = 'red';
        colors[1] = 'red';
        var a = colors.toString();
        console.log(a);
        console.log(colors);
    }else{
    	console.log('hh');
}
```

[^isArray();判断是否为数组类型]: 

数组的堆方法、队列方法和排序方法：

```javascript
//分割字符串
var colors = ['red','blue','pink'];
var a = colors.join('|');
console.log(a);

//栈方法(last-in-first-out)push() pop() 队列方法
var newlength = colors.push('purple');
console.log(newlength);
console.log(colors);

//pop()从数组删除最后一项
var lastItem = colors.pop();
console.log(lastItem);
console.log(colors);

//队列先进先出shift() unshift()
newlength = colors.unshift('yellow');
console.log(newlength);
console.log(colors);
var firstItem = colors.shift();
console.log(firstItem);
console.log(colors);

//降序排列
var values = [0,5,2,10,98];
// values.reverse(); 数组倒序
// console.log(values);

//sort升序排列
values.sort();//会调用toString ASII码
console.log(values);// [0, 10, 2, 5, 98]

var c = '10';
var d = '3';
c.charCodeAt();//查询ASII码值 如果ASII码值相等，则先比较高位再比较低位
console.log(c);

function compare1(a,b){
    //a位于b之前 a小于b
     if(a < b){
         return -1;
     }else if(a > b){
         return 1;
     }else{
         return 0;
     }
    return a - b;
}
function compare2(a,b){
    //a位于b之前
     if(a < b){
         return 1;
     }else if(a > b){
         return -1;
     }else{
         return 0;
     }
    return b - a;
}
/* values.sort(compare1);//s升序
console.log(values); */
values.sort(compare1);//s升序
console.log(values);

values.sort(compare2);//s降序
console.log(values);
```

数组的操作方法（增、删、改）：

```javascript
//操作方法 concat() slice() splice()
// 1.concat() 数组合并
var colors = ['red','green'];
// var newColors = colors.concat('pink');
newColors = colors.concat({name:'zhangsan'});
console.log(newColors);

var newColors2 = colors.concat({name:"liujing"},[1,2,3,4]);
console.log(newColors2);

//2.slice()将当前数组中的一个或者多个项创建一个新数组
newColors2 = newColors2.slice(1,2);
newColors2 = newColors2.slice(-4,-3);//7-2;7-1
console.log(newColors2);

//3. splice()删除 插入 替换
//3.1 删除
var names = ['张三','李四','lili','nini'];
// names.splice(0,2);
// console.log(names);

//3.2 插入
// names.splice(1,0,'dada','jack');
// console.log(names);

//3.3 替换
names.splice(1,1,'xiaoxiao');
console.log(names);
```

数组的迭代方法：

```javascript
//迭代方法
//1.filter() s数组过滤方法
var numbers = [1,23,4,5,12,78]
var filterResult = numbers.filter(function(item,index,array){
    console.log('item: '+item);
    console.log('index: '+index);
    console.log('array: '+array);

    return item>10;
})
//将数组内容每一个和10相比较
console.log(filterResult);

//2. map()方法
var mapResult = numbers.map(function(item,index,array){
	return item*2;
})
console.log(mapResult);

for(var i=0;i<mapResult.length;i++){
	console.log(mapResult[i]);
}
//3.forEach()
mapResult.forEach(function(item,index){
	console.log(item);
})
```

map方法的应用：

```javascript
var oldArray = [
    {
        name:'zhansan',
        age:18
    },
    {
        name:'lili ',
        age:20
    },
    {
        name:'nini',
        age:24
    },
    {
        name:'xiaoxiao',
        age:18
    }
];
// var newNames = [];
// var newAges = [];
// for(var i = 0;i<oldArray.length;i++){
//     var myName = oldArray[i].name;
//     var myAge= oldArray[i].age;
//     newNames.push(myName);
//     newAges.push(myAge);  
// }
// console.log(newNames);
// console.log(newAges);

var newNames = oldArray.map(function(item,index){
	return item.name;
})
var newAges = oldArray.map(function(item,index){
	return item.age;
})
console.log(newNames);
console.log(newAges);
```

字符串的属性和方法：

```javascript
// 属性
// length
// 方法
字符方法
// 1.charAt()//获取指定字符
// 2.charCodeAt()//获取指定字符对应的ASII码
// 3.concat()//一般不适用它做拼接 用+
切片方法：
// 4.slice()//第一个参数是起始的位置，第二个参数是结束的位置 顾头不顾尾
// 5.substr()//第二个参数是返回的字符数
// 6.substring()
// 7.indexOf()//查找字符的位置，并返回位置信息
// 8.lastIndexOf()
// 9.trim()//清除当前字符串前后空格
// 10.toLowerCase()
// 11.toUpperCase()
// 12.toLocaleUpperCase()//去外国地区用，常用
// 13.toLocaleUpperCase()//去外国地区用，常用

var str = 'hello world';
// console.log(str.length);//获取字符串的长度是11

// console.log(str.charAt(1));//获取指定字符
// console.log(str.charCodeAt(1));//获取指定字符对应的ASII码
// console.log(str.concat(' liujing','jack'));//一般不适用它做拼接 用+

//第一个参数是起始的位置，第二个参数是结束的位置 顾头不顾尾
// console.log(str.slice(2));
// console.log(str.substring(2));

//第二个参数是返回的字符数
// console.log(str.substr(2));
// console.log(str.slice(2,4));
// console.log(str.substring(2,4));
// console.log(str.substr(2,4));

// console.log(str.slice(-3));
// console.log(str.slice(-3,-1));//slice(8,10)

console.log(str.indexOf('o'));
console.log(str.lastIndexOf('o'));
console.log(str.indexOf('o',6));
console.log(str.lastIndexOf('o',6));

var str = '    hello world     ';
console.log(str.trim());//清除当前字符串前后空格
console.log(str);


var str = 'hello liujing';
console.log(str.toUpperCase());
console.log(str.toLowerCase());
```

如何查找当前字符的所有位置：

```javascript
//查找e在str中所有的位置
var str = 'He unfolded the map and set it on the floor';
var arr = [];
var pos = str.indexOf('e');
console.log(pos);

while(pos>-1){
    //找到当前e字符对应的位置;
    arr.push(pos);
    pos = str.indexOf('e',pos+1);

}
console.log(arr);
```

## Javascript中Date日期对象：

```javascript
//UTC 1970.1.1保留毫秒日期的到285616年
//Date日期对象
// var now = new Date();//Mon Aug 17 2020 23:40:35 GMT+0800 (中国标准时间)
// console.log(now);

// var xma = new Date('December 25,1995 13:30:00');//Mon Dec 25 1995 13:30:00 GMT+0800 (中国标准时间)
// console.log(xma);
// var xmas = new Date(1995,11,25);
// console.log(xmas);
// var xmas = new Date(1995,11,25,14,30,00);
// console.log(xmas);

var now = new Date();
//常用方法
console.log(now.getDate());//获取月份的第几天(1-31)
console.log(now.getMonth());//获取月份(0-11)
console.log(now.getFullYear());//获取年份
console.log(now.getDay());//获取一星期的第几天(0-6)0星期天
console.log(now.getHours());//获取小时(0-23)
console.log(now.getMinutes());//获取分钟(0-59)
console.log(now.getSeconds());//获取秒(0-59)


//2018-09-10 日期格式化
console.log(now.toDateString());//星期日 月 日 年 Tue Aug 18 2020
console.log(now.toTimeString());//时 分 秒 时区 09:25:24 GMT+0800 (中国标准时间)
console.log(now.toLocaleDateString());//2020/8/18  常用
console.log(now.toLocaleTimeString());//上午12:08:21 常用
console.log(now.toLocaleString());//2020/8/18 上午12:08:21  常用
console.log(now.toUTCString());//Mon, 17 Aug 2020 16:10:05 GMT

//UTC 国际标准时间
```

如何显示数字时钟的格式时间应用：

```javascript
function nowTime(){
    //0-23;
    //6:27:35 P.M.
    //6:30:01 P.M.
    //6:04:01 A.M.
    var now = new Date();
    var hour = now.getHours();
    var minute = now.getMinutes();
    var second = now.getSeconds();
    //18>12?(18-12):8
    var temp ='' + (hour > 12 ? hour - 12 : hour);
    if(hour === 0){
    temp = '12';
    }else if(hour < 10 && hour != 10){
    temp = '0' + temp;
    }
    temp = temp + (minute < 10 ? ':0' : ":") + minute;
    temp = temp + (second < 10 ? ':0' : ":") + second;
    temp = temp + (hour >= 12 ? ' P.M.' : ' A.M.');
    return temp;
}
console.log(nowTime());
```

## global对象的编码和解码方法：

URI 统一通用资源标识符：

```javascript
// console.log(global);
// console.log(Globle);
//URI 统一通用资源标识符 encodeURIComponent()
var uri = 'http://www.apeland.cn/web index.html?name=zhangsan';
console.log(encodeURI(uri));//只能识别空格
console.log(encodeURIComponent(uri));//既能识别空格，也能识别所有 常用

var encodeuri = 'http://www.apeland.cn/web%20index.html?name=zhangsan';
var encodeuri2 = 'http%3A%2F%2Fwww.apeland.cn%2Fweb%20index.html%3Fname%3Dzhangsan';
//解码 decodeURIComponent()
console.log(decodeURI(encodeuri));
console.log(decodeURIComponent(encodeuri2));//常用 解码函数
```

## window对象：

```javascript
//window对象相当于globle
var a = 3;
console.log(window.a);
function hello(){
	alert(window.a);
}
window.hello();

console.log(global);
```

## Math属性对象：

Math常用方法：

```javascript
// Math
var a =Math.E;
console.log(a);
var b = Math.LN10;
console.log(b);
var c = Math.LN2;
console.log(c);
var d = Math.LOG2E;
console.log(d);
var e = Math.PI;
console.log(e);
var f = Math.SQRT2;//2的平方根
console.log(f);
var f2 = Math.SQRT1_2//1/2的平方根
console.log(f2);

//方法 min() max()
var max = Math.max(2,56,11,56);
console.log(max);
var min = Math.min(2,56,11,56);
console.log(min);

var arr = [2,56,11,77];
QQ = Math.max.apply(null,arr);//js高级语句
console.log(QQ);

var max2 = Math.max(arr[0],arr[1],arr[2],arr[3]);
console.log(max2);

//ceil() floor() round()
var num = 24.36;
console.log(Math.ceil(num));//天花板函数,1-9都往前进以，向上取整
console.log(Math.floor(num));//地板函数  向下取整
console.log(Math.round(num));//标准四舍五入 取整

//随机数 random() 0=<random<1
console.log(Math.random());
```

Math属性方法的应用：

```javascript
//1.获取min到max之间的整数
//获取min到max之间的整数(1~100)
//Math.random()*(max-min) + min;
function random(min,max){
	return Math.floor(Math.random()*(max-min)+min);
}
console.log(random(100,1));
------------------------------------------------------------------------------------------------------------
//2.获取随机颜色 rgb(0-255,0-255,0-255)
function randomColor(){
    var r = random(0,256),g = random(0,256),b = random(0,256);
    //模板字符串 ` es6中的语法（反引号，和tab键同一个）
    var result = `rgb(${r},${g},${b})`;
    // console.log(result);           
    return result;
}
var color = randomColor();
console.log(color);
document.body.style.backgroundColor = color;

//3.随机验证码 四位 数字+字母(大写) 65Yh
------------------------------------------------------------------------------------------------------------
function creatCode(){
//设置默认空的字符串
var code = '';
//设置长度;
var codeLength = 4;
var randomCode = [0,1,2,3,4,5,6,7,8,9,'A','B','C','D','E','F','G','H','I','J','K','L','M','N','O','P','Q','R','S','T','U','V','W','X','Y','Z','a','b','c','d','e','f','g','h','i','j','k','l','m','n','o','p','q','r','s','t','u','v','w','x','y','z'];
for(var i = 0;i < codeLength; i++){
//设置随机范围0~36
        var index = random(0,62);
        code += randomCode[index];
    }
		return code;
}
------------------------------------------------------------------------------------------------------------
var rndcode = creatCode();
console.log(rndcode);
document.write(`<h1>${rndcode}</h1>`);
```

