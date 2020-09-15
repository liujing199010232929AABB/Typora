# 第三章 DOM操作

## 快速认识DOM

DOM(DOM Object Model):文档对象模型；

js对象分类三种： 

1. 用户定义对象 

2. 内建对象Array Date Math 

3. 宿主对象 例如：window对象

   

## DOM中节点中分类

1. 元素节点(element node) 
2. 文本节点(text node) 
3. 属性节点

[^没有内容的文档是没有任何价值的，大多数内容都是由文本提供]: 



## 获取元素节点对象的方式

html代码：

```html
<h2 title="书籍">你要买什么课程？</h2>
<p title="请您选择购买的课程">本课程是web全栈课程，期待你的购买！</p>
<ul id="classList">
    <li class="item">JavaScript</li>
    <li class="item">css</li>
    <li>DOM</li>
</ul>
```

### 1.document.getElementById('classList') 单个对象

```javascript
var eleNode = document.getElementById('classList');
console.log(eleNode);
console.log(typeof eleNode);
```

### 2.document.getElementsByTagName() 获取出来的是一个节点对象集合 

[^有点像数组，但不是数组]: 

```javascript
var oLis = document.getElementsByTagName('li');
var oTitle = document.getElementsByTagName('h2');
console.log(oTitle[0]);
console.log(oLis.length);      
for(var i =0;i<oLis.length;i++){
	console.log(oLis[i]);
}
console.log(typeof oLis);
```

### 3.document.getElementsByClassName('item')

```javascript
var oItem = document.getElementsByClassName('item');
console.log(oItem);
```



## DOM中节点中分类

html代码：

```html
<h2>你要买什么课程？</h2>
<p title="请您选择购买的课程">本课程是web全栈课程，期待你的购买！</p>
<ul id="classList">
    <li class="item">JavaScript</li>
    <li class="item">css</li>
    <li>DOM</li>
</ul>
```

### 在文档对象模型(DOM)中，每一个节点都是一个对象，DOM节点有三个重要属性：

1. nodeName:节点名称；

   1.1nodeName属性：节点的名称，是只读

   1.2属性节点的nodeName与属性名相同

   1.3文本节点的nodeName永远是#Text

   1.4文档节点的nodeName永远是#document

2. nodeValue:节点的值；

   2.1元素节点的nodeValue是undefined或null

   2.2文本节点的nodeValue是文本自身

   2.3属性节点的nodeValue是属性的值

3. nodeType:节点类型；

   3.1节点的类型，是只读的

以下常用的几种节点类型：

| 元素类型 | 节点类型 |
| -------- | -------- |
| 元素     | 1        |
| 属性     | 2        |
| 文本     | 3        |
| 注释     | 8        |
| 文档     | 9        |

```javascript
//1.元素节点
var oDiv = document.getElementById('box');
console.log(oDiv.nodeName+"|"+oDiv.nodeValue+"|"+oDiv.nodeType);

//2.获取属性节点
var attrNode = oDiv.attributes[0];
console.log(attrNode.nodeName+"|"+attrNode.nodeValue+"|"+attrNode.nodeType);

//3.获取文本节点
var textNode = oDiv.childNodes[0];
console.log(textNode.nodeName+"|"+textNode.nodeValue+"|"+textNode.nodeType);
//#text|我是一个文本节点|3
var commentNode = oDiv.childNodes[1];
console.log(commentNode.nodeName+"|"+commentNode.nodeValue+"|"+commentNode.nodeType);
//#comment| 我是注释 |8

console.log(document.nodeType);
```



## setAttribute()和getAttribute()用法：

style代码：

```css
<style type="text/css">
    #box{
        color:red;
    }
</style>
```

html代码：

```html
<h2 title="书籍">你要买什么课程？</h2>
<p title="请您选择购买的课程">本课程是web全栈课程，期待你的购买！</p>
<ul id="classList">
    <li class="item">JavaScript</li>
    <li class="item">css</li>
    <li>DOM</li>
</ul>
```

javascript代码：

```javascript
var oP = document.getElementsByTagName('p')[0];
//获取属性值有一个必需的参数，这个节点对象的名字
var title = oP.getAttribute('title');
var className = oP.getAttribute('class');
console.log(title);
console.log(className);

//设置属性值setAttribute(name,value)  先静态，再动态
oP.setAttribute('id','box');
```

## 节点对象的常用属性：

html代码：

```html
<div class="previous">我是上一个兄弟</div><div id="father"><p>lili</p><p>lili2</p></div><div class="sibling">我是兄弟</div>
```

JavaScript代码：

```javascript
var oFather = document.getElementById('father');
console.log(oFather.childNodes);
// console.log(oFather.childNodes[0].nodeType);//1
console.log(oFather.childNodes[0]);//<p>lili</p>
console.log(oFather.firstChild);

console.log(oFather.childNodes[oFather.childNodes.length-1]);//<p>lili2</p>
console.log(oFather.lastChild);
console.log(oFather.parentNode);//body标签

console.log(oFather.nextSibling);
console.log(oFather.previousSibling);
```

## 节点对象属性在各浏览器兼容性处理：

html代码：

```html
<div class="previous">我是上一个兄弟</div>
<div id="father">
    <p>lili</p>
    <p>lili2</p>
</div>
<div class="sibling">我是兄弟</div>
```

JavaScript代码：

```javascript
var oFather = document.getElementById('father');
console.log(oFather.childNodes);

function get_childNodes(fatherNode){
var nodes = fatherNode.childNodes;
var arr = [];
for(var i = 0;i<nodes.length;i++){
if(nodes[i].nodeType === 1){
		arr.push(nodes[i]);
	}
}
	return arr;
}
var childnodes = get_childNodes(oFather);
console.log(childnodes[0]);

function get_nextSibling(n){
	var x = n.nextSibling;
	while(x && x.nodeType != 1){
		x = x.nextSibling;
	}
	return x;
}
console.log(get_nextSibling(oFather));
```

## 元素节点对象的增删改查方法：

### 动态的操作节点:

1. 创建节点 createElement()

2. 插入节点 appendChild()

   insertBefore(newNode,node);

3. 删除节点 removeChild(childNode)

4. 换节点 replaceChild(newNode,node)

5. 创建文本节点 createTextNode()

style代码:

```css
<style type="text/css">
	.active{color:red;font-size:30px;}
</style>
```

html代码：

```html
<div id="box">
	<p id="active">liujing</p>
</div>
```

JavaScript代码：

```javascript
var oDiv = document.getElementById('box');
var oActive = document.getElementById('active');//<p id="active">liujing</p>
var newNode = document.createElement('p');//1097576.COM
var newNode2 = document.createElement('p');//1111.COM
var newNode3 = document.createElement('a');//

console.log(newNode === newNode2);
        
newNode.setAttribute('class','active');
newNode3.setAttribute('href','http://www.pnp.cn');
oDiv.appendChild(newNode);
//第一个参数是新插入的节点，第二个参数是参考的节点
oDiv.insertBefore(newNode2,oActive);//1111.COM 放在liujing
        

// var textNode = document.createTextNode('alex');
// newNode.appendChild(textNode);

newNode.innerHTML = '<a href="#">1097576.COM</a>';
// newNode.innerText = '<a href="#">1097576.COM</a>';

// newNode = null;//释放对象 在内存
newNode2.innerHTML = '<a href="#">1111.COM</a>';//
newNode3.innerHTML = '<a href="#">PHP中文网</a>';//

// oDiv.removeChild(oActive);
oDiv.replaceChild(newNode3,newNode);//PHP中文网 把 1097576.COM 替换掉
```

## 样式设置(动态操作样式):

style代码：

```css
<style type="text/css">
    .hightLight{
        background-color:black;
        color:white;
        height:300px;
        width:240px;
        font-size:23px;
        text-align:center;
        line-height:300px;
    }
</style>
```

html代码：

```html
<p id="box" style="color:red;">liujing</p>
```

JavaScript代码：

```javascript
//nodeObject.style
var oP = document.getElementById('box');
第一种方法：
console.log(oP.style);
oP.style.color = 'white';
oP.style.backgroundColor = 'black';
oP.style.width = '250px';
oP.style.height = '250x';
oP.style.textAlign = 'center';
oP.style.lineHeight = '250px';
oP.style.fontSize = '30px';
第二种方法：	
oP.setAttribute('class','hightLight');
```

## 事件介绍和onclick事件：

### 常用的事件

| onclick     | 鼠标单击事件         |
| ----------- | -------------------- |
| onmouseover | 鼠标经过事件         |
| onmouseout  | 鼠标移开事件         |
| onchange    | 文本框内容改变事件   |
| onselect    | 文本框内容被选中事件 |
| onfocus     | 光标聚焦事件         |
| onblur      | 光标失焦事件         |
| onload      | 网页加载事件         |

style代码：

```css
<style type="text/css">
#box{
    width:100px;
    height:100px;
    background-color:blue;
}
</style>
```

html代码：

```html
<div id="box" ></div>
```

JavaScript代码：

事件介绍和onclick事件

```javascript
var oDiv = document.getElementById('box');
var isBlue = true;
oDiv.onclick = function(){
// alert('事件被触发');
//this指向当前的元素节点对象
// console.log(this);//<div id="box"></div>

    if(isBlue){
        this.style.backgroundColor = 'red';
        isBlue = false;    
    }else{
        this.style.backgroundColor = 'blue';
        isBlue = true;    
    }
}

// var dd = function(){
//     alert('事件被触发');
// }
// oDiv.onclick = dd;
```

鼠标悬浮事件：

```javascript
//1.找到触发的事件对象  2.事件 3.事件处理程序
var oDiv = document.getElementById('box');
// 2.鼠标滑过事件
oDiv.onmouseover = function(){
    //3.事件处理程序
    this.style.backgroundColor = 'green';
}
oDiv.onmouseout = function(){
    //3.事件处理程序
    this.style.backgroundColor = 'blue';
}
```

光标聚焦和失焦事件：

```javascript
style代码：
<style type="text/css">
.text{
    color:red;
    font-size:24px;
}
</style>

html代码：
<form action="">
<p class="name">
    <label for="username">用户名</label>
    <input type="text" name="user" id="username">
</p>
<p class="pwd">
    <label for="password">密码</label>
    <input type="password" name="password" id="password">
</p>
	<input type="submit" name="">
</form>

javascript代码:
var username = document.getElementById('username');
var newNode = document.createElement('span');
username.onfocus = function(){
    // console.log('请输入用户名');
    newNode.innerHTML='请输入用户名';
    newNode.setAttribute('class','text');
    username.parentNode.appendChild(newNode);
}
username.onblur = function(){
    // console.log('请输入正确用户名');
    newNode.innerHTML='请输入正确用户名';
    newNode.setAttribute('class','text');
    username.parentNode.appendChild(newNode);
}
```

表单控件上内容选中和改变事件:

```javascript
html代码：
<textarea cols="30" rows="10">111111111</textarea>
<input type="text" name="" value="liujing">

javascript代码：
// onselect onchange
var textArea = document.getElementsByTagName('textarea')[0];
var inputObject = document.getElementsByTagName('input')[0];
textArea.onselect = function(){
	console.log('内容被选中了');
}
inputObject.onchange = function(){
	console.log('内容被改变了');
}
inputObject.oninput = function(){//实时监测输入框内容的改变
    console.log('内容被实时的改变');
    console.log(this.value);
}
        
```

窗口加载事件：

```javascript
script代码：
<script type="text/javascript">       
// setTimeout(function(){
//         var oDiv = document.getElementById('box');
//         console.log(oDiv);
//         oDiv.onclick = function(){
//         this.innerHTML = 'jingjing222';
//     }
// },0)
//等待文档元素加载完成才会调用onload()   覆盖现象 有多个onload的时候
window.onload = function(){
    var oDiv = document.getElementById('box');
    console.log(oDiv);
    oDiv.onclick = function(){
        this.innerHTML = 'jingjing';
    }
}
        
</script>

html代码：
<div id="box">liujing</div> 
```

