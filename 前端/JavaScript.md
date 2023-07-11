# JavaScript

## 1 js简介

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

## 2 变量

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


js 变量的命名规范
	1. 变量名只能是 
    	数字 字母 下划线 $
    2. 变量名命名规范
    	js 中推荐使用驼峰式命名
        userName
        dataOfDb
	3. 不能用关键字作为变量名

js 代码的书写位置
	1. 可以单独开设js文件书写
    2. 可以在浏览器提供的console界面书写
     	在浏览器书写js的时候，左上角的清空按钮只是清空当前页面, 代码其实还在
		如果想要重来, 重新打开网站
```

## 3 常量

```javascript
const pi = 3.14
```

## 4 数据类型

```javascript
js 动态类型
js 也是一门面向对象的编程语言，即一切皆对象

name = "java"
name = 123
name = [1, 2, 3, 4]

// name 可以指向任意的数据类型
// 但是有些语言， 变量名指向一种类型，后续不能更改
```

### 4.1 数值类型 （number）

```javascript
var a = 11
var b = 11.11
// 如何查看当前数据类型

typeof a;
"number"

// 特殊的 NaN 数值类型，表示的意思  不是一个数字  "NOT A NUMBER"
```

```js
// 类型转换
parseInt("123")
parseFloat("11.11")

parseInt("11.11")				// 11
parseInt("12afagadg14214afaf")	// 12 	只要字符串开头有数字即可
parseInt("agaggddasg13141")		// NaN


```

### 4.2 字符类型 (string)

```js
var s1 = "jas";
var s2 = 'jas';
var s3 = '''das''';		//	 报错， 不支持三引号

// 模板字符串
var s3 = `
123
455
gahdlag
`;

// 模板字符串除了可以定义多行文本之外，还可以实现字符串的格式化操作
var name = "ges";
var age = 123;
var s = `my name is ${name} and my age is ${age}`;

// my name is jason and my age is 18'
// ${}  会自动去前面找 {}内的变量值， 如果变量没有定义，则直接俄报错

// 在书写js代码的时候， 不需要管左侧箭头的内容

// 字符串的拼接
// python 中不推荐使用 + 拼接使用 join
// 在 js 中推荐使用 + 
```

#### 4.2.1 字符串类型的常用方法

```js
.length			返回长度							len()

.trim()			移除空白							strip()
// 不能加括号指定去除的内容

.trimLeft()		移除左边的空白						  lstrip()

.trimRight()	移除右边的空白

.charAt(n)		返回第n个字符

.concat(value, ...)			拼接							join()
        
.indexOf(substring, start)	子序列位置

.substring(from, to)		根据索引获取子序列				[] 索引取值
// name2.substring(0, -1);  不支持

.slice(start, end)			切片
// slice 可以，以后切片通一用slice

.toLowerCase()				小写							lower()

.toUpperCase()				大写							upper()

.split(delimiter, limit)	分割							split()
```

```js
name3 = "aga,gadga,fasfa,fasfasf,fafa,faga";
// 'aga,gadga,fasfa,fasfasf,fafa,faga'
name3.split(",", 2);
// (2) ['aga', 'gadga']


name = "hello";
p = 1111
name.concat(p);		
// js 是弱类型语言， 内部会自动转成相同的数据类型做操作
// "hello1111"
```

### 4.3 布尔值 (boolean)

```js
python中
	True	False
js中
	true	false

// 布尔值是false 的有哪些
空字符串、0、null、undefined、NaN
```

* null 与 undefined

```js
null	表示值为空，一般是指定或者清空一个变量时使用
	name = 'hello'
	name = null

undefined	表示声明了一个变量，但是没有做初始化操作（没有给值）
	函数没有指定返回值的时候，返回的也是 undefined
```

### 4.4 对象 (object)

#### 4.4.1 数组

* 类似于python中的列表

```js
var l = [11, 22, 33, 44, 55];

typeof l;
// 'object'

var l = [11, "hello", true];

l[1]
// 'hello'

l[-1]	// 报错，不支持负数索引
```

* 数组中对应的方法

```js
.length			数组的大小

.push			尾部追加元素

.pop()			获取尾部的元素

.unshift(ele)	头部插入元素

.shift()		头部移除元素

.slice(start, end)	切片

.reverse()		反转

.join(seq)		将数组元素连接成字符串
// l.join(":")

.concat(val, ...)	连接数组
// Python 中的 extend        
        
.sort()        排序

.forEach()		将数组的每个元素传递给回调函数

.splice()		删除元素，并向数组添加新元素

.map()			返回一个数组元素调用函数处理后的值的新数组
```

```js
.forEach()		将数组的每个元素传递给回调函数

var ll = [111, 222, 333, 444, 555];
ll.forEach(function(value) {console.log(value)}, ll);

// 111	一个参数就是数组里面每个元素的对象
// 222
// 333
// 444
// 555

ll.forEach(function(value, index) {console.log(value, index)}, ll);

// 111 0	两个参数: 元素 + 索引值
// 222 1
// 333 2
// 444 3
// 555 4


// 111 0 (5) [111, 222, 333, 444, 555]		三个参数: 元素 + 索引 + 数据的来源
// 222 1 (5) [111, 222, 333, 444, 555]
// 333 2 (5) [111, 222, 333, 444, 555]
// 444 3 (5) [111, 222, 333, 444, 555]
// 555 4 (5) [111, 222, 333, 444, 555]

// 最多三个
```

```js
.splice()		删除元素，并向数组添加新元素

var ll = [1, 2, 3, 4, 5, 65];

ll.splice(0, 3)
// [4, 5, 65]	两个参数，第一个是起始位置，第二个是删除的个数

ll.splice(0, 1, 222);
// [222, 5, 65]	三个参数，先删除，后添加

ll.splice(0, 1, [111, 222, 333, 444]);
// [Array(4), 5, 65]
```

```js
.map()			返回一个数组元素调用函数处理后的值的新数组

var oo = [11, 22, 33, 44, 55];

oo.map(function(value){console.log(value)}, oo);
// VM2284:1 11
// VM2284:1 22
// VM2284:1 33
// VM2284:1 44
// VM2284:1 55

oo.map(function(value, index){return value * 2}, oo);
// [22, 44, 66, 88, 110]

oo.map(function(value, index, arr){return value * 2}, oo);
```

#### 4.4.2 运算符

```js
// === 算术运算符 ===

var x = 10;
var res1 = x++;		// 先做赋值，后自增
var res2 = ++x;		// 先自增，后赋值

// === 比较运算符 ===

1 == '1';			// 弱等于, 内部转换成相同的数据类型比较
// true

1 === '1';			// 强等于, 内部不做类型转换
// false

1 != '1';	// false
1 !== '1';	// true

// === 逻辑运算符 ===
	// python 中 and or not
	// js 中 &&	||	!

5 && '5';
// '5'

0 || 1;
// 1

// === 赋值运算符 ===
=
+=
-=
```

#### 4.4.3 流程控制

```js
// if 判断
var age = 10;
if (条件) {条件成立之后执行的代码快}

if (age > 18){
    console.log("来啊，来啊")
}

if (age > 18){
    console.log("hello")
}else{
    console.log("world")
}

if (age < 18){
    console.log("你好")
} else if (age < 24) {
    consloe.log("小姐姐")
} else {
    console.log("你是一个好人")
}

// 在 js 中代码是没有缩进的，处于习惯人为加上去的
()	条件
{}	代码块
```

```js
// switch 语法
// 提前列举好可能出现的条件和解决方式
var num = 2;
switch (num) {
    case 0:
        console.log("抽烟");
        break;		// 不加break 匹配到一个之后，就会一直往下执行
    case 1:
        console.log("喝酒");
        break;
    case 2:
        console.log("烫头");
        break;  
    default: 
        console.log("条件都没有匹配上，默认走的流程")
}

```

```js
// for 循环

// 打印 0-9
for (let i = 0; i < 10; i++) {
    console.log(i);
}
```

```js
// while 循环
var i = 0;
while (i < 100) {
    console.log(i);
    i++;
}
```

```js
// 三元运算符
res = 1 > 2 ? 1:3;
// 条件成立取 ? 后面的值，不成立取 : 后面的值 
```

#### 4.4.4 函数

```js
定义函数关键字 function

function 函数名(形参1, 形参2...) {
    函数体代码
}
```

```js
// 无参函数
function func1() {
	console.log("hello, world")    
}

func1()		// 加括号调用
```

```js
// 有参函数
function func2(a, b) {
    console.log(a, b);
} 

func2(1, 2)		// 1, 2
func2(1, 2, 3, 4, 5)	// 1, 2		传多了，只要对应的数据
func3(1)		// 1, undefined 	传少了没关系

// 关键字 arguments
function func2(a, b) {
    console.log(arguments);		// 能够获取到函数接收到的所有的参数
    console.log(a, b);
} 

function func2(a, b) {
    if (arguments.length < 2) {
        console.log("传少了")
    } else if (arguments.length > 2) {
        console.log("传多了")
    }
    else{
        console.log(a, b);
    }    
} 
```

```js
// 函数的返回值
function index() {
    return 666
}

function index() {
    return 666, 777, 888, 999
}
res = index();	// res 只能拿到 999

function index() {
    return [666, 777, 888, 999]
}
```

```js
// 匿名函数
function() {
    console.log("hello, world")
}

var res = function() {
    console.log("hello, world")
}
```

```js
// 箭头函数 (需要用到),处理简单的业务逻辑，类似于Python中的匿名函数
var func1 = v => v;

// 等价于
var func1 = function(v) {
    return v;
}

var func2 = (arg1, arg2) => arg1 + arg2;

// 等价于
var func2 = function(arg1, arg2) {
    return arg1 + arg2;
}
```

```js
// 函数的全局变量与局部变量
// 跟python 查找变量的顺序一致


// 词法分析，直接忽略掉
```

#### 4.4.5 自定义对象

```js
// 可以看成python中的字典，但是操作起来更加方便
```

```js
// 创建自定义对象 {}
var d = {
    "name": "hello",
    "age": 18,
};

d["name"]	// "hello"
d.name		// "hello"	比 python 字典获取值更加的方便
d.age		// 18

for(let i in d) {
    console.log(i, d.i)
};
// 支持for 循环，暴露在外界可以直接获取的也是键
```

```js
// 第二种创建自定义对象方式，关键字 new
var d2 = new Object();		// 空字典 {}

d2.name = "hello";
d2.age = 18;
```

#### 4.4.6 日期(Date)对象

```js
let d3 = new Date();
// Mon Jul 03 2023 15:49:27 GMT+0800 (GMT+08:00)	结构化时间

d3.toLocaleString()
// '2023/7/3 15:49:27'			格式化时间

// 也支持自己手动输入时间
let d4 = new Date("2200/11/11 11:11:11");	// Tue Nov 11 2200 11:11:11 GMT+0800 (GMT+08:00)
d4.toLocaleString();

let d5 = new Date(1111, 11, 11, 11, 11, 11);
d5.toLocaleString();	// '1111/12/11 11:11:11'	月份是从0开始的 0-11月
```

```js
// 时间对象具体方法
let d6 = new Date();
d6.getDate();		// 获取日
d6.getDay();		// 获取星期几
d6.getMonth();		// 获取月份 0-11月
d6.getFullYear();	// 获取完整的年份
d6.getHours();		// 小时
d6.getMinutes();	// 分钟
d6.getSeconds();	// 秒
d6.getMilliseconds();	// 毫秒
d6.getTime();		// 时间戳
```

#### 4.4.7 JSON 对象 

```python
# python

import json
json.JSONEncoder

Python			JSON	
dict			object
list,tuple		array
str				string
int,float		number
True			true
False			false
None			null
```

```js
//在js 中也有序列化和反序列化

JSON.stringify()		dumps

JSON.parse()			loads
```

```js
let d7 = {
    "name": "hello",
    "age": 18
};

let res = JSON.stringify(d7);
// '{"name":"hello","age":18}'

JSON.parse(res)
// {name: 'hello', age: 18}
```

#### 4.4.8 RegExp 对象 （正则）

```python
在 python 使用正则， 借助 re 模块 
```

```js
在 js 中， 需要创建正则对象

// 第一种定义正则的方式
let reg1 = new RegExp("^[a-zA-Z][a-zA-Z0-9]{5,11}");

// 第二种 推荐
let reg2 = /^[a-zA-Z][a-zA-Z0-9]{5,11}/;

// 匹配内容
reg1.test("hellowdsb");		// true
reg2.test("hellowdsb");		// true


let sss = "egon dsb dsb";
sss.match(/s/);
// ['s', index: 6, input: 'egon dsb dsb', groups: undefined] 拿到一个就停止了

// 全局匹配
sss.match(/s/g)		// g 表示全局模式
//  ['s', 's']
```

```js
// 全局匹配模式吐槽点
// 全局模式有一个 lastIndex 属性
let reg3 = /^[a-zA-Z][a-zA-Z0-9]{5,11}/g;
reg3.test("hellowdsb");
// true
reg3.test("hellowdsb");
// false
reg3.test("hellowdsb");
// true
reg3.lastIndex;
// 9
......
```

```js
// 吐槽点二
let reg4 = /^[a-zA-Z][a-zA-Z0-9]{5,11}/;
reg4.test();	// 什么都不传，默认传的是 undefined
// true

reg4.test(undefined);
// true

let reg5 = /undefined/;
reg5.test();

// 总结： 书写js 正则的时候一定要注意上述问题
// 一般接触不到
```

#### 4.4.9 Math 对象

```js
abs(x)		// 返回绝对值
exp(x)		// 返回e的指数
floor(x)	// 对数进行下舍入
log(x)		// 返回数的自然对数(底为 e)
max(x, y)	// 返回最大值
min(x, y)	// 返回最小值
pow(x, y)	// x ^ y
random()	// 0-1之间的随机数
round(x)	// 把数四舍五入为最接近的整数
sin(x)		// 返回数的正弦
sqrt(x)		// 返回数的平方根
tan(x)		// 返回角的正切
```

## 5 BOM 与 DOM

```JS
// 目前跟浏览器和html文件还是一点关系都没有
/*
BOM
	浏览器对象模型		Browser Object Model
	js操作浏览器
DOM
	文档对象模型		Document Object Model
	js代码操作标签
*/
```

### 5.1 BOM操作

#### 5.1.1 window 对象

window 对象指代的就是浏览器窗口

```js
window.innerHeight		// 浏览器窗口的高度

window.innerWidth		// 浏览器窗口的宽度

window.open("https://www.bilibili.com/", "", "height=600px, width=400px, top=200px, left=400px");
// 新建窗口打开页面，第二个窗口写空即可，第三个参数写窗口的大小和位置
// 扩展父子页面通信，window.opener()		了解

window.close()			// 关闭当前页面
```

#### 5.1.2 window 子对象

```js
window.navigator.appName	
'Netscape'

window.navigator.appVersion
'5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/114.0.0.0 Safari/537.36'

window.navigator.userAgent
'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/114.0.0.0 Safari/537.36'
// 用来表示当前是否是一个浏览器

/*
拓展：防爬措施
	1. 最简单最常用的一个就是校验当前请求的发起者是否是一个浏览器
		User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/114.0.0.0 Safari/537.36
	
		如何破解该措施
		在你的代码中加上user-agent配置即可
*/ 

window.navigator.platform
'Win32'	// 平台

navigator.platform
'Win32'	// 如果是 window子对象，window 可以省略不写

window.screen
// Screen {availWidth: 1474, availHeight: 864, width: 1536, height: 864, colorDepth: 24, …}


```

#### 5.1.3 history 对象

```js
window.history.back()		// 回退到上一页

window.history.forward()	// 前进到下一页

// 对应的就是浏览器左上方的两个箭头
```

#### 5.1.4 location 对象

```js
window.location.href		// 获取当前页面的url

window.location.href = url	// 跳转到指定的url

window.location.reload()	// 刷新页面		浏览器左上方的小圆圈
```

#### 5.1.5 弹出框

* 警告框

```js
alert("你不要过来啊！！！");
```

* 确认框

```js
confirm("你确定真的要这么做吗?");
// 确认 true
// 取消 false
```

* 提示框

```js
prompt("提示信息", "你好吗？");
// 可以给出具体的提示信息，例如 "你好吗" 变成 '不好'
```

#### 5.1.6 计时器相关

* 过一段时间之后触发（一次）

```js
<script>
    function func1() {
        alert(123)
    };
    let t = setTimeout(func1, 3000);    // 毫秒为单位，3秒之后自动执行 func1 函数

    clearTimeout(t)               
	// 取消定时任务, 如果你想要清除定时任务，需要提前用变量指代定时任务

</script>
```

* 每隔一段时间触发一次（循环）

```js
<script>

    function func2() {
        alert(123)
    };
    function show() {
        let t = setInterval(func2, 3000);		// 每隔3秒执行一次
        function inner() {
            clearInterval(t)					// 清除定时器
        };
        setTimeout(inner, 9000);				// 9秒之后触发
    };

    show();

</script>
```

### 5.2 DOM操作

DOM（Document Object Model）是一套对文档的内容进行抽象和概念化的方法。

当网页被加载时，浏览器回创建页面的文档对象模型（Document Object Model）

HTML DOM 模型被构造为对象的树

​                                                                            文档

​																			根元素

​									元素head																	元素body

​									元素title                                    属性 herf       元素 <a>                 元素<h1>

​							文本：”文档标题“                                            文本："我的链接"     文本："我的标题"

DOM 标准规定了HTML文档中每一个成分都是一个节点

```js
// JavaScript 可以通过DOM创建动态的HTML:
// 		JS 能够改变页面中所有的HTML元素、属性、CSS样式和对所有事件做出反应

// DOM操作操作的是标签，而HTML页面上的标签有很多
	1. 先学如何查找标签
    2. 再学DOM如何操作标签
```

#### 5.2.1 查找标签

* 直接查找

```js
/*
id  查找
类  查找
标签 查找
*/

// DOM 操作 需要用 document 起手
// 注意三个方法的返回值是不一样的，

document.getElementById("d2");
// <div id=​"d2">​…​</div>​

document.getElementsByClassName("c1");
// HTMLCollection [p.c1]

document.getElementsByTagName("div")
// HTMLCollection(3) [div#d2, div, div, d2: div#d2]
```

```js
document.getElementsByTagName("div")[0]
// <div id=​"d2">​…​</div>​

let divEle = document.getElementsByTagName("div")[1]

divEle;
// <div>​div>div​</div>​

// 当你用变量名指代标签对象的时候，一般情况下都推荐你书写成 xxxEle
// aEle		pEle		divEle
```

* 间接查找

```JS
let pEle = document.getElementsByClassName("c1")[0]

pEle.parentElement						// 拿父节点
// <div id=​"d2">​…​</div>​

pEle.parentElement.parentElement
// <body>​…​</body>​

pEle.parentElement.parentElement.parentElement
// <html>...
```

```js
et divEle = document.getElementById("d2");	

divEle.children						// 获取所有的子标签

divEle.children[0]					
// <div>​div>div​</div>​

divEle.firstElementChild			
// <div>​div>div​</div>​

divEle.lastElementChild
// <p>​div>p​</p>​

divEle.nextElementSibling			// 同级别下面第一个
// <div>​div 下面的 div ​</div>​

divEle.previousElementSibling		// 同级别上面第一个
// <div>​div 上面的 div ​</div>​
```

#### 5.2.2 节点操作

通过DOM操作动态的创建img标签，并且给标签添加属性

最后将标签添加到文本中

```js
let imgEle = document.createElement("img");		// 创建标签

imgEle
// <img>​

imgEle.src = "111.png";							// 给标签设置默认属性
// '111.png'

imgEle
// <img src=​"111.png">​

imgEle.setAttribute("username", "诺艾尔");			// 既可以设置默认属性，也可以自定义属性

imgEle
// <img src=​"111.png" username=​"诺艾尔">​

imgEle.title = "一张图片";
imgEle
// <img src=​"111.png" username=​"诺艾尔" title=​"一张图片">​

let divEle = document.getElementById("d1");

divEle.appendChild(imgEle);						// 标签内部添加元素 (尾部追加)
// <img src=​"111.png" username=​"诺艾尔" title=​"一张图片">​

divEle
/*
<p id="d1">
    "div>p"
	<img src="111.png" username=​"诺艾尔" title=​"一张图片">
</p>
*/
```

创建 a 标签，设置属性，设置文本，添加到指定的标签的上面

```js
let aEle = document.createElement("a");

aEle.href = "https://www.mzitu.com/";

aEle.innerText = "电玩，有好看的";			// 给标签设置文本内容

let divEle = document.getElementById("d1");

let pEle = document.getElementById("d2");

divEle.insertBefore(aEle, pEle);		// 添加标签内容指定位置添加
// <a href=​"https:​/​/​www.mzitu.com/​">​电玩，有好看的​</a>​
```

* 额外补充

```js
appendChild()		// 添加子节点

removeChild()		// 移除

replaceChild()		// 替换

setAttribute()		// 设置属性

getAttribute()		// 获取属性

removeAttribute()	// 移除属性
```

* innerText 与 innerHTML

```js
divEle.innerText 	// 获取标签内部所有的文本

divEle.innerHTML	// 内部文本与标签都拿到


divEle.innerText = "<h1>哈哈哈</h1>";		// 不识别 html 标签

divEle.innerHTML = "<h1>哈哈哈</h1>";		// 识别 html 标签
```

#### 5.2.3 获取值操作

```js
// 获取用户数据标签内部的数据
let inputEle = document.getElementById("d1");

inputEle.value;
// 'fagaagagas'
```

```js
let seEle = document.getElementById("d2");

seEle.value
// '111'
```

* 补充

```js
// 如何获取用户上传的文件

let fileEle = document.getElementById("d3");

fileEle.value;
// 'C:\\fakepath\\微信图片_20230706151051.jpg'

fileEle.files;

fileEle.files[0]		// 获取文件数据
```

#### 5.2.4 class、css 操作

```js
let divEle = document.getElementById("d1");

divEle.classList;						// 获取标签所有的类属性
// DOMTokenList(3) ['cl', 'bg_green', 'bg_red', value: 'cl bg_green bg_red']

divEle.classList.remove("bg_red");		// 移除类属性

divEle.classList.add("bg_red");			// 添加类属性

divEle.classList.contains("c1");		// 是否包含某个类属性, true

divEle.classList.toggle("bg_red");		// 有则删除，无则添加
divEle.classList.toggle("bg_red");
divEle.classList.toggle("bg_red");
```

```js
// DOM操作 操作标签样式，统一用style起手
// 将css下横杠去掉，后面的单词首字母大写

let pEle = document.getElementsByTagName("p")[0];

pEle.style.color = "red";

pEle.style.fontSize = "28px";

pEle.style.backgroundColor = "yellow";

pEle.style.border = "3px solid red";
```

### 5.3 事件

到达某个事先设定的条件 自动触发的动作 
```js
// 常用事件

onclick        当用户点击某个对象时调用的事件句柄。
ondblclick     当用户双击某个对象时调用的事件句柄。

onfocus        元素获得焦点。           // 练习：输入框
onblur         元素失去焦点。 应用：用于表单验证,用户离开某个输入框时,代表输入完了,我们可以对它进行验证.
onchange       域的内容被改变。 应用：通常用于表单元素,当元素内容被改变时触发.（select联动）

onkeydown      某个键盘按键被按下。      应用场景: 当用户在最后一个输入框按下回车按键时,表单提交.
onkeypress     某个键盘按键被按下并松开。
onkeyup        某个键盘按键被松开。
onload         一张页面或一幅图像完成加载。
onmousedown    鼠标按钮被按下。
onmousemove    鼠标被移动。
onmouseout     鼠标从某元素移开。
onmouseover    鼠标移到某元素之上。

onselect      在文本框中的文本被选中时发生。
onsubmit      确认按钮被点击，使用的对象是form。
```

```html
// 绑定事件的两种方式

<body>
    <button onclick="func1()">点我</button>
    <button id="d1">点我</button>

    <script>
        // 第一种绑定方式
        function func1() {
            alert(123)
        }

        // 第二种绑定方式
        let btnEle = document.getElementById("d1");
        btnEle.onclick = function () {
            alert(222)
        }
    </script>
</body>
```

**script标签既可以放在head内 也可以放在body内**

**但是通常情况下都是放在body内的最底部**

```html
// script	放在最上面
// 等待页面加载完毕之后再执行代码

<head>
    <script>
    	window.onload = function () {
            // 第一种绑定方式
            function func1() {
                alert(123)
            }

            // 第二种绑定方式
            let btnEle = document.getElementById("d1");
            btnEle.onclick = function () {
                alert(222)
            }
        }
    </script>
</head>   
```

### 5.4 原生 js 事件绑定

* 开关灯案例

```js
<body>
    <div id="d1" class="c1 bg_red bg_green"></div>
    <button id="d2">变色</button>

    <script>
        let btnEle = document.getElementById("d2")
        let divEle = document.getElementById("d1")
        btnEle.onclick = function () {      // 绑定点击事件
            // 动态的修改div 标签的类属性
            divEle.classList.toggle("bg_red");
        }
    </script>
</body>
```

* input 框获取焦点、失去焦点案例

```js
<body>
    <input type="text" value="你好, 时间" id="d1">
    
    <script>
        let iEle = document.getElementById("d1")
        // 获取焦点事件
        iEle.onfocus = function () {
            // 将 input 框内部值去除
            iEle.value = ""
            // .value 就是获取，等号赋值就是设置
        }

        // 失去焦点事件
        iEle.onblur = function () {
            // 给 input 框重新赋值
            iEle.value = "哈哈哈哈"
        }
    </script>
</body>
```

* 实时展示当前时间

```js
<body>
    <input type="text" id="d1", style="display:block; height:50px; width:200px;">
    <br>
    <button id="d2">开始</button>
    <button id="d3">结束</button>

    <script>
        // 先定义一个全局 定时器变量
        let t = null

        // 1. 访问页面之后，将访问的时间展示到 input 框中
        // 2. 动态展示当前时间
        // 3. 页面上加两个按钮，一个开始，一个结束
        let inputEle = document.getElementById("d1")
        let startBtnEle = document.getElementById("d2")
        let endBtnEle = document.getElementById("d3")

        function showTime() {
            let currentTime = new Date();
            inputEle.value = currentTime.toLocaleString()
        }

        startBtnEle.onclick = function () {
            // 限制定时器只能开一个
            if (!t) {
                t = setInterval(showTime, 1000) // 每次点击就会开设一个定时器,t只指代最后一个 
            }
        }

        endBtnEle.onclick = function () {
            clearInterval(t)
            // 还应该将t重置为空
            t = null
        }

    </script>
</body>
```

* 省市联动

```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <select name="" id="d1">
        <option value="" selected disabled>--请选择省--</option>
    </select>
    <select name="" id="d2">

    </select>

    <script>
        let proEle = document.getElementById("d1")
        let cityEle = document.getElementById("d2")

        // 先模拟省市数据
        data = {
        "北京": ["朝阳区", "海淀区", "昌平区"], 
        "山东": ["威海市", "烟台市", "临沂"],
        "上海": ["浦东新区", "静安区", "黄浦区"],
        "深圳": ["南山区", "宝安区", "福田区"]
        };

        // 先 for 循环 获取省
        for (let key in data) {
            // 将省信息做成一个个option 标签 添加到第一个 select 框中
            // 1. 创建option 标签
            let opEle = document.createElement("option")
            // 2. 设置文本
            opEle.innerText = key
            // 3. 设置value
            opEle.value = key       // <option value="省">省</option>
            // 4. 将创建好的 option 标签 添加到第一个select中
            proEle.appendChild(opEle)
        }

        // 文本域变化事件  change事件   select 
        proEle.onchange = function () {
            // 清空市 select 中所有的 option
            cityEle.innerHTML = ''
            // 自己加一个请选择
            cityEle.innerHTML = "<option selected disabled>--请选择市--</option>"

            // 先获取用户选择的省
            let currentPro = proEle.value
            // 获取对应的市信息
            let currentCityList = data[currentPro]

            // for循环所有的市，渲染到第二个select中
            for (let i = 0; i < currentCityList.length; i++) {
            // 1. 创建option 标签
            let currentCity = currentCityList[i]
            
            let opEle = document.createElement("option")
            // 2. 设置文本
            opEle.innerText = currentCity
            // 3. 设置value
            opEle.value = currentCity    
            // 4. 将创建好的 option 标签 添加到第一个select中
            cityEle.appendChild(opEle)
            }
        }

    </script>
</body>
</html>
```

