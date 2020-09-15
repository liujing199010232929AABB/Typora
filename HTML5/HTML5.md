# HTML5

#### HTML5有趣的新特性

- 用于媒介回放的video和audio元素
- 新的特殊内容元素：比如`article`,`footer`,`header`,`nav`,`section`
- 新的表单控件：比如`calendar`、`date`、`time`、`email`、`url`、`search`
- 2D/3D绘图&效果
- 支持对本地离线存储

#### HTML5浏览器支持

最新版本的五个主流浏览器都支持某些HTML5特性，IE9以上浏览器支持HTML5新特性。但是IE8以下的浏览器不支持

IE8以下(包含IE8)以下版本浏览器兼容HTML5的方法，我们必须使用htmlshiv垫片包，让其支持HTML5新特性

```html
<!--[if lt IE 9]>
<script src="https://cdn.jsdelivr.net/npm/html5shiv@3.7.3/dist/html5shiv.min.js"></script>
<![endif]-->
```

### HTML5新标签

#### 8个新语义元素和新属性

`header`,`section`,`footer`,`aside`,`nav`,`main`,`article`,`figure`所有的这些元素都是**块级**元素

##### 所有的标签都支持HTML5新属性

| 属性            | 描述                                                         | 浏览器支持性                    |
| --------------- | ------------------------------------------------------------ | ------------------------------- |
| contenteditable | 规定是否可编辑元素的内容                                     | All                             |
| contextmenu     | 指定一个元素的上下文菜单。当用户右击该元素，出现上下文菜单   | 只有Firefox浏览器支持           |
| data-*          | 嵌入自定义数据                                               | All                             |
| draggable       | 规定元素是否可拖动。链接和图像默认是可拖动的。经常用它实现拖放操作 | ie8以下浏览器都支持，其它不支持 |
| hidden          | 规定对元素进行隐藏。如果使用该属性，则会隐藏元素，隐藏的元素不会被显示，可以通过js来设置hidden属性为true,使得元素变得可见 | All                             |

##### 1.nav标签

`<nav>` 标签定义导航链接的部分。

并不是所有的 HTML 文档都要使用到`<nav>`元素。`<nav>`元素只是作为标注一个导航链接的区域。

通过HTML5的`nav`标签我们可以这样表示：

```html
<nav>
    <a href="#">python</a>
    <a href="#">linux</a>
    <a href="#">web前端</a>
    <a href="#">Java</a>
    <a href="#">Go</a>
</nav>
```

##### 2.header标签

`header`标签定义文档的头部区域，它是作为网页的头部介绍内容或者是导航链接栏的容器。在一个文档中，你可以定义多个`header`元素

[^注意：header标签不能被放在footer、address或者另一个header元素内部]: 

```html
<header contenteditable data-name='mjj' id="box">
    <div class="container">
        <a href="">logo</a>
        <nav>
            <a href="#">python</a>
            <a href="#">linux</a>
            <a href="#">web前端</a>
            <a href="#">Java</a>
            <a href="#">Go</a>
    	</nav>
    </div>
</header>
<h2 draggable="true" hidden>hello world</h2>
<div id="mjj"></div>
```

```javascript
<script type="text/javascript">
    var h = document.getElementById('box');
    console.log(h.dataset);//DOMStringMap {my: "mjj"}
    console.log(h.dataset.name);//"mjj"
    // var q = document.getElementById(h.dataset.name);
    var q = document.querySelector('#'+h.dataset.name);
    h.onclick = function(){ 
        q.style.backgroundColor = 'blue';
    }

    var h2 = document.getElementsByTagName('h2')[0];
    h2.hidden = false;
</script>
```

查看div标签的所有属性和方法

```
consoloe.dir(h)
```

![](D:\web\Typora\HTML5\static\images\1.png)

[^console.log()可以取代alert()或document.write()，在网页脚本中使用console.log()时，会在浏览器控制台打印出信息。]: 

##### 3.main标签

标签规定文档的主要内容。一个文档中只有一个main元素。

##### 4.aside标签

`aside`标签定义`article`标签外的内容。aside的内容应该与附近的内容相关

##### 5.section标签

section标签定义了文档的某个区域。比如章节、头部、顶部或者文档的其他区域

```html
<section>
    <h2>小猿圈重大新闻</h2>
    <p>
        本阶段讲解HTML5+CSS3部分，下个阶段老师开始做直播课。欢迎大家光临。小猿圈上的视频全部免费
    </p>
</section>
```

##### 6.article标签

定义页面独立的内容。必须是独立于文档的其余部分。

article通常都应用在：

- 论坛帖子
- 博客文档
- 新闻故事
- 评论

##### 7.figure标签

`figure`标签规定独立流的内容(图像、图标、照片、代码等)

`figure`元素的内容应该与主内容有关，同时元素的位置相对于主内容是独立的。如果被删除，则不应对文档流产生影响

```html
<figure>
  <img src="flower.jpg" alt="My 女神" width="304" height="228">
  <figcaption>我的女神照</figcaption>
</figure>
```

##### 8.footer标签

定义文档或者文档一部分区域的页脚。该元素会包含文档闯作业的姓名、文档的版权信息、使用条款的链接、联系信息等等。在一个文档中，可以定义多个footer。

比如小猿圈的底部就是用footer

#### 其它的新语义化标签

##### mark

高亮文本标签

```html
<p><mark> 元素用于 <mark>高亮</mark> 文本</p>
```

##### progress

用来显示一项任务的完成进度。

属性：

- max 最大值
- value 当前进度

```html
<progress value="70" max="100">70 %</progress>
```

##### address

个人或者某个组织的联系信息等等

```html
<address>
  <a href="mailto:jim@rock.com">mjj67890@163.com</a><br>
  <a href="tel:+13115552368">(311) 555-2368</a>
</address>
```

以上标签都是我们在网页中常见到的。还有很少可以在网页中见到的HTML5新标签。在这里我就不一一赘述了。大家可以参考这个链接去查阅相关资料：[新标签](https://developer.mozilla.org/zh-CN/docs/Web/Guide/HTML/HTML5/HTML5_element_list)

#### 新的表单特性

HTML5新增了新的表单元素

- datalist
- keygen
- output

##### datalist

该元素规定输入域的选项列表。规定了form或input应该拥有自动完成功能，都能够用户在自动完成域中开始输入是，浏览器应该在该域中显示填写的选项：

```html
<!-- list和datalist 中id想连接 -->
<form action="">
    <input type="text" list="class">
    <datalist id="class">
    <option value="hello world"></option>
    <option value="hello web"></option>
    <option value="hello Go"></option>
    <option value="hello python"></option>
    </datalist>
    <input type='submit'/>
</form>
```

![](D:\web\Typora\HTML5\static\images\2.png)

##### kegen(了解)

是提供一种验证用户的可靠方法，当提交表达时，会生成两个键，一个是私钥，一个是公钥。

私钥存储于客户端，公钥则被发送给服务器。公钥可用于之后验证用户的客户端证书

```html
<form action="hello.asp" method="get">
用户名: <input type="text" name="usr_name">
加密: <keygen name="security">
<input type="submit">
</form>
```

##### output

用于不同类型的输出，比如计算或脚本输出

```html
<form oninput="x.value=parseInt(a.value)+parseInt(b.value)">
    0<input type="range" id="a" value="50">100 +
    <input type="number" id="b" value="50">=
    <output name="x" for="a b"></output>
</form>
```

#### HTML5新的表单属性

##### form新属性

**autocomplete属性**

此属性规定form或input应该拥有自动完成功能

当用户在自动完成域中开始输入时，浏览器应该在该域中显示填写的选项

```html
<form action="" autocomplete="on">
    用户名: <input type="text" name="usr_name" >
    <input type="submit">
</form>
```

**novalidate属性**

是一个布尔值，当为true时，表示规定在提交表单时，不应该验证form或input域

如果给input的type改成email。则我们在输入邮箱时通常自动验证。

```html
<form action="" autocomplete="on" novalidate="">
    用户名: <input type="text" name="usr_name" >
    email: <input type="email">
    <input type="submit">
</form>
```

##### input新属性

**autofocus属性**

在页面加载时，是否自动获得焦点

```html
用户名: <input type="text" name="usr_name" autofocus>
```

![](D:\web\Typora\HTML5\static\images\3.png)

**formaction属性**

该属性用于描述表单提交的URL地址。会覆盖form元素中的action属性。该属性用于`type='submit'`。

```html
<form action="" autocomplete="on" novalidate="">
    用户名: <input type="text" name="usr_name" autofocus>
    email: <input type="email">
    <input type="submit" value='提交到当前服务器'>
    <input type="submit" formaction="http://www.baidu.com" value='提交到百度服务器'>
</form>
```

**formenctype属性**

该属性描述了表单提交到服务器的数据编码(只对form表单中 method=’post‘表单)

第一个提交按钮已默认编码发送表单数据，第二个提交按钮以 “multipart/form-data” 编码格式发送表单数据:

```html
<form action="" autocomplete="on" novalidate="" method='post'>
    用户名: <input type="text" name="usr_name" autofocus>
    email: <input type="email">
    <input type="submit" value='提交到当前服务器'>
    <input type="submit" formenctype="multipart/form-data" value="以 Multipart/form-data 提交">
</form>
```

**formmethod属性**

formmethod 属性定义了表单提交的方式。

formmethod 属性覆盖了 `<form>`元素的 method 属性。

**注意:** 该属性可以与 type=”submit” 和 type=”image” 配合使用。

```html
<form action="" autocomplete="on" novalidate="" method='get'>
    用户名: <input type="text" name="usr_name" autofocus>
    email: <input type="email">
    <input type="submit" value='get提交'>
    <input type="submit" formmethod = 'post' formenctype="multipart/form-data" value="post提交">
</form>
```

**formnovalidate属性**

novalidate 属性是一个 boolean 属性.

novalidate属性描述了 `<input>` 元素在表单提交时无需被验证。

formnovalidate 属性会覆盖 `<form>` 元素的novalidate属性.

**注意:** formnovalidate 属性与type=”submit一起使用

```html
<form action="">
    E-mail: <input type="email" name="userid">
    <input type="submit" value="提交">
    <input type="submit" formnovalidate value="不验证提交">
</form>
```

![](D:\web\Typora\HTML5\static\images\4.png)

**formtarget属性**

formtarget属性指定一个名称或一个关键字来指明表单提交数据接收后的展示。

```html
<form action="">
    用户名: <input type="text">
    密码: <input type="password">
    <input type="submit" formtarget="_self" value="提交到一个新的页面上"> 
</form>
```

**height和width属性**

定义一个图像提交按钮，使用width和height属性

```html
<input type="image" src="img_submit.gif" width="30" height="30">
```

**list属性**

```html
<form action="">
    <input type="text" list="class">
    <datalist id="class">
        <option value="hello world"></option>
        <option value="hello web"></option>
        <option value="hello Go"></option>
        <option value="hello python"></option>
    </datalist>
    <input type='submit'/>
</form>
```

**multiple属性**

规定`input`元素可以选择多个值。适用于像input标签：file

```html
上传多个文件:
选择图片:<input type='file' name= 'img' multiple>
```

**pattern属性**

描述了一个正则表达式用于验证`input`元素的值

注意：适用于以下类型`input`标签:text,search,url,tel,email,passworld

```html
<form action="">
    请输入正确的账号: <input type="text" style="width: 200px;" placeholder="由字母,数字,下划线 组成,以字母开头 4-16位" pattern="^[a-zA-Z]\w{3,15}$" title="输入的账号不合法">
    <input type="submit" value="提交" />
</form>
```

![](D:\web\Typora\HTML5\static\images\5.png)

**required属性**

规定必须在提交之前输入框不能为空。

```html
<form action="">
    用户名: <input type="text" name="usrname" required>
    <input type="submit" value="提交" >
</form>
```

##### 新的input类型

HTML5拥有多个表单的输入类型。这些新特性提供了更好的输入控制和验证

新的输入类型

```html
color : 取色
date : 日期选择器
datetime ：选择UTC时间
datetime-local: 选择一个日期和时间(无时区)
email：提交表单时。自动验证email的值是否有效
month：选择月份
number:输入数值
range：包含一定范围内数字值的输入域
search:搜索域
tel:输入电话号码字段
time:选择一个时间
url:输入包含URL地址
week:选择周和年
```

<img src="D:\web\Typora\HTML5\static\images\6.png"  />

<img src="D:\web\Typora\HTML5\static\images\7.png"  />

```html
<form action="">
    <input type="color" value="取色器" ><br>
    <input type="date" value="日期选择器">
    <input type="email">
    <input type="number">
    <input type="range">
    <input type="search">
    <input type="submit">
</form> 
```

### HTM5中的API

#### 新的操作方法

##### 1.获取元素的方法

获取单个元素,参数可以是我们任意的选择器。

```html
document.querySelect('选择器');
```

```javascript
var oDiv = document.querySelector('header #box');
console.log(oDiv);
```

```javascript
 function $(select){
 	return document.querySelector(select);
 }
 console.log($('#box'));
```

获取多个元素，参数是任意的选择器

```html
document.querySelectAll('选择器');
```

```javascript
var oDivs = document.querySelectorAll('div');
console.log(oDivs);
```

![](D:\web\Typora\HTML5\static\images\8.png)

##### 注意：

```javascript
var oDivs = document.querySelectorAll('div');
console.log(oDivs);
// 没有foreach方法
var oDivs2 = document.getElementsByTagName('div');
oDivs.forEach(function(ele,index){
    console.log(ele);
})
console.log(oDivs2);
```

![](D:\web\Typora\HTML5\static\images\9.png)

##### 2.类的操作

**添加类**

```javascript
oDiv.classList.add('类名');
```

**移除类**

```javascript
oDiv.classList.remove('类名');
```

**检测类**

```javascript
oDiv.classList.contains('类名');
```

**切换类**

```javascript
oDiv.classList.toggle('类名');//有则删除，无则添加
```

##### 3.自定义属性

我们可以通过`data-自定义属性名`来给元素添加自定义的属性名。一旦添加完成之后。通过JS可以获取以及设置自定义属性。

比如定义一个`data-test`属性名

**获取自定义的属性名**

```javascript
oDiv.dataset.test
```

**设置自定义属性**

```javascript
oDiv.dataset.自定义属性名 = 值
```

html代码如下：

```html
<header>
    <button>切换</button>
    <div id="box" class="box active" data-test="name"></div>
    <button class="btn" data-rel = "p1">按钮1</button>
    <button class="btn" data-rel = "p2">按钮2</button>
    <button class="btn" data-rel = "p3">按钮3</button>
    <p id="p1" style="display:none;">1</p>
    <p id="p2" style="display:none;">2</p>
    <p id="p3" style="display:none;">3</p>
</header>
```



```javascript
var oBtns = document.querySelectorAll('.btn');
oBtns.forEach(function(ele,i){
    ele.onclick = function(){
        document.querySelectorAll('p').forEach(function(e){
            e.style.display = 'none';
        })
        document.querySelector(`#${ele.dataset.rel}`).style.display = 'block';
    }
})
```

```javascript
var oDiv = document.querySelector('#box');
var oBtn = document.querySelector('button');
console.log(oDiv.dataset);//DOMStringMap {test: "name"}
console.log(oDiv.dataset.test);//name
oDiv.dataset.age = 38;

console.log(oDiv.__proto__);//如下图所示 html:34
console.log(oDiv.classList);//classList为属性对象   html:35

oDiv.classList.add('container');
oDiv.classList.remove('box');

console.log(oDiv.classList.contains('box'));
```

![](D:\web\Typora\HTML5\static\images\10.png)

![](D:\web\Typora\HTML5\static\images\11.png)

```javascript
oBtn.onclick = function(){
    // if(oDiv.classList.contains('list')){
    //     oDiv.classList.toggle('list');
    // }else{
    //     oDiv.classList.toggle('list');
    // }

    oDiv.classList.toggle('list');
}
```

![](D:\web\Typora\HTML5\static\images\12.png)

#### 文件读取

读取文件，首先先得将文件上传，可以通过input的type为file的表单控件实现

```html
<input type='file' name=''>
```

其次，通过FileReader读取文件。读取完文件之后，会将结果存储在result属性中，而不是直接返回

```javascript
//创建读取器
var reader = new FileReader();
```



##### FileReader常用方法

```javascript
1.readAsBinaryString: 将文件读取为二进制编码
2.readAsDataURL: 将文件读取为DataURL
3.readAsText:将文件读取为文本
```

##### FileReader提供的事件

```javascript
1.onabort
读取文件中断时触发
2.onerror
读取文件出错时触发
3.onload
读取文件完成时触发，只在读取成功后触发
4.onloadend
读取文件完成时触发，无论读取成功或失败都会触发
5.onloadstart
读取文件开始时触发
6.onprogress
正在读取文件
```

##### 读取文件实例

```javascript
<script type="text/javascript">
//1.获取元素
var fileInput = document.querySelector('input');
//2.监听选中
fileInput.onchange = function(){
	//3.获取文件
    // console.log(this.files[0]); //html:16
    var file = this.files[0];
    //4.创建读取器
    var reader = new FileReader();
    // console.log(reader);
    // 1.readAsBinaryString: 将文件读取为二进制编码
    // 2.readAsDataURL: 将文件读取为DataURL
    // 3.readAsText:将文件读取为文本

    //5.开始读取
    reader.readAsText(file);//undefined
    //6.监听文件的读取状态
    reader.onload = function(){
    	console.log(reader.result);//读取结果都会存到result属性中
    }
}
</script>
```

![](D:\web\Typora\HTML5\static\images\13.png)

console.log(this.files);得到的结果的如下图所示：

![](D:\web\Typora\HTML5\static\images\14.png)

#### 获取网络状态

`window.navigator.online器的网络状态。联网状态返回true,断网状态时返回false。

```javascript
<script type="text/javascript">
    // 获取当前网络状态
    var state = window.navigator.onLine;
    if (state) {
    	alert("联网状态");
    }
    else {
    	alert("断网状态");
    }
</script>
```

#### 地理位置定位

地理位置api允许用户向web应用程序提供他们的位置。处于隐私考虑，报告地理位置前会先请求用户许可

console.log(navigator);得到的结果如下

![](D:\web\Typora\HTML5\static\images\16.png)

##### geolocation对象

地理位置API通过`navigator.geolocation`提供

```javascript
<script type="text/javascript">
	if(navigator.geolocation){
    	/*地理位置服务可用*/
        navigator.geolocation.getCurrentPosition(function(position){
        	console.log(position);//html:13
            //coords: GeolocationCoordinates {latitude: 34.0522342, longitude: -118.24368489999999, altitude: null, accuracy: 27541, altitudeAccuracy: null, …} timestamp: 1599228998922
            position.coords.latitude;//经度
            position.coords.longitude;//纬度
        })

	}else{
    	/*地理位置服务不可用*/
   	}
</script>
```

![](D:\web\Typora\HTML5\static\images\15.png)

##### 百度地图定位

所以，为了实现定位，我们还是用第三方的好啊。比如 [百度地图开发平台](http://lbsyun.baidu.com/index.php?title=jspopular3.0)，真的！！这个平台上想要什么都有。

```html
<!DOCTYPE html>
<html>
<head>
    <meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
    <meta name="viewport" content="initial-scale=1.0, user-scalable=no" />
    <style type="text/css">
    body, html,#allmap {width: 100%;height: 100%;overflow: hidden;margin:0;font-family:"微软雅黑";}
    </style>
    <script type="text/javascript" src="http://api.map.baidu.com/api?v=2.0&ak=V0SIZMbnrOVWH5HWvQqoYG3IPQxVlvTf"></script>
    <title>地图展示</title>
</head>
<body>
    <div id="allmap"></div>
</body>
</html>
<script type="text/javascript">
    // 百度地图API功能
    var map = new BMap.Map("allmap");    // 创建Map实例
    map.centerAndZoom(new BMap.Point(120.75, 30.75), 15);  // 初始化地图,设置中心点坐标和地图级别
    //添加地图类型控件
    map.addControl(new BMap.MapTypeControl({
        mapTypes:[
            BMAP_NORMAL_MAP,
            BMAP_HYBRID_MAP
        ]}));      
    map.setCurrentCity("嘉兴");          // 设置地图显示的城市 此项是必须设置的
    map.enableScrollWheelZoom(true);     //开启鼠标滚轮缩放
</script>
```

其中您的秘钥需要到

```html
<script type="text/javascript" src="http://api.map.baidu.com/api?v=2.0&ak=您的密钥"></script>
```

![](D:\web\Typora\HTML5\static\images\17.png)

##### JSON的介绍和使用

JSON格式特点：1.书写简单，一目了然2.符合JS语法

JSON对象就是一个值，可能是数组或者对象

```javascript
{
    "key1":value1,
    "key2":value2，
    "key3":{

    }
}
```

规定：

​    1.复杂类型的值只能是数组或对象，不能是函数 正则 日期

​    2.基本数据类型的值只有4种：字符串 数值 布尔值 null 不能使用NaN Infinity undefined

​    3.字符串必须使用双引号表示，不能使用单引号

​    4.数组或者对象最后一个成员的后面，不能加逗号

```javascript
 <script type="text/javascript">
 	//1.看出JSON对象  其实数据库中的值
    var myJson = {
    	"data":{
            "key1":["one","two"],
            "age":28,
            "friends":[
                {
                    "name":"alex",
                    "age":29
                },
                {
                    "name":"mjj",
                    "age":23
                },
                {
                    "name":"jingjing",
                    "age":23
                }
             ]
         }
     }

    // 本地存储：Application->Local Storage->session Storage
    //1.把JSON对象转换成JSON字符串
    var jsonStr = JSON.stringify(myJson);
    console.log(jsonStr);//html:50
    console.log(typeof jsonStr);//html:51

    //2.将JSON字符串转换成JSON对象
    var json = JSON.parse(jsonStr);
	console.log(json.data);//55
    console.log(json.data.friends);//html:56
    console.log(typeof json);//html:57
</script>
```

![](D:\web\Typora\HTML5\static\images\18.png)

#### 本地存储

HTML5 web存储，一个比cookie更好的本地存储方式。

随着互联网的快速发展，基于网页的应用越来越普遍，同时也变得越来越复杂。为了满足各种各样的需求，会经常在本地存储大量的数据，传统方式我们会以document.cookie来进行存储，但是由于其存储大小只有4k左右，并且解析也相当的复杂，给开发带来诸多不便，HTML5规范提出解决方案，使用sessionStorage和localStorage存储数据

##### localStorage

特点：

1. 永久存储
2. 多窗口共享
3. 容量大约为20M

**常用方法**

```javascript
window.localStorage.setItem(key,value); //设置存储的内容
window.localStorage.getItem(key); //获取内容
window.localStorage.removeItem(key);//删除内容
window.localStorage.clear(); //清空内容
```

```javascript
 var dataJson = {
     "name":"mjj",
     "pwd":123
 }
 //json对象转换为json字符串
 window.localStorage.setItem('name',JSON.stringify(dataJson));
```

![](D:\web\Typora\HTML5\static\images\19.png)

```javascript
 //获取数据显示到网页中
var jsonStr = window.localStorage.getItem('name');
console.log(window.localStorage);//html:30
console.log(jsonStr);//html:31
console.log(JSON.parse(jsonStr).name);//html:32
document.write(JSON.parse(jsonStr).name);

// window.localStorage.removeItem('name');//删除内容
// window.localStorage.clear(); //清空内容
```

![](D:\web\Typora\HTML5\static\images\20.png)

##### sessionStorage

1. 生命周期为关闭当前浏览器窗口
2. 可以在同一个窗口下访问
3. 数据大小为5M左右

```javascript
window.sessionStorage.setItem(key,value); //设置存储的内容
window.sessionStorage.getItem(key); //获取内容
window.sessionStorage.removeItem(key);//删除内容
```

#### 案例_使用本地存储

html代码：

```html
 <div class="login">
     <p>
         <label for="user">用户名：</label>
         <input type="text" name="user" id="user">
     </p>
     <p>
         <label for="pwd">密码：</label>
         <input type="password" name="pwd" id="pwd"> 
     </p>
     <p>
         <span class="showName"></span>
         <input type="button" value="login">
         <input type="button" value="logout">
     </p>
 </div>
```

javascript代码：

```javascript
	<script type="text/javascript">
        window.onload = function(){
            //1.获取标签
            function $(select){
                return document.querySelector(select);
            }

            isUserInfo();
            //当前用户显示的函数
            function isUserInfo(){
                //(1)获取当前存储的内容，看它是否存在
                var userInfoStr = window.localStorage.getItem('name');
                var userObj = JSON.parse(userInfoStr);

                if(userObj){
                    //(2)表示用户已经登录
                    $('.showName').style.display = 'inline-block';
                    $('.showName').innerHTML = userObj.username;
                    $('input[value=login]').style.display = 'none';
                    console.log('用户登录');
                }else{
                    //用户没有登录
                    console.log('用户未登录');
                    $('.showName').style.display = 'none';
                    $('input[value=login]').style.display = 'inline-block';
                }
            }
            
            //2.监听登录按钮的点击事件
            $('input[value=login]').onclick = function(){
                //3.获取用户名和密码
                var username = $('input[name=user]').value;
                var pwd = $('input[name=pwd]').value;
                if(username && pwd){
                    //4.将json对象转换成json字符串
                    var jsonStr = JSON.stringify({
                        "username":username,
                        "pwd":pwd
                    })
                    //5.用户名和密码保存到localStorage中
                    window.localStorage.setItem('name',jsonStr);

                    //6.显示当前用户名到网页中，并且登录按钮不存在
                    isUserInfo();

                    //7.清空输入框
                    $('input[name=username]').value = '';
                    $('input[name=pwd]').value = '';

                }
            }
            //退出操作
            $('input[value=logout]').onclick = function(){
                window.localStorage.clear();
                isUserInfo();
                $('input[name=username]').value = '';
                $('input[name=pwd]').value = '';
            }

        }
    </script>
```

![](D:\web\Typora\HTML5\static\images\21.png)

