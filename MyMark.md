# 轮播图

## 1.轮播图特效的两种方式

- 方式1:将ul设置成position: absolute;其父盒子设置成position: relative;ul中的li浮动(float: left;),然后用js改变ul的left值,来进行轮播效果
- 方式2:利用css3中的transform: translateX();来控制ul的移动,达到轮播效果
- 方式3:利用函数在屏幕改变时,动态获取的li的高度,并赋给ul的父盒子,此时可以动态给ul添加绝对定位,然后以其left值来做轮播效果
```css
    function resize(){
     	  var bannerHeight = liList[0].offsetHeight;
      	document.getElementsByClassName("banner")[0].style.height = bannerHeight + "px";
    }
    window.onresize = resize();
    resize();
```   

注:在移动端中,使用后者较好,因为前者需要给ul的父盒子添加具体的高度,不然移动端ul后面展示的内容会将ul覆盖掉;加了固定高度后,在另一个不同屏幕分辨率的移动端中,显示又会出现问题,所以此时选用方式1;PC端上皆可。

# ajax

## 1.服务器

- 网页服务:apache/nginx/tomcat/iis 
- 文件上传下载服务:vsftp
- 邮件服务:posfix
```
var qq = {} 
	qq放到了栈内存(放的东西较小);{}里东西放到了堆内存中(较大)
qq = null;/free qq;		
	释放了qq这个对象的内存
```
- 死循环中无限创建变量(对象)会占用内存直至内存耗满,导致电脑死机卡顿
程序一开始是在磁盘上的,想要运行必须首先加载在内存  	 CPU→内存→磁盘

## 2.通信协议

- http/ftp(文件传输协议 文件上传下载时出现)/smtp或pop3(邮件传输协议)
ssh(远程登录协议)

- 多方之间的通信
	- 计算机IP地址:xxx.xxx.xxx.xxx,每个数字不大于255,相当于每个计算机的名字
	- 端口:用来区分电脑上的特定的应用的网络应用程序
	- 域名:(http://www.baidu.com),IP地址的别名,在美国维护,最新的ipv6有128位
	- 多个域名可以指向同个ip,一个ip可以有多个域名

- 访问http//www.baidu.com时
	- 1.域名解析(DNS域名系统)
	- 2.通过解析得到IP地址找到对应的计算机

## 3.网络开发环境:

- 测试环境:Test						
- 开发环境:Development					
- 生产环境:Production					

## 4.WAMP

- Windows  				操作系统
- Apache 				提供网页服务的应用程序
- Mysql 				数据库 (mysql/oracle/sqlserver)
- Php					后台编程语言  做动态网站 (.net/jsp/Python)
- 计算机编程语言两种类型:
 	- 解释型	:	javascript/java/php
	- 编译型	:	c/c++

## 5.网站:一系列的网页

- 静态网站
	- 弊端:在网站数量多的时候,更新维护不方便
- 动态网站
	- 原理:动态生成html页面	
```	
php命名方法和js一样;php中连接字符串是.. 不是js中的++
<?php 
$flag = $_GET['param'];
if($flag==1){
	echo "<div>苹果</div>";
}else if ($flag==2){
	echo "<div>香蕉</div>";
}else {
	echo "<div>橘子</div>";
}
?>
```
- B/S:browser 浏览器;server 服务器	
	- 有内容更新,在没有发放到客户端时,可以及时更改
- C/S:client 客户端(应用程序:qq迅雷);server 服务器  	
	- 有内容跟新,客户端必须要更新才能看到,不更新则不可以看到

## 6.同步和异步

- 浏览网页的时候出现的情况
	- 1.白屏(同步 彼此等待--阻塞)				
	- 2.页面不刷新(异步 各做各的--非阻塞) 

- 通过XHR(xml http request)向服务器提出请求,同时能够继续干别的事,实现异步局部更新(不使用XMLHttpRequest)或者使用iframe标签

注:原生ajax要做浏览器兼容

## 7.ajax

[asynchronous javascript and xml  异步js和xml(可扩展标记语言)]

### 1.XMLHttpRequest对象 

- header("Content-Type:text/html;charset=utf-8");php文件中出现乱码也要写这个头部
- header("location:地址");提交后还是待在原页面中

### 2.创建对象()

- var xhr = createXHR()

### 3.准备发送请求,配置发送请求的一些行为

- xhr.open(方式,地址,true/false)						
(方式:get/post/...;地址:.php/...文件)
```
注意:
	当使用ajax请求的open中的async=false时,此时为同步请求,不要写onstatereadychange这个回调函数,将数据处理语句直接写到send()函数之后即可。
例如:
	xhr.open("GET","test1.txt",false);
	xhr.send();
	document.getElementById("myDiv").innerHTML
	=xhr.responseText;
```
- 常用的请求方法(get,post,put,delete),其中get和post用的较多
	- get:
		- 会获取数据 会在地址栏后面出现传的参数,所以传的参数数量是有限制的;
		- get获得的参数是中文时,在低版本中如果出现乱码需要用 
	encodeURIComponent()包裹一下就行了;
		- get没有请求体,发送的数据在地址栏中，使用request.send(null)
		- get可以通过在请求URL上添加请求参数
		- 所有的请求都是get请求
		- get发送的数据在地址栏中
	- post:
		- 加密性的数据或数据量大或隐私性的数据用post
		- Post发送的数据在请求体中,用户看不到
		- 可以通过request.send('name=itcast&age=10')
		- 需要设置请求头:request.setRequestHeader('content-type','application/x-www-form-urlencode'),作用是告诉服务器,发送给服务器的数据格式,和url的参数格式一样
		
### 4.回调函数

- xhr.onreadystatechange=function(){}					
	- 指定一些回调函数:js是单线程的执行,比如延时1s执行,那么就要等线程上的所有代码执行完毕后,该函数才能延时1s执行,即使该函数执行前,线程走了2s,也是要等2s后再延时1s执行
	- xhr.readyState == 0~4
		- 回调函数外面只有0和1;浏览器控制的5中状态:完成创建xhr对象后==0;发送的初始化完成,但没有发送==1;已经发送完成==2;服务器已经返回了数据(但不可使用)==3;服务器返回的数据已经可以使用==4
	- xhr.status:		
		- 200:http请求成功 400:语法错误导致服务器不识别 401:请求需要用户识别 404:页面找不到 500/503:服务器出错
	- var data = xhr.responseText/responseXML;
		- 当同时满足上述两个条件时,才执行这条语句,获得数据格式:
		json:TEXT获得的主要是json文本数据,获得的是个字符串;
		XML:XML获取的是整个文档的全部内容,是个对象,但是信息量大,有可能辅助标签比实际内容要多,解析起来比较费劲
							
### 5.执行发送的动作

- xhr.send(null)										 
	- 位置可以放到前面,也可以放到最后 加null是为了兼容老版本的浏览器,新的浏览器可以不需要加null
```
<a href="#" onclick="javascript:void(0)"></a>
在标签内阻止a标签的默认事件
```
### 6.两种数据格式

- xml格式												
	- XML获取的是整个文档的全部内容,是个对象,但是信息量大,有可能辅助标签比实际内容要多,解析起来比较费劲
	- 比json更加通用的网络协议,在所有的语言中都通用,描述了数据的格式(html语言类似)
	
- json格式						
	- 通过字面量来表示一个对象,从简单到复杂都可以使用,多用于前端	
	- JSON.parse(字符串);将字符串转换成对象(登录注册,验证表单时向后台传递数据时,要用这个转换)
	- eval("字符串") 将字符串转换成对象 但是有安全性的问题(可能会传来非法的js代码,传来的代码可以执行)						
	- JSON.stringify(对象);将对象转换成字符串javascript objection notation javascript对象标记语言

- 服务器响应
	- responseText:获得字符串形式的响应数据
		- 然后使用上面的JSON.parse()/eval()来将该字符串转换成对象格式,进行操作
	- responseXML:获得XML形式的相应数据
```
XML格式的响应数据及写法:
XML数据:
<bookstore>
	<book category="children">
		<title lang="en">Harry Potter</title>
		<author>J K. Rowling</author>
		<year>2005</year>
		<price>29.99</price>
	</book>
	<book category="cooking">
		<title lang="en">Everyday Italian</title>
		<author>Giada De Laurentiis</author>
		<year>2005</year>
		<price>30.00</price>
	</book>
	<book category="web" cover="paperback">
		<title lang="en">Learning XML</title>
		<author>Erik T. Ray</author>
		<year>2003</year>
		<price>39.95</price>
	</book>
	<book category="web">
		<title lang="en">XQuery Kick Start</title>
		<author>James McGovern</author>
		<author>Per Bothner</author>
		<author>Kurt Cagle</author>
		<author>James Linn</author>
		<author>Vaidyanathan Nagarajan</author>
		<year>2003</year>
		<price>49.99</price>
	</book>
</bookstore>
XML数据处理写法:
<script type="text/javascript">
function loadXMLDoc(){
	var xmlhttp;
	var txt,x,i;
	if (window.XMLHttpRequest){
		// code for IE7+, Firefox, Chrome, Opera, Safari
		xmlhttp=new XMLHttpRequest();
	}else{
		// code for IE6, IE5
		xmlhttp=new ActiveXObject("Microsoft.XMLHTTP");
	}
	xmlhttp.onreadystatechange=function(){
  		if (xmlhttp.readyState==4 && xmlhttp.status==200){
    		xmlDoc=xmlhttp.responseXML;
    		txt="";
		    x=xmlDoc.getElementsByTagName("title");
   			for (i=0;i<x.length;i++){
   				txt=txt + x[i].childNodes[0].nodeValue + "<br />";
      		}
   		document.getElementById("myDiv").innerHTML=txt;
   	}
}
	xmlhttp.open("GET","/example/xmle/books.xml",true);	
	xmlhttp.send();
}
</script>
```
	
### 7.try-catch函数

- try{}catch(e){}finally{}
	- 如果try中的代码出错try中的代码不执行了,执行catch中的代码,并继续执行
- 异常捕捉
	- catch中的代码也出错了,那么整个代码都不执行了,console.log(e)可以将错误打印出来 e.massage属性
- finally里的代码总会执行
- 抛出异常:throw new Error(error)[异常对象]
- with(对象){方法}:重复同一对象的方法,减少了对象的书写

## 8.javascript代码运行分两个阶段

- 1.预解析:将所有的函数定义,变量声明提前,变量的赋值不提前var fn = function 会将var fn提前但不会赋值 所以此方法不能先调用后执行 
- 2.执行:从上到下执行  (setTimeout;setInterval;ajax中的回调函数;绑定事件除外)
	- 有名函数在前在后调用都可以执行,因为javascript执行到该调用的函数时,会先将该函数名对应的函数加载出来执行
		- var f=function只能在后面调用,不能放到var之前调用
		- var f=new Function(多个参数,执行语句);	

## 9.artTemplate模板引擎	

```
<a href="http://libs.baidu.com/jquery/1.10.2/jquery.min.js"></a>
```	

#跨域

```
相关链接:http://blog.csdn.net/joyhen/article/details/21631833
```

## 使用script的src属性和jsonp来进行跨域

- 代码如下:

```css
这是一个跨域获取车站信息的demo
核心是script标签不受同源策略的影响
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8"/>
    <meta http-equiv="Access-Control-Allow-Origin" content="*">
    <title></title>
    <script>
        function myFunc(data) {
            var box = document.getElementById("list");
            if (data.reason == '查询成功') {
                var list = data.result.list;
                var len = list.length;
                for(var i = 0 ; i < len ; i++) {
                    var lis = document.createElement("li");
                    lis.innerHTML = "车站名:"+ list[i].name + "; 电话:" +list[i].tel+"; 地址:"+list[i].adds;
                    box.appendChild(lis);
                }
            }
        }
    </script>
</head>
<body>
<input type="text" id="inp"/>
<input type="button" id="btn" value="加载"/>
<ul id="list"></ul>
<script type="text/javascript">
    window.onload = function () {
        var inp = document.getElementById('inp');
        var btn = document.getElementById('btn');
        btn.onclick = function () {
            var script = document.createElement('script');
            script.src = "http://op.juhe.cn/onebox/bus/query?station=" +
                inp.value +   "&key=5f00c660627facda64e9a6f73beaac49&format=json" +
                "&callback=myFunc";
            var head = document.getElementsByTagName('head')[0];
            head.appendChild(script);
        }
    }
</script>
</body>
</html>
```

- 相关链接:http://blog.csdn.net/joyhen/article/details/21631833

## 使用原生ajax

- 这种方法目前需要服务器端和前端共同都需要有个声明,就是
	- 服务器端在输出数据的时候,需要在前面加上header（""Access-Control-Allow-Origin: "*");
	- 前端meta上加上 <meta http-equiv="Access-Control-Allow-Origin" content="*">这个标签
	
## 使用iframe的src属性

- 这种方法可以将数据直接加载到页面中,但是不能进行处理,数据保持的是后台传来的数据格式,可以用eval()方法来解析,但是解析出的东西,用typeOf来检测是string格式,但依然不可以进行处理.

# canvas

## 1.canvas设置宽高在其标签内设置,不建议用css设置。

```css
	<canvas width="600' height="600"></canvas>
	或通过canvas对象的属性来设置
	<script>
		canvas.width = 600;
		canvas.height = 600;
	</script>
```

## 2.canvas的兼容性问题

- canvas是h5新出的标签,IE9以上可以支持,IE9以下的版本浏览器会自动将canvas标签看成一个div标签,不执行里面的效果,可以在canvas标签内写上提示升级浏览器的文字,移动端用的多

```css
	<canvas>
		您的浏览器版本过低,请升级浏览器
	</canvas>	
```

## 3.canvas方法(绘制出的是矢量图,不失真)

```css
	canvas.getContext('2d')				获取canvas的上下文(2d)
	ctx.moveTo(x,y);					画笔移动到点(x,y)位置
										不设置moveTo当前画笔没用位置
	ctx.lineTo(x,y);					直线绘制到点(x,y)
	ctx.closePath();					闭合路径	
	ctx.lineWidth = 					描边的宽度(内外各一半宽)
	ctx.strokeStyle = ""				描边的样式
	ctx.stroke();						描边
	ctx.fillStyle = 					填充样式
	ctx.fill();							填充之前绘制的路径		
	ctx.beginPath();					
	开启一个新状态,当前的状态可以继承之前状态的样式;
	但是当前状态设置的新的样式只能影响当前的状态的样式,不影响属性;
	没有beginPath(),新写的样式(style)会将之前的样式给层叠掉,设置的属性不会层叠;
	绘制新的图时,使用,绘制的第一条图前面的beginPath()可以省略,但是不建议
	ctx.rect(x,y,宽,高)					绘制矩形(xy坐标,矩形宽高)
										需要写ctx.stroke()
	ctx.strokeRect(x,y,宽,高)			绘制矩形并描边
	ctx.clearRect(x,y,宽,高)				清除矩形中的内容(从x轴,y轴
开始将canvas宽高的内容给全部擦除)或者用canvas.width=canvas.width重新设置canvas画布的宽高(里面的所有东西都会被清除,不建议使用)
	ctx.arc(x,y,r,sAngle,eAngle,counterclockwise)
	绘制圆形(xy坐标,半径,起始角度,结束角度,逆时针绘制(ture/false)) 
	注:角度都是弧度制 转换公式  弧度 = 度数 * Math.PI / 180;
	ctx.strokText("文字",x,y)			绘制空心的文字
	ctx.fillText("文字",x,y)				绘制填充的文字
设置填充字体的样式
	ctx.font =							设置字体大小,字体类型等属性
	ctx.textBaseline =					设置字体底线对齐绘制的基线(middle/top/bottom/Alphabetic)
	ctx.textAlign =						设置字体对齐方式(start/end/middle/left/right)
	Math()函数的正弦,余弦值都是弧度制
	Math.sin()/Math.cos()等
	ctx.measureText()					返回括号内的文本宽度
文字绘制的x轴和y轴坐标公式
	 x = x0 + Math.cos(弧度值) * (r + 20); //获取文本的x轴坐标
     y = y0 + Math.sin(弧度值) * (r + 20); //获取文本的y轴坐标
绘制图片
	var img = new Image();				创建一个图片对象和通过id来获取的图片对象一样
	img.src = 图片路径					设置加载图片的路径
	img.onload = function(){			图片加载完成后开始函数操作
	ctx.drawImage(img,x,y,width,height)	通过ctx接口绘制图片后面的
width和height拉伸图片 (图片对象,x坐标,y坐标,想要缩放的宽高)
想要等比例缩放图片公式如下:
	var oh = img.height;	//获取图片的宽高
   	var ow = img.width;		
	ctx.drawImage(img,50,50,320,320*oh/ow); //进行缩放
截取图片:
	ctx.drawImage(img,sx,sy,swidth,sheight,x,y,width,height)
	(图片,截取的x坐标,截取的y坐标,截取的宽,截取的高,放置的x坐标,放置的y坐标,放置的图片宽,放置的图片高)
变换(重点,变换之前设置的不受影响):
	ctx.scale()			将画布xy缩放(x,y)
	ctx.translate()		把画布移到(x轴,y轴)位置
	ctx.rotate()		把画布旋转(弧度)度
	ctx.save()			保存上面一次的context
	ctx.restore()		加载之前保存的context
	ctx.globalAlpha =  	将画布全局透明
		想要在变换后,继续使用正常的上下文,可以在变换前使用ctx.save()来将之前的状态保存;在变换后用ctx.restore()来将保存的状态给提出来;变换的内容还能保留下来
canvas画布渲染画布(创建两个画布,在一上面绘制完成后.将画布一的内容画到画布2的上下文的10,10的位置上)
	var canvas1 = -------
	var canvas2 = -------
 	var ctx1 = canvas1.----
	var ctx2 = canvas2.----
	ctx1.fillRect(20,20,40,40)
	ctx2.drawImage(canvas1,10,10)
画布保存base64编码内容
	canvas.toDataURL(type,options)
	type:"image/jpg";"image/png";"image/jpeg"等
	options:只有当type为image/jpeg和image/webp时才会起作用;
	使用方法是先获取想要加上这个画布的img对象,然后设置它的src属性是上面的canvas.toDataURL(t,o)就可以了
```
```
不常用(性能差,需要计算,需要执行js代码,CPU消耗就大,步入用图片代替)
	ctx.shadowColor			阴影色
	ctx.shadowBlur			阴影模糊级别(越大越接近透明)
	ctx.shadowOffsetX		阴影x轴偏移量
	ctx.shadowOffsetY		阴影y轴偏移量
	ctx.fillRect	
创建线性渐变:
var grd=ctx.createLinearGradient(x0,y0,x,y)
grd.addColorStop(0,颜色)
grd.addColorStop(.5,颜色)
grd.addColorStop(1,颜色)
ctx.fillStyle = grd
ctx.fllRect(0,0,300,300)
创建圆形渐变:
var rlg=ctx.createRadialGradient(x0,y0,起始半径,x,y,结束半径)
rlg.addColorStop(0,颜色)
rlg.addColorStop(.5,颜色)
rlg.addColorStop(1,颜色)
ctx.fillStyle = rlg
ctx.fllRect(0,0,300,300)
绘制图片背景:
ctx.createPattern(img,repeat)	指定方向重复元素
	第一个参数规定要用的图片,画布或视频元素
	第二个参数:默认repeat/repeat-x/repeat-y/no-repeat
```	

## 4.canvas绘图的步骤

- 拿到canvas标签,可以通过document.getElementById()等方式获得

- 拿到上下文getContext();

- 绘制	 

## 5.Konva库

```
进度条案例:
<script src="js/konva.js"></script> //引包
 var stage = new Konva.Stage({	//创建舞台对象
        container: "bx", //必须是id,用来存放画布
        width:window.innerWidth,	//舞台属性
        height:window.innerHeight
    });
    var layer = new Konva.Layer({//创建层
        x:0,					//层属性
        y:0,
        width:stage.width(),//获取宽高靠内部的方 height:stage.height() 法				
    })
    var x = 1/8*window.innerWidth,y = 1/8*window.innerHeight
    var maxWidth = 3/4*window.innerWidth;
	//创建进度条内部的框对象并设置属性
    var innerRect = new Konva.Rect({
        x: x,			
        y: y,
        fill: "blue",
        opacity: .6,
        width: 0,
        height: 10,
        cornerRadius: x/2,
    })
	//创建进度条外框架对象
    var outRect = new Konva.Rect({
        x: x,
        y: y,
        stroke: "blue",
        width: maxWidth,
        height: 10,
        cornerRadius: x/2,
    })
	//创建完对象要将他们添加到层上
    layer.add(innerRect);
    layer.add(outRect);
	//最后将层添加到舞台上
    stage.add(layer);
    //加载图片改变innerRect的进度
    var imgIndex = 0;
    var srcArr = ["images/0.jpg","images/1.jpg","images/2.jpg","images/3.jpg","images/4.jpg","images/5.jpg","images/6.jpg","images/7.jpg","images/8.jpg","images/9.jpg","images/10.jpg","images/11.jpg","images/12.jpg","images/22.jpg","images/23.jpg","images/24.jpg","images/25.jpg"]
    for(var i=0;i<srcArr.length;i++){
        var img = document.createElement("img");
        document.body.appendChild(img);
        img.src = srcArr[i];
		//进度条内宽度可以设置成读取图片的进度
        img.onload = function(){
            imgIndex+=1;
		//将内部宽度按照百分比来增加
            innerRect.to({
                width: maxWidth*imgIndex/srcArr.length,
            })
        }
    }
	//最后将舞台给画到canvas画布上
    stage.draw();
进度条面向对象版:
	见: F:\练习\canvas\day03
```

# 面向对象

## 0.对象的分类

### 1.内置对象
- Date
- Math
- Number
- String
- Boolean
- Null
- Error
- Cookie
- Session
- Array
- RegExp
- Object
- Function
	- 函数都是Function对象的实例
	- 对象都是函数实现的
	- 是所有对象的鼻祖
- 对象搜索机制总结:
	- 内置对象.__proto__ 指向 Function.prototype;               Function.prototype.__proto__ 指向 Object.prototype;   Object.prototype.__proto__ 指向 null
	- 实例对象.__proto__ 指向 构造函数.prototype; 
	构造函数.prototype.__proto__ 指向 Object.prototype; Object.prototype.__proto__ 指向 null
- 由上面可以看出,原型链最顶层的是 Object.prototype
- 属性搜索机制
	- 首先通过实例对象的__proto__属性在其构造函数内搜索方法,如果没有则再通过构造函数的__proto__属性一级一级的往上找,直到__proto__为null为止
	- 停止条件:系统已经设置好的,Object.prototype.__proto__为null,是所有链式属性搜索的终点
	
### 2.BOM对象

- History
	- 方法:go(-1)返回上一页;back();forward()
- Location
	- 属性:href设置或返回完整的URL;search设置或返回从问号(?)开始的URL
	- 方法:reload()重新加载完整的文档
- Screen
- Navigator
- Window
	- window内置全局属性:infinite;NaN;undefined;null
	- window内置全局方法:eval();isFinite();isNaN();parseInt();parseFloat();decodeURI();decodeURIComponent();encodeURI();encodeURIComponent();对话框:alert();confirm();prompt();窗体控制:moveBy(x,y)移动;moveTo(x,y)移动到;resizeBy(w,h)缩放;resizeTo(w,h)缩放到;scrollTo(x,y)滚动到;scrollBy(x,y)滚动 
- Document
	- 集合属性:forms[]对文档中所有的Form对象引用的数组;images[]对文档中所有的Image对象引用的数组;links[]对文档中所有的Area和Link对象引用的数组;anchors[]对文档中所有的Anchor对象引用的数组
	- 方法:getElementById();open();write();close();getElementsByTagName()

### 3.自定义对象

- 主要用于面向对象编程

## 1.创建对象的方式
	
- 方式1:json的方式创建(推荐用于存储数据的情况)

```css
	var o = {key:value};
	var arr = [1,2,3,{}];
json就是上面两种的结合方式:
	var o = {
		name: "xl",
		age: 18,
		sayHi: console.log("name" + this.name +" "+ this.age)
		//这里的this指向的是o
}
```

	- 缺点: 
		不能把json对象当成一个模版,进行new操作,只能临时使用
	- 给json对象添加属性
		o.color = "red"
		o.show = function(){} 
		javascript是个动态语言,可以添加属性和方法

- 方式2:new Object()

```css
	var o2 = new Object();
	o2.name = "xl";
	o2.show = function(){};
```

	- 缺点: 
		不能把json对象当成一个模版,进行new操作,只能临时使用

- 方式3:通过构造函数来构造一个对象

```css
	function Cat(){				//构造函数
		//在这里有个看不见的操作,就是将this指向一个空对象{} 
		//进行new操作之后,会在构造函数内部创建一个空对象,这个空对象会和构造函数的原型关联起来,再和里面的this关联
		this.age = 18;
		this.show = function(){console.log(this.age)};
		this.name = "xl";	//如果name前没有加this指向,那么这个name就会自动变成一个全局变量,变为window.name="xl",这样就会污染全局
}
		var cat = new Cat();	//普通实例对象
		当出现上面声明的时候,会在构造函数Cat的一开始,有个操作:
			将Cat里面的空对象赋值给cat  var cat = {};
			把里面的this赋值给cat      this = cat;
			还有一个关键的步骤就是		 cat_proto_=Cat.prototype
		那么cat就拥有了构造函数里面的属性和方法了
		cat.show(); 
```

- 方式4:通过普通函数的方式

	- 格式:
		var p = function(){}
		p.prototype = {
			属性:值,				实例不共享
			函数:function(){},	实例共享
			对象:{}				实例共享
			数组:[]				实例共享
		}
	- 值类型和引用类型
		- 值类型:数值、布尔值、null、undefined
		- 引用类型：对象、数组、函数

- 方式5:设计模式--工厂模式

```css
		function createPerson(data){
			var obj = new Object();
			obj.name = data.name;
			obj.des = data.des;
			....
			return obj
		}
		source//数据来自后台
		var p = createPerson(source);
		name.innerHTML = p.name //将数据填入到页面中
		和构造函数的差别:工厂模式没有显式的创建对象--不需要new;构造函数没有return语句
```

- 方式6:拷贝方式

```css
		function extend(target,source){
			for(var i in source){
				target[i] = source[i];
			}
			return target;
		}
```

## 2.原型对象

- 今后所有的对象的方法、公共的属性值和常量值都封装到构造函数的原型里去:

	在创建了一个新的对象后,新的对象会占有一定的内存,而对象的方法不需要在每个新的对象中出现,所以将对象的方法放到原型中去,新的对象中不需要存放方法了,节省了空间;而对象的属性要保留,因为是每个新对象的特性

```css
简单的版本:
	 function Person(){					构造一个Person函数
        this.age = 18;					里面有age属性
//        this.show = function(){		还有个show方法
//            console.log(this.age);
//        }
    }
//  var p = new Person();	
    Person.prototype = {	上面的show方法也可以	
		show : function(){	写到Person这个构造
        console.log(this.age);函数的原型里这创 
		}					  建	的新对象就不会占					
	}						用多余的内存;这种写
							法等同上面的写法,且					
							对内存的消耗更加小var p = new Person();先声明p对象后给Person原					 
							型加方法时,后面调用	
    p.age = 20;				这个方法是无效的
    p.show();
    console.log(p.__proto__)
```
```
升级版:
	在创建对象的时候,将该对象的值,当作一个json对象传到构造函数中,方便新对象的属性管理和设置
	function Cat(option){
		this.name = option.name;
		this.age = option.age;
	}
	var cat1 = new Cat({
		name: "zhangsan",
		age: 19
	})	
```
```
终极版:
	function Cat(option){
		this._init(option);
	}
	Cat.prototype = {
		_init:function(option){
			this.name = option.name;
			this.age = option.age;
		},
		show:function(){
			console.log(this.name);
			console.log(this.age);
		},
		call:function(){
			return 1;
		}
	}
1.当new一个新的cat对象时,首先在构造函数中创建了一个空对象,这个空对象指向了新的对象;
2.而this又指向了这个空对象,所以这个新对象就调用了_init方法,这个方法在原型对象中;
3.将Cat的原型对象定义为一个空对象,里面放置了_init方法以及其他的公用方法,这样方便管理,不会造成函数分离的现象;
4.新的对象调用_init方法后,效果和该方法内的部分写在构造函数中一样,可以说_init方法中放置的是这个对象的的属性;
5.当原型对象定义的方法函数中有返回值,那么调用这个函数,该函数就是这个返回值
```

- js中创建了个新对象,然后使用方法,会先在构造函数中找有没有这个方法,如果没有则会去它的原型对象中找;只要是在构造函数或者函数原型中设置了属性或方法,那么创建的对象都可以使用这个属性和方法,同时也可以动态的创建新的方法和属性到原型中,那么之后创建的对象就都会有这些属性和方法了

## 3.new操作符的内部原理(使用定义好的对象的方法:new实例化一个实例)

- 第一步:在内存中开辟一个空间
- 第二步:创建一个新的空对象
- 第三步:把this指向到这个空对象中
- 第四步:把空对象的内部原型(.__proto__)指向构造函数的原型对象(.prototype)
- 第五步:当构造函数执行完成后,如果没有return,那就把当前的空对象返回;如果有return{}那么创建新对象,只会有返回的对象里的属性和值;如果return的是this,效果和没有return一样

## 4.面向对象的作用

### 1.面向对象的编程

- 单个实例开发页面 
	- 订单,购物车和产品详细信息
	- 单一职责原则:每个方法封装一个功能,为的就是易于发现问题(解耦和)
-如何做对象编程

### 2.描述数据

- 对象的字面量形式,json数据,ajax交互,即和后台的交互
- 数据绑定:将后台返回的json数据和前端的元素进行绑定
	- 原生js join()方法拼接字符串
	- jQuery
	- 模板
		- 好处:不用考虑拼接
```css
1.formate模板:
	function formateString(str,data){
        return str.replace(/#\((\w+)\)/g,function(match,key){
            return data[key]});
    }
	(#可以换成别的符号都可以,如$)
    var user = {name: "zhangsan"};(参数可以是多个)
    var str = formateString("欢迎#(name)来到火焰世界",user);
    document.write(str);
2.artTemplate模板
//先引入模板
<script src="template.js"></script>
//再写模板的固定格式,有个id,并将标签、变量写进去
	注:变量用{{}}来包裹
<script id="artemple" type="text/html">
    <h4>最新电影:{{name}}</h4>
    <span>上映时间:{{year}}</span><br>
    <span><strong style="color:red">主演</strong>:{{actor}}</span>
</script>
//然后写数据,这里的数据根据arttemplate模板的template(id,数据)的方法来追加到页面上
<script>
var film = {
        name: "美人鱼",
        year: 2016,
        actor: "邓超"
    }
    var h = template("artemple",film);
    document.write(h);
</script>
```

### 3.封装框架

## 5.函数调用模式

### 1.函数执行模式

```css
	function add(a,b) {
        console.log(this);
        console.log(a + b);
    }
    add(1,2);   //this指向window
```

### 2.对象调用方法模式

```css
	function Cat() {
        this.show = function(){
            console.log(this);	this指向构造函数					创建出的实例对象
        }
    }
    var a = new Cat();
    a.show();  	// 对象调用自己的方法
```

### 3.构造器的调用模式

```css
	css
	function Cat() {
        this.show = function(){
            console.log(this);	this指向构造函数					创建出的实例对象
        }
    }
    var a = new Cat();
```

- 与第二种效果一样,定义不同:
	- 第二种通过对象自己来调用自己的函数
	- 第三种通过new来执行

### 4.call和apply调用模式

#### 1.call

- 一个对象想要调用另一个对象的方法:
	- 1.将另一个对象的方法拷贝过来使用(代码具有重复性,不建议使用)
```css
	拷贝模式
		function extend(target,source){
			for(var i in source){
				target[i] = source[i]
			}
			return target;
		}
```
	- 2.使用call()方法	
		- 用法:被借用的对象名.被借用的方法名.call(借用的对象名,被借用方法的参数)
		- 机制:call()方法不仅会借用其他对象的方法,也会将其他对象方法中的this(指针)指向自己;即call()方法能够更改对象的指针;用过后还原,方法不会保存在借用的对象中
		- 作用:
			1.借用另一个对象的方法,而不用拷贝
			2.将伪数组改成真数组

#### 2.伪数组

- 定义:一个含有length属性的json对象,不是一个真数组
	- 伪数组var json = {0:"苹果",1:"香蕉",length:2}   
	- 伪数组每次都要自己添加更改数据,并自己计算length的长度
	- 真数组var arr = ["苹果","香蕉"]					
	- 真数组可以通过pop,shift等方法更改数据,且自动生成其length属性

- 哪些是伪数组
	- arguments;document.getElementsByTagName;document.getElementsByClassName;jQuery框架就是通过伪数组实现的$(".class")等获取的都是伪数组 
	- 如何将伪数组改为真数组: var 变量名(接收真数组) = Array.prototype.slice.call(伪数组对象名)

#### 3.apply

- call和apply的功能一样
	- 唯一区别:
	- call传递参数是平铺的
		被借用的对象名.被借用的方法名.call(借用的对象名,参数1,参数2,参数3...)
	- apply是通过数组来传递参数的
		被借用的对象名.被借用的方法名.apply(借用的对象名,[参数1,参数2,参数3...])

#### 4.arguments对象

- 只有当代码运行的时候才起作用
- arguments是一个伪数组数组,保存函数的实参
- caller判断函数是否被别的函数调用
- callee是arguments对象的一个属性,表示函数对象本身的引用  	arguments.callee.length可以获取实参参数个数
	- 用法:
		- 1.判断实际参数和形参是否一致                        	
		- 2.用于递归算法:一个方法自己调用自己,用上一次计算的结果作为这次的参数
	- callee方式做递归
		var fn = (function(n){	
			if(n>0) return n+arguments.callee(n-1);
			return 0;
		})(10)	
		alert("callee方式"+fn)
	- 传统方式做递归
		var fn = function(n){
			if(n>0) return n+fn(n-1);	
			return 0
		}
		alert("传统方式"+fn(10))
	传统方式的缺点:
		1.当函数名更改时,需要更改的地方很多
		2.fn是个全局变量,fn内部一般使用局部变量,而这里是一个全局变量,这是一个潜在的全局变量污染

## 6.json

### 1.什么是json

- json是前端用的较为广泛的js语法点,可以通过字面量来表示一个对象
	- js的字面量形式(name,age可以加引号也可以不加,json一定要加,json的格式就相似于js字面量的格式)
- 字面量其实就是原型对象的一个实例,在使用对象的字面量形式(json)的时候,不需要再实例化
```css
	var obj = {
		name:"obj1",
		age:10,
	}
```

### 2.json协议和json对象的区别

- 二者毫无关系
- json协议是一种国际标准,只要我们将数据转换成这个格式,就能实现传输
- json对象是js中的一个语法点

## 7.面向对象的三个性质

### 1.封装性

- 就是把普通的对象进行封装，对象的属性设为私有的，对外提供get和set方法，其他类只能通过get和set对对象属性值进行操作。
	- 函数就是工具,对象就是工具包,好处是隐藏细节,隐藏复杂
	
### 2.继承性

- 是发生在两个类之间，一个类继承另一个类是说这个类属于另一个类，具有另一个类的所有属性和方法，同时它还可以有另一个类不具备的方法和属性。
	- 父类 == 基类;子类 == 派生类
	- 继承的方式
		- 构造函数继承
		- 组合继承
		- 拷贝继承
		- 寄生组合继承
	- 继承的格式(组合继承)
```css
		var Person = function(){
            this.name = "jjj";
        }
        Person.prototype = {
            say:function (){
                console.log("fuc");
            }
        }
        var Student = function(){
            Person.call(this,arguments);			   //重要
            this.age=19;
//            this.sayHi =function(){
//                console.log("yyy")
//            };
        }
        //继承了基类/父类的方法
                //注:这句话将Student的实例通过__proto__属性指向了Student.prototype
                    //因为Student.prototype = new Person();即等于Person构造函数的一个实例
                    //Student.prototype.__proto__属性指向了Person的一个实例
                    //Person的实例通过__proto__属性指向了Person.prototype
                    //Person.prototype.__proto__属性指向了Object.prototype
                    //这就是继承的原型链
        Student.prototype = new Person();				//重要
        //拥有了自己的方法
        //注:给自己添加方法时,用分散的添加方式,不要用Student.prototype = {}的方式
        Student.prototype.sayHi = function(){
            console.log("hahaha")
        }
        /*这样写,改变了Student的原型对象,之后实例化的对象无法访问到Person中的属性和方法
        Student.prototype = {
            age:19,
            sayHi:function(){
                console.log("yyy")
            }
        }
        */
        var s1 = new Student();
        console.log(s1.name,s1.age)
        s1.say()
        s1.sayHi()
                //实例的prototype属性是undefined
        console.log(s1.__proto__)
```
	- 存在的问题
		- 在Student构造方法中,无法使用new Student(参数)创建对象的方式来传递参数,初始化属性
		- 只能通过点的形式进行赋值  实例对象.属性	
		
### 3.多态性

- 是建立在继承的基础上的，一个父类对象可以产生多个不同的子类对象，根据这些子类对象的不同可以具备不同的方法，也就是说表现出了不同的形态即多态
 	- 一件事情多种形态:
	 	- 一场比赛结束后,更新比赛结果信息,更新队员信息,更新教练信息,更新赛程信息,更新排名信息;队员,教练,比赛,排名等都可以放在一个对象里,都有一个更新的方法,但是更新的内容不一样
		- 封装一个计算形状的周长,其内部计算的方法不一样,这也是多态
	- 实现多态的方法
		- 方法多态
		- 继承多态
			- 比如购买:电子书的购买步骤(有个开通看全书的步骤)和衣服的购买方式(有物流的步骤)不同
		  	- 动物多态:鸡 鸭 鱼 人等

### 4.重载

- 一个方法,名字相同,通过传入的参数个数不同或参数类型不同,执行不同的功能
- js没有重载功能,但能通过arguments和typeof来达到重载的效果
	- arguments方法:
```css
		function add1(){
			 var sum = 0;
			 for(var i=0;i<arguments.length;i++){
				sum+=arguments[i];
			}
			 return sum;
		}
		console.log(add1(1,2))		//3
		console.log(add1(1,2,3))	 //6
```
	- typeof方法:
```css
		var myClass = function (){
			var addNum = function(a,b){
				 return a+b;
			 };
			var addString = function(a,b){
				return "i am here" + a + b;
			 }
			 this.add = function (a,b){
					if(typeof(a) == 'number' && typeof(b) == 'number'){
					  return addNum(a,b); //return不能丢,返回的是一个内部函数执行后的结果
				 }else{
					  return addString(a,b);
				}
			}
		 }
		 var s = new myClass()
		 console.log(s.add(1,2))
		 console.log(s.add("jjjjsj",2))
```

## 8.拓展

  前端分为设计师(UI设计),制作师(切图),前端开发工程师(和后台的交互)

### 1.代码编写的方式

- 函数===工具;对象===工具包
- 传统方式:整个代码时一坨一坨的
- 对象方式:一个一个零件写,然后对他们进行组装
- 针对单个对象,整体进行编程
- 属性:整个js都是面向对象的
- 工具:函数 -- 方法(学名)
- 如何使用对象中的方法(工具)
	- 点语法 
	document.getElementById()

### 2.万物皆对象

- 所有的函数,全部都是某个对象的方法
整个js是一种全面向对象的语言
- 对象的好处:
	- 将一些相类似的函数(工具)进行分类管理,比如docunment
- 封装的好处:
	- 将对象的函数,方法进行封装细节,使用的时候只要输入相应的参数即可,方便使用

## 9.写好代码然后打开浏览器发生了什么

- 1.电脑将代码转换成二进制数字
- 2.将二进制数字写到内存中
- 3.CPU读取内存中的二进制数据,通过一大堆复杂程序显示成页面

当我们实例化的时候,其实就是将构造函数的属性拷贝一份,同时在内存中开辟一个新的区域来保存这些值
但不会拷贝原型对象中的属性
构造函数是不分配内存的,只有实例化才会分配内存

- 对象实例化的过程发生了什么:
	- 对象实例化的过程就是拷贝对象原型属性的过程;
	- 同时,还会自动生成一个constructor属性,用于识别是根据哪个构造函数创建的实例constructor是构造函数的一个隐藏属性一旦实例给了值,那么就会将内存空间的值给替换掉
- 为什么实例有constructor这个属性
	- 因为实例的属性都是拷贝自构造函数,constructor是构造函数的隐藏属性,包含定义的也包含自动生成的属性

- 指针:定义了一个变量,在内存中的栈和堆中分别开辟一段段区域,堆中保存变量的值,栈中保存变量名,这里保存的是它的值在堆中的地址,一般通过这个地址找到这个值,这个地址叫做指针
- 实例在属性搜索时,由于拷贝了构造函数的属性,所以能够在实例中直接使用这些属性,而原型对象中的属性(方法)能够访问到,是因为在实例中拷贝了一个隐藏属性(__proto__),该属性储存了原型对象的地址,链接了原型对象,从而找到了该属性

- 对象是如何在内存中储存的
	- 在内存中会开辟两端区域,一片区域(栈)储存变量名(地址),一段区域(堆)才储存对象的真正值,地址值很小,不占空间

- 原型对象不管实例化多少次,原型对象都只生成一次
var str = ""会自动转换成String对象,具体在什么时候转换要看浏览器引擎

## 10.对象法则

### 1.双对象法则

- 对象包含的两个独立的对象:构造函数对象和原型对象;
- 他们通过实例中的__proto__属性连接起来(V8,火狐 最新的微软浏览器支持);
- 早期的微软浏览器不支持,__proto__成为ES5的一种标准

### 2.铁锁连舟(原型链)

- 首先遍历自己的属性(从构造函数那拷贝来的属性),如果找到则返回
- 如果没找到,则根据原型链(__proto__属性)找到原型对象,一次遍历原型对象中的属性,如果找到同名属性则返回
上面的链式访问形式叫做原型链

### 3.属性屏蔽理论

- 构造函数和原型函数中有相同属性时,则返回构造函数的属性值
- delete 对象.属性名	构造函数和原型函数中有相同属性时,那么delete的是构造函数中的值,这样就可以访问到原型对象中被屏蔽的属性值
- 或者使用 构造函数名.prototype.属性或方法名 的方法来屏蔽,即使用到原型函数中的属性或方法

## 11.构造函数和普通函数的区别

- 构造函数其实就是通过普通函数实现的(一开始js语言不支持面向对象,只做些脚本,验证)
- 用函数实现,this+new,模拟构造函数,定义对象;后来引入原型的概念,来达到继承性
- 函数也是个对象,js与其他语言不同的地方就是js对象是用函数来实现的

## 12.函数声明和函数表达式

- 声明 function fn(){}  					函数声明可以提升
- 表达式 var fn2 = new Function(){}  	函数表达式不可以提升
我们定义一个函数表达式时,在这个表达式之前是访问不到的

# 检测数据类型

## 1.typeof	
			
- 当检测json和数组时会出现错误,都显示为Object
	
## 2.toString.call()		

- 当检测json和数组时,返回的不一样
	
## 3.instanceof			

- 检测前面的是否是后面的类型
	
## 4.使用jQuery			

- jQuery.isArray();jQuery.isFunction()等

# 正则表达式

## 1.符号含义

*: 	代表匹配前面的字符0个或任意个
+: 	代表匹配前面的字符1个或多个
?: 	代表匹配前面的字符0个或1个
[]: 表示一个字符集合
	[a-z]:小写字母集合 [a-zA-Z]:大小写字母集合 [0-9]:数字集合 
{}: 指定重复前面一个字符多少遍
	{N}:重复N遍 {n,m}:重复n~m遍 {n,}:至少重复n遍 {,m}:至多重复m遍
(): 将括号内的东西看做一个整体
$: 	以...结尾
\s: 匹配一个空格字符,包括:空格,换行,回车,tab,等价于[ \n\r\t\f]
\S: 匹配非空字符 等价于[^\f\n\r\t\v]
\w: 表示字母或数字 等价于[a-zA-Z0-9]
\W: 非字母且非数字 等价于'[a-zA-Z0-9_]'
\d: 表示十进制的数字 等价于[0-9]
\D: 匹配一个非数字字符 等价于[^0-9]
\b:	匹配一个单词边界 'er\b'可以匹配"never";不能匹配"verb"
\B:	匹配非单词边界 'er\B'可以匹配"verb";不能匹配"never"
i:	表示不区分大小写 /a/i,表示"a"或"A"都满足条件

## 2.常用正则表达式

- 验证数字: var regex = /^[0-9]+$/;
	       var regex = /^[0-9]{9}$/ 刚好9位数字
		   var regex = /^[a-zA-Z]{6,12}$/
- 匹配汉字: [u4e00-u9fa5]
- 匹配双字节字符: [^x00-xff]
- 需要验证的对象.match(regex)
![](http://i.imgur.com/cAvIbyR.png)

# replace

## 1.replace的用法(一般结合正则表达式来使用)
	
- 替换字符(简易用法)
- replace.(正则表达式,"$1 $2 ...") 
	- $1,$2...表示将正则表达式中的的一个括号,第二个括号...的内容进行处理
	- 也可以在$1,$2之间添加字符串
	- 还可以写成"$2 $1",来换括号里内容的位置
- 和函数结合使用,对正则表达式选出的字符串进行处理
```css
	str = "aaa bbb ccc";
	str = str.replace(/\b\w+\b/g,function(word){				return word.substring(0,1).toUpperCase()+word.substring(1);
		});
	将字符串的的第一个字符转换成大写
	word是个参数可以任意写,系统会自动识别成前面的正则选出的字符串
	formateString
	function formateString(str,data){
		return str.replace(/@\((\w+)/)/g,function(match,key){
			return data[key]
	})
}
```

# AngularJS

- 通过指令扩展了HTML,通过表达式绑定数据到HTML
- 优点(特性)
	- 最大程度上减少了页面上的DOM操作
	- 让js代码更专注于业务逻辑代码
	- 通过简单的指令结合页面结构与逻辑数据
	- 通过自定义指令实现组件化编程
	- 代码结构更合理
	- 维护成本更低
	- 解放了传统js中频繁的DOM操作
- 特性:
	- MVC 
	- 模块化
	- 自动化双向数据绑定
	- 输入同步刷新
	- 指令系统
- 功能:
	- 视图模型双向绑定
	- 过滤器
	- 自定义指令
	- 路由----可实现单一页面应用
	- 依赖注入
- MVC:
	- model-view-controller	
		- model:保存数据;
		- view:表现层(html和css)展示数据;
		- controller:处理业务逻辑,初始化模型
- MVVM:
	- model-view-viewmodel	
		- 数据视图双向绑定
- CDN:内容传递网络
	- 快
	- 节省自己服务器的带宽压力和流量
- 指令(ng-xxx)
```css
<div ng-app ng-init="name='zhangsan'">
    <p>在此输入内容:</p>
    <p>姓名:
        <input type="text" ng-model="name">
    </p>
    <p>{{name}}</p>
</div>
<script src="js/angular.js"></script>
ng-app:			告诉AngularJS,div元素是AngularJS,div是ag应用程序管理的边界
ng-model		双向数据绑定指令,效果是将当前元素的value和模型中的username建立绑定关系
{{name}}		表达式就是把应用程序变量name绑定到某个段落的innerHTML 默认的是双向绑定;单向绑定是{{::name}}
ng-controller	当前元素交给控制器控制
ng-cloak		斗篷 页面加载时,有表达式的标签会先出现表达式,然后再代入数				据,而angular为了改善效果,加了ng-cloak,但是效果不行,所以我				们可以选择ng-cloak的类名给他加个样式隐藏;
				因为angular为了消除加载时出现表达式的情况,会给表达式的标签加上ng-cloak这个类名,然后去除掉,所以可以使用添加隐藏样式的方法来解决加载出现表达式的现象
				或者给表达式的标签加上ng-cloak类名,然后加上db也行
ng-bind			当给元素标签加上这个指令时,相当于在标签中加入了一个表达式				{{}},但是不会出现渲染出{{}}的问题
				当指令添加的是HTML时,会自动转义,目的是为了安全.防止跨站脚本攻击
ng-bind-html	这个指令是绑定HTML,同时要去引入angular的另一个包					(angular-sanitize.js),然后再写一个script来将上面的js模				块引入自己的模块中angular.module(自己的模块名,						["ngSanitize"]);但是不建议去使用
ng-repeat		这个指令用于循环生成元素并绑定数据
				内部还拥有:item(相应的对象);$first(当是第一个元素时,值为true);$index(当前元素的序列号);$last(当是最后一个元素时,值为true);$even(奇数时,值为true);$odd(偶数时,值为true)
注:当ng-repeat遇到相同数据绑定时
	使用track by $index 如:item in students track by $index来将重复的数据添加到标签内
	item.startsWith() 以...开头的item
<body ng-app="myApp">
<ul ng-controller="listApp">
    <li data-id="{{item.id}}" ng-repeat="item in sList">
        <strong>{{item.name}}</strong>
        <span>{{item.age}}</span>
    </li>
</ul>
<script src="js/angular.min.js"></script>
<script>
    var myApp = angular.module("myApp",[]);
    myApp.controller("listApp",["$scope",function ($scope) {
        $scope.sList = [];
        for(var i=0;i<10;i++){
            $scope.sList.push({
                id: i,
                name: "我的天" + i,
                age: 10 + i,
            })
            //或者用下面的方式添加数组对象
//            $scope.sList[$scope.sList.length] = {
//                id: i,
//                name: "我的天" + i,
//                age: 10 + i,
//            }
        }
    }])
</script>
</body>
将两个模块组装成一个新的模块:
	angular.bootstrap(DOM对象,[对应的模块名])				
		这种方法标签中共有两个(ng-app=第一个模块名;ng-app=第二个模块名);DOM对象用document.querySelector()方法获取
<body>
<div ng-app="myApp1" ng-controller="app1">
    <input type="button" value="点击1" ng-click="do1()">
</div>
<div ng-app="myApp2" ng-controller="app2">
    <input type="button" value="点击2" ng-click="do2()">
</div>
<script src="js/angular.min.js"></script>
<script>
    var myApp1 = angular.module("myApp1", []);
    myApp1.controller("app1", ["$scope", function ($scope) {
        $scope.do1 = function () {
            console.log(1111);
        }
    }]);
    var myApp2 = angular.module("myApp2", []);
    myApp2.controller("app2", ["$scope", function ($scope) {
        $scope.do2 = function () {
            console.log(2222);
        }
    }]);
    angular.bootstrap(document.querySelector('[ng-app="myApp2"]'), ['myApp2']);
</script>
</body>
	angular.module(新的模块名,[第一个模块名,第二个模块名])		这种方式标签中只有一个ng-app=新的模块名
<body ng-app="mod">
<div ng-controller="app1">
    <input type="button" value="点击1" ng-click="do1()">
</div>
<div ng-controller="app2">
    <input type="button" value="点击2" ng-click="do2()">
</div>
<script src="js/angular.min.js"></script>
<script>
    var myApp1 = angular.module("mod1",[]);
    myApp1.controller("app1",["$scope",function ($scope) {
        $scope.do1 = function () {
            console.log(1111);
        }
    }]);
    var myApp2 = angular.module("mod2",[]);
    myApp2.controller("app2",["$scope",function ($scope) {
        $scope.do2 = function () {
            console.log(2222);
        }
    }]);
    angular.module("mod",["mod1","mod2"]);
</script>
</body>
```
- 模块和控制器
```css
			<!-- ng-app划分ng的控制区域 
	 		ng-controller给这个控制区域加上一个控制器
			-->
	<div ng-app='myApp' ng-controller = 'DemoController'>
		<p>在此输入内容:</p>
		<p>姓名:
			<input type="text" ng-model="user.name">
		</p>
		<p>{{user.name}}</p>
			<!-- 控制器里面的事件 ng-事件="函数" -->
		<input type="button" ng-click="show()" value="点击">
	</div>
	<script src="js/angular.min.js"></script>
	<script>
			//注册模块,通过module函数
			//第一个参数是模块的名字
			//第二个参数是这个模块所依赖的模块,如果不依赖任何模块也必须传递第二个参数(参数为一个空数组[]),如果没有第二个参数,angular.module就不是创建一个模块,而是获取一个模块
		//angular.module返回的是刚刚创建的模块对象
		var app = angular.module('myApp',[]);
			//app.controller用于创建一个控制器,所创建的控制器属于myApp模块
			//控制器的命名最好以contorller结尾,方便管理
			//控制器函数中有个参数是$scope,这是固定格式,一定是这么写
		app.controller('DemoController',function ($scope) {
			//当控制器执行时,就会自动执行后面的函数 
			$scope.user = {};
			$scope.user.name = "张三";
			$scope.show = function () {
				console.log($scope.user);
			}
		})
	</script>
注意:
	app.controller('DemoController',function ($scope) {})这种形式来写在代码压缩的时候会出现问题,代码压缩的时候会将变量改成另一个字符,此时,angular中控制器后面的函数就不认识那个变量,导致报错
	解决方法:
	将控制器函数的第二个参数以数组的形式来传递,函数放在数组最后
		一般在生产环境中使用这种方法;
	app.controller('DemoController',['$scope','$http',function (a,b) {}])
```

# node

## 1.node环境

- Node.js 可能类似jquery.js
- 不是JS文件，也不是一个JS框架（）
- 而是Server side Javascript runtime, 服务端的一个JS运行时
- 我们可以在NODE运行JS代码
- alert();ECMAScript  JS- ES  BOM  DOM
- node中只能运行ECMAScript，无法使用 BOM 和 DOM
- 目前我们的JS是运行在浏览器内核中
- PHP是什么？是一门脚本语言也是一个运行环境
- 为什么Node选中了JS，
- 说到底就是一个JS运行环境
- 目前有两个分支
  + Node.js 0.12.7 官方版本 要求尽善尽美
  + IO.js 是社区的产物，不是官方的东西，io.js有很多新特性，迭代非常快，社区推进非常快
  + 15年两者合并，发布node第一个正式版 4.0， 迭代速度又慢了
  + node 5.x == io.js
  + node 4.0 == node

## 2.node环境搭建

### 1.Mac

- 安装包的方式
  + [pkg](https://nodejs.org/dist/v5.5.0/node-v5.5.0.pkg)
- NVM（Node Version Manager）

  ```bash
  $ curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.30.2/install.sh | bash
  $ echo '. ~/.nvm/nvm.sh' >> .bash_profile
  $ nvm install stable
  $ nvm alias default stable
  ```

### 2.Windows

- 安装包的方式
  + [msi_x64](https://nodejs.org/dist/v5.5.0/node-v5.5.0-x64.msi)
  + [msi_x86](https://nodejs.org/dist/v5.5.0/node-v5.5.0-x86.msi)
- NVM（Node Version Manager）
- nvm(node version manager)
- 因为NODE版本比较多，开发人员可能依赖很多版本
- 通过NVM，可以轻松切换于不同的版本之间

  ```command
  
  ```
NVM_HOME=C:\Develop\nvm

NVM_SYMLINK=C:\Develop\nodejs

NPM_HOME=C:\Develop\nvm\npm

PATH=%NVM_HOME%;%NVM_SYMLINK%;%NPM_HOME%

### 3.环境变量

- 环境变量就是操作系统提供的系统级别用于存储变量的地方

- 系统变量和用户变量
- 系统变量指的是所用当前系统用户共享的变量
- 自己的电脑一般只有一个用户
- 建议将自己配置的环境变量放在用户变量中，用户变量比较干净

- 环境变量的变量名是不区分大小写的

- 变量间运行相互引用

- 特殊值：
- PATH变量（不区分大小写）
- PATH 相当于一个路径的引用
- 只要添加到PATH变量中的路径，都可以在任何目录下搜索

- 命令行
- 可以用来执行当前目录下的文件
- 命令

cd :change directory

- Node.js是一个轻内核（本身没有什么功能）的东东，所有的功能都要功能包提供
- node官方提供了一些最基础的包

## 3.NPM

### 1.什么是NPM

https://www.npmjs.com/
- Node Package Manager
- Node应用程序依赖包的管理工具
- 安装卸载更新之类的操作

### 2.为什么使用NPM

- 包很多
- 场景：我需要用一个A，A依赖B，B依赖C
- 常见的包管理工具都有循环依赖的功能
- 你只需记住你要什么东西

### 3.常见的NPM操作

// 安装一个包，默认安装最新稳定版本
npm install package_name
// --save
// 初始化操作，给项目添加一个配置文件
npm init 
// --yes参数走默认配置
//卸载一个包
npm uninstall package_name

- 如果官方数据源太慢使用
- https://npm.taobao.org/

## 4.Bower

### 1.什么是Bower

- [官网](http://bower.io/)
- web应用程序依赖项管理工具


### 2.为什么使用Bower

- 方便便捷的方式管理包，zhuangbi

### 3.Bower实践

- npm install -g bower // -g:global

- 修改npm全局路径，就是在用户目录下添加.npmrc文件

## 5.Gulp

### 1.Gulp简介

- 链接：
  + [官网](http://gulpjs.com/)
  + [中文网](http://www.gulpjs.com.cn/)
- 就是用来机械化的完成重复性质的工作
- gulp的机制就是将重复工作抽象成一个个的任务，

### 2.Gulp准备工作

- 安装Node.js
- 安装 gulp 命令行工具
  + `npm install -g gulp`
- 初始化 gulp 项目
- 创建任务 - gulpfile.js

### 3.基本使用

### 4.常用插件

- [编译 Less：gulp-less](https://www.npmjs.com/package/gulp-less)
- [创建本地服务器：gulp-connect](https://www.npmjs.com/package/gulp-connect)
- [合并文件：gulp-concat](https://www.npmjs.com/package/gulp-concat)
- [最小化 js 文件：gulp-uglify](https://www.npmjs.com/package/gulp-uglify)
- [重命名文件：gulp-rename](https://www.npmjs.com/package/gulp-rename)
- [最小化 css 文件：gulp-minify-css](https://www.npmjs.com/package/gulp-minify-css)
- [压缩html文件 gulp-minify-html](https://www.npmjs.com/package/gulp-minify-html)
- [最小化图像：gulp-imagemin](https://www.npmjs.com/package/gulp-imagemin)

## 6.GIT

### 1.命令操作

- 初始化一个本地GIT仓储

```shell
cd 当前项目目录
git init // 初始化一个本地的仓库
```

> 就是在本地文件夹中添加了一个.git的文件夹用于记录所有的项目变更信息

- 查看本地仓储的变更状态

git status
用于查看本地仓储的状态
第一次查看，显示的是一坨没有被跟踪的文件

git status -s // -s 是输出简要的变更日志

- 添加本地暂存（托管）文件

git add
可以将一个没有被跟踪的文件添加到跟踪列表

类似于node_modules这种性质的文件是不应该被跟踪

- 添加本地GIT忽略清单文件

在代码库文件夹的根目录添加一个.gitignore文件
此文件用于说明忽略的文件有哪些

- 提交被托管的文件变化到本地仓储

git commit
将本地的变化提交的本地的仓库文件夹归档
一般在有了一个小单元的整体变化后再提交

- 对比差异

git diff
可以用于对比当前状态和版本库中状态的变化

- 提交日志

git log 
可以查看提交日志

- 回归到指定版本

git reset --hard

- 为仓储添加远端（服务器端）地址

- 将本地仓储的提交记录推送到远端的master分支

- 拉取远端master分支的更新记录到本地

- 回归到指定版本

### 2.GITHUB基本使用

- https://github.com/
- GITHUB是一个GIT服务的提供商，
- 
- 提出社交化编程

http://zoomzhao.github.io/code-guide/
https://github.com/jobbole/awesome-javascript-cn
https://github.com/jobbole/awesome-css-cn

# less

## 1.特点		 	

- 一款预处理css,支持变量,混合(Mixin),函数,嵌套,循环等特点;css语言的超集;支持原来的css语法

## 2.语法

- 定义变量:		@变量名:值;			颜色值不用加引号
```css
@width:200px;
@mainColer:#e92322;
.container{
	width:@width;
	.row{
		height: @width;
		div{
			border:1px solid @mainColer;
			>a{						直接子代元素
				color:red;
				&:hover{			当前元素a的hover
					color:green;
				}
			}
		}
	}
}
```
- 定义一个类(函数)
	- .类名(参数:默认值){}
- 将颜色变亮/暗百分之...(包括rgb值)
	- lighten(颜色,百分比)	
	- darken(颜色,百分比)
- 导入其他less文件 被引用的文件,文件名前要加_文件名
	- @import url('文件路径');
	- 原文件中定义的变量,在引入的文件中任然能够生效

## 3.node配置环境变量

- 右键'我的电脑',选择属性
- 选择环境变量
- 双击用户变量中的PATH
- 将nodeJS路径粘贴到变量值之前并加上";"
- 一直点击确定,然后重启资源管理器
- 将lessc/lessc.cmd/node_modules文件夹黏贴到下面的路径中
C:\Users\Ray丶X\AppData\Roaming\npm

## 4.less文件转换成css文件:

- 1.打开命令行,输入less文件的路径

```
	如: F:\练习\移动开发\one\less>
```

- 2.在路径后面加个空格 输入lessc main.less文件,此时是将less文件编译出来,看是否出现错误,接着进行下一步

```
	如: F:\练习\移动开发\one\less> lessc main.less
		body {
		  background-color: #9e2322;
		}
```

- 3.编译并生成css文件

```
	F:\练习\移动开发\one\less> lessc main.less > main.css
```

- 4.改动后向原来编译好的css文件追加样式

```
	F:\练习\移动开发\one\less> lessc main.less >> main.css
```
	
## 5.导入less文件中写的样式方法:

- 方法1:

    - 1.先引入less文件

```<link rel="stylesheet/less" href="less/main.less">```

	- 2.再引入less.js文件

```<script src="less/less.js"></script>```

- 方法2:
    - 直接将编译好的css文件引入

```
        <link rel="stylesheet" href="less/main.css">
```

# php

## 1.变量

- 变量以$开头 字母/数字/下划线 不能以数字开头
- 大小写敏感
- 变量作用域 global local *static

## 2.内容输出

- echo：输出简单数据类型，如字符串、数值
- print_r()：输出复杂数据类型，如数组
- var_dump()：输出详细信息，如对象、数组

## 3.数据类型

- 字符型、整型、浮点型、布尔型、数组、对象、NULL
	- gettype() 检测数据类型
	- is_string() 是否是字符
	- is_array() 是否是数组
- 运算符

## 4.基本与Javascript语法一致

- .号表示链接符
- 分支、循环语句与Javascript基本一致
	- foreach()
	- switch()
- 函数与Javascript基本一致
	- 函数名对大小写不敏感
	- 默认参数

## 5.文件引入

- include 引入失败后程序继续执行
- require 引入失败后程序终止执行

## 6.超全局变量

- $_GLOBALS
- $_SERVER
- $_GET
- $_POST
- $_REQUEST
- $_FILES
- $_COOKIE
- $_SESSION
- $_ENV

## 7.表单处理

- 表单name属性的是用来提供给服务端接收所传递数据而设置的
- 表单action属性设置接收数据的处理程序
- 表单method属性设置发送数据的方式
- 当上传文件是需要设置 enctype="multipart/form-data"
	- $_GET接收 get 传值
	- $_POST接收 post 传值
	- $_FILES接收文件上传

#css问题

解绑hover事件,使用jQuery的 jQuery对象.unbind("mouseenter mouseleave")即可;

#移动web常出现的问题及解决方案

## 1.安卓浏览器看背景图片，有些设备会模糊

- 经过研究，是devicePixelRatio作怪，因为手机分辨率太小，如果按照分辨率来显示网页，这样字会非常小，所以苹果当初就把iPhone 4的960640分辨率，在网页里只显示了480320，这样devicePixelRatio＝2。现在android比较乱，有1.5的，有2的也有3的。

- 想让图片在手机里显示更为清晰，必须使用2x的背景图来代替img标签（一般情况都是用2倍）。例如一个div的宽高是100100，背景图必须得200200，然后background-size:contain;，这样显示出来的图片就比较清晰了。

代码如下：
```css
	background:url(../images/icon/all.png) no-repeat center center;
	-webkit-background-size:50px 50px;
	background-size: 50px 50px;
	display:inline-block; 
	width:100%; 
	height:50px;
```

或者设置
```css
background-size:contain;
```

## 2.防止手机中网页放大和缩小

- 最简单的方法就是设置viewport

```css
	<meta name="viewport" 
		  content="width=device-width,
				   initial-scale=1.0,
				   maximum-scale=1.0,
				   user-scalable=0"/>
``` 
user-scalable=0,有的人也写成user-scalable=no

- 比较复杂的方式就是XHTML中设置DTD的方法,如果是HTML5页面,则不需要用这种方法

```css
	<!DOCTYPE html PUBLIC "-
				//WAPFORUM//DTD XHTML Mobile 1.0
				//EN" "http://www.wapforum.org/DTD/xhtml-mobile10.dtd">
```

## 3.上下拉动滚动条时卡顿、慢

- 给body设置touch属性

```css
	body {
	 -webkit-overflow-scrolling: touch;
	 overflow-scrolling: touch;
	}
```

## 4.iphone及ipad下输入框默认内阴影

- 设置css属性

```css
	Element{
 		-webkit-appearance: none;
	}
```

## 5.ios和android下触摸元素时出现半透明灰色遮罩

- 设置css属性

```css
	Element {
 	-webkit-tap-highlight-color:rgba(255,255,255,0)
	}
```

## 6.动画定义3D启用硬件加速

- 设置css属性

```css
	Element {
 	-webkit-transform:translate3d(0, 0, 0)
 	transform: translate3d(0, 0, 0);
	}
```

## 7.旋转屏幕时，字体大小调整的问题

- 设置css属性

```css
	html, body, form, fieldset, p, div, h1, h2, h3, h4, h5, h6 {
 	-webkit-text-size-adjust:100%;
	}
```

## 8.transition闪屏

- 设置内嵌的元素在 3D 空间如何呈现：保留3D

- 设置进行转换的元素的背面在面对用户时是否可见：隐藏 

```css
	-webkit-transform-style: preserve-3d;
	-webkit-backface-visibility:hidden;
```

## 9.圆角bug

- 某些Android手机圆角失效

```css
	background-clip: padding-box;
```

## 10.顶部状态栏背景色

- 用meta标签声明

```css
	<meta name="apple-mobile-web-app-status-bar-style" 			          	  content="black" />
```
 
说明：

- 除非你先使用apple-mobile-web-app-capable指定全屏模式，否则这个meta标签不会起任何作用

- 如果content设置为default，则状态栏正常显示。如果设置为blank，则状态栏会有一个黑色的背景。如果设置为blank-translucent，则状态栏显示为黑色半透明 

- 如果设置为default或blank，则页面显示在状态栏的下方，即状态栏占据上方部分，页面占据下方部分，二者没有遮挡对方或被遮挡

- 如果设置为blank-translucent，则页面会充满屏幕，其中页面顶部会被状态栏遮盖住（会覆盖页面20px高度，而iphone4和itouch4的Retina屏幕为40px）
默认值是default。

## 11.设置缓存

- 手机页面通常在第一次加载后会进行缓存，然后每次刷新会使用缓存而不是去重新向服务器发送请求。如果不希望使用缓存可以设置no-cache

```css
	<meta http-equiv="Cache-Control" content="no-cache" />
```

## 12.桌面图标

```css
<link rel="apple-touch-icon" href="touch-icon-iphone.png" />
<link rel="apple-touch-icon" sizes="76x76" href="touch-icon-ipad.png" />
<link rel="apple-touch-icon" sizes="120x120" href="touch-icon-iphone-retina.png" />
<link rel="apple-touch-icon" sizes="152x152" href="touch-icon-ipad-retina.png" />
```

- iOS下针对不同设备定义不同的桌面图标。如果不定义则以当前屏幕截图作为图标。

- 上面的写法可能大家会觉得会有默认光泽，下面这种设置方法可以去掉光泽效果，还原设计图的效果！

```css
<link rel="apple-touch-icon-precomposed" href="touch-icon-iphone.png" />
```

- 图片尺寸可以设定为57*57（px）或者Retina可以定为114*114（px），ipad尺寸为72*72（px)

## 13.唤起select的option展开

- 原生js方式

```css
	function showDropdown(sltElement) {
 		var event;
 		event = document.createEvent('MouseEvents');
 		event.initMouseEvent('mousedown', true, true, window);
 		sltElement.dispatchEvent(event);
};
```

- zepto方式:

```css
	$(sltElement).trigger("mousedown");
```

## 14.适配各种手机端的字体单位

### 网页链接
This is an [example link](http://www.imooc.com/article/1115)
```css
	html {
		font-size:10px
	} 
	@media screen and (min-width:480px) and (max-width:639px) { 
		html { 
			font-size: 15px 
		} 
	} 
	@media screen and (min-width:640px) and (max-width:719px) { 
		html { 
			font-size: 20px 
		} 
	} 
	@media screen and (min-width:720px) and (max-width:749px) { 
		html { 
			font-size: 22.5px 
		} 
	} 
	@media screen and (min-width:750px) and (max-width:799px) { 
		html { 
			font-size: 23.5px 
		} 
	} 
	@media screen and (min-width:800px) and (max-width:959px) { 
		html { 
			font-size: 25px 
		} 
	} 
	@media screen and (min-width:960px) and (max-width:1079px) { 
		html { 
			font-size: 30px 
		} 
	} 
	@media screen and (min-width:1080px) { 
		html { 
			font-size: 32px 
		} 
	}
```