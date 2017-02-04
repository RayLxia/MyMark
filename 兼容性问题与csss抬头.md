- css抬头:

```css
*,
::before,
::after {
    margin: 0;
    padding: 0;
    -webkit-tap-highlight-color: transparent; /* 清除点击默认的高亮效果 */
    -webkit-box-sizing: border-box; /* 以border开始计算宽高 */
}
body {
    font-size: 14px;
    font-family: "Microsoft YaHei", sans-serif; /* 第二个是手机的默认字体 */
    color: #000;
}
a{
    text-decoration: none;
    color: #000;
}
ul {
    list-style: none;
}
input,textarea {
    border: 0;
    resize: none;
    outline: none; /* 清除选中的效果(蓝色的光环) */
    -webkit-appearance: none; /* 清除浏览器input的默认的样式 */
}
/* 清除浮动 */
.clearfix::before,
.clearfix::after {
    content: '';
    height: 0;
    line-height: 0;
    display: block;
    visibility: hidden; /* 隐藏但占有位置
                           overflow:hidden;隐藏超出部分
                           display:none; 隐藏不占有位置
                        */
    clear: both;
}
```

- 问题症状：随便写几个标签，不加样式控制的情况下，各自的margin 和padding差异较大。
    - 碰到频率:100%
    - 解决方案：css里    *{margin:0;padding:0;}
    - 备注：这个是最常见的也是最易解决的一个浏览器兼容性问题，几乎所有的css文件开头都会用通配符*来设置各个标签的内外补丁是0。

- 浏览器兼容问题二：块属性标签float后，又有横行的margin情况下，在ie6显示margin比设置的大
    - 问题症状:常见症状是ie6中后面的一块被顶到下一行
    - 碰到频率：90%（稍微复杂点的页面都会碰到，float布局最常见的浏览器兼容问题）
    - 解决方案：在float的标签样式控制中加入 display:inline;将其转化为行内属性
    - 备注：我们最常用的就是div+css布局了，而div就是一个典型的块属性标签，横向布局的时候我们通常都是用div float实现的，横向的间距设置如果用margin实现，这就是一个必然会碰到的兼容性问题。

- 浏览器兼容问题三：设置较小高度标签（一般小于10px），在ie6，ie7，遨游中高度超出自己设置高度
    - 问题症状：ie6、7和遨游里这个标签的高度不受控制，超出自己设置的高度
    - 碰到频率：60%
    - 解决方案：给超出高度的标签设置overflow:hidden;或者设置行高line-height 小于你设置的高度。
    - 备注：这种情况一般出现在我们设置小圆角背景的标签里。出现这个问题的原因是ie8之前的浏览器都会给标签一个最小默认的行高的高度。即使你的标签是空的，这个标签的高度还是会达到默认的行高。

- 浏览器兼容问题四：行内属性标签，设置display:block后采用float布局，又有横行的margin的情况，ie6间距bug（类似第二种）
    - 问题症状：ie6里的间距比超过设置的间距
    - 碰到几率：20%
    - 解决方案：在display:block;后面加入display:inline;display:table;
    - 备注：行内属性标签，为了设置宽高，我们需要设置display:block;(除了input标签比较特殊)。在用float布局并有横向的margin后，在ie6下，他就具有了块属性float后的横向margin的bug。不过因为它本身就是行内属性标签，所以我们再加上display:inline的话，它的高宽就不可设了。这时候我们还需要在display:inline后面加入display:talbe。

- 浏览器兼容问题五：图片默认有间距
    - 问题症状：几个img标签放在一起的时候，有些浏览器会有默认的间距，加了问题一中提到的通配符也不起作用。
    - 碰到几率：20%
    - 解决方案：使用float属性为img布局
    - 备注：因为img标签是行内属性标签，所以只要不超出容器宽度，img标签都会排在一行里，但是部分浏览器的img标签之间会有个间距。去掉这个间距使用float是正道。

- 浏览器兼容问题六：标签最低高度设置min-height不兼容
    - 问题症状：因为min-height本身就是一个不兼容的css属性，所以设置min-height时不能很好的被各个浏览器兼容
    - 碰到几率：5%
    - 解决方案：如果我们要设置一个标签的最小高度200px，需要进行的设置为：{min-height:200px; height:auto !important; height:200px; overflow:visible;}
    - 备注：在B/S系统前端开时，有很多情况下我们又这种需求。当内容小于一个值（如300px）时。容器的高度为300px；当内容高度大于这个值时，容器高度被撑高，而不是出现滚动条。这时候我们就会面临这个兼容性问题。

- 浏览器兼容问题七：透明度的兼容css设置
	- filter:alpha(opacity=80);opacity:0.8; 
	- ie6设置透明度颜色filter:progid:DXImageTransform.Microsoft.Gradient(GradientType=0,StartColorStr='#66000000',EndColorStr='#66000000') 

- box-sizing现代浏览器都支持，但IE家族只有IE8版本以上才支持，虽然现代浏览器支持box-sizing，但有些浏览器还是需要加上自己的前缀，Mozilla需要加上-moz-，Webkit内核需要加上-webkit-，Presto内核-o-,IE8-ms-，所以box-sizing兼容浏览器时需要加上各自的前缀：
```css
Element {
     -moz-box-sizing: content-box;  
     -webkit-box-sizing: content-box; 
     -o-box-sizing: content-box; 
     -ms-box-sizing: content-box; 
     box-sizing: content-box; 
  }
  Element {
     -moz-box-sizing: border-box;  
     -webkit-box-sizing: border-box; 
     -o-box-sizing: border-box; 
     -ms-box-sizing: border-box; 
     box-sizing: border-box; 
  }
``` 

- 万能 float 闭合(非常重要!) 可以用这个解决多个div对齐时的间距不对，  
  - 关于 clear float 的原理可参见 [How To Clear Floats Without Structural Markup]   
```css 
 将以下代码加入Global CSS 中,给需要闭合的div加上 class=”clearfix” 即可,屡试不爽.  
 代码:  
<style>  
/* Clear Fix */  
.clearfix:after {  
	content:".";  
	display:block;  
	height:0;  
	clear:both;   
	visibility:hidden;  
}   
.clearfix {
   display:inline-block;
  }   
/* Hide from IE Mac \*/  
.clearfix {display:block;}  
/* End hide from IE Mac */  
/* end of clearfix */ 
</style> 
```

- !important(不是很推荐，用下面的一种感觉最安全)  
  	随着IE7对!important的支持, !important 方法现在只针对IE6的兼容.(注意写法.记得该声明位置需要提前.)  
  	代码:  <style>  #wrapper {   width: 100px!important; /* IE7+FF */  width: 80px; /* IE6 */  }   </style>

- 尽量减少出现兼容性问题的方法
	- 每写一小段代码（布局中的一行或者一块）我们都要在不同的浏览器中看是否兼容，当然熟练到一定的程度就没这么麻烦了。建议经常会碰到兼容性问题的新手使用。很多兼容性问题都是因为浏览器对标签的默认属性解析不同造成的，只要我们稍加设置都能轻松地解决这些兼容问题。如果我们熟悉标签的默认属性的话，就能很好的理解为什么会出现兼容问题以及怎么去解决这些兼容问题。

/* css hack*/
我很少使用hacker的，可能是个人习惯吧，我不喜欢写的代码ie不兼容，然后用hack来解决。不过hacker还是非常好用的。
使用hacker 我可以吧浏览器分为3类：ie6 ；ie7和遨游；其他（ie8 chrome ff safari opera等）
ie6认识的hacker 是下划线_ 和星号 *
ie7 遨游认识的hacker是星号 * （包括上面问题6中的 !important也算是hack的一种。不过实用性较小。）
比如这样一个css设置 height:300px;*height:200px;_height:100px;
ie6浏览器在读到 height:300px的时候会认为高时300px；继续往下读，他也认识*heihgt， 所以当ie6读到*height:200px的时候会覆盖掉前一条的相冲突设置，认为高度是200px。继续往下读，ie6还认识_height,所以他又会覆盖掉200px高的设置，把高度设置为100px；
ie7和遨游也是一样的从高度300px的设置往下读。当它们读到*height200px的时候就停下了，因为它们不认识_height。所以它们会把高度解析为200px；
剩下的浏览器只认识第一个height:300px;所以他们会把高度解析为300px。
因为优先级相同且想冲突的属性设置后一个会覆盖掉前一个，所以书写的次序是很重要的。
最后说一下，严谨型的开发人员会有一套合适自己的RESET.CSS。结合自己的经验尽量规避容易出现不兼容的问题。以减少hack的使用，尽量符合W3C的标准。