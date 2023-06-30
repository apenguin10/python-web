# 前端简介

* HTML: 网页的骨架，没有任何的样式
* CSS：  给骨架添加各种样式，变得好看
* JS:        控制网页的动态效果



* 前端框架：BOOTSTRAP、JQuery、Vue



参考博客<https://www.cnblogs.com/Dominic-Ji/p/10864457.html>

## 1 浏览器窗口输入网址回车发生了几件事

```html
1 浏览器朝服务端发送请求
2 服务端接受请求(eg:请求百度首页)
3 服务端返回相应的响应(eg:返回一个百度首页)
4 浏览器接收响应 根据特定的规则渲染页面展示给用户看


浏览器可以充当很多服务端的客户端
	百度 腾讯视频 优酷视频....
 
如何做到浏览器能够跟多个不同的客户端之间进行数据交互？
	1.浏览器很牛逼 能够自动识别不同服务端做不同处理
    2.制定一个统一的标准 如果你想要让你写的服务端能够跟客户端之间做正常的数据交互
		那么你就必须要遵循一些规则
```

## 2 HTTP协议

```html
超文本传输协议 用来规定服务端和浏览器之间的数据交互的格式...
```

* 四大特性

```html
1. 基于请求响应
2. 基于TCP/IP作用于应用层之上的协议
3. 无状态
	不保存用户的信息
	eg:一个人来了一千次 你都记不住 每次都当他如初见
	由于HTTP协议是无状态的 所以后续出现了一些专门用来记录用户状态的技术
    cookie、session、token...
4. 无/短链接
	请求来一次我响应一次 之后我们两个就没有任何链接和关系了
 
	长链接:双方建立连接之后默认不断开 websocket(后面讲项目的时候会讲)
```

* 请求数据格式

```html
请求首行(标识HTTP协议版本，当前请求方式)
请求头(一大堆k,v键值对)
\r\n
请求体(并不是所有的请求方式都有get没有post有 存放的是post请求提交的敏感数据)
```

* 响应数据格式

```html
响应首行(标识HTTP协议版本，响应状态码)
响应头(一大堆k,v键值对)
\r\n
响应体(返回给浏览器展示给用户看的数据)
```

* **响应状态码**

```html
用一串简单的数字来表示一些复杂的状态或者描述性信息  404:请求资源不存在

1XX: 服务端已经成功接收到了你的数据正在处理，你可以继续提交额外的数据
2XX: 服务端成功响应了你想要的数据(200 OK请求成功)
3XX: 重定向(当你在访问一个需要登陆之后才能看的页面 你会发现会自动跳转到登陆页面)
4XX: 请求错误
     404:请求资源不存在
  	 403:当前请求不合法或者不符合访问资源的条件
5XX: 服务器内部错误(500)
```

* **请求方式**

```html
1.get请求
	朝服务端要数据
	eg:输入网址获取对应的内容
2.post请求
	朝服务端提交数据
	eg:用户登陆 输入用户名和密码之后 提交到服务端后端做身份校验

url: 统一资源定位符(网址)
```

# HTML

## 1 超文本标记语言

```html
<h1>hello world</h1>
<a href="https:www.baidu.com">这是百度</a>
<img src="https://img.zcool.cn/community/016d485681d8e832f8759f04b664ca.jpg?x-oss-process=image/auto-orient,1/resize,m_lfit,w_1280,limit_1/sharpen,100">
```

## 2 HTML就是书写网页的一套标准

```html
注释

<!--单行注释-->
<!--
多行注释1
多行注释2
-->

由于HTML代码非常的杂乱无章并且很多，所以我们习惯性的用注释来划定区域方便后续的查找
<!--导航条开始-->
导航条所有的html代码
<!--导航条结束-->

<!--左侧菜单栏开始-->
左侧菜单栏的HTMl代码
<!--左侧菜单栏结束-->
```

## 3 HTML文档结构

```html
<html>
	<head></head>: head内的标签不是给用户看的 而是定义一些配置主要是给浏览器看的
    <body></body>: body内的标签 写什么浏览器就渲染什么 用户就能看到什么
</html>

注意: HTML代码是没有格式的，可以全部写在一行都没有问题，只不过我们习惯了缩进来表示代码
```

* 两种打开 HTML文件的方式

  		1. 找到文件所在位置右键选择浏览器打开

  2. 在编辑器安装对应插件打开

## 4 标签

### 4.1 标签的分类

```html
<h1></h1>
<a href="https://www.mzitu.com/"></a>
<img>

1. 双标签
2. 单标签
```

### 4.2 head 内的常用标签

注：在书写HTML代码的时候 你只需要写标签名 然后tab就能自动补全

```html
<!DOCTYPE html>
<html>
<head>
      <meta charset="UTF-8">

      <title>哔哩哔哩</title>       <!--网页标题-->

      <style>                      
            h1 {
                  color: greenyellow;
            }
      </style>                     <!--CSS代码-->

      <script>
            alert(123)              
      </script>                    <!--JS代码-->
      <script src="my.js"></script>

      <link rel="stylesheet" href="mycss.css">   <!--引入外部CSS代码-->
      
      <meta name="keywords" content="淘宝,掏宝...">   
      当你在用浏览器搜索的时候 只要输入了keywords后面指定的关键字那么该网页都有可能被百度搜索出来展示给用户

      <meta name="description" content="淘宝网 - 亚洲较大的...">
      网页的描述性信息

</head>

<body>
            
</body>

</html>
```

### 4.3 body内常用标签

#### * 基本标签

```html
<body>
      <h1>一级标题</h1>
      <h6>六级标题</h6>      
      <b>加粗</b>
      <i>斜体</i>
      <s>删除线</s>
      <p>段落</p>
      <br>  <!--换行-->
      <hr>  <!--水平分割线-->
</body>
```

#### * 标签的分类

```html
1 块儿级标签:独占一行
		h1~h6	p  div
  	1 块儿级标签可以修改长宽 行内标签不可以 修改了也不会变化
    2 块儿级标签内部可以嵌套任意的块儿级标签和行内标签
    	但是p标签虽然是块儿级标签 但是它只能嵌套行内标签 不能嵌套块儿级标签
      	如果你套了 问题也不大 因为浏览器会自动帮你解开(浏览器是直接面向用户的 不会轻易的报错 哪怕有报错用户也基本感觉不出来)
       
    总结:
      	只要是块儿级标签都可以嵌套任意的块儿级标签和行内标签
        但是p标签只能嵌套行内标签（HTML书写规范）

2 行内标签:自身文本多大就占多大
		i u s b span
  	行内标签不能嵌套块儿级标签 可以嵌套行内标签
```

#### * 特殊符号

```html
&nbsp;  空格
&gt;   	大于号
&lt;   	小于号
&amp;  	&
&yen;  	¥
&copy;	©
&reg;   商标®
```

#### * 常用标签

```html
div		块儿级标签
span	行内标签

上述的两个标签是在构造页面初期最常使用的 页面的布局一般先用div和span占位之后再去调整样式 尤其是div使用非常的频繁

div你可以把它看成是一块区域 也就意味着用div来提前规定所有的区域
之后往该区域内部填写内容即可
而普通的文本先用span标签 
```

#### * img 标签

```html
# 图片标签
<img src="" alt="" title="" height="" width="">

src	
	1.图片的路径	可以是本地的也可以是网上的
    2.url 自动朝该url发送get请求获取数据

alt="这是我的前女友"
	当图片加载不出来的时候 给图片的描述性信息

title="新垣结衣"
	当鼠标悬浮到图片上之后 自动展示的提示信息

height="800px" 
		
width=""
	高度和宽度当你只修改一个的时候 另外一个参数会等比例缩放
   如果你修改了两个参数 并且没有考虑比例的问题 那么图片就会失真
```

#### * a标签

```html
链接标签
<a href="" target=""></a>

当a标签指定的网址从来没有被点击过 那么a标签的字体颜色是蓝色
如果点击过了就会是紫色（浏览器给你记忆了）
```

```html
href
	1. 放url，用户点击就会跳转到该url页面
    2. 放其他标签的id值 点击即可跳转到对应的标签位置

<a href="https://www.baidu.com">百度</a>

<a href="" id="d1">顶部</a>
<h1 id="d111">hello world</h1>
<div style="height: 1000px;background-color: red"></div>
<a href="" id="d2">中间</a>
<div style="height: 1000px;background-color: greenyellow"></div>
<a href="#d1">返回顶部</a>
<a href="#d2">回到中间</a>
<a href="#d111">回到顶部</a>

target
	控制是否在当前页面跳转
	_self
	_blank
```

#### * 标签的两个重要属性

```html
id值
	类似于标签的身份证号 在同一个html页面上id值不能重复
class值
	该值有点类似于面向对象里面的继承 一个标签可以继承多个class值
```

标签既可以有默认的书写也可以有自定义的书写

```html
<p id="d1" class="c1" username="jason" password="123"></p>
```

#### * 列表标签

* 无序列表

```html
<ul>
    <li>第一</li>
    <li>第二</li>
    <li>第三</li>
</ul>

虽然ul标签很丑 但是在页面布局的时候 只要是排版一致的几行数据基本上用的都是ul标签
```

* 有序列表

```html
<ol type="a" start="b">
    <li>111</li>
    <li>222</li>
    <li>333</li>
</ol>

type	1 A I a ... 
```

* 标题标签

```html
<dl>
    <dt>标题1</dt>
    <dd>内容1</dd>
    <dt>标题2</dt>
    <dd>内容2</dd>
    <dt>标题三</dt>
    <dd>内容三</dd>
</dl>
```

#### * 表格标签

```html
<table>
        <thead>
  			<tr>  一个tr就表示一行
                <th>username</th>  加粗文本
                <td>username</td>  正常文本
            </tr>
  		</thead>   表头(字段信息)
        <tbody>
        	<tr>
                <td>jason</td>
                <td>123</td>
                <td>read</td>
            </tr>
        </tbody>	 表单(数据信息)
</table>

<table border="1"  cellpadding="5" cellspacing="5">  加外边宽, 内边距，外边距
<td colspan="2">egon</td>  水平方向占多行
<td rowspan="2">DBJ</td>   垂直方向占多行
```

## 5 表单标签

```html
能够获取前端用户数据(用户输入的、用户选择、用户上传...)基于网络发送给后端服务器

写一个注册功能
<form action=""></form>  在该form标签内部书写的获取用户的数据都会被form标签提交到后端

action:控制数据提交的后端路径(给哪个服务端提交数据)
  	1.什么都不写  默认就是朝当前页面所在的url提交数据
    2.写全路径:https://www.baidu.com  朝百度服务端提交
    3.只写路径后缀action='/index/'  
    	自动识别出当前服务端的ip和port拼接到前面
      host:port/index/ 

  <h1>注册页面</h1>
  <form action="">
  <p>
        <label for="d1">
              用户名: <input type="text" id="d1">
        </label>
  </p>
  <p>
        <label for="d2">密码:</label>
        <input type="password" id="d2">
  </p>   
  <p>
        <label for="d3">生日:</label>
        <input type="date" id="d3">
  </p>   
  <p>性别:
        <input type="radio" name="gender">男
        <input type="radio" name="gender">女
        <input type="radio" name="gender">其他
  </p>
  <p>爱好:
        <input type="checkbox">乒乓球
        <input type="checkbox" checked>游戏
        <input type="checkbox">动漫
  </p>     
      
  <p>上传文件:
        <input type="file" multiple>
  </p>
      
  <p>大段文本框:
        <textarea name="" id="" cols="30" rows="10" maxlength="20"></textarea>
  </p>    
      
  <p>省市:
        <select name="" id="">
              <option value="">北京</option>
              <option value="" selected="selected">上海</option>
              <option value="">深圳</option>
        </select>
  </p>
  <p>前女友 : 
        <select name="" id="" multiple>
              <option value="" selected>新恒结衣</option>
              <option value="">雏田唯</option>
              <option value="">高圆圆</option>
              <option value="" selected>格温</option>
        </select>       
  </p>      
    <p>带有标题的:
    <select name="" id="">
          <optgroup label="上海"></optgroup>
                <option value="">浦东</option>
                <option value="">黄埔</option>
          <optgroup label="北京"></optgroup>
                <option value="">朝阳</option>
                <option value="">昌平</option>
          <optgroup label="南京"></optgroup>
                <option value="">浦口</option>
                <option value="">鼓楼</option>                        
    </select>
    </p>
  <!-- 触发form表单提交数据的动作 -->
  <input type="submit" value="注册">        
  <!-- 普通的按钮, 本身没有功能，用js可以自定义各种功能 -->
  <input type="button" value="按钮">
  <!-- 重置内容 -->
  <input type="reset" value="重置">
  <button>按钮</button>
  </form>

```

* **input**

```html
label input 都是行内标签

input	通过type 属性变形
	text: 普通文本		placeholder="提示信息"
	password: 密文
	date: 日期


submit:用来触发form表单提交数据的动作
button:就是一个普普通通的按钮 本身没有任何的功能 但是它是最有用的，学完js之后可以给它自定义各种功能
reset:重置内容

radio:单选
    默认选中要加checked='checked'
  <input type="radio" name="gender" checked='checked'>男
  当标签的属性名和属性值一样的时候可以简写
  <input type="radio" name="gender" checked>女

	checkbox:多选
  		<input type="checkbox" checked>DBJ
  
file: 获取文件  也可以一次性获取多个
    <input type="file" multiple>

hidden: 隐藏当前input框
    钓鱼网站
```

* **select**

```html
select标签 默认是单选 可以加mutiple参数变多选 默认选中selected
<select name="" id="" multiple>
    <option value="" selected>新垣结衣</option>
</select>
```

* **textarea**

```html
<p>大段文本框:
    <textarea name="" id="" cols="30" rows="10" maxlength="20"></textarea>
</p>
```

```html
# 能够触发form表单提交数据的按钮有哪些(一定要记住)
		1、<input type="submit" value="注册">
		2、<button>点我</button>
    
# 所有获取用户输入的标签 都应该有name属性
	name就类似于字典的key
  用户的数据就类似于字典的value
  <p>gender:
            <input type="radio" name="gender">男
            <input type="radio" name="gender">女
            <input type="radio" name="gender">其他
  </p>
```

## 6 验证form表单提交数据

```python
# 接下来的框架代码无需掌握  看一下效果即可
pip3 install FLASK

from flask import Flask, request


app = Flask(__name__)

# 当前url 既可以支持get请求，也可以支持post请求
# 不写，只能支持get请求
@app.route("/index/", methods=["GET", "POST"])  
def index():
    print(request.form)     # form 表单提交的非文件数据
    # ImmutableMultiDict([('username', 'fafafaf'), ('gender', 'on')])

    print(request.files)    # 获取文件数据
    file_obj = request.files.get("myflie")
    print(file_obj.name)
    file_obj.save(file_obj.name)
    # ImmutableMultiDict([('myflie', <FileStorage: 'aca_in.txt' ('text/plain')>)])
    return "OK"
app.run()
```

```html
form表单默认提交数据的方式 是get请求  数据是直接放在url后面的
	http://127.0.0.1:5000/index/?username=sdadasdsda&gender=on
你可以通过method指定提交方式
	<form action="http://127.0.0.1:5000/index/" method="post">
  
针对用户选择的标签 用户不需要输入内容 但是你需要提前给这些标签添加内容value值
<p>gender:
            <input type="radio" name="gender" value="male">男
            <input type="radio" name="gender" checked value="female">女
            <input type="radio" name="gender" value="others">其他
</p>
<p>hobby:
            <input type="checkbox" name="hobby" value="basketball">篮球
            <input type="checkbox" checked name="hobby" value="football">足球
            <input type="checkbox" checked name="hobby" value="doublecolorball">双色球
</p>
<p>province：
            <select name="province" id="">
                <option value="sh">上海</option>
                <option value="bj" selected>北京</option>
                <option value="sz">深圳</option>
            </select>
</p>


form表单提交文件需要注意
	1.method必须是post
	2.enctype="multipart/form-data"
		enctype类似于数据提交的编码格式
			默认是urlencoded 只能够提交普通的文本数据
			formdata 就可以支持提交文件数据
<form action="http://127.0.0.1:5000/index/" method="post" enctype="multipart/form-data">


# 针对用户输入的标签。如果你加了value 那就是默认值
<label for="d1">username:<input type="text" id="d1" name="username" value="默认值"></label>
    
disable 禁用
readonly只读    
```

# CSS

层贴样式表：给HTML标签添加样式的，让它变得更加的好看

```CSS
# 注释

/* 单行注释 */
/*
多行注释1
多行注释2
*/

通常我们在写css样式的时候也会用注释来划定样式区域(因为HTML代码多所以对呀的css代码也会很多)
/*这是博客园首页的css样式文件*/
/*顶部导航条样式*/
...
/*左侧菜单栏样式*/
...
/*右侧菜单栏样式*/
...

```

## 1 CSS语法

```css
/* css的语法结构 */
选择器 {
  属性1:值1;
  属性2:值2;
  属性3:值3;
  属性4:值4;
}


/* css的三种引入方式 */
/* 1. style标签内部直接书写 */
<style>
    h1  {
        color: burlywood;
    }
</style>

/* 2.link标签引入外部css文件(最正规的方式 解耦合) */
	<link rel="stylesheet" href="mycss.css">

/* 3.3.行内式(一般不用) */
	<h1 style="color: green">老板好 要上课吗?</h1>
```

```css
/* css的学习流程
	1. 先学如何查找标签
		css查找标签的方式你一定要学会
		因为后面所有的框架封装的查找语句都是基于css来的
		css选择器很简单很好学不要有压力!!!
	
	2. 之后再学如何添加样式
*/
```

## 2 CSS选择器

### 2.1 基本选择器

```css
/*
# id选择器

# 类选择器

# 元素/标签选择器

# 通用选择器
*/

<style>
    /*id选择器*  !*找到id是d1的标签 将文本颜色变成绿黄色*!*/
    #d1 {  
       color: greenyellow;
    }

    /* 类选择器 * !*找到class值里面包含c1的标签*!*/
    .c1 {  
       color: red;
    }

    /* 元素(标签)选择器 * !*找到所有的span标签*!*/
    span {  
       color: red;
    }

    /* 通用选择器 * !*将html页面上所有的标签全部找到*!*/
    * {  
       color: green;
    }
</style>

p#d1.c1	 	<p id="d1" class="c1"></p>		emmet 插件
```

### 2.2 组合选择器

```css
/*
在前端 我们将标签的嵌套用亲戚关系来表述层级
<div>div
    <p>div p</p>
    <p>div p
        <span>div p span</span>
    </p>
    <span>span</span>
    <span>span</span>
</div>

  div里面的p span都是div的后代
  p是div的儿子
  p里面的span是p的儿子 是div的孙子
  div是p的父亲
  ...
*/

/*
# 后代选择器
# 儿子选择器
# 毗邻选择器
# 弟弟选择器
*/

/* 后代选择器 */
div span {
    ...
}

/* 儿子选择器 */
div>span {
    
}

/* 毗邻选择器 同级别紧挨着的下面第一个 */
div+span {
    ...
}

/* 弟弟选择器 同级别下所有span */
div~span {
    
}
```

### 2.3 属性选择器

```css
/*
# 含有某个属性
# 含有某个属性并且有某个值
# 含有某个属性并且有某个值的某个标签

# 属性选择器是以 [] 作为标志的
*/

[username] {	/* 将所有含有属性名是username的标签的样式改为... */
    ...
}

[username="jaon"] {		/* 找到所有属性名是username并且属性值是jaon的标签 */
    ...
}

input[username="jaon"] {	/* 找到所有属性名是username并且属性值是jason的input标签 */
    ...
}
```

### 2.4 分组与嵌套

```css
div,
p,
span {  /*逗号表示并列关系*/
            color: yellow;
        }
#d1,.c1,span {
            color: orange;
        }
```

### 2.5 伪类选择器

```css
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <style>
        body {
            background-color: black;
        }
        a:link {  				
            color: red;			/*访问之前的状态*/
        }
        a:hover {  /*需要记住*/
            color: aqua;  		/*鼠标悬浮态*/
        }
        a:active {
            color: black;  		/*鼠标点击不松开的状态  激活态*/
        }
        a:visited {
            color: darkgray;  	/*访问之后的状态*/
        }
        p {
            color: darkgray;
            font-size: 48px;
        }
        p:hover {
            color: white;
        }
        
        input:focus {  			/*input框获取焦点(鼠标点了input框)*/
            background-color: red;
        }
    </style>
</head>
<body>
<a href="https://www.jd.com/">在不在?</a>
<p>点我有你好看</p>
<input type="text">
</body>
</html>
```

### 2.6 伪元素选择器

```css
p:first-letter {			/* 调整段落首字母颜色 */
    font-size: 48px;
    color: orange;
}

p:before {					/* 在文本开头 添加 css 内容 */
    content: "ni hao";
    color: blue;
}

p:after {					/* 在文本最后 添加 css 内容 */
    content: "?";
    color: green;
}

ps: before和after通常都是用来清除浮动带来的影响:父标签塌陷的问题
```

### 2.7 选择器优先级

```css
/*
id 选择器
类
标签选择器
行内式
*/
# 选择器相同
	就近原则: 谁离标签近，就听谁的
# 选择器不同:
	行内 > id选择器 > 类选择器 > 标签选择器
	精确度越高越有效

!important 强制让标签采用你的样式，不推荐使用
```

## 3 CSS属性相关（操作标签样式）

### 3.1 设置长宽

```css
<style>
    p {
        background-color: red;
        height: 200px;
        width: 400px;
        /* 块儿级标签默认占浏览器一整行，块儿级标签的高度也取决于标签内部文本的高度，但是可以通过CSS设置 */
    }
    span {
        background-color: green;
        height: 200px;
        width: 400px;
        /* 行内标签无法设置长宽 由内部文本决定 */
    }
</style>
```

### 3.2 字体属性

```css
<style>
p {
    font-family: "Arial Black", "微软雅黑", "...";  
    /* 第一个不生效就用后面的, 写多个备用 */

    font-size: 25px;

    font-weight: inherit;
    /* bolder 粗
      lighter 细
      100~900 粗细调节
      inherit 继承父元素的粗细值
    */

    color: red;
    /* 直接写颜色英文 */

    color: #ee762e;
    /* 颜色编号 */

    color: rgb(128, 128, 128);
    /* 三基色, 数字范围 0-255 */

    color: rgba(55, 18, 128, 0.1)
    /* 第四个参数是颜色的透明度, 范围是 0-1, 越小越透明*/

    /* 
    当你想要一些颜色的时候 可以利用现成的工具
            1. pycharm提供的取色器
            2. qq或者微信截图功能
    */
}
</style>
```

### 3.3 文字属性

```css
<style>
    p {
        /* 文本对齐 居中, 右, 左, 两端对齐 */
        text-align: cen    <style>
        p {
            /* 文本对齐 居中, 右, 左, 两端对齐 */
            text-align: center;
            text-align: right;
            text-align: left;
            text-align: justify;

            /* 文字修饰 下划线, 上划线, 删除线, 无*/
            text-decoration: underline;
            text-decoration: overline;
            text-decoration: line-through;
            text-decoration: none;

            /* 字体大小 */
            font-size: 33px;

            /* 文本缩进 */
            text-indent: 33px;
        }
        
        a {
            text-decoration: none;
        }
        /* 主要用于给 a 标签去掉自带的下划线 */
    </style>ter;
        text-align: right;
        text-align: left;
        text-align: justify;
    }
</style>
```

### 3.4 背景图片

```css
<style>
    #d1 {
        height: 500px;
        background-color: red;
    }
    #d2 {
        height: 500px;
        background-color: greenyellow;
    }
    #d3 {
        height: 600px;
        background-image: url("诺艾尔.png");
        background-attachment: fixed;
    }
    #d4 {
        height: 500px;
        background-color: aqua;
    }
</style>

ps: 在调样式的时候，可以借助浏览器快速的微调，然后将调好的参数修改到CSS样式中
```

### 3.5 边框

```css
<style>
    p {
        background-color: red;

        /* 边界框 */
        border-width: 5px;
        border-style: solid;
        border-color: green;
    }
    div[id="1"] {
        border-right-width: 5px;
        border-right-style: dotted;
        border-right-color: red;

        border-left-width: 10px;
        border-left-style: solid;
        border-left-color: greenyellow;

        border-top-width: 10px;
        border-top-style: dashed;
        border-top-color: deeppink;

        border-bottom-width: 10px;
        border-bottom-style: solid;
        border-bottom-color: greenyellow;
    }
    div[id="2"] {
        border: 3px solid red;
        /* 3者位置可以随意 */
    }
    #d1 {
        background-color: greenyellow;
        height: 400px;
        width: 400px;
        border-radius: 50%; 
        /* 直接写50%即可 长宽一样就是圆 不一样就是椭圆 */
    }
</style>
```

### 3.6 display属性

```css
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        #d1 {
            height: 100px;
            width: 100px;
            background-color: red;

            display: none;
            /* 隐藏标签不展示到前端页面并且原来的位置也不占有了，但是还存在于文档上 */
        }
        #d2 {
            height: 100px;
            width: 100px;
            background-color: yellowgreen;

            display: inline;
            /* 将标签设置为行内标签的特点 */
        }
        #s1 {
            height: 100px;
            width: 100px;
            background-color: blue;

            display: block;
            /* 将标签设置为块儿级标签的特点 */            
        }
        #s2 {
            height: 100px;
            width: 100px;
            background-color: pink;

            display: inline-block;
            /* 标签即可以在一行显示又可以设置长宽 */   
        }
        #d3 {
            height: 100px;
            width: 100px;
            background-color: black;

            display: inline-block;
            /* 标签即可以在一行显示又可以设置长宽 */   
        }

    </style>
</head>
<body>
    <div id="d1">div1</div>
    <div id="d2">div2</div>
    <span id="s1">span1</span>
    <span id="s2">span2</span>
    <div id="d3">div3</div>
</body>
</html>
```

### 3.7 盒子模型

```css
/*
盒子模型
	就以快递盒为例
		快递盒与快递盒之间的距离		  标签与标签之间的距离 		margin  外边距
		盒子的厚度					  标签的边框 				border  边框
		盒子里面的物体到盒子的距离		 内容到边框的距离  			 padding 内边距
		物体的大小					  内容 					  content 内容
	
	
	如果你想要调整标签与标签之间的距离 你就可以调整margin
	
	浏览器会自带8px的margin，一般情况下我们在写页面的时候，上来就会先将body的margin去除
	
*/

<style>
    body {
        margin: 0; 
        /* 上下左右全是0 */

        /* margin: 10px 20px;   */
        /* 第一个上下 第二个左右 */

        /* margin: 10px 20px 30px;   */
        /* 第一个上  第二个左右  第三个下 */

        /* margin: 10px 20px 30px 40px;  */
        /* 上 右 下 左 */
    }

    p {
        margin-top: 0;
        margin-right: 0;
        margin-bottom: 0;
        margin-left: 0;
    }

    #d1 {
        margin-bottom: 50px;
    }
    #d2 {
        margin-top: 20px;        /* 不叠加，只取大的 */
    }
    #d3 {
        border: 1px solid orange;
        height: 50px;
        width: 50px;
        background-color: blue;
        margin: auto;         /* 只能做到标签的水平居中 */
    }
    p {
        border: 3px solid red;
        padding-left: 10px;
        padding-top: 20px;
        padding-right: 20px;
        padding-bottom: 50px;

        padding: 10px;
        padding: 10px 20px;
        padding: 10px 20px 30px;
        padding: 10px 20px 30px 40px;  /*规律和margin一模一样*/
    }
</style>
```

### 3.8 浮动

```css
/*  
浮动的元素 没有块儿级一说 本身多大浮起来之后就只能占多大
只要是设计到页面的布局一般都是用浮动来提前规划好
*/

<style>
    #d1 {
        height: 200px;
        width: 200px;
        background-color: red;
        float: left;    /* 浮动, 浮到空中往左飘 */
    }
    #d2 {
        height: 200px;
        width: 200px;
        background-color: greenyellow;
        float: right;   /* 浮动, 浮到空中往右边飘 */
    }
</style>
```

### 3.9 解决浮动带来的影响

```css
/* 浮动带来的影响 */
会造成父标签塌陷的问题

1. 自己加一个div设置高度
2. 利用clear属性
            clear: left;  /* 该标签的左边(地面和空中)不能有浮动的元素 */
            /* clear: right; */
            /* clear: both; */

3. 通用解决方法， 谁塌陷就给谁加 clearfix 属性即可
    <style>
        .clearfix:after {
            content: '';
            display: block;
            clear: both;
        }
    </style>
</head>
<body>
    <div id="d1" class="clearfix">
        <div id="d2"></div>
        <div id="d3"></div>
        <div id="d4"></div>
    </div>
</body>

```

### 3.10 溢出属性

```css
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        p {
            height: 100px;
            width: 100px;
            border: 3px solid red;
            overflow: visiable;     /* 默认是可见 溢出还是展示 */
            overflow: hidden;       /* 溢出部分隐藏 */
            overflow: scroll;        /* 设置成上下滚动条的形式 */
            overflow: auto;         /* 自动 */
        }
    </style>
</head>
<body>
    <p>概括来说嘎拉哈就哈哈旮角了哈哈  哈就好啦 啊哈哈哈哈  概括来说嘎拉哈就哈哈旮角了哈哈  哈就好啦 啊哈哈哈哈  </p>
</body>
```

```css
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        body {
            margin: 0;
            background-color: #4e4e4e;
        }
        #d1 {
            height: 200px;
            width: 200px;
            border-radius: 50%;
            border: 5px solid white;
            margin: auto;
            overflow: hidden;
        }
        #d1>img {
            max-width: 100%;
            /* width: 100%; */
            /* 占标签100%比例 */
        }
    </style>
</head>
<body>
    <div id="d1">
        <img src="诺艾尔.png" alt="">
    </div>
</body>
</html>
```

### 3.11 定位

* 静态

​			所有的标签默认都是静态的 static，无法改变位置

* 相对定位

​			相对于标签原来的位置做移动 relative

* 绝对定位 （常用）

​			相对于已经定位过的父标签做移动 （如果没用父标签，以body为参照）

​			eg：小米网站购物车

​			当你不知道页面其它标签的位置和参数

* 固定定位（常用）

​			相对于浏览器固定在某个位置

​			eg：右侧小广告

```css
/* 相对定位 */
    <style>
        body {
            margin: 0;
        }
        #d1 {
            height: 100px;
            width: 100px;
            background-color: red;
            left: 50px;    /* 从左往右, 如果是负数方向相反 */
            top: 50px;    /* 从上往下, 如果是负数方向相反 */
            /* position: static  默认 */
            position: relative;
            /* 
            相对定位, 标签由static变为relative, 它的性质就从原来没用定位的标签变成了已经定位过的标签
            虽然没用动, 但是性质也已经改变了
            */
        }
    </style>


ps: 浏览器优先展示文本内容，如果被挡住了，文本优先
```

```css
/* 绝对定位 */
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        body {
            margin: 0;
        }
        #d2 {
            height: 100px;
            width: 200px;
            background-color: red;
            position: relative;
        }
        #d3 {
            height: 200px;
            width: 400px;
            background-color: yellowgreen;
            position: absolute;
            left: 200px;
            top: 100px;
        }
    </style>
</head>
<body>
    </div>
    <div id="d2">
        <div id="d3"></div>
    </div>
</body>
</html>
```

```css
/* 固定定位 */
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        body {
            margin: 0;
        }
        #d4 {
            position: fixed;
            bottom: 10px;
            right: 20px;
            height: 50px;
            width: 100px;
            background-color: white;
            border: 3px solid black;
        }
    </style>
</head>
<body>
    </div>
    <div id="d2">
        <div id="d3"></div>
    </div>
    <div style="height: 500px; background-color: red;"></div>
    <div style="height: 500px; background-color: greenyellow;"></div>
    <div style="height: 500px; background-color: blue;"></div>
    <div id="d4">回到顶部</div>
</body>
</html>
```

### 3.12 验证浮动和定位是否脱离文档流

```css
原来的位置是否还保留
/*
浮动	
相对定位
绝对定位
固定定位
*/


/* 不脱离文档流 */
相对定位

/* 脱离文档流 */
浮动、绝对定位、固定定位
```

### 3.13 z-index之模态框案例

```css
eg: 百度登录页面 三层结构
	1. 最底部为正常内容（z=0）	最远
	2. 黑色的透明区（z=99）		 中间
	3. 白色的注册区域（z=100）	离用户最近

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        body {
            margin: 0;
        }
        .cover {
            position: fixed;
            left: 0;
            top: 0;
            right: 0;
            bottom: 0;
            background-color: rgba(0, 0, 0, 0.4);
            z-index: 99;
        }
        .modal {
            background-color: white;
            height: 200px;
            width: 400px;
            position: fixed;
            left: 50%;
            top: 50%;
            z-index: 100;
            margin-left: -200px;
            margin-top: -100px;
        }
    </style>
</head>
<body>
    <div>
        最底层的页面内容
    </div>
    <div class="cover"></div>
    <div class="modal">
        <h1>登录页面</h1>
        <p>
            username:
            <input type="text">
        </p>
        <p>
            password:
            <input type="password">
        </p>
        <button>点击</button>
    </div>
</body>
</html>
```

### 3.14 透明度 opacity

```css
不单单可以修改颜色的透明度，还可以修改字体的透明度
rgba 只能影响颜色
opacity 可以修改颜色和字体
```

### 3.15 博客园首页搭建

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>BLOG</title>
    <link rel="stylesheet" href="BOKE.css">
</head>
<body>
    <div class="blog-left">
        <div class="blog-avatar">
            <img src="诺艾尔.png" alt="">
        </div>
        <div class="blog-title">
            <p>诺艾尔</p>
        </div>
        <div class="blog-info">
            <p>3皇冠、赤角、华馆套</p>
        </div>
        <div class="blog-link">
            <ul>
                <li><a href="">西风见习骑士</a></li>
                <li><a href="">女仆</a></li>
                <li><a href="">岩王帝姬</a></li>
            </ul>
        </div>
        <div class="blog-tag">
            <ul>
                <li><a href="">华馆</a></li>
                <li><a href="">角斗士</a></li>
                <li><a href="">六命</a></li>
            </ul>
        </div>
    </div>
    <div class="blog-right">
        <div class="article">
            <div class="article-title">
                <span class="title">论华馆套的重要性</span>
                <span class="date">2023/6/29</span>
            </div>
            <div class="article-body">
                <p>大风车大风车大风车大风车大风车大风车大风车大风车大风车</p>
            </div>
            <div class="article-bottom">
                <span>#大风车&nbsp;&nbsp;&nbsp;&nbsp;</span>
                <span>#大风车</span>
            </div>
        </div>
        <div class="article">
            <div class="article-title">
                <span class="title">论角斗士的重要性</span>
                <span class="date">2023/6/30</span>
            </div>
            <div class="article-body">
                <p>A4下A4下A4下A4下A4下A4下A4下A4下A4下A4下A4下A4下A4下</p>
            </div>
            <div class="article-bottom">
                <span>A4下&nbsp;&nbsp;&nbsp;&nbsp;</span>
                <span>A4下</span>
            </div>
        </div>
        <div class="article">
            <div class="article-title">
                <span class="title">论赤角的重要性</span>
                <span class="date">2023/6/31</span>
            </div>
            <div class="article-body">
                <p>防御爆伤防御爆伤防御爆伤防御爆伤防御爆伤防御爆伤防御爆伤</p>
            </div>
            <div class="article-bottom">
                <span>防御爆伤&nbsp;&nbsp;&nbsp;&nbsp;</span>
                <span>防御爆伤</span>
            </div>
        </div>
        <div class="article">
            <div class="article-title">
                <span class="title">论白影剑的重要性</span>
                <span class="date">2023/7/1</span>
            </div>
            <div class="article-body">
                <p>防御攻击防御攻击防御攻击防御攻击防御攻击防御攻击防御攻击</p>
            </div>
            <div class="article-bottom">
                <span>#防御攻击&nbsp;&nbsp;&nbsp;&nbsp;</span>
                <span>#防御攻击</span>
            </div>
        </div>
    </div>
</body>
</html>
```

```css
/* 这是BLOG首页的样式文件 */

/* 通用样式 */
body {
    margin: 0;
    background-color: #eeeeee;
}
a {
    text-decoration: none;
}
ul {
    list-style-type: none;
    padding-left: 0;
}

/* 左侧样式 */
.blog-left {
    float: left;
    width: 20%;
    height: 100%;
    position: fixed;
    background-color: #4e4e4e;
}

.blog-avatar {
    height: 200px;
    width: 200px;
    border-radius: 50%;
    border: 8px solid white;
    margin: 20px auto;
    overflow: hidden;
}
.blog-avatar>img {
    max-width: 100%;
}

.blog-title, .blog-info {
    color: darkgray;
    font-size: 20px;
    text-align: center;
}

.blog-link, .blog-tag {
    font-size: 24px;
}
.blog-link a, .blog-tag a {
    color: darkgray;
}
.blog-link ul, .blog-tag ul {
    text-align: center;
    margin-top: 80px;
}
.blog-link a:hover, .blog-tag a:hover {
    color: white;
}

/* 右侧样式 */
.blog-right {
    float: right;
    width: 80%;
    height: 1000px;
}

.article {
    background-color: white;
    margin: 20px 30px 40px 30px;
    box-shadow: 5px 5px 5px rgba(0, 0, 0, 0.5);
}

.title {
    font-size: 28px;
    margin: 6px px;
}
.date {
    float: right;
    font-weight: bolder;
    margin: 10px 20px;
}
.article-title {
    color: orange;
    border-left: 5px solid gold;
    text-indent: 16px;
}

.article-body {
    font-size: 18px;
    text-indent: 36px;
    border-bottom: 1px solid black;
}
.article-bottom {
    padding: 10px 36px ;
    font-size: 15px;
}
```

# JavaScript

## 1. jS简介

```javascript
js 也是一门编程语言, 它也是可以写后端代码的
	nodejs  支持js代码跑在后端服务器上
    
# js注释

// 单行注释
/*
多行注释1
多行注释2
*/
```

```javascript
# 两种引入方式
	1. script 标签内部直接书写js代码
    2. script 标签src 属性引入外部js代码
    
# js 语法结构
	js是以分号作为语句的结束
    但是如果不写分号，问题不大，也可以正常执行，但是相当于没有结束符
    
# js学习流程
	变量
    数据类型
    流程控制
    函数
    对象
    内置方法/模块

https://www.cnblogs.com/Dominic-Ji/p/9111021.html
```

## 2. 变量

```javascript
在js中 首次定义一个变量名的时候需要用关键字声明
	1. 关键字var
		var name='jason'
	2. es6推出的新语法
		let name='jason'
        
	如果你的编辑器支持的版本是5.1那么无法使用let
	如果是6.0则向下兼容 var let
    
var 与 let 的区别
n = 10
for n in range(5):
  print(n)
print(n)  

# var 5		let 10

var在for循环里面定义也会影响到全局
let在局部定义只会在局部生效

```

## 3. 常量

```javascript
const pi = 3.14
```

