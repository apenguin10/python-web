# 一 入门基础

##  1 变量

命名风格
```python
age_of_name = 18    # 小写加上下划线
```

三个特征
```python
name = "ehco"   # 变量值
id(name)        # 内存地址
type(name)      # 变量类型

is  比较id即内存地址是否相同
==  比较值是否相等
```

小整数池
从python解释器启动那一刻开始，就会在内存中事先申请

```python
m = 10
n = 10
m is n  # True
```

常量：不变的量
```python
AGE_OF_NAME = 18
```

##  2 基本数据类型

数字类型
```python
整形    int
浮点型  float
```

字符串类型str
```python
用引号（''，""，''' '''，""" """，）包含的一串字符
字符串可以相加拼接，但是效率极低
```

列表
```python
索引对应值, 索引从0开始
```

字典
```python
key 对应 value, 其中key通常为字符串类型
```

bool
```python
用来记录真假这两种状态
is_ok = True
is_ok = False
```

* 总结：选取合适的类型来记录状态, 存的目的是为了方便的取

##  3 垃圾回收机制

**引用计数**

```python
x = 10  			# 直接引用, 计数 +1
y = x   
l = ['a', 'b', x]   # 间接引用, 计数 +1
del y   			# 计数 -1

# 当引用计数为0的时候进行回收
```

**标记清除**

```python
# 用来解决循环引用带来的内存泄漏问题
a = [1, 2]
b = [3, 4]
a.append(b)     # b的引用计数为2
b.append(a)     # a的引用计数为2
del a  			# a指向的对象还存在
del b   		# b指向的对象还存在
# a, b 的引用计数都为1 无法删除

# python 中标记清除是通过2个容器实现的: 死亡容器、存活容器
del a
    # 1. 对执行删除(-1)后的每个引用-1，那么a的引用为0，b的引用为1，将a放进死亡容器，将b放入存活容器
    # 2. 循环存活容器，发现b引用a，复活a：将a放入存活容器中
    # 3. 删除死亡容器内的所有对象。
```

**分代回收**
新生代 ===> 青春代 ===> 老年代
权重达到阈值。 标记清除扫描频率依次降低

##  4 基本运算符

接收用户的输入
```python
python3
    age = input()   # 字符
python2
    age = raw_input()   # 等同于Python3中的 input()
    age = input()   # 输入是思路类型，age就是什么类型
```

字符串格式化输出

**%s 可以接收任意类型**

```python
res = "hello %s" % "lili"
res = "hello %s %s" % ("lili", "18")
res = "hello %(name)s" % {"name":"lili"}

"age is %s" % 18
"list is %s" % [1, 2, 3]
"dict is %s" % {"a": 1}

%d 只能接收int
"age is %d" % 18
```
**str.format  兼容性好**

```python
"name is {}, age is {}".format("lili", 18)
"name is {1} {0}, age is {0} {1}".format("lili", 18)
"name is {name}, age is {age}".format(age=18, name="lili")

# 填充与格式化
"{0:*<20}".format("左对齐20个字符，不够用*填充")
"{0:*>20}".format("左对齐20个字符，不够用*填充")
"{0:*^20}".format("居中，20个字符，不够用*填充")

# 精度与进制
"{salary:.3f}".format(salary=1.3142)    # 保留三位小数，四舍五入
"{0:b}".format(123) 					# 转换成二进制
"{0:o}".format(123) 					# 转换成八进制
"{0:x}".format(123) 					# 转换成十六进制
"{0:,}".format(812939393931)  			# 千分位格式化   812,939,393,931
```
**f   python3.5 以后出，速度最快**

```python
{}  中的字符串可以被当作表达式执行
f"name is {name:.3f}, age is {age:*>20}"
```

## 5 算数运算符

* 【 + 】加
* 【 - 】减
* 【 * 】乘
* 【 / 】结果带小数
* 【 // 】只保留整数部分, 向下取整
* 【 % 】取模取余数
* 【 ** 】取次方
##  6 比较运算符

* 【 >  】
* 【 >= 】
* 【 <  】 
* 【 <= 】
* 【 == 】
* 【 != 】
##  7 赋值运算符

```python
age = 18
age += 1
x = y = z = 1   # 链式赋值

m = 10
n = 20
m, n = n, m     # 交叉赋值

x_list = [1, 2, 3]
x, y, z = x_list    解压赋值
```

*可以帮助我们取两头的值, 会将没有对应关系的值存成列表然后赋值给紧跟其后的那个变量名

```python
x, y, *_ = x_list   
*_, x, y = x_list
x, *_, y = x_list

解压字典默认是字典的 key
x, y, z = {"a":1, "b":2, "c":3}
```

##  8 可变不可变类型

可变类型
值改变，id 不变

```python
list、dict
```

不可变类型
值改变，id 同样改变

```python
int、float、str、bool
```

补充：{}字典内，key是不可变类型, value 任意

##  9 流程控制

条件 显示布尔值  隐式布尔值

```python
age = 10
age > 16
0、None、空（字典、列表、字符串）=> 表示 False

not 取反
and 逻辑与
or 逻辑或
# 上述优先级从上到下, not > and > or , 可以遵循偷懒原则
3 > 1 or False or 1 != 1

# 成员运算符
"hello" in "hello world"
1 in [1, 2, 3]
"k1" in {"k1":2}

# 身份运算符  is 判断 id 是否相等

if ...
elif ...
else ...
```

## 10 深浅copy

```python
list2 = list1 # 不是copy ，因为指向同一个地址
  
"""
1、拷贝一下原列表产生一个新的列表
2、想让两个列表完全独立开，并且针对的是改操作的独立而不是读操作 
"""
# 浅copy:是把原列表第一层的内存地址不加区分完全copy一份给新列表
[1, 2, 3]可变类型：可以修改
[[1, ], ]不可变类型：修改list1 , list2 也会跟着改变
  
list2 = list1.copy()
  
# 深copy:可以区分开可变类型与不可变类型的copy机制
import copy
list3 = copy.deepcopy(list2)
```
##  11 循环

```python
while 条件:
    ...
    break
    continue
else:
    # 没有被break打断的情况下，正常结束会执行
    ...
  
while True:     
    # 死循环、纯计算无 io 会导致致命的效率问题
    1 + 1
  
  
for 循环    在循环取值（遍历取值）比while 更高效
  
for 变量名 in 可迭代对象：
    ...
    break
    continue
else:
    ...
```
## 12 基本数据类型及内置方法

###  12.1 数字(int, float)

定义
```python
age = 10
```

类型转换
```python
int("1002")

# 10 进制 <---> 2 进制
bin(11)     		# 0b1011
int("0b1011", 2)

# 10 进制 <---> 8 进制
oct(11)     		# 0o13
int("0o13", 8)

# 10 进制 <---> 16 进制
hex(11)     		# 0xb
int('0xb',16)

float("3.1")

x = 1 + 5j  # 复数类型
x.imag
x.real
```

位运算
```python
# 位运算 0 正数   1 负数
10000000     表示 -128

# 用位运算 & 取代 % 取模
# 注意：用位运算&来取代%取模需要被取模的数必须是2 ^ n
x % 2 ^ n = x & (2 ^ n - 1)

# 将一个数左移n位，相当于乘以了2的n次方右移n位，相当于除以2的n次方取整
x << n == x * 2 ^ n
x >> n == x / 2 ^ n

# 判断奇偶
(a & 1) == 0 
a % 2 == 0

# 【交互数值】
# 累加和
a = 10
b = 20
a = a + b
b = a - b
a = a - b

# ^ 运算符  异或
a ^ a = 0
a ^ 0 = a
a ^ (b ^ c) = (a ^ b) ^ c

a = 10
b = 20
a = a ^ b
b = b ^ a
a = a ^ b
```

###  12.2 字符串(str)

定义
```python
msg = str("hello")
```

类型转换
```python
str({"a": 1})   # str 可以把任意类型转换成字符串
```
#### 内置方法

索引取值 
```python
msg[1]
msg[-1]
```

切片（顾头不顾尾）
```python
msg[0:5]
msg[0:5:2]  # 步长
msg[5:0:-1] # 反向步长
msg[::-1]   # 字符串反转
```

len
```python
len(msg)
```

in 和 not in
```python
"a" in msg
```

.strip() 移除字符串左右两侧的符号，默认空格
```python
msg = '=*//==ee=//**='.strip('*/=') # ee
```

.split() 按照分隔符切分成列表，默认空格
```python
info = 'ee:18:male'
res = info.split(':')
res = info.split(':', 1)  # 指定分隔次数

.rsplit()
info.split(':',1)   # ["egon", "18:male"]
info.rsplit(':',1)  # ["egon:18", "male"]
```

循环
```python
for x in info:
    ...
```

```python
.strip()

.lstrip()

.rstrip()

.lower()

.upper()    	# 大小写

.startswith()

.endswith()   	# return bool 值

.format()

.isdigit()   	# 判断字符串是否由纯数字组成
```

"?".join()   把字符串列表拼接成字符串
```python
l = ["a", "b", "c"]
":".join(l)     # "1:2:3" 
```

.replace()
```python
msg.replace("you", "YOU")
msg.replace("you", "YOU", 1)    # 可以指定次数
```

了解
```python
msg = "hello eee"
msg.find("ee")  # 返回要查找的字符串在大字符串中的起始索引
# 找不到, 返回 -1
msg.index("kkk")    # 找不到抛出异常

msg.rfind("")  # 倒着找，返回顺着的索引

msg.rindex("")

msg.count("e")  # 计数

"""
居中，左对齐，右对齐，右对齐（补0）
"""
'eee'.center(50,'*')
  
'eee'.ljust(50,'*')
  
'eee'.rjust(50,'*')
  
'eee'.zfill(10)    # 前面补0

"""
首字母大写，大小写交换，每个单词首字母大写
"""
msg.capitalize()
  
msg.swapcase()
  
msg.title()

"""
is 系列
"""
.islower()      # 是不是全小写
  
.isupper()      # 是不是全大写
  
.istitle()      # 是不是首字母大写
  
.isalnum()      # 是不是由字母和数字组成
  
.isnumeric()    # 是不是由数字组成
  
.isalpha()      # 由字母组成
  
.isspace()      # 由空格组成
  
.isidentifier() # 标识符是否合法，不能数字开头

"""
识别是否是数字
"""
num1 = b'4'     # bytes
num2 = u'4'     # unicode,python3中无需加u就是unicode
num3 = '四'     # 中文数字
num4 = 'Ⅳ'     # 罗马数字
  
isdigit()       # 只能识别：num1, num2
isnumberic()    # 识别：num2, num3, num4
isdecimal()     # 十进制，只能识别num2
```

###  12. 3 列表(list)

作用：按位置存放多个值

定义
```python
l = list([1, 2, 3, 'a'])
```

类型转换
```python
# 能被for循环遍历的类型都可以当作参数传给list()转成列表
res = list('hello')
res = list({'a':123, 'b':214})
```
#### 内置方法

按索引存取值（正向存取、反向存取）：即可以取也可以改
```python
l = [111, 'af', 'ss']
l[0] = 222
l[3] = 33     # 索引不存在则报错
```

切片（顾头不顾尾，步长）
```python
l = [111, 'a', 'b', 222, 'c', 'd', ['daf', 333]]
l[0 : 3]
l[0 : 5 : 2]
list2 = l[:]    # 切片等同于拷贝行为，而且相当于浅copy
l[::-1]     	# 取反
```

长度 len()

成员 in 和 not in

往列表中添加值
```python
.append()

.insert(index, item)   	#  插入值

.extend()   			# 添加值(列表)
list1 = [1, 2]
list2 = [3, 4]
list2.extend(list1)     # [1, 2, 3, 4]
```

删除
```python
del l[1]    没有返回值

x = l.pop(index)    # 不指定默认删除最后一个， 返回删除的值

l.remove("ee")  # 根据元素删除，返回 None
```

循环
```python
for x in l:
    ...
```

需要掌握的操作
```python
.count(item)    		# 计数

.index(item)    		# 返回第一个索引

.clear()        		# 清空列表

.reverse()      		# 取反, 返回 None

.sort(reverse=True) 	# 排序 默认升序 reverse=False

# 字符串可以比大小，按照对应的位置的字符依次pk
# 字符串的大小是按照ASCI码表的先后顺序加以区别，表中排在后面的字符大于前面的
'abz'> 'abc'

# 列表也可以比较大小，原理同字符串一样，但是对应位置的元素必须是同种类型
l1 = [1, 'abz', 'zzz']
l2 = [3, 'fa', 'fag']
l1 > l2    False
```

补充：队列堆栈
```python
# 1.队列：FIFO, 先进先出
l = []
# 入队操作
l.append('first')
l.append('second')
# 出队操作
l.pop(0)
l.pop(0)

# 2.  堆栈: LIFO, 后进先出
l.append('first')
l.append('second')
l.pop(0)
l.pop(0)
```

###  12. 4 元组(tuple)

元组就是‘一个不可变的列表’===========内存地址不可变

作用：按照索引/位置存放多个值, 只能用于读不用于改

定义：() 内用逗号分隔开多个任意类型的元素
```python
t = tuple((1, 2, 3, "faf"))

x = (10)    int
x = (10, )  tuple   
```

类型转换
```python
tuple('hello')
tuple([1, 2, 3])
tuple({'a':123, 'b':333})
```
#### 内置方法

* 按索引值取值(正向，反向): 只能取
* 切片(同list)
* 长度 len()
* 成员运算 in 和 not in
* 循环
* 索引

###  12.5 字典(dict)

作用

定义：{}内用逗号分隔开多个key: value， 其中value可以是任意类型，但是key必须是不可变类型，且不能重复

```python
d = {              
    'k1': 111
    (1, 2, 3): 'fag'
}

d = dict(x=1, y=2, c=3)

info = [
    ['name', 'liu'],
    ['age', '11'],
    ['gender', 'male'],
]
d = dict(info)

keys = ['name', 'age', 'gender']
d = {}.fromkeys(keys, None)
```
#### 内置方法

按key存取值: 可存可取
```python
d = {'k1': 'daf'}
针对赋值操作, key存在, 则修改; key不存在, 则创建新值
d['k1'] = 222
d['k2'] = 333
```

长度 len 
```python
len(d)
```

成员运算 in   not in, 看的是key

删除
```python
d = dict(x=1, y=2, z=3)
del d['x']

res = d.pop('x')    # 根据key删除，返回value

res = d.popitem()   # 随机删除，返回元组(key, value)
```

键 keys(), 值 values(), 键值对items()==>在python3得到的是老母鸡
```python
d = {'k1': 111,'k2': 2222}

在python2 中
d.keys()    # ['k2','k1']
d.values()  # [2222,111]
dict(d.items()) # {'k2':2222, 'k1': 111}

python3 中
d = {'x':1, 'y': 2, 'z': 3}
d.keys()    # dict_keys(['x', 'y', 'z'])
type(d.keys())  # <class'dict_keys'>

for k in d:
for k in d.keys():
for v in d.values():
for k, v in d.items():
```

其它操作

```python
.clear()        清空
.update({...})  增加
.get(key)       根据key取值,容错性好,key不存在,返回None
.setdefault(key, value) 如果key存在, pass;如果key不存在,则增加.===>(返回key对应值)

d = {'x': 111}
d.clear()
d.update(dict(x=1, y=2,z=3))
d.get('x')
d.setdefault('zz', 415)
```

###  12.6 集合(set)

作用

关系运算

去重

定义：在{}内用逗号分隔开多个元素，多个元素满足一下三个条件
		(1) 集合内元素必须为不可变类型
		(2) 集合内元素无序
		(3) 集合内元素没有重复

```python
s = {1, 2} <==>  s = {1, 1, 1, 2}   # s = set({1, 2})

s = {}      # 默认空字典

s = set()   # 定义空集合
```

类型转换
```python
set({1, 2, 3})
res = set('hellolllll')
res = set({'k1':1,'k2':2})    # {'k1', 'k2'}
```
#### 内置方法

关系运算符
```python
friends1= {"zero", "kevin", "jason", "egon"}
friends2= {"Jy", "ricky", "jason", "egon"}

# 取交集: &
res = friends1 & friends2
friends1.intersection(friends2)

# 取并集: |
res = friends1 | friends2
friends1.union(friends2)

# 取差集: -
friends1 - friends2
friends1.difference(friends2)

# 对称差集: ^
friends1 ^ friends2
friends1.symmetric_difference(friends2)

# 父子集: 包含的关系 >  <
friends1 > friends2
friends1.issuperset(friends2)     # >
friends1.issubset(friends2)      # <

# 互相包含 == 
friends1 == friends2
friends1.issuperset(friends2)
friends2.issuperset(friends1)
```

去重
```python
# 1. 只能针对不可变类型去重
set([1, 2, 1, 1, 1, 2])

# 2. 无法保证原来的顺序
l=[1, 'a', 'b', 'z', 1, 1, 1, 2]
l=list(set(l))
```

长度 len()

成员运算 in   not in

循环

删除  discard
```python
s = {1, 2, 3}
s.discard(4)    # 删除元素不存在  do nothing
s.remove(4)     # 删除元素不存在，报错
```

增加元素 update
```python
s.update({4, 2, 65})
```

随机删除
```python
res = s.pop()
```

add()
```python
s.add(4)
```

两个集合完全独立, 没有共同部分, 返回True
```python
s.isdisjoint({3, 4, 5, 6})
```

s.difference_update({3, 4, 5}) 
```python
set1 = {1, 2, 3, 4, 5}
set2 = {4, 5, 6, 7, 8}

set1.difference_update(set2)

print(set1)  # 输出: {1, 2, 3}
```

## 13 字符编码

* **ASCII 表**
  * 只支持英文字符串
  * 采用8位二进制数对应一个英文字符串
* **GBK表**
  * 支持英文字符、中文字符
  * 采用8位（8bit = 1Bytes）二进制数对应一个英文字符串，采用16位二进制数对应一个中文字符
* **unicode：内存中统一使用unicode**
  * 兼容万国字符
  * 采用16位（16bit=2Bytes）二进制数对应一个中文字符串个别生僻会采用4Bytes、8Bytes
  * unicode ：内存 ====> 硬盘：GBK   ,  shift-JIS
  * 老的字符编码都可以转换成unicode，但是不能通过unicode互转
* **utf-8**：unicode transforms format-8

​			英文 1 Bytes		汉字 3 Bytes

* **结论**

​			以什么文件格式存， 就该以什么文件格式取

​			python 解释器默认读取文件编码

* 指定文件头修改的默认的编码：可以保证不乱码

```python
#coding:gbk
```

* python3中的str类型默认直接存成unicode格式，无论如何都不会乱码
* python2的str类型不乱码

```python
x = u'上'

# python2解释器有两种字符串类型：str、unicode
# str类型
x = '上' 		# 字符串值会按照文件头指定的编码格式存入变量值的内存空间
# unicode类型
x = u'上' 		# 强制存成unicode
```

```python
x = '上'
res = x.encode('gbk')    	# unicode---->gbk
print(res, type(res))
print(res.decode('gbk'))	# gbk---->unicode
```

## 14 文件处理

### 14.1 open() 文件

控制文件读写内容的模式：t和b

强调：t和b不能单独使用，必须跟r/w/a连用

* **t文本**（默认的模式）

				1. 读写都以str（unicode）为单位的
				2. 文本文件
				3. 必须指定encoding='utf-8'

* **b二进制/bytes**
* 控制文件读写的模式

​			r 		只读模式
​			w 	   只写模式
​			a 		只追加写模式

​			+         r+、w+、a+

**打开文件**

```python
# windows 路径分隔符问题 'C:\a.txt\nb\c\d.txt'
# 解决方案一: 推荐
open(r'C:\a.txt\nb\c\d.txt')

# 解决方案二:
open('C:/a.txt/nb/c/d.txt')

f = open(r'aaa/a.txt', mode='rt')    # f的值是一种变量，占用的是应用程序的内存空间
print(f)							 # <_io.TextIOWrapper name='a.txt' mode='rt' encoding='cp936'>
```

**操作文件**

读/写文件，应用程序对文件的读写请求都是在向操作系统发送系统调用

然后由操作系统控制硬盘把输入读入内存、或者写入硬盘

```python
res = f.read()
print(type(res))	# <class 'str'>
```

**关闭文件**

```python
f.close()    # 回收操作系统资源

# f.read()    # 变量f存在，但是不能再读了
def f    # 回收应用程序资源
```

**with... as ...**

文件对象又称为文件句柄

```python
with open('a.txt', mode='rt') as f1,\    # f1 = open('a.txt', mode='rt')
    open('b.txt', mode='rt') as f2:
    res1 = f1.read()
    res2 = f2.read()
    print(res1)
    print(res2)
# f.close()在with之外自动执行       
```

```python
"""
强调：t和b不能单独使用，必须跟r/w/a连用 
t文本（默认的模式）
1. 读写都以str（unicode）为单位的
2. 文本文件
3. 必须指定encoding='utf-8'
4. 没有指定encoding参数操作系统会使用自己默认的编码
5. linux系统默认utf-8
6. windows系统默认gbk
"""

# 内存：utf-8格式的二进制-----解码-----》unicode
# 硬盘（c.txt内容：utf-8格式的二进制）

with open('c.txt',mode='rt',encoding='utf-8') as f:
    res = f.read()     		# t模式会将f.read()读出的结果解码成unicode  
```

### 14.2 文件操作模式详解

##### **以 t 模式为基础进行内存操作**

```python
"""
r（默认的操作模式）: 只读模式，当文件不存在时报错，当文件存在时文件指针跳到开始位置
"""
with open('c.txt', mode='rt', encoding='utf-8') as f:
    print('第一次读'.center(50, '*'))
    res = f.read()    # 把所有内容从硬盘读到内存
    print(res)
```

```python
"""
w : 只写模式，当文件不存在时会创建空文件，当文件存在会清空文件，指针位于开始位置
"""
with open('d.txt',mode='wt',encoding='utf-8') as f:
     # f.read()  # 报错，不可读
     f.write('hello\n')
    
# 强调1: 在以w模式打开文件没有关闭的情况下，连续写入，新的内容总是跟在旧的之后  
with open('d.txt',mode='wt',encoding='utf-8') as f:
    f.write('hh1\n')
    f.write('ww2\n')
    f.write('zz3\n')
    
# 强调2: 如果重新以w模式打开文件，则会清空文件内容    

# 案例: w模式 copy 工具
src_file = input('源文件路径>>: ').strip()
dst_file = input('目标文件路径>>: ').strip()
with open(r'{}'.format(src_file), mode='rt', encoding='utf-8') as f1,\
    open(r'{}'.format(dst_file), mode='wt', encoding='utf-8') as f2:
        res = f1.read()
        f2.write(res)
```

```python
"""
a：只追加写，在文件不存在时会创建空文档，在文件存在时文件指针会直接调到末尾
"""
with open('e.txt', mode='at', encoding='utf-8') as f:
    f.read()     # 报错，不能读
    f.write('11\n')
    f.write('22\n')
```

**+不能单独使用，必须配合 r w a 指针覆盖**

```python
with open('g.txt',mode='rt+',encoding='utf-8') as f:
    print(f.read())
    f.write('中国')
    
with open('g.txt',mode='w+t',encoding='utf-8') as f:
    f.write('111\n')
    f.write('222\n')
    f.write('333\n')
    print('====>', f.read())  
    
with open('g.txt',mode='a+t',encoding='utf-8') as f:
    print(f.read())
    f.write('444\n')
    f.write('5555\n')
    print(f.read())    
```

##### **b模式** 

```python
控制文件读写内容的模式
t：
    1、读写都是以字符串（unicode）为单位
    2、只能针对文本文件
    3、必须指定字符编码，即必须指定encoding参数
b：binary模式
    1、读写都是以bytes为单位
    2、可以针对所有文件
    3、一定不能指定字符编码，即一定不能指定encoding参数
    
总结
	1、在操作纯文本文件方面t模式帮我们省去了编码与解码的环节，b模式则需要手动编码与解码，所以此时t模式更为方便
	2、针对非文本文件（如图片、视频、音频等）只能使用b模式
```

```python
with open(r'g.txt', mode='rb') as f:
    res = f.read() 				# utf-8的二进制
    print(res,type(res))        # b'111\r\n222\r\n333\r\n444\r\n5555\r\n' <class 'bytes'>
    print(res.decode('utf-8'))
    
with open(r'f.txt',mode='wb') as f:
    f.write('你好hello'.encode('utf-8'))
    f.write('哈哈哈'.encode('gbk'))  
```

**循环读取文件的方式**

```python
# 方式1: 自己控制每次读取数据的数据量
with open(r'test.jpg', mode='rb') as f:
    while True:
        res = f.read(1024)
        if len(res) == 0:
            break
# 方式2: 以行为单位读，当一行内容过长时会导致一次性读入内容的数据量过大
with open('a.jpg', mode='rb') as f:
    for line in f:
        print(line)
```

##### **文件操作的其它方法**

```python
"""读操作"""
# 1: 读相关操作 readline ===> 一次读一行
with open('b.mp4', mode='rb') as f:
    while True:
        res = readline()
        if len(res) == 0:
            break
        print(res)

# 2. readlines: 一次读取，生成列表
with open(r'g.txt',mode='rt',encoding='utf-8') as f:
    res = f.readlines()
    print(res)    

# 强调: f.read()与f.readlines()都是将内容一次性读入内存，如果内容过大会导致内存溢出    

"""写操作"""
f.writelines()		# 列表字符串中循环写入
with open('h.txt', mode='wt', encoding='utf-8') as f:
    # f.write('111\n\222\n\333\n')
    l = ['111\n', '222\n', '333\n']
    f.writelines(l)

# 补充1: 如果是纯英文字符, 可以直接加前缀b得到bytes 类型
with open('h.txt', mode='wb') as f:
    l = [
    b'1111aaa1\n',
    b'222bb2',
    b'33eee33'
    ]
    f.writelines(l)
    
# 补充2: '上'.encode('utf-8') 等同于 bytes('上', encoding='utf-8')    
```

**flush 告诉操作系统立马写入硬盘中**

```python
with open('h.txt', mode='wt',encoding='utf-8') as f:
    f.write('哈')
    f.flush()    
```

```python
f.readable()    # 是否可读
f.writeable()   # 是否可写
f.encoding()    # 返回编码
f.name          # 返回文件名
f.closed        # 文件是否关闭
```

##### 控制文件指针的移动 f.seek(n， 模式)	

```python
"""
指针移动的单位都是以bytes/字节为单位, n指的是移动的字节个数
只有一种情况特殊 : t模式下的read(n), n代表的是字符个数
*****强调 : 只有0模式可以在t下使用，1、2必须在b模式下用

    模式0 : 参照物是文件开头位置
    模式1 : 参照物是当前指针所在位置
    模式2 : 参照物是文件末尾位置，应该倒着移动
"""

with open('h.txt', mode='rb') as f:
    # 将指针跳到文件末尾
    f.seek(0, 2)
```

##### 文件修改的两种方式

```python
"""
方式一: 文本编辑采用的就是这种方式
    实现思路: 将文件内容一次性全部读入内存，然后在内存中修改完毕后再覆盖写回源文件
    优点: 在文件修改的过程中同一份数据只有一份
    缺点: 会过多的占用内存
"""
with open('h.txt', mode='rt', encoding='utf-8') as f:
    res = f.read()
    print(type(res))
    data = res.replace('aaa', 'bbb')

with open('h.txt', mode='wt', encoding='utf-8') as f:
    f.write(data)
    
"""
方式二 : import os
    实现思路: 以读的方式打开源文件, 以写的方式打开一个临时文件，一行行读取源文件内容，修改完后写入临时文件
             删除源文件，将临时文件重命名源文件名
    优点: 不会占用过多的内存
    缺点: 在文件修改过程中同一份数据存了两份  
"""  
import os

with open('c.txt',mode='rt', encoding='utf-8') as f, \
        open('.c.txt.swap', mode='wt',encoding='utf-8') as f1:
    for line in f:
        f1.write(line.replace('aaaa', 'tsp'))
 
os.remove('c.txt')
os.rename('.c.txt.swap', 'c.txt')
```

## 15 函数

### 15.1 函数入门

```python
def 函数名(参数1, 参数2, ...):    
    """文档描述"""
    函数体
    # pass
    return 值

"""
申请内存空间保存函数体代码，将内存地址绑定函数名
定义函数不会执行函数体代码，但是会检测函数体语法
通过函数名找到函数的内存地址，然后()触发函数体代码的执行
返回多个值，返回元组类型
函数定义最理想状态: 函数调用只跟函数本身有关，不被其他代码改变
"""
```

### 15.2 函数参数

形参：在函数定义阶段定义的参数称之为形式参数

```python
def foo(x, y):
    print(x, y)
```

实参：在函数调用阶段传入的值称之为实际参数

```python
foo(1, 2)
```

位置参数

```python
# 位置形参: 函数定义阶段
def func(x, y):
    pass
# 位置实参:
func(0, 1)
```

关键字参数（关键字实参）： 函数调用阶段

```python
func(y=2, x=1)
```

默认参数(默认形参)：函数定义阶段，就已经被赋值的形参，赋与值的内存地址

位置形参必须在默认形参的左边

```python
def func(x, y=3):
    pass
```

不推荐使用可变类型

```python
def func(x, l=None):
    if l is None:
        l = []
    l.append(x)
    print(l)
```

**可变长度的参数**

```python
# 可变长度指的是在调用函数时，传入的值(实参)的个数不固定
# 针对溢出的实参，必须有对应的形参来接收

'''用来接收溢出的位置实参, * , *保存成元组的形式，然后赋值给args'''
def func(x, y, *args):
    pass

func(1, 2, *[3, 4, 5])

'''保存成字典, ** , kwargs'''
def foo(x, y, **kwargs):
    pass

foo(1, 2, **{'a':3, 'b':4})


# * 、 ** 用在实参中, 实参中带* 、 ** 
def func(x, y, z):
    print(x, y, z)

func(*[1, 2, 3])
func(**{'x':1, 'y':2, 'z':3})

# 混用, * 和 **
# *args 在 **kwargs 之前
def foo(*args, **kwargs):
    pass
```

命名关键字形参

```python
# a, b: 必须使用关键字实参传值
def func(x, y, *, a=1, b):
    print(x, y)
    print(a, b)

func(1, 2, b=4)
```

组合使用

```python
def foo(x, y=1, *args, z, **kwargs):
    pass
```

### 15.3 名称空间和作用域

namespaces	存放变量名字的地方, 栈区划分3片，可以存放相同的名字

* 内置名称空间(1个) 

* 全局名称空间(1个) 

* 局部名称空间(...多个) 1 + 2 ===> 全局范围

*  3 =======> 局部范围

**内置名称空间（built-in**

​		存放的名字: python解释器内置的名字

​		存活周期: python解释器启动产生，关闭则销毁

**全局名称空间**

​		存放的名字: no函数内, no内置的   

​		存活周期: py文件执行产生，运行完毕销毁

**局部名称空间**

​		存放的名字: 调用函数时，函数体代码产生的变量名    

​		存活周期: 调用函数时候产生，调用结束销毁



加载顺序: 内置 >>> 全局 >>> 局部

销毁顺序: 局部 >>> 全局 >>> 内置	

名字的查找优先级: 当前所在位置向上一层一层查找



**名称空间的‘嵌套关系’是以函数定义阶段为准，与调用位置无关**

```python
x = 1
def func():
    print(x)
def foo():
    x = 22
    func()
foo()    	# x => 1
```

作用域 ==> 作用范围 

​		全局作用域(内置、全局):    全局存活、全局有效(被所有函数共享) 

​		局部作用域(局部):    临时存活、局部有效

**LEGB**	

Local（局部） - 函数内部定义的变量属于局部作用域。这些变量只能在函数内部访问，函数外部无法访问。

Enclosing（闭包） - 闭包指的是函数内部定义的函数。内部函数可以访问包含它的外部函数的变量，外部函数的变量属于闭包作用域。

Global（全局） - 在函数外部定义的变量属于全局作用域。它们可以在整个程序中的任何位置被访问。

Built-in（内建） - 内建作用域包含了Python解释器中内置的函数和变量名。这些函数和变量可以在任何地方被直接访问。

```python
# built-in
# global
def f1():
    # enclosing
    def f2():
        # enclosing
        def f3():
            # local
            pass
```

global  在局部修改全局名字对应的值(不可变类型)

nonlocal	修改闭包内的值

```python
x = 11
def foo()
    global x 
    x = 22
    print(x)    
foo()   	 # ==> 22
print(x)     # ==> 22

x = 0
def f1():
    x=1
    def f2():
        nonlocal x
        x=2
    f2()
    print(x)
f1()
```

### 15.4 函数对象

函数可以成变量用

```python
def foo():
    pass
f = foo
f()
```

可以当作参数传入

```python
def func(x):
    print(x)
func(foo)
```

可以当作函数的返回值

```python
def func(x)
    return x
res = func(foo)
```

可以当作容器类型的一个元素

```python
l = [func, ]
l[0]()
```

### 15.5 函数嵌套

函数的嵌套调用

​		在调用一个函数的过程中又调用其他函数

```python
def max2(x, y):
    if x > y:
        return x
    else:
        return y

def max4(a, b, c, d):
    s1 = max2(a, b)
    s2 = max2(s1, c)
    s3 = max(s2, d)
    return s3
```

函数的嵌套定义

​		在函数内定义函数

```python
def f1():
    def f1():
        pass
```

### 15.6 闭包函数

即 名称空间与作用域 + 函数嵌套 + 函数对象

什么是闭包函数：'闭'内嵌函数、'包'包含对外层函数作用域名字的引用(不是全局)

```python
def foo1():
    x = 1
    def foo2():
        print(x)
    foo2()
    return foo2
f = foo1()
f()
```

应用：为函数传参

```python
def f1(x):
    def f2():
        print(x)
    return f2
f2 = f1()
f2()
```

### 15.7 装饰器（无参，有参）

即 *args ，**kwargs，名称空间与作用域，函数对象，函数的嵌套定义，闭包函数

装饰器定义：定义一个函数(类), 为其他函数(类)添加额外的功能

为什么有装饰器：

​		开放封闭原则: 对拓展功能开放, 对修改源代码封闭

​		不修改装饰器对象源代码及调用方式的前提下为装饰器添加新功能

需求：在不修改index源代码和调用方式的前提下, 添加统计运行时间的功能

```python
def index(x, y):
    time.sleep(3)
    print('index {} {}'.format(x, y))
    return 123
res = index(1, 2)
```

```python
import time
def timmer(foo):
    def wrapper(*args, **kwargs):
        start = time.time()
        res = foo(*args, **kwargs)
        stop = time.time()
        print(stop - start)
        return res
    return wrapper

@timmer    			# 语法糖  即  index = timmer(index)
def index(x, y):
    time.sleep(3)
    print('index {} {}'.format(x, y))
    return 123
```

**无参装饰器的模板**

```python
from functools import wraps
# 无参装饰器的模板, 参数、返回值、属性相同
def outter(func):
    @wraps(func)    # 获得和原函数func一样的属性
    def wrapper(*args, **kwargs):
        # 1、调用原函数
        # 2、为其增加新功能
        res=func(*args,**kwargs)
        return res
    # wrapper.__name__ = func.__name__
    # wrapper.__doc__ = func.__doc__
    return wrapper
```

**有参装饰器模板** 

```python
# 由于@outter 限制, func = outter(func),不能传其他参数
def auth(x, y, z, w, m):
    def outter(func):
        def wrapper(*args, **kwargs):
            res = func(*args, **kwargs)
            return res
        return wrapper
    return outter


@auth(1, 2, 3, 4, 5)
def home(name):
    pass
```

叠加多个装饰器的加载、运行分析

```python
from functools import wraps

def deco1(func):
    @wraps(func)
    def wrapper(*args, **kwargs):
        res = func(*args, **kwargs)
        return res
    return wrapper

# 加载顺序自下而上
@deco1         # index = deco3(index)         # index <== wrapper1的内存地址 
@deco2         # index = deco2(index)         # index <== wrapper2的内存地址
@deco3(111)    # index = deco3(111)(index)    # index <== wrapper3的内存地址
def index(x, y):
    pass

# 执行顺序自上而下
index(1, 2)     # wrapper1(1, 2)  ==> wrapper2(1, 2) ==> wrapper3(1, 2) ==> index(1, 2)
```



### 15.8 迭代器

什么是迭代器:

​		迭代器指的是迭代取值的工具，迭代是一个重复的过程

​		每次重复都是基于上一次的结果而继续的

​		单纯的重复不是迭代

为何有迭代器

​		迭代器是迭代取值的工具，列表、字符串、元组、字典、集合、文件

如何使用迭代器

```python
# 可迭代对象: 内置有__iter__方法的都称之为可迭代对象
# 调用可迭代对象的__iter__方法会将其转成迭代器对象

d = {'a': 1, 'b': 2, 'c': 3}
d_iter = d.__iter__()
print(d_iter.__next__())    # a
print(d_iter.__next__())    # b
print(d_iter.__next__())    # c
# print(d_iter.__next__())  # 抛出异常, StopIteration
# 在一个迭代器取值取干净的情况下，再对其取值取不到

while True:
    try:
        print(d_iter.__next__())
    except StopIteration:
        break
```

可迭代对象与迭代器对象详解

```python
可迭代对象('可以转换成迭代器的对象'): 内置__iter__方法
            # 可迭代对象.__iter__() ===> 迭代器对象
迭代器对象: 内置有__next__方法, 并且内置有__iter__方法
            # 迭代器对象.__next__() ===> 迭代器的下一个值
            # 迭代器对象.__iter__() ===> 迭代器对象(等于自身)
        
for 循环工作原理 ====> 迭代器循环
# 1. d.__iter__()得到迭代器对象
# 2. 迭代器对象.__next__()拿到返回值, 赋值给k
# 3. 循环步骤2, 直到抛出异常StopIteration, 然后结束for循环

# 可迭代对象: 字符串、列表、元组、字典、集合、文件
# 迭代器对象: 文件 (可迭代对象)
with open('a.txt', mode='rt', encoding='utf-8') as f:
    f.__iter__()
    f.__next__()
```

迭代器的优缺点总结

```python
优点: 
    1.序列和非序列类型统一迭代取值方式 
    2.同一时间在内存中只有一个值
缺点: 
    1.无法获得迭代器的长度 
    2.只能取下一个值 
```

### 15.9 生成器（自定义迭代器）

如何得到自定义的迭代器

函数内一旦存在yield关键字，调用函数不会执行函数体代码

会返回一个生成器对象，生成器即自定义的迭代器

```python
yield expression
# 此形式用于在生成器函数中产生一个值，并将其返回给调用方。
# 生成器函数会在此处暂停执行，并保留当前状态。下次调用生成器函数时，会从暂停的位置继续执行。

def func():
    print('第一次')
    yield 1
    print('第二次')
    yield 2
    print('第三次')

g = func()    # g 生成器就是迭代器
g.__iter__()
res1 = g.__next__()    # 会触发函数体代码的运行, 遇到yield停下来, 将yield后的值
                       # 当作本次调用的结果返回, res1 == 1 
res2 = g.__next__()        

next(g)  # g.__next__()    
```

应用案例

```python
def my_range(start, stop, step=1):
    print('start...')
    while start < stop:
        start += step
        yield start
    print('end...')

 for i in my_range(1, 10, 1):
     print(i)   
```

yield 表达式形式    （挂起运行）

```python
value = yield expression
# 此形式用于从生成器函数接收一个值，并将其返回给调用方。
# 生成器函数会在此处暂停执行，并保留当前状态。下次调用生成器函数时，会从暂停的位置继续执行，并将下一个值产生出来。

def dog(name):
    food_list = []
    print('dog {} eat'.format(name))
    while True:
        # x 拿到的是yield接收到的值
        x = yield food_list # x = '一只骨头'
        print('dog{} eat {}'.format(name, x))
        food_list.append(x)


g = dog('hahaha')
res = g.send(None)  # 等同于next(g)
print(res)
res = g.send('一根骨头')
print(res)
g.close()
# g.send(1)    # 关掉之后就无法传值
```

### 15.10 生成式 (没有元组生成式)

**三元表达式**

```python
if 条件:
    res = 111
else:
    res = 222
    
res = 111 if 条件 else 222    
```

**列表生成式**

```python
l = ['a_sb', 'b_sb', 'c_sb', 'd', 'e']
new_l = []
for name in l:
    if name.endswith('sb'):
        new_l.append(name)
print(new_l)

new_l = [name for name in l if name.endswith('sb')]
print(new_l)
```

**字典生成式**	

```python
keys = ['name', 'age', 'gender']
dic = {key: None for key in keys}
# {'name': None, 'age': None, 'gender': None}

items = [('name', 'egon'), ('age', 18), ('gender', 'male')]
{k:v for k, v in items if k != 'gender'}
```

**集合生成式**

```python
sets = ['name', 'age', 'gender']
sets = {b for v in sets}
```

**生成器表达式**

```python
g = (i for i in range(10) if i >3)    
# 强调！！！！！！！！！！！此时g内部一个值也没有
# Out[2]: <generator object <genexpr> at 0x000001DC9483F2E0>
next(g)
```

应用 统计文件有多少个字符

```python
with open('db.txt', mode='rt', encoding='utf-8') as f:
    size_of_line = (len(line) for line in f)
    print(sum(size_of_line)) 
    print(sum(len(line) for line in f))    # 可以简写
```

### 15.11 函数的递归调用

调用一个函数的过程中有直接或间接的调用到函数本身   

 递归的本质就是循环

直接调用本身

```python
# 直接调用本身
def f1():
    print('hello')
    f1()
f1()
# RecursionError: maximum recursion depth exceeded while calling a Python object

import sys
sys.getrecursionlimit()     # 1000
sys.setrecursionlimit(2000)
```

间接调用

```python
def f1():
    print('====>f1')
    f2()
    
def f2():
    print('===>f2')
    f1()
f1()
```

**python没有尾递归优化，不应该无限的调用下去，必须满足某种条件下结束递归调用**

```python
def func(n):
    if n == 10:
        return
    print(n)
    n += 1
    func(n)

func(0)
```

递归的两个阶段

**回溯** 一层一层调用下去 

**递推 **满足某种结束条件，结束递归调用，一层一层返回

```python
def age(n):
    if n == 1:
        return 18
    return age(n-1) + 10

print(age(5))
```

二分法

```python
# 有一个按照从小到大顺序排列的数字列表
# 需要从该数字列表中找到我们想要的那个数字, 如何做更高效

def binary_search(l, find_num):
    print(l)
    mid_index = len(l) // 2
    if len(l) == 0:
        print('not find')
    elif l[mid_index] == find_num:
        print('find it')
    elif find_num < l[mid_index]:
        binary_search(l[:mid_index], find_num)
    else:
        binary_search(l[mid_index+1:], find_num)
```

### 15.12 函数式 （匿名函数）

**面向过程的编程思想  范式**

```python
"过程" 即流程, 指做事的步骤: 先什么、再什么、后干什么
    优点: 复杂问题流程化、进而简单化
    缺点: 扩展性非常差
```

**函数式编程**

* lambda 		用于定义匿名函数
* map
* reduce
* filter

```python
def func(x, y):    # 有名函数 func=函数的内存地址
    return x + y

lambda x, y: x + y
```

调用匿名函数

```python
# 临时调用一次的场景: 更多的是将匿名函数与其他函数配合使用
res = (lambda x, y: x + y)(2, 3)
print(res)
```

**匿名函数的应用场景**

```python
salaries = {
    'siry': 3000,
    'tom':  6000,
    'lili': 90000,
    'jack': 100
}

# 比大小
res = max(salaries, key=lambda x: salaries[x])		# lili
res = min(salaries, key=lambda x: salaries[x])		# jack

# 排序
res = sorted(salaries, key=lambda x: salaries[x], reverse=True)

# 映射 map
l = ['alex', 'lxw', 'ws', 'lyc']
new_l = (name + '_dsb' for name in l)    

res = map(lambda name: name + 'dsb', l)    # 生成器

# 过滤filter
l = ['alex_sb', 'lxw_sb', 'ws', 'lyc']
res = (name for name in l if name.endswith('_sb'))
print(list(res))

filter(lambda name: name.endswith('_sb'), l)    # filter(True, l) 留下了结果为True的值

# reduce(python3) 合并累加
from functools import reduce
reduce(lambda x, y: x + y, [1, 2, 3], 100)   			# (((100 + 1)+2)+3)  ===> 106
reduce(lambda x, y: x + y, ['a', 'b', 'c'], 'hello')    # helloabc 
```

### 15.13 类型提示

```python
# 类型提示Type hinting, python3.5之后
# (str, int, tuple)

def register(name:str, age:int=18, hobbies:tuple)->int:
    print(name)
    print(age)
    print(hobbies)
    return 111    # int

print(register.__annotations__)  
# {'name': <class 'str'>, 'age': <class 'int'>, 'hobbies': <class 'tuple'>, 'return': <class 'int'>}
```

### 15.14 内置函数

绝对值

```python
abs(-1)    				# 1
```

检查可迭代对象中的所有元素是否都为真（True）

```python
all([11, 'aaa', ''])    # False
all([])    				# True
```

判断可迭代对象中是否存在至少一个满足指定条件的元素

```python
any([0, None, 1])    	# True
any([])   			    # False
```

进制转换

```python
bin(11)    				# '0b1011'
oct(11)    				# '0o13'
hex(11)    				# '0xb'
```

布尔值

```python
bool('')    			# False
```

定义函数、类

```python
def func():
    pass
class Foo:
    pass
```

变量对应的值可不可以被调用

```python
callable(Foo)    		# True	
```

ASCII 转换

```python
chr(65)    # 'A'
ord('A')    # 65
```

不可变集合

```python
s = frozenset({1, 2, 3})
```

不可变类型的哈希值

```python
hash(不可变类型)
```

保留小数单位

```python
round(1.521, 2)
```

幂次取余

```python
10 ** 2 % 3
pow(10, 2, 3)
```

创建切片对象

```python
# slice(start, stop, step)

s = slice(1, 4, 2)
l1 = ['a', 'b', 'c', 'd', 'e']
l2 = [1, 2, 3, 4, 5]
l1[s]    						# ['b', 'd']  ==> l1[1:4:2]
l2[s]   						# [2, 4]
```

==========以下都需要掌握==========

将多个可迭代对象（例如列表、元组等）按照索引位置一一对应地打包成元组的序列

返回一个可迭代的 zip 对象

```python
v1 = 'hello'
v2 = [1, 2, 3, 4, 5, 6, 7]
res = zip(v2, v1)
print(list(res))
# [(1, 'h'), (2, 'e'), (3, 'l'), (4, 'l'), (5, 'o')]
```

取商和余数

```python
divmod(10000, 33)
# (303, 1)  
# return (商, 余数)
```

类有哪些属性

```python
class Foo:
    pass
obj = Foo()
obj.xxx = 111
dir(obj)    # obj.哪些属性
['__class__',
 '__delattr__',
 '__dict__',
 '__dir__',
 '__doc__',
 '__eq__',
 '__format__',
 '__ge__',
 '__getattribute__',
 '__gt__',
 '__hash__',
 '__init__',
 '__init_subclass__',
 '__le__',
 '__lt__',
 '__module__',
 '__ne__',
 '__new__',
 '__reduce__',
 '__reduce_ex__',
 '__repr__',
 '__setattr__',
 '__sizeof__',
 '__str__',
 '__subclasshook__',
 '__weakref__',
 'xxx']
```

于将一个可迭代对象（如列表、元组或字符串）组合为一个索引序列，同时返回索引和对应的元素

```python
for i, v in enumerate(['a', 'b', 'c']):    # (索引，值)
    print(i, v)
```

计算字符串表达式的值, 返回表达式的结果

```python
eval('1+2')    # 3
eval('{"a": 2}')    # {'a': 2}
```

判断类型

```python
class Foo:
    pass
obj = Foo()
isinstance(obj, Foo)    # True
isinstance([], list)    # True， 推荐使用
type([]) is list    # 不推荐使用
```

地址 导入模块

```python
# import 'time' # 错误
time = __import__('time')
time.sleep(3)
```

反射  在元类 类 对象 那节

```python
setattr
getattr
delattr
hasattr
```

## 16 模块

### 16.1 模块

```python
"""
什么是模块
	模块是一系列功能的集合体，分为三大类
    		内置模块
  			第三方的模块
			自定义的模块，python，c，c++
				一个python文件本身就是一个模块，文件名m.py ，模块名m
				
ps:  模块有四种形式
	使用python编写的.py文件
	已被编译为共享库或DLL的c或c++拓展
	把一系列模块组织到一起的文件夹(注：文件夹下有一个__init__.py文件，该文件称之为包)
	使用c编写并链接到python解释器的内置模块

为何要用模块
	内置与第三方的模块，拿来就用，无需定义，提升开发效率
	自定义的模块，可以将程序的各部分功能提取出来放到一个模块中为大家共享，减少代码冗余
"""
```

foo.py

```python
x = 2
def f1():
    print(x)

def f2():
    pass

class fu:
    def f3():
        pass
    
# 模块名  foo    
```

**首次导入模块**

```python
# import foo
1. 执行foo.py
2. 产生foo.py的名称空间
3. 在当前文件中产生一个名字foo，该名字指向2中产生的名称空间
# 之后的导入, 都是直接引用首次导入产生的foo.py 的名称空间，不会重复执行代码
```

引用

```python
print(foo.f1)
# 强调1: 模块名.名字 不会与当前名称空间名字发生冲突
f1 = 1111
# 强调2: 无论是查看还是修改操作的都是模块本身，与调用位置无关
x = 1
foo.f1()    # 2
```

逗号分隔符一行导入多个模块

```python
import time
import foo
# 建议上面导入方式
import time, foo
```

**导入模块的规范**

```python
# 1. python 内置模块
# 2，第三方模块
# 3. 自定义的模块		自定义模块的命名应该采用 纯小写 + 下划线风格
import time
import numpy
import foo
```

```python
import ... as ...
import abcdefghijkfagahadhas as fff    
fff.f1()
```

**模块是第一类对象**

1. 可以被赋值给变量或数据结构中的元素。
2. 可以作为函数的参数进行传递。
3. 可以作为函数的返回值返回。
4. 可以在运行时创建。

```python
import foo
f = foo
```

可以在函数内导入模块

```python
def func():
    import foo
```

一个python文件有几种用途

​		(1) 当作程序运行  

​		(2) 当作模块

```python
print(__name__)
# 当foo.py被运行时候, __name__值为 ‘__main__’
# 当foo.py被当作模块导入时候, __name__的值为‘foo’

if __name__ == '__main__':
    f1()
    f2()
else:
    #　被当作模块导入时候做的事情
    pass
```

import 导入模块 必须加前缀

```python
import foo
foo.f1


"""
1. 产生一个模块的名称空间
2. 产生foo.py的名称空间
3. 当前名称空间拿到一个名字，指向模块名称空间中的某一个内存地址
"""
from foo import f1
from foo import f2
# 优点: 不需要加前缀，代码精简
# 缺点：容易与当前名称空间混淆

from foo import f1, f2    # (不推荐)

__all__=['f1', 'f2', 'f3']    #　控制＊代表的名称有哪些
from foo import *    	  #　导入模块中所有的名字(不推荐)

from foo import f1 as g
```

循环导入 （垃圾程序）

```python
# 解决方案1

# m1.py
def f1():
    from m2 import y
    print(y)
x = 'res'

# m2.py    
def f2():
    from m1 import x
    print(x)
y = 'hello'

# m3.py
import m1
m1.f1()

# 解决方案2
# x 和 y 前提
```

**模块的查找优先级**

```python
# 1. 内存(内置模块)
# 2. 硬盘(按照sys.path中存放的文件顺序一次查找)

import sys    	   		# 存放了 一堆文件夹
print(sys.path)    		# 值为列表，存放了 一堆文件夹, 
                   		# 其中第一个文件夹是当前执行文件所在的文件夹
                   		# 第二个文件夹是项目的文件夹(pycharm)，当作不存在
        
print(sys.modules)    	# 以及加载到内存中的模块 {key:value}
print('foo' in sys.modules)          


import foo    		# 从硬盘中找
foo.f1()
import time
time.sleep(10)

# 删除 foo.py
import foo    		# 从内存中找
foo.f1()
```

找foo.py就把foo.py的文件夹添加到环境变量中，临时加

```python
import sys

sys.path.append(r'/.../.../.../.../aa')    # /aa/foo
import foo
```

**编写一个规范的模块**

```python
编写一个规范的模块
mm.py
1.    """注释"""
2.    import sys.py    # 导入其他模块
3.    x = 1            # 全局变量，如果非必须，最好使用局部变量
4.    class foo:       # 定义类    
        '''注释'''
        pass
    
5.    def f1():        # 定义函数
        '''注释'''
        pass
    
6.    if __name__ == '__main__':    # 主程序, 被当前脚本运行
          test()
```

### 16.2 包

```python
1. 包含有__init__.py文件的文件夹
    # 包的本质是模块的一种形式
2. 包被用来当作模块导入
    (a) 产生一个名称空间
    (b) 运行包下的__init__.py, 产生__init__.py的名称空间
    (c) 当前名称空间拿到一个名字，指向模块名称空间中的某一个内存地址

    # sys.path是以执行文件为准的。
```

绝对导入 以包的文件夹作为起始来进行导入

```python
- 包  mmm
    - __init__.py
    - m1.py    # def f1()
    - m2.py    # def f2()
    - m3.py    # def f3()
    
# __init__.py
from mmm.m1 import f1
from mmm.m2 import f2
from mmm.m3 import f3

# import mmm.m3.f3 as f3    # 语法错误 . 的左边必须是个包

# main.py
import mmm
mmm.f1()

from mmm import f1
f1()

# 强调:  导入时候  .的左边必须是个包
```

相对导入，仅限于包内使用，不能跨出包，包内模块之间的导入, 推荐使用相对导入

```python
# m4.py
from mmm.m1 import f1    
f1()


# . : 当前文件夹
# .. : 上一层文件夹
# __init__.py
from .m1 import f1
from .m2 import f2
from .m3 import f3
```

### 16.3 time、datetime 模块

#### 16.3.1 time

* 时间戳: 1970到现在经过的秒数

```python
# time.time() 用于时间间隔的计算
import time
print(time.time())
```

* 2020-03-30 11:11:11

```python
# time.strftime() 用于展示时间
print(time.strftime('%Y-%m-%d %H:%M:%S %p'))
print(time.strftime('%Y-%m-%d %X'))
```

* 结构化时间

```python
# time.localtime() 用于单独获得时间的某一部分
res = time.localtime()
print(res)
# time.struct_time(tm_year=2023, tm_mon=4, tm_mday=10, tm_hour=20,tm_min=3, tm_sec=22, tm_wday=0, tm_yday=100, tm_isdst=0)

print(res.tm_year)
```

#### 16.3.2 datetime

```python
import datetime
print(datetime.datetime.now())
# 2023-04-10 20:08:05.515974

print(datetime.datetime.now())
print(datetime.datetime.now() + datetime.timedelta(days=3))
# 2023-04-10 20:11:25.588241
# 2023-04-13 20:11:25.588241
```

#### 16.3.3 时间格式的转换

格式化时间 <---> 结构化时间 <---> 时间戳 

* 结构化时间 <---> 时间戳

```python
import time
s_time = time.localtime()	# 
print(time.mktime(s_time))	# 结构 --> 时间戳
# 1681128895.0 

tp_time = time.time()
print(time.localtime(tp_time))     # 时间戳 --> 结构 本地时区
print(time.gmtime(tp_time))        # 世界标准时间
```

* 格式化字符串时间 <---> 结构化时间

```python
s_time = time.localtime()
print(time.strftime('%Y-%m-%d %H:%M:%S', s_time))	# 结构 --> 格式

print(time.strptime('2023-4-10 20:21:30', '%Y-%m-%d %H:%M:%S'))		# 格式 --> 结构
```

* '2023-4-10 20:21:30'  续费7天 !!!!!

```python
# 需要结构化时间过渡
import time
struct_time = time.strptime('2023-4-10 20:21:30', '%Y-%m-%d %H:%M:%S')
tp_time = time.mktime(struct_time)
tp_time_now = tp_time + 7 * 24 * 60 * 60
struct_time_now = time.localtime(tp_time_now)
s_time_now = time.strftime('%Y-%m-%d %H:%M:%S', struct_time_now)
```

#### 16.3.4 其它

* 程序睡眠

```python
import time
time.sleep(3)
```

* Mon Apr 10 20:37:16 2023		时间格式

```python
print(time.asctime())
# Mon Apr 10 20:37:16 2023

# 结构化时间 ---> asctime() ---> Mon Apr 10 20:37:16 2023 <--- ctime() <--- 时间戳
print(time.asctime(time.localtime()))
print(time.ctime(time.time()))
```

```python
import datetime
print(datetime.datetime.now())
print(datetime.datetime.utcnow())	# 世界标准时间
# 2023-04-10 20:40:43.617516
# 2023-04-10 12:40:43.617516

print(datetime.datetime.fromtimestamp(4444444444))	# 时间戳 --> 格式化时间
# 2110-11-03 15:54:04
```

### 16.4 random 模块

```python
import random

print(random.random())      			# (0, 1) --> float
print(random.randint(1, 3))     		# [1, 3] --> int
print(random.randrange(1, 3))   		# [1, 3) --> int
print(random.choice([1, 2, 3, '4']))    # 随机取一个
print(random.sample([1, 2, 3, '4'], 2)) # 随机取2个组合
print(random.uniform(1, 3))     		# 1 < float <3

item = [1, 3, 5, 7, 9]
random.shuffle(item)    				# 打乱item顺序
print(item)
```

应用: 随机验证码

```python
def make_code(n):
    res = ''
    for i in range(n):
        alpha_upper = chr(random.randint(97, 122))
        alpha_low = chr(random.randint(65, 90))
        number = chr(random.randint(49, 57))
        res += random.choice([alpha_upper, alpha_low, number])
    print(res)
make_code(6)
```

### 16.5 os 模块

```python
# 获得当前python脚本的工作目录
os.getcwd() 
```

```python
# 改变当前脚本的工作目录, 相当于shell的cd
os.chdir('dirname')
```

```python
#  返回当前目录:  '.'
os.curdir

# 获得当前目录的父目录字符串名:  '..'
os.pardir
```

```python
# 可生成多层递归目录
os.makedirs('dirname1/dirname2') 
```

```python
# 若目录为空则删除，并递归到上一级目录，若为空，删除
os.removedirs('D:\pythonProject\dirname1\dirname2')
```

```python
# 生成单级目录 相当于shell中的 mkdir dirname
os.mkdir('dirname') 
```

```python
# 删除空目录, 若目录不为空则无法删除,报错,相当于shell中的rmdir dirname
os.rmdir('dirname') 
```

```python
# 列出指定目录下所有文件和子目录，包括隐藏文件，并以列表方式打印
os.listdir('dirname')

print(os.listdir(os.curdir))
```

```python
# 删除一个文件
os.remove()  
```

```python
# 重命名文件/目录
os.rename('oldname', 'newname')
```

```python
# 获取文件/目录信息
os.stat('path/filename')
```

```python
# 输出操作系统特定的路径分隔符, win下'\\'    linux '/'
os.sep
```

```python
# 输出当前平台使用的行终止符, win下'\t\n'    linux '\r\n'
os.linesep
```

```python
# 输出分割文件路径的字符串, win ;    linux :
os.pathsep
```

```python
# 输出字符串指示的当前使用平台, win 'nt'    linux 'posix'
os.name
```

```python
# 运行shell命令, 直接显示
os.system("bash command")
```

```python
# 获取系统环境变量
os.environ
```

```python
# 返回path规范化的绝对路径
os.path.abspath('D:/pythonProject')
```

```python
# 将path分割成目录和文件名的二元组返回
os.path.split(path)
```

```python
# 返回path的目录, 就是os.path.split(path)[0]
os.path.dirname(path)
```

```python
# 返回path的最后文件名os.path.split(path)[1], 如果path以/或者\结尾, 返回空
os.path.basename(path)
```

```python
# 如果path存在, 返回True    否则返回 False
os.path.exists(path) 
```

```python
# 如果path是绝对路径, 则返回True
os.path.isabs(path)
```

```python
# 将多个路径组合后返回, 第一个绝对路径之前的参数将被忽略
os.path.join('a', 'b', 'c', 'd')
```

```python
# 返回path所指向文件或目录最后存取时间
os.path.getatime(os.getcwd())
```

```python
# 返回path所指向文件或目录的最后修改时间
os.path.getmtime(os.getcwd())
```

```python
# 返回path大小
os.path.size(path)
```

```python
# 规范化指定路径
os.path.normpath()
```

```python
import os

BASE_DIR = os.path.dirname(os.path.dirname(__file__))
print(BASE_DIR)

BASE_DIR = os.path.normpath(os.path.join(
    __file__,
    '..',
    '..'
))
print(BASE_DIR)
```

```python
from pathlib import Path

root = Path(__file__)
res = root.parent.parent
print(res)
```

```python
res = Path('a\\b\\c') / 'd/e.txt'
print(res) 				 # a\b\c\d\e.txt
print(res.resolve())     # a\b\c\d\e.txt
```

### 16.6 sys 模块

```PYTHON
import sys

sys.argv    # 命令行参数List , 第一个元素是程序本身路径
```

```python
import sys

src_file = sys.argv[1] 
dst_file = sys.argv[2]

# copy文件  
# cmd 中输入下面命令
python copy.py src_file dst_file
```

```python
# 退出程序，正常退出时eixt(0)
sys.exit(n) 
```

```python
# 获取python解释器的版本信息
sys.version
```

```python
# 返回模块的搜索路径，初始化时使用PYTHONPATH环境变量的值
sys.path
```

```python
# 返回操作系统平台的名称
sys.platform
```

* 打印进度条

```python
import time

recv_size = 0
total_size = 3333333


def progress(precent, width):
    if precent > 1:
        precent = 1
    res = int(50 * precent) * '#'
    print(f'\r[%-{width}s] %d%%' % (res, int(precent*100)), end='')


while recv_size < total_size:
    time.sleep(0.01)
    recv_size += 1024
    precent = recv_size / total_size
    progress(precent, 50)

# [##################################################] 100%
```

### 16.7 shutil 模块

高级的文件、文件夹、压缩包、处理模块

```python
import shutil
```

```python
# 将文件内容拷贝到另外一个文件中
shutil.copyfileobj(open('e.txt', 'r'), open('f.txt', 'w'))
```

```python
# 拷贝文件
shutil.copyfile('e.txt', 'eee.txt')     # 目标文件无需存在
```

```python
# 仅拷贝权限。内容、组、用户均不变
shutil.copymode('e.txt', 'eee.txt')     # 目标文件必须存在
```

```python
# 仅拷贝状态信息，包括 mode bits, atime, mtime, flags
shutil.copystat('e.txt', 'eee.txt')     # 目标文件必须存在
```

```python
# 拷贝文件和权限
shutil.copy('e.txt', 'ee.txt')     	 	# 目标文件无需存在
```

```python
# 拷贝文件和状态信息
shutil.copy2('e.txt', 'eeee.txt')   	# 目标文件无需存在
```

```python
# 递归的去拷贝文件夹
shutil.ignore_patterns(*patterns)
shutil.copytree(src, dst, symlinks=False, ignore=None)
shutil.copytree('folder1', 'folder2', ignore=shutil.ignore_patterns('*.txt', '*.tmp'))
```

```python
# 递归的去删除文件
shutil.rmtree('floder1')
```

```python
# 递归的移动文件
shutil.move('floder1', 'floder2')
```

```python
# 创建压缩包并返回文件路径, 例如 zip, tar
shutil.make_archive(base_name, format, ...)

base_name: 	压缩包的文件名, 也可以时压缩包的路径。只是文件名时保存至当前目录, 否则保存至指定路径
format: 	压缩种类, zip  tar  bztar  gztar
root_dir: 	要压缩的文件夹路径, 默认当前目录
owner: 		用户, 默认当前用户
group: 		组, 默认当前组
logger: 	用于记录日志, 通常是logging.Logger对象

ret = shutil.make_archive('/temp/data_bak', 'gztar', root_dir='/data')
```

```python
# 压缩
t = shutil.open('/temp/egon.tar', 'w')
t.add('/test1/a.py', arcname='a.bak')
t.add('/test1/b.py', arcname='b.bak')
t.close()
```

```python
t = tarfile.open('/temp/data_bak.gztar', 'r')
t.extractall('/hello')
t.close()
```

### 16.8 json 模块

* 什么是序列化

```python
序列化指的是把内存的数据类型转换成一个特定的格式的内容
该格式的内容可用于存储或者传输给其他平台使用
    
内存中的数据类型 ---> 序列化 ---> 特定的格式(json格式、pickle格式)
内存中的数据类型 <--- 反序列化 <--- 特定的格式(json格式、pickle格式)
```

* 为何要有序列化

```python
(a)用于存储 --> 用于存档
(b)传输给其他平台使用 --> 跨平台数据交互

    python ------------------ java
    列表      特定的格式       数组
        
强调:
    针对（a）,可以是一种专用的格式 ---> pickle 只有python可以识别
    针对（b）通用的,能够被所有语言识别的格式 --> json
```

* 如何序列化与反序列化

```python
import json

res = json.dumps([1, 'hello', True, False, {'a': 1234}])
print(res, type(res))
# [1, "hello", true, false, {"a": 1234}] 
# <class 'str'>

with open('test.json', mode='wt', encoding='utf-8') as f:
    f.write(res)
    
with open('test.json', mode='rt', encoding='utf-8') as f:
    json_res = f.read()
    res = json.loads(json_res)   

print(res, type(res))
# [1, 'hello', True, False, {'a': 1234}]
# <class 'list'>
```

```python
# 将序列化的结果写入文件的简单方法
with open('test.json', mode='wt', encoding='utf-8') as f:
    json.dump([1, 'hello', True, False, {'a': 1234}], f)
    
# 从文件读取json格式的字符串简单方法
with open('test.json', mode='rt', encoding='utf-8') as f:
    res = json.load(f)
```

```python
# json验证: json格式兼容的是通用数据类型，不能识别某一语言所独有的类型
json.dumps({1, 2, 3})    # 报错

res = json.loads('[1, false, true, "ello"]')
print(res)

# json 在python2.7和3.6之后都可以传bytes类型, 但是3.5不可以
l = json.loads(b'[1, false, true, "ello"]')
print(l)
```

* 猴子补丁 与 ujson

```python
import json
import ujson


# （在首次导入json的位置）在入口处, 进行打补丁
def monkey_patch_json():
    json.__name__ = 'ujson'
    json.dumps = ujson.dumps
    json.loads = ujson.loads


monkey_patch_json()  # 在入口文件处运行
```

### 16.9 pickle 模块

```python
import pickle

res = pickle.dumps({1, 2, 3, 4})
print(res, type(res))
# b'\x80\x04\x95\r\x00\x00\x00\x00\x00\x00\x00\x8f\x94(K\x01K\x02K\x03K\x04\x90.'
# <class 'bytes'>

s = pickle.loads(res)
print(s, type(s))
# {1, 2, 3, 4} <class 'set'>
```

```python
import pickle

with open('te.pkl', mode='wb') as f:
    # python2不支持protocol>2 默认python3中protocol=4
    # 所以python3中的dump操作应该指定protool=2
    pickle.dump('还有一件事', f, protocol=2)

with open('te.pkl', mode='rb') as f:
    res = pickle.load(f)
    print(res)
```

### 16.10 xml 模块

```python
# 古时候用的xml
```

### 16.11 shelve 模块

```python
# shelve 读成字典
```

### 16.12 configparser 模块

```python
python2 中  ConfigParser

用于加载某种特定格式的配置文件  xxx.ini
[section1]
k1 = v1
k2:v2

[section2]
k1 = v1
```

```python
import configparser

config = configparser.ConfigParser()
config.read('xxx.ini')

# 获取sections
print(config.sections())
# ['section1', 'section2']

# 获取某一section下所有options
print(config.options('section1'))
# ['k1', 'k2']

# 获取items
print(config.items('section1'))
# [('k1', 'v1'), ('k2', 'v2')]

# 获取指定的
print(config.get('section1', 'k1'))
# v1
print(config.getint('section2', 'age'))
print(config.getboolean('section3', 'is_admin'))
print(config.getfloat(..., ...))
```

### 16.13 hashlib 模块

```python
哈希hash:
    该算法接收传入的内容，经过运算得到一串hash值
hash的特点:
    1. 传入的内容一样，hash值一样    ===>    1 + 2 文件完整性校验
    2. 传入内容大小变化, hash值大小不变化    ===>    1 + 2 文件完整性校验
    3. hash不能反解出内容    ===>    密码加密
```

```python
import hashlib

m = hashlib.md5('nihao '.encode('utf-8'))
m.update('hello'.encode('utf-8'))
m.update('world'.encode('utf-8'))

res = m.hexdigest()   		# 'nihao helloworld'的哈希校验结果
print(res)
# fc5e038d38a57032085441e7fe7010b0

# 文件完整性
f = open('a.txt', mode='rb')
f.seek()
f.read(2000)
m.update()

# 密码加盐
# 提升撞库的成本
m = hashlib.md5()
m.update('哟'.encode('utf-8'))
m.update('www1111'.encode('utf-8'))
m.update('呵呵'.encode('utf-8'))
res = m.hexdigest()
print(res)
```

### 16.14 subprocess 模块

```python
import subprocess

obj = subprocess.Popen('ls /', shell=True,
                 stdout=subprocess.PIPE,
                 stderr=subprocess.PIPE,
                 )

print(obj)
err_res = obj.stderr.read()     # 错误结果
print(err_res.decode('gbk'))
res = obj.stdout.read()         # 正确结果
print(res.decode('gbk'))
```

### 16.15 日志模块

> https://www.cnblogs.com/linhaifeng/articles/6384466.html#_label12

1. 日志级别

```python
CRITICAL = 50   # FATAL = CRITICAL
ERROR = 40
WARNING = 30    # WARN = WARNING
INFO = 20
DEBUG = 10
NOTSET = 0  	# 不设置
```

2. 默认级别为warning, 默认打印到终端

```python
import logging

logging.debug("调试debug")
logging.info("消息info")
logging.warning("警告warn")
logging.error("错误error")
logging.critical("严重critical")

# WARNING:root:警告warn
# ERROR:root:错误error
# CRITICAL:root:严重critical
```

3. 为logging模块指定全局配置，针对所有logger有效，控制打印到文件中

```python
可在logging.basicConfig()函数中通过具体参数来更改logging模块默认行为，可用参数有

(a) filename：用指定的文件名创建FiledHandler，这样日志会被存储在指定的文件中。

(b) filemode：文件打开方式，在指定了filename时使用这个参数，默认值为“a”还可指定为“w”。

(c) format：指定handler使用的日志显示格式。 

(d) datefmt：指定日期时间格式。 

(f) level：设置rootlogger（后边会讲解具体概念）的日志级别 

(e) stream：用指定的stream创建StreamHandler。可以指定输出到sys.stderr,sys.stdout
  或者文件，默认为sys.stderr。若同时列出了filename和stream两个参数，则stream参数会被忽略。
  
%(name)s：Logger的名字，并非用户名，详细查看

%(levelno)s：数字形式的日志级别

%(levelname)s：文本形式的日志级别

%(pathname)s：调用日志输出函数的模块的完整路径名，可能没有

%(filename)s：调用日志输出函数的模块的文件名

%(module)s：调用日志输出函数的模块名

%(funcName)s：调用日志输出函数的函数名

%(lineno)d：调用日志输出函数的语句所在的代码行

%(created)f：当前时间，用UNIX标准的表示时间的浮 点数表示

%(relativeCreated)d：输出日志信息时的，自Logger创建以 来的毫秒数

%(asctime)s：字符串形式的当前时间。默认格式是 “2003-07-08 16:49:45,896”。逗号后面的是毫秒

%(thread)d：线程ID。可能没有

%(threadName)s：线程名。可能没有

%(process)d：进程ID。可能没有

%(message)s：用户输出的消息

import logging

logging.basicConfig(filename='a.log',
                    format='%(asctime)s - %(name)s - %(levelname)s - %(module)s : %(message)s',
                    datefmt='%Y-%m-%d %H:%M:%S %p',
                    level=10)

logging.debug("调试debug")
logging.info("消息info")
logging.warning("警告warn")
logging.error("错误error")
logging.critical("严重critical")

# a.log中的内容
# 2023-04-16 14:49:03 PM - root - DEBUG - hello : 调试debug
# 2023-04-16 14:49:03 PM - root - INFO - hello : 消息info
# 2023-04-16 14:49:03 PM - root - WARNING - hello : 警告warn
# 2023-04-16 14:49:03 PM - root - ERROR - hello : 错误error
# 2023-04-16 14:49:03 PM - root - CRITICAL - hello : 严重critical
```

4. logging模块的Formatter，Handler，Logger，Filter对象

```python
(a) logger：产生日志的对象

(b)Filter：过滤日志的对象

(c) Handler：接收日志然后控制打印到不同的地方，FileHandler用来打印到文件中，
             StreamHandler用来打印到终端

(d) Formatter对象：可以定制不同的日志格式对象，然后绑定给不同的Handler对象使用，
                   以此来控制不同的Handler的日志格式
                   
import logging

# 1. logger对象: 负责产生日志，然后交给Filter过滤，然后交给不同的Handler输出
logger = logging.getLogger(__file__)

# 2. Filter对象: 不常用，略

# 3. Handler对象: 接收logger传来的日志, 然后控制输出
h1 = logging.FileHandler('t1.log')
h2 = logging.FileHandler('t2.log')
h3 = logging.FileHandler('t3.log')

# 4. Formatter对象: 日志格式
formatter1 = logging.Formatter('%(asctime)s - %(name)s - %(levelname)s - %(module)s: %(message)s',
                               datefmt='%Y-%m-%d %H:%M:%S %p')

formatter2 = logging.Formatter('%(asctime)s: %(message)s',
                               datefmt='%Y-%m-%d %H:%M:%S %p')

formatter3 = logging.Formatter('%(name)s - %(message)s')

# 5. 为Handler对象绑定格式
h1.setFormatter(formatter1)
h2.setFormatter(formatter2)
h3.setFormatter(formatter3)

# 6. 将Handler添加给logger并设置日志级别
logger.addHandler(h1)
logger.addHandler(h2)
logger.addHandler(h3)
logger.setLevel(10)

# 7. 测试

logger.debug("调试debug")
logger.info("消息info")
logger.warning("警告warn")
logger.error("错误error")
logger.critical("严重critical")
```

5. Logger与Handler的级别

```python
logger是第一级过滤，然后才能到handler，我们可以给logger和handler同时设置level


import logging

logger = logging.getLogger(__file__)

form = logging.Formatter('%(asctime)s - %(name)s - %(levelname)s - %(module)s: %(message)s',
                               datefmt='%Y-%m-%d %H:%M:%S %p')

H = logging.StreamHandler()
H.setFormatter(form)
H.setLevel(20)

logger.addHandler(H)
logger.setLevel(10)

logger.debug('l1 debug')

# 此时终端没有输出
```

6. Logger的继承（了解）

```python
import logging

formatter = logging.Formatter('%(asctime)s - %(name)s - %(levelname)s -%(module)s:  %(message)s',
                              datefmt='%Y-%m-%d %H:%M:%S %p', )

ch = logging.StreamHandler()
ch.setFormatter(formatter)

logger1 = logging.getLogger('root')
logger2 = logging.getLogger('root.child1')
logger3 = logging.getLogger('root.child1.child2')

logger1.addHandler(ch)
logger2.addHandler(ch)
logger3.addHandler(ch)
logger1.setLevel(10)
logger2.setLevel(10)
logger3.setLevel(10)

logger1.debug('log1 debug')
logger2.debug('log2 debug')
logger3.debug('log3 debug')

# 2023-04-16 15:17:44 PM - root - DEBUG -hello:  log1 debug
# 2023-04-16 15:17:44 PM - root.child1 - DEBUG -hello:  log2 debug
# 2023-04-16 15:17:44 PM - root.child1 - DEBUG -hello:  log2 debug
# 2023-04-16 15:17:44 PM - root.child1.child2 - DEBUG -hello:  log3 debug
# 2023-04-16 15:17:44 PM - root.child1.child2 - DEBUG -hello:  log3 debug
# 2023-04-16 15:17:44 PM - root.child1.child2 - DEBUG -hello:  log3 debug
```

7.  **应用**

```python
"""
logging 配置
"""

import os
import logging.config

# 定义三种日志输出格式 开始

standard_format = "[%(asctime)s][%(threadName)s:%(thread)d][task_id:%(name)s]"\
                  "[%(filename)s:%(lineno)d][%(levelname)s][%(message)s]"

simple_format = "[%(levelname)s][%(asctime)s][%(filename)s:%(lineno)d] %(message)s"

id_simple_format = "[%(levelname)][%(asctime)s] %(message)s"

# 定义日志输出格式 结束

logfile_dir = os.path.dirname(os.path.abspath(__file__))    # log文件的目录

logfile_name = 'all2.log'   # log文件名

# 如果不存在定义的日志目录就创建一个

if not os.path.isdir(logfile_dir):
    os.mkdir(logfile_dir)

# log文件的全路径
logfile_path = os.path.join(logfile_dir, logfile_name)

# log配置字典

LOGGING_DIC = {
    'version': 1,
    'disable_existing_loggers': False,
    'formatters': {
        'standard': {
            'format': standard_format
        },
        'simple': {
            'format': simple_format
        },
    },
    'filters': {},
    'handlers': {
        # 打印到终端的日志
        'console': {
            'level': 'DEBUG',
            'class': 'logging.StreamHandler',  # 打印到屏幕
            'formatter': 'simple'
        },
        # 打印到文件的日志,收集info及以上的日志
        'default': {
            'level': 'DEBUG',
            'class': 'logging.handlers.RotatingFileHandler',  # 保存到文件
            'formatter': 'standard',
            'filename': logfile_path,  # 日志文件
            'maxBytes': 1024*1024*5,  # 日志大小 5M
            'backupCount': 5,
            'encoding': 'utf-8',  # 日志文件的编码，再也不用担心中文log乱码了
        },
    },
    'loggers': {
        # logging.getLogger(__name__)拿到的logger配置
        '': {
            'handlers': ['default', 'console'],  # 这里把上面定义的两个handler都加上，即log数据既写入文件又打印到屏幕
            'level': 'DEBUG',
            'propagate': True,  # 向上（更高level的logger）传递
        },
    },
}


def load_my_logging_cfg():
    logging.config.dictConfig(LOGGING_DIC)  # 导入上面定义的logging配置
    logger = logging.getLogger(__name__)  # 生成一个log实例
    logger.info('It works!')  # 记录该文件的运行状态


if __name__ == '__main__':
    load_my_logging_cfg()
```

8. diango 的配置

```python
#logging_config.py
LOGGING = {
    'version': 1,
    'disable_existing_loggers': False,
    'formatters': {
        'standard': {
            'format': '[%(asctime)s][%(threadName)s:%(thread)d][task_id:%(name)s][%(filename)s:%(lineno)d]'
                      '[%(levelname)s][%(message)s]'
        },
        'simple': {
            'format': '[%(levelname)s][%(asctime)s][%(filename)s:%(lineno)d]%(message)s'
        },
        'collect': {
            'format': '%(message)s'
        }
    },
    'filters': {
        'require_debug_true': {
            '()': 'django.utils.log.RequireDebugTrue',
        },
    },
    'handlers': {
        #打印到终端的日志
        'console': {
            'level': 'DEBUG',
            'filters': ['require_debug_true'],
            'class': 'logging.StreamHandler',
            'formatter': 'simple'
        },
        #打印到文件的日志,收集info及以上的日志
        'default': {
            'level': 'INFO',
            'class': 'logging.handlers.RotatingFileHandler',  # 保存到文件，自动切
            'filename': os.path.join(BASE_LOG_DIR, "xxx_info.log"),  # 日志文件
            'maxBytes': 1024 * 1024 * 5,  # 日志大小 5M
            'backupCount': 3,
            'formatter': 'standard',
            'encoding': 'utf-8',
        },
        #打印到文件的日志:收集错误及以上的日志
        'error': {
            'level': 'ERROR',
            'class': 'logging.handlers.RotatingFileHandler',  # 保存到文件，自动切
            'filename': os.path.join(BASE_LOG_DIR, "xxx_err.log"),  # 日志文件
            'maxBytes': 1024 * 1024 * 5,  # 日志大小 5M
            'backupCount': 5,
            'formatter': 'standard',
            'encoding': 'utf-8',
        },
        #打印到文件的日志
        'collect': {
            'level': 'INFO',
            'class': 'logging.handlers.RotatingFileHandler',  # 保存到文件，自动切
            'filename': os.path.join(BASE_LOG_DIR, "xxx_collect.log"),
            'maxBytes': 1024 * 1024 * 5,  # 日志大小 5M
            'backupCount': 5,
            'formatter': 'collect',
            'encoding': "utf-8"
        }
    },
    'loggers': {
        #logging.getLogger(__name__)拿到的logger配置
        '': {
            'handlers': ['default', 'console', 'error'],
            'level': 'DEBUG',
            'propagate': True,
        },
        #logging.getLogger('collect')拿到的logger配置
        'collect': {
            'handlers': ['console', 'collect'],
            'level': 'INFO',
        }
    },
}


# -----------
# 用法:拿到俩个logger

logger = logging.getLogger(__name__) #线上正常的日志
collect_logger = logging.getLogger("collect") #领导说,需要为领导们单独定制领导们看的日志
```

### 16.16 re 模块 （正则表达式）

```python
\w  =====>  匹配字母数字下划线    
\W  =====>  匹配非字母数字下划线
\s  =====>  匹配任意空白字符，等价于['\r\t\n\f']
\S  =====>  匹配非空字符
\d  =====>  匹配任意数字，等价于[0-9]
\D  =====>  匹配任意非数字
\A  =====>  匹配字符串开始   ^
\Z  =====>  匹配字符串结束, 如果存在缓缓，只匹配到换行符前的结果
\z  =====>  匹配字符串结束    $
\G  =====>  匹配最后匹配完成的位置
\n  =====>  匹配一个换行符
\t  =====>  匹配一个制表符
^   =====>   匹配字符串的开头
$   =====>  匹配字符串的结尾
.   =====>  匹配除了\n之外的任意一个字符, 指定re.DOTALL才能匹配换行符
[...]   =====> 用来表示一组字符，单独列出：[amk]   匹配'a' 'm' 或 'k'
[^...]  =====> 不再[]中的字符：[^abc]匹配除了a, b, c 之外的字符
*  =====> 匹配0个或多个的表达式
+  =====>   匹配1个或多个表达式
?  =====>   匹配0个或1个由前面的正则表达式定义的片段，非贪婪模式
{n}     =====>  精确匹配n个前面表达式
{n, m}  =====>  匹配n到m次由前面的正则表达式定义的片段，贪婪方式
a|b     =====> 匹配a或者b
()      =====>  匹配括号内的表达式，也表示一个组


重复匹配    . * ? .* .*?  {n, m}
.    匹配除了\n之外的任意一个字符, 指定re.DOTALL才能匹配换行符
*    左侧字符重复0次或者无穷次，贪婪
+    左侧字符重复1次或者无穷次，贪婪
?    左侧字符重复0次或者1次，贪婪
{n, m}    左侧字符重复n次到m次，贪婪    {0,} => *  {1,} =>  {0,1} => ?
```

> http://blog.csdn.net/yufenghyc/article/details/51078107

```python
正则是一门独立的语言
在python中想要使用正则需要借助re模块
面试题
	1. re模块中的常用方法
    	findall 分组优先展示
        ^j.*(n|y)$
        	不会展示所有正则表达式匹配到的内容，而仅仅展示括号内正则表达式匹配到的内容
        match 	从头匹配
        search 	从整体匹配
    2. 贪婪匹配和非贪婪匹配
        	
```

```python
# 正则匹配 import re
```

```python
# 	\W	\w		匹配字母数字下划线 

re.findall("\w", "hello egon 123")
# ['h', 'e', 'l', 'l', 'o', 'e', 'g', 'o', 'n', '1', '2', '3']

re.findall("\W", "hello egon 123")
# [' ', ' ']
```

```python
# 	\s	\S		匹配任意空白字符，等价于['\r\t\n\f']

re.findall('\s','hello\r\t\n\f')
# ['\r', '\t', '\n', '\x0c']

re.findall('\S','hello\r\t\n\f')
# ['h', 'e', 'l', 'l', 'o']
```

```python
# 	\d	\D		匹配任意数字，等价于[0-9]

re.findall('\d','hello egon 123')
# ['1', '2', '3']

re.findall('\D','hello egon 123')
# ['h', 'e', 'l', 'l', 'o', ' ', 'e', 'g', 'o', 'n', ' ']
```

```python
# 	\A	\Z		匹配字符串开始	结尾   

re.findall('\Ahe','hello egon 123')
# ['he']	没有则为空

re.findall('123\Z','hello egon 123')
# ['he']	没有则为空
```

```python
# 	^	$		匹配字符串开始	结尾

re.findall('^h','hello egon 123')	# ['h']

e.findall('3$','hello egon 123')	# ['3']
```

```python
# 	.			匹配除了\n之外的任意一个字符, 指定re.DOTALL才能匹配换行符

re.findall('a.b','a1b a*b a b aaab')	# ['a1b', 'a*b', 'a b', 'aab']

re.findall('a.b','a\nb')				# []

re.findall('a.b','a\nb',re.S)			# ['a\nb']

re.findall('a.b','a\nb',re.DOTALL)		# ['a\nb'] 同上一条意思一样
```

```python
# 	*			匹配0个或多个的表达式

re.findall('ab*','bbbbbbb')				# []

re.findall('ab*','a')					# ['a']

re.findall('ab*','abbbb') 				# ['abbbb']
```

```python
# 	？		    匹配0个或1个由前面的正则表达式定义的片段，非贪婪模式

re.findall('ab？','a')					# ['a']

re.findall('ab？','abbbb') 				# ['ab']
```

```python
# 	.*			默认为贪婪匹配

re.findall('a.*b','a1b22222222b')		# ['a1b22222222b']
```

```python
# 	.*?			为非贪婪匹配：推荐使用

re.findall('a.*?b','a1b22222222b')		# ['a1b']
```

```python
# 	+			左侧字符重复1次或者无穷次，贪婪

re.findall('ab+','a')					# []

re.findall('ab+','abbb')				# ['abbb']
```

```python
# 	{n, m}		匹配n到m次由前面的正则表达式定义的片段，贪婪方式

re.findall('ab{2}','abbb')				# ['abb']

re.findall('ab{2,4}','abbb')			# ['abb']

re.findall('ab{1,}','abbb')				# 即'ab+'

re.findall('ab{0,}','abbb')				# 即'ab*'
```

```python
# 	[]			用来表示一组字符，单独列出：[amk]   匹配'a' 'm' 或 'k'
# 	[]内的都为普通字符了，且如果-没有被转意的话，应该放到[]的开头或结尾
# 	[]内的^代表的意思是取反


re.findall('a[1*-]b','a1b a*b a-b')		# ['a1b', 'a*b', 'a-b']

re.findall('a[^1*-]b','a1b a*b a-b a=b')	# ['a=b']

re.findall('a[0-9]b','a1b a*b a-b a=b')		# ['a1b']

re.findall('a[a-z]b','a1b a*b a-b a=b aeb')	# ['aeb']

re.findall('a[a-zA-Z]b','a1b a*b a-b a=b aeb aEb')	# ['aeb', 'aEb']
```

```python
# 	\	对于正则来说a\\c确实可以匹配到a\c,但是在python解释器读取a\\c时，会发生转义，然后交给re去执行，所以抛出异常
# r代表告诉解释器使用rawstring，即原生字符串，把我们正则内的所有符号都当普通字符处理，不要转义
	
re.findall(r'a\\c','a\c')				# ['a\\c']

re.findall('a\\\\c','a\c')				# ['a\\c']
```

```python
# 	()		分组  匹配括号内的表达式，也表示一个组

re.findall('ab+','ababab123') 			# ['ab', 'ab', 'ab']
re.findall('(ab)+123','ababab123') 		# ['ab']  匹配到末尾的ab123中的ab

re.findall('(?:ab)+123','ababab123') 	# ['ababab123']
# findall的结果不是匹配的全部内容，而是组内的内容, ?:可以让结果为匹配的全部内容

re.findall('href="(.*?)"','<a href="http://www.baidu.com">点击</a>')
# ['http://www.baidu.com']

re.findall('href="(?:.*?)"','<a href="http://www.baidu.com">点击</a>')
# ['href="http://www.baidu.com"']
```

```python
# 	|		匹配a或者b

print(re.findall('compan(?:y|ies)',\
                 'Too many companies have gone bankrupt, and the next one is my company'))

# ['companies', 'company']
```

* **re 模块提供的方法**

```python
import re

re.findall('e','alex make love')
# ['e', 'e', 'e'],返回所有满足匹配条件的结果,放在列表里

re.search('e','alex make love').group()
# e, 只到找到第一个匹配然后返回一个包含匹配信息的对象,
# 该对象可以通过调用group()方法得到匹配的字符串,如果字符串没有匹配，则返回None。

re.match('e','alex make love')
# None,同search,不过在字符串开始处进行匹配,完全可以用search+^代替match

re.split('[ab]','abcd')
# ['', '', 'cd']，先按'a'分割得到''和'bcd',再对''和'bcd'分别按'b'分割

re.sub('a','A','alex make love')
# Alex mAke love，不指定n，默认替换所有

re.sub('a','A','alex make love', 1)
# Alex make love

re.sub('^(\w+)(.*?\s)(\w+)(.*?\s)(\w+)(.*?)$',r'\5\2\3\4\1','alex make love')
# love make alex

re.subn('a','A','alex make love')
# ('Alex mAke love', 2),结果带有总共替换的个数
```

```python
obj=re.compile('\d{2}')

obj.search('abc123eeee').group()	 # 12
obj.findall('abc123eeee') 			 # ['12'],重用了obj
```

```python
# 补充1

import re
print(re.findall("<(?P<tag_name>\w+)>\w+</(?P=tag_name)>","<h1>hello</h1>")) 			# ['h1']
print(re.search("<(?P<tag_name>\w+)>\w+</(?P=tag_name)>","<h1>hello</h1>").group()) 	# <h1>hello</h1>
print(re.search("<(?P<tag_name>\w+)>\w+</(?P=tag_name)>","<h1>hello</h1>").groupdict()) # {'tag_name': 'h1'}

re.search(r"<(\w+)>\w+</(\w+)>","<h1>hello</h1>").group()		# <h1>hello</h1>
re.search(r"<(\w+)>\w+</\1>","<h1>hello</h1>").group()			# <h1>hello</h1>
```

```python
# 补充2

import re
# 使用|，先匹配的先生效，|左边是匹配小数，而findall最终结果是查看分组，所有即使匹配成功小数也不会存入结果
# 而不是小数时，就去匹配(-?\d+)，匹配到的自然就是，非小数的数，在此处即整数

re.findall(r"-?\d+\.\d*|(-?\d+)","1-2*(60+(-40.35/5)-(-4*3))") 
# 找出所有整数['1', '-2', '60', '', '5', '-4', '3']

re.findall('\D?(\-?\d+\.?\d*)',"1-2*(60+(-40.35/5)-(-4*3))") 
# ['1','2','60','-40.35','5','-4','3']
```

```python
# 为何同样的表达式search与findall却有不同结果

# (\d)+相当于(\d)(\d)(\d)(\d)...,是一系列分组
re.search('(\d)+','123').group() 		# group的作用是将所有组拼接到一起显示出来 123
re.findall('(\d)+','123')				# findall结果是组内的结果,且是最后一个组的结果 ['3', '2']
```

> 在线调试工具  tool.oschina.net/regex/

## 17  软件开发目录规范

```python
-ATM    
	-bin    			# 放一些可执行文件
		-start.py
	-conf    			# 配置文件
		-settings.py
	-db    				# 数据库相关操作
		-db_handle.py
	-lib    			# 程序共享库
		-common.py
	-core    			# 核心代码逻辑
		-src.py
	-api    			# 接口文件
		-api.py
	-run.py     		# 程序的启动文件
	-setup.py     		# 安装、部署、打包的脚本
	-requirements.txt   # 存放软件依赖的外部Python包列表
	-README       		# 项目说明文件
```

