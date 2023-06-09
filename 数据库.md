# 数据库

## 1. 概念

​	将数据保存部分全部统一起来，所有人操作数据都来一个地方操作

​	数据库集群，防止单个数据库服务器丢失



​	数据库本质：**一款基于网络通信的应用程序**

​	那其实每个人都可以开发数据库软件，因为它仅仅就是一个基于网络通信的应用程序。



​	数据库软件有很多很多

​	关系型存数据，非关系型用来当作缓存，减轻数据库压力

​	关系型数据库

​	MySQL、oracle、db2、access、sql server

​	非关系型数据库

​	redis、mongodb、memcache



- 关系型

- - 1. 数据库之间彼此有关系或者约束

  - 2. 存储数据的表现形式通常是以表格存储

  - - - name	password		hobby
      - jason	123			学习
      - tank		234			玩游戏

    - 每个字段还有存储类型的限制，比如姓名只能是字符串

- 非关系型

  ​	存储数据通常是以 k，v 键值对的形式











## 2. MySQL配置

### 2.1 介绍

任何基于网络通信的应用程序底层用的都是socket

-服务端

		-基于socket通信
	
		-收发消息
	
		-SQL语句

-客户端

		-基于socket通信
	
		-收发消息
	
		-SQL语句

MySQL不单单支持MySQL自己的客户端app还支持其他编程语言来充当客户端操作

如何解决语言沟通的障碍？

\# 1 让服务端兼容所有的语言(一个人精通多国语言)

\# 2 采用统一的语言(SQL语句)

### 2.2 重要概念介绍

```python
库			文件夹
表			文件
记录			文件内一行行的数据

表头			表格的第一行字段
字段			name、password、hobby
表单			除表头下面的数据就是表单
```

### 2.3 MySQL的安装

参考网站

>  [ https://www.mysql.com/](https://www.mysql.com/)

```shell
服务端	mysqld.exe
客户端 mysql.exe

在前期配置MySQL的时候 cmd终端尽量以管理员的身份运行

windows+r 输入cmd  进入的是普通用户终端 有一些命令是无法执行的
搜索cmd右键 以管理员身份运行
```

### 2.4 启动

```shell
常见软件的默认端口号
    MySQL  		3306
    redis  		6379
    mongodb 	27017
    django  	8000
    flask   	5000
   
MySQL第一次以管理员身份进入是没有密码的 直接回车即可
	mysql -u root -p
客户端连接服务端完整命令
    mysql -h 127.0.0.1 -P 3306 -uroot -p
    
```

### 2.5 sql 语句初识

```sql
1. MySQL中的sql语句是以分号作为结束的标志

2. 基本命令
		show databases;		查看所有的库名
 
3. 连接服务端的命令可以简写
		mysql -uroot -p
    
4. 当你输入的命令不对 又不想让服务端执行并返回报错信息 可以用\c取消
		错误命令 \c
    
5. 客户端退出	加不加分号都可
		exit
    	quit

6. 当你在连接服务端的时候 发现只输入mysql也能连接
    	但是你不是管理员身份 而只是一个游客模式         
```

### 2.6 环境变量配置及系统服务制作

```shell
1. 如何查看当前具体进程
		tasklist
    	tasklist |findstr mysqld

2. 如何杀死具体进程(只有在管理员cmd窗口下才能成功)
		taskkill /F /PID PID号
    
3. 将mysql服务端制作成系统服务(开机自启动)
	查看当前计算机的运行进程数
    	services.msc
    将mysql制作成系统服务
    	mysqld --install
    移除mysql系统服务
    	mysqld --remove
```

### 2.7 设置密码

```shell
mysqladmin -uroot -p 1234 password 123456
					原密码			 新密码
改命令直接在终端输入即可 无序进入客户端
```

### 2.8 破解密码

```sql
你可以将mysql获取用户名和密码校验的功能看成是一个装饰器
装饰在了客户端请求访问的功能上
我们如果将该装饰器移除 那么mysql服务端就不会校验用户名和密码了

1. 先关闭当前mysql服务端
	命令行的方式启动(让mysql跳过用户名密码验证功能), 即跳过授权表
    	mysqld --skip-grant-tables

2. 直接以无密码的方式连接
		mysql -uroot -p   直接回车
    
3. 修改当前用户的密码
	update mysql.user set password=password(123456) where user='root' and host='localhost';
    update mysql.user set authentication_string=password(123456) where user='root' and host='localhost';
    
	真正存储用户表的密码字段 存储的肯定是密文 
	只有用户自己知道明文是什么 其他人都不知道 这样更加的安全
	密码比对也只能比对密文  

4. 立刻将修改数据刷到硬盘
    	flush privileges; 
  
5. 关闭当前服务端 然后以正常校验授权表的形式启动        
```

### 2.9 统一编码

```sql
# mysql默认的配置文件
# my-default.ini 
# ini结尾的一般都是配置文件
# 程序启动会先加载配置文件中的配置之后才真正的启动
# 需要你自己新建一个my.ini的配置文件

# 验证配置是否真的是自动加载
[mysql]
print('hello world')

# 修改配置文件后一定要重启服务才能生效
# 可以将管理员的用户名和密码也添加到配置文件中
[mysqld]
character-set-server=utf8
collation-server=utf8_general_ci
[client]
default-character-set=utf8
[mysql]
user="root"
password=123456
default-character-set=utf
```

### 2.10 基本sql语句

大部分程序的业务逻辑其实都是增删改查

**针对库的增删改查（文件夹）**

```sql
# 增
create database db1;
create database db2 charset='gbk';

# 查 
show databases;		# 查所有
show create database db1;	# 查单个

# 改
alter database db2 charset='utf8';

# 删
drop database db2;
```

**针对表的增删改查(文件)**

```sql
# 在操作表（文件）的时候需要指定其所在的库（文件夹）

# 查看当前所在库的名字
select database();
# 切换库
use db1;

# 增
create table t1(id int, name char(4));

# 查
show tables;		# 查看当前库下面的所有的表名
show create table t1;
describe t1;	
desc t1;

# 改
alter table t1 modify name char(16);

# 删
drop table t1;

"""
create table db2.t1(id int);	 # 也可以用绝对路径的形式操作不同的库 
"""
```



**针对数据的增删改查(一行行数据)**

```sql
"""
一定要现有库 有表 才能操作记录
"""

# 增
insert into t1 values(1, "hello");
insert into t1 values(1, "hello"),(2, "world"),(3, "nihao");

# 查
select * from t1;	# 该命令当数据量特别大的时候不建议使用
select name from t1;

# 改
update t1 set name="dsp" where id > 1;

# 删
delete from t1 where id > 1;
delete from t1 where name='hello';

# 清空表的数据
delete from t1;
```

## 3. MySQL数据类型

### 3.1存储引擎

日常生活中文件格式有很多中，并且针对不同的文件格式会有对应不同存储方式和处理机制(txt,pdf,word,mp4...)

针对不同的数据应该有对应的不同的处理机制来存储

存储引擎就是不同的处理机制

* **MySQL主要存储引擎**

  * Innodb

    是MySQL5.5版本及之后默认的存储引擎

    存储数据更加的安全			支持事务、行锁、外键

    ​		- t1.frm 表结构文件

    ​		- t1.ibd 表数据文件

  * myisam

    是MySQL5.5版本之前默认的存储引擎

    速度要比Innodb更快 但是我们更加注重的是数据的安全

    ​		- t2.frm 表结构文件

    ​		- t2.MYD 表数据文件

    ​		- t2.MYI 索引（index）文件

  * memory

    内存引擎(数据全部存放在内存中) 断电数据丢失

    ​		- t3.frm 表结构

  * blackhole

    无论存什么，都立刻消失(黑洞)

    ​		t4.frm 表结构  数据在内存中

```mysql
# 查看所有的存储引擎
show engines;

# 不同的存储引擎在存储表的时候 异同点
create table t1(id int) engine=innodb;
create table t2(id int) engine=myisam;
create table t3(id int) engine=blackhole;
create table t4(id int) engine=memory;

# 存数据
insert into t1 values(1);
insert into t2 values(1);
insert into t3 values(1);
insert into t4 values(1);
```

#### 创建表的完整语法

```sql
# 语法
create table 表名(
	字段名1 类型(宽度) 约束条件,
    字段名2 类型(宽度) 约束条件,
    字段名3 类型(宽度) 约束条件
)

# 注意
1 在同一张表中字段名不能重复
2 宽度和约束条件是可选的(可写可不写) 而字段名和字段类型是必须的
	约束条件写的话 也支持写多个
    字段名1 类型(宽度) 约束条件1 约束条件2...,
	create table t5(id);  报错
3 最后一行不能有逗号
	create table t6(
        id int,
        name char,
    );   报错
```

```sql
"""补充"""
# 宽度
	一般情况下指的是对存储数据的限制
	create table t7(name char);  默认宽度是1
    insert into t7 values('jason');
    insert into t7 values(null);  关键字NULL
    
    针对不同的版本会出现不同的效果
    5.6版本默认没有开启严格模式 规定只能存一个字符你给了多个字符，那么我会自动帮你截取
    5.7版本及以上或者开启了严格模式 那么规定只能存几个 就不能超，一旦超出范围立刻报错 
    		Data too long for ....
    		
"""严格模式到底开不开呢？"""
使用数据库的准则:
	能尽量少的让数据库干活就尽量少 不要给数据库增加额外的压力
```

```sql
# 约束条件 null  not null不能插入null
create table t8(id int,name char not null);

"""
宽度和约束条件到底是什么关系
	宽度是用来限制数据的存储
	约束条件是在宽度的基础之上增加的额外的约束
""" 
```

#### 严格模式

```sql
# 如何查看严格模式
show variables like "%mode";

模糊匹配/查询
	关键字 like
		%:匹配任意多个字符
        _:匹配任意单个字符

# 修改严格模式
	set session  只在当前窗口有效
    set global   全局有效
    
    set global sql_mode = 'STRICT_TRANS_TABLES';
    
    修改完之后 重新进入服务端即可
```

### 3.2 整形

| 类型           | 大小（字节） | 范围（有符号） | 范围（无符号） | 用途       |
| :------------- | :----------- | :------------- | :------------- | :--------- |
| TINYINT        | 1            | -128，127      | 0, 255         | 小整数值   |
| SMALLINT       | 2            | -32768，32767  | 0, 65535       | 大整数值   |
| MEDIUMINT      | 3            |                |                | 大整数值   |
| INT or INTEGER | 4            |                |                | 大整数值   |
| BIGINT         | 8            |                |                | 极大整数值 |

* 存储年龄、等级、id、号码等等

```sql
"""
整型默认情况下都是带有符号的
mysql 5.6 超出限制只存最大可接受值
mysql 5.7 报错
"""
create table t9(id tinyint);
insert into t9 values(-129),(256);

# 约束条件之unsigned 无符号
create table t10(id tinyint unsigned);

create table t11(id int);

# 针对整型 括号内的宽度到底是干嘛的
create table t12(id int(8));
insert into t12 values(123456789);

"""
特例:只有整型括号里面的数字不是表示限制位数
id int(8)
	如果数字没有超出8位 那么默认用空格填充至8位
	如果数字超出了8位 那么有几位就存几位(但是还是要遵守最大范围)
"""
create table t13(id int(8) unsigned zerofill);
# 用0填充至8位

# 总结:
针对整型字段 括号内无需指定宽度 因为它默认的宽度以及足够显示所有的数据了
```

### 3.3 浮点型

| 类型    | 大小（字节）                            | 范围（有符号）   | 范围（无符号）   | 用途                          |
| :------ | :-------------------------------------- | :--------------- | :--------------- | :---------------------------- |
| FLOAT   | 4                                       |                  |                  | 单精度               浮点数值 |
| DOUBLE  | 8                                       |                  |                  | 双精度               浮点数值 |
| DECIMAL | 对 DECIMAL(M,D),如果M>D，为M+2否则为D+2 | 依赖于M和D的取值 | 依赖于M和D的取值 | 小数值                        |

* 作用：身高、体重、薪资

```sql
# 存储限制
float(255,30)  			# 总共255位 小数部分占30位
double(255,30)  		# 总共255位 小数部分占30位
decimal(65,30)  		# 总共65位 小数部分占30位

# 精确度验证
create table t15(id float(255,30));
create table t16(id double(255,30));
create table t17(id decimal(65,30));

insert into t15 values(1.111111111111111111111111111111);
# 1.111111164093017600000000000000

insert into t16 values(1.111111111111111111111111111111);
# 1.111111111111111200000000000000 

insert into t17 values(1.111111111111111111111111111111);
# 1.111111111111111111111111111111

float < double < decimal
# 要结合实际应用场景 三者都能使用
```

### 3.4 字符类型

* **char**

  ​	定长

  ​	char(4)	 数据超过四个字符直接报错 不够四个字符空格补全

* **varchar**

  ​	变长

​		    varchar(4)  数据超过四个字符直接报错 不够有几个存几个

```python
create table t18(name char(4));
create table t19(name varchar(4));

insert into t18 values('a');
insert into t19 values('a');

# 介绍一个小方法 char_length统计字段长度
select char_length(name) from t18;
select char_length(name) from t19;

"""
首先可以肯定的是 char硬盘上存的绝对是真正的数据 带有空格的
但是在显示的时候MySQL会自动将多余的空格剔除
"""

# 再次修改sql_mode 让MySQL不要做自动剔除操作
set global sql_mode = 'STRICT_TRANS_TABLES,PAD_CHAR_TO_FULL_LENGTH';

set global sql_mode = 'ONLY_FULL_GROUP_BY,STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION,PAD_CHAR_TO_FULL_LENGTH'
```

* **char 与 varchar 对比**

```python
char
	缺点:浪费空间
	优点:存取都很简单
		直接按照固定的字符存取数据即可
		jason egon alex wusir tank 
		存按照五个字符存 取也直接按照五个字符取
		
varchar
	优点:节省空间
	缺点:存取较为麻烦
		1bytes+jason 1bytes+egon 1bytes+alex 1bytes+tank 
		
		存的时候需要制作报头
		取的时候也需要先读取报头 之后才能读取真实数据
		
以前基本上都是用的char 其实现在用varchar的也挺多


补充:
    进来公司之后你完全不需要考虑字段类型和字段名
    因为产品经理给你发的邮件上已经全部指明了
```

### 3.5 日期类型

* 分类
  * date				年月日   			2023-6-1
  * datealtertime        年月日时分秒   2023-6-1 11:11:11
  * time                时分秒              11:11:11
  * year                年                      2023

```python
create table stu(
	id int,
    name varchar(16),
    born_year year,
    birth date,
    study_time time,
    reg_time datetime
);
insert into stu values(1,'egon','1880','1880-11-11','11:11:11','2020-11-11 11:11:11');
```

### 3.6 枚举与集合类型

* **枚举 enum  **		多选一
* **集合 set**		       多选多

```python
# 枚举字段 后期在存数据的时候只能从枚举里面选择一个存储 
create table user(
	id int,
    name char(16),
    gender enum('male','female','others')
);
insert into user values(1,'jason','male');  正常
insert into user values(2,'egon','xxxxooo');  报错


# 集合可以只写一个  但是不能写没有列举的
create table teacher(
	id int,
    name char(16),
    gender enum('male','female','others'),
    hobby set('read','DBJ','hecha')
);
insert into teacher values(1,'jason','male','read');  正常
insert into teacher values(2,'egon','female','DBJ,hecha');  正常
insert into teacher values(3,'tank','others','生蚝'); 报错
```

## 4 表关系

### 4.1 约束条件

* not null			非空
* zerofill              用0补齐  （整形）
* unsigned         无符号      （整形）
* **default**            默认值

```sql
# 插入数据的时候可以指定字段
create table t1(
	id int,
    name char(16),
    gender enum("male", "female", "others") default "male"
);

insert into t1(id, name) values(1, "hello");
```

* **unique**             唯一

```sql
# 单列唯一
create table t2(
	id int unique,
    name char(16)
);
insert into t2 values(1, "hello"),(2, "world");
insert into t2 values(1, "bye");				
# 报错: ERROR 1062 (23000): Duplicate entry '1' for key 'id'

# 联合唯一
"""
ip port 
单个可以重复，加在一起是唯一的
"""
create table t3(
	id int, 
    ip char(16), 
    port int,
    unique(ip, port)
);
insert into t3 values(1, "127.0.0.1", 8080);
insert into t3 values(2, "127.0.0.1", 9090);
insert into t3 values(2, "127.0.0.1", 8080);		# 报错
```

* **primary key**   主键

```sql
# 1. 单从约束效果上来看primary key等价于not null + unique	（非空且唯一）
create table t4(id int primary key);
insert into t4 values(1);
insert into t4 values(null) (1);	# 报错

# 2. 是innodb存储引擎组织数据的依据
# innodb存储引擎在创建表的时候必须要有primary key
# 一张表有一个主键，没有设置，则从上往下搜索直到遇到一个非空且唯一的字段自动升级为主键
create table t5(
	id int,
    name char(16) not null unique
);

# 3. 一张表应该都有一个主键字段，并且通常使用 id/uid/sid字段作为主键
create table t6(
	uid int primary key
);

# 4. 联合主键（多个字段联合起来作为表的主键，本质还是一个主键）
create table t7(
	ip char(16),
    port int,
    primary key(ip, port)
);
```

* **auto_increment** 自增

```sql
# 当编号特别多的时候，人为的去维护太麻烦
# 注意auto_increment通常都是加在主键上的 不能给普通字段加
create table t8(
	id int primary key auto_increment,
    name char(16)
);
insert into t8(name) values('h1'), ("h2"), ("h3");

# 若加给其它字段，报错
ERROR 1075 (42000): Incorrect table definition; there can be only one auto column and it must be defined as a key
```

补充

```sql
id int primary key auto_increment	# 结论: 在创建表的时候id字段一定要加primary key 和 自增

delete from t1		# 删除表中数据后 主键的自增不会停止

truncate t1  		# 清空表数据并且重置主键
```

### 4.2 表与表之间建关系 （约束）

拆分表，建立表间联系。	

​	可以减少磁盘空间浪费，增强数据扩展性，结构更加清晰。

* **外键**

```sql
# 外键就是用来帮助我们建立表与表之间关系的
foreign key
```

* **表关系**

```sql
表与表之间只有四种关系
	一对多关系
		在MySQL的关系中没有多对一一说
		一对多 多对一 都叫一对多！！！
	多对多关系
	一对一关系
	没有关系
```

* **一对多**

```sql
"""
判断表与表之间关系的时候，换位思考  分别站在两张表的角度考虑
foreign key 
	1. 一对多表关系   外键字段建在多的一方
	2. 在创建表的时候 一定要先建被关联表 
	3. 在录入数据的时候 也必须先录入被关联表
"""
# sql 语句建立表关系
create table dep(
	id int primary key auto_increment,
    dep_name char(16)
);

create table emp(
	id int primary key auto_increment,
    name char(16),
    dep_id int,
    foreign key(dep_id) references dep(id)
);

insert into dep(dep_name) values("部门1"),("部门2");
insert into emp(name, dep_id) values("s1", 1),("s2", 2),("s3", 1);
```

修改和删除

```sql
# 修改dep表里面的id字段
update dep set id=200 where id=2;  不行
# 删除dep表里面的数据
delete from dep;  不行

# 1. 先删除员工数据，再删除部门
# 2. 做到数据之间有关系 	级联删除  级联更新

create table dep(
	id int primary key auto_increment,
    dep_name char(16)
);

create table emp(
	id int primary key auto_increment,
    name char(16),
    dep_id int,
    foreign key(dep_id) references dep(id)
    on update cascade		# 同步更新
    on delete cascade		# 同步删除
);
```

* **多对多**

```sql
# 多对多需要单独开始一张表，建立两个一对多关系，以此来联系两张表
# 例子: 图书表 和 作者表
create table book(
    id int primary key auto_increment,
    title varchar(32)
);

create table author(
	id int primary key auto_increment,
    name varchar(32)
);

create table book2author(
	id int primary key auto_increment,
    author_id int,
    book_id int,
    foreign key(author_id) references author(id)
    on update cascade
    on delete cascade,
    foreign key(book_id) references book(id)
    on update cascade
    on delete cascade    
);
```

* **一对一**

```sql
# 如果一个表字段特别多，每次查询所有字段不是都用得上，可以拆分表
# 单向的一对多都不成立，则表关系为 一对一 或者 没有关系
# 一对一 外键字段建在任意一方都可以 但是推荐建在查询频率比较高的表中
create table userdata(
    id int primary key auto_increment,
    phone int,
    addr varchar(64)
);


create table user(
	id int primary key auto_increment,
    name varchar(16),
    userdata_id int unique,		# 与一对多区别是在于唯一
    foreign key(userdata_id) references userdata(id)
    on update cascade
    on delete cascade
);
```

### 4.3 修改表的完整语法大全

```sql
# MySQL对大小写是不敏感的
```

* 修改表名

```sql
alter table 表名 rename 新表名;
```

* 增加字段

```sql
alter table 表名 add 字段名 字段类型(宽度) 约束条件;

alter table 表名 add 字段名 字段类型(宽度) 约束条件 first;

alter table 表名 add 字段名 字段类型(宽度) 约束条件 after 字段名;
```

* 删除字段

```sql
alter table 表名 drop 字段名
```

* 修改字段

```sql
alter table 表名 modify 字段名 字段类型(宽度) 约束条件;

alter table 表名 change 旧字段名 新字段名 字段类型(宽度) 约束条件;
```

### 4.4 复制表

```sql
# sql语句查询的结果其实也是一张虚拟表

create table 表名 select * from 旧表;  不能复制主键 外键 ...

create table new_dep2 select * from dep where id>3;
```

## 5 查询表

### 5.1 单表操作

* 准备表

```sql
create table emp(
  id int not null unique auto_increment,
  name varchar(20) not null,
  sex enum('male','female') not null default 'male', 
  age int(3) unsigned not null default 28,
  hire_date date not null,
  post varchar(50),
  post_comment varchar(100),
  salary double(15,2),
  office int, 
  depart_id int
);

#三个部门：教学，销售，运营
insert into emp(name,sex,age,hire_date,post,salary,office,depart_id) values
('jason','male',18,'20170301','张江第一帅形象代言',7300.33,401,1),	# 以下是教学部
('tom','male',78,'20150302','teacher',1000000.31,401,1),
('kevin','male',81,'20130305','teacher',8300,401,1),
('tony','male',73,'20140701','teacher',3500,401,1),
('owen','male',28,'20121101','teacher',2100,401,1),
('jack','female',18,'20110211','teacher',9000,401,1),
('jenny','male',18,'19000301','teacher',30000,401,1),
('sank','male',48,'20101111','teacher',10000,401,1),
('哈哈','female',48,'20150311','sale',3000.13,402,2),				# 以下是销售部门
('呵呵','female',38,'20101101','sale',2000.35,402,2),
('西西','female',18,'20110312','sale',1000.37,402,2),
('乐乐','female',18,'20160513','sale',3000.29,402,2),
('拉拉','female',28,'20170127','sale',4000.33,402,2),
('僧龙','male',28,'20160311','operation',10000.13,403,3), 		# 以下是运营部门
('程咬金','male',18,'19970312','operation',20000,403,3),
('程咬银','female',18,'20130311','operation',19000,403,3),
('程咬铜','male',18,'20150411','operation',18000,403,3),
('程咬铁','female',18,'20140512','operation',17000,403,3);
```

```sql
# 当表字段特别多 展示的时候错乱 可以使用\G分行展示
select * from emp\G;

# 旧win 插入中文的时候还是会出现乱码或者空白的现象 可以将字符编码统一设置成GBK
```

* 几个重要的关键字的执行顺序

```sql
# 书写顺序
select id,name from emp where id > 3;
# 执行顺序
from
where
select
```

#### 5.1.1 where 筛选条件

​	对整体数据的一个筛选操作

* 查询id大于等于3小于等于6的数据

```sql
select id,name from emp where id>=3 and id<=6;
select id,name from emp where id between 3 and 6;  # 两者等价
```

* 查询薪资是20000或者18000或者17000的数据

```sql
select * from emp where salary=20000 or salary=18000 or salary=17000;
select * from emp where salary in (20000,18000,17000);
```

* 查询员工姓名中包含字母o的员工的姓名和薪资

```sql
"""
模糊查询
	like
		%  匹配任意多个字符
		_  匹配任意单个字符
"""
select name,salary from emp where name like '%o%';
```

* 查询员工姓名是由四个字符组成的 姓名和薪资  char_length()   _

```sql
select name,salary from emp where name like '____';
select name,salary from emp where char_length(name) = 4;
```

* 查询id小于3或者id大于6的数据

```sql
select * from emp where id not between 3 and 6;
select * from emp where id < 3 or id >6;
```

* 查询薪资不在20000,18000,17000范围的数据

```sql
select * from emp where salary not in (20000,18000,17000);
```

* 查询岗位描述为空的员工姓名和岗位名  针对null不用等号 用is

```sql
select name,post from emp where post_comment = NULL;	# 空, 查询不到值
select name,post from emp where post_comment is NULL;
select salary from emp where post_comment is not null;
```

#### 5.1.2 group by 分组

* 按照部门分组

```sql
select * from emp group by post;

"""
分组之后 
	最小可操作单位应该是组 还不再是组内的单个数据
	上述命令在你没有设置严格模式的时候是可正常执行的 返回的是分组之后 每个组的第一条数据 但是这不符合分组的规范
	分组之后不应该考虑单个数据 而应该以组为操作单位(分组之后 没办法直接获取组内单个数据)
	如果设置了严格模式 那么上述命令会直接报错 
"""

set global sql_mode = 'strict_trans_tables,only_full_group_by';
# 设置严格模式之后  分组 默认只能拿到分组的依据

select post from emp group by post; 
# 按照什么分组就只能拿到分组 其他字段不能直接获取 需要借助于一些方法(聚合函数)
# 聚合函数
		max
		min
		sum
		count
		avg
```

* 获取每个部门的最高薪资

```sql
select post,max(salary) from emp group by post;
select post as '部门',max(salary) as '最高薪资' from emp group by post;
select post '部门',max(salary) '最高薪资' from emp group by post;
# # as可以给字段起别名 也可以直接省略不写 但是不推荐 因为省略的话语意不明确 容易错乱
```

* 获取每个部门的最低薪资

```sql
select post,min(salary) from emp group by post;
```

* 获取每个部门的平均薪资

```sql
select post,avg(salary) from emp group by post;
```

* 获取每个部门的工资总和

```sql
select post,sum(salary) from emp group by post;
```

* 获取每个部门的人数

```sql
select post,count(id) from emp group by post;  			# 常用 符合逻辑
select post,count(salary) from emp group by post;
select post,count(age) from emp group by post;
select post,count(post_comment) from emp group by post;  # count  对于null不行
```

* 查询分组之后的部门名称和每个部门下所有的员工姓名 

```sql
"""
group_concat不单单可以支持你获取分组之后的其他字段值 还支持拼接操作
concat不分组的时候用 
concat_ws 多个字段的连接符是相同的情况下
"""

select post,group_concat(name) from emp group by post;
select post,group_concat(name,'_DSB') from emp group by post;
select post,group_concat(name,':',salary) from emp group by post;

select concat('NAME:',name),concat('SAL:',salary) from emp;

select concat_ws(":", name, age, sex) from emp;
```

#### 5.1.3 补充

* as语法不单单可以给字段起别名 还可以给表临时起别名

```sql
select emp.id,emp.name from emp;  
select emp.id,emp.name from emp as t1;   报错
select t1.id,t1.name from emp as t1;
```

* 查询每个人的年薪  12薪

```sql
select name,salary*12 from emp;
```

#### 5.1.4 分组注意事项

```sql
# sql_mode 中的 ONLY_FULL_GROUP_BY 需要取消
"""
关键字where和group by同时出现的时候group by必须在where的后面
where先对整体数据进行过滤之后再分组操作
where筛选条件不能使用聚合函数
"""

select max(salary) from emp;  	# 不分组 默认整体就是一组
```

*  统计各部门年龄在30岁以上的员工平均薪资

```sql
# 1 先求所有年龄大于30岁的员工
select * from emp where age>30;

# 2 再对结果进行分组
select * from emp where age>30 group by post;

select post,avg(salary) from emp where age>30 group by post;
```

#### 5.1.5 having 分组之后的筛选条件

```sql
"""
having的语法根where是一致的
只不过having是在分组之后进行的过滤操作
即having是可以直接使用聚合函数的
"""
```

* 统计各部门年龄在30岁以上的员工平均工资并且保留平均薪资大于10000的部门

```sql
select post, avg(salary) from emp
    where age > 30
    group by post
    having avg(salary) > 10000
;
```

#### 5.1.6 distinct 去重

```sql
"""
一定要注意 必须是完全一样的数据才可以去重！！！
有主键存在的情况下 是不可能去重的


ORM  对象关系映射   让不懂SQL语句的人也能够非常牛逼的操作数据库
表								类
一条条的数据						对象
字段对应的值						对象的属性

你再写类 就意味着在创建表
用类生成对象 就意味着再创建数据
对象点属性 就是在获取数据字段对应的值
目的就是减轻python程序员的压力 只需要会python面向对象的知识点就可以操作MySQL
"""

select distinct id,age from emp;
select distinct age from emp;
```

#### 5.1.7 order by 排序

```sql
select * from emp order by salary;			# 默认升序
select * from emp order by salary asc;		# 升序
select * from emp order by salary desc;		# 降序
```

*  先按照age降序排  如果碰到age相同 则再按照salary升序排

```sql
select * from emp order by age desc,salary asc;
```

* 统计各部门年龄在10岁以上的员工平均工资并且保留平均薪资大于1000的部门,然后对平均工资降序排序

```sql
select post, avg(salary) from emp where
	age > 10
	group by post
	having avg(salary) > 1000
	order by avg(salary) desc
;	
```

#### 5.1.8 limit 限制展示条数

```sql
select * from emp;

"""针对数据过多的情况 我们通常都是做分页处理"""
select * from emp limit 3;  # 只展示三条数据

select * from emp limit 0,5;
select * from emp limit 5,5;
# 第一个参数是起始位置, 且不包括
# 第二个参数是展示条数
```

#### 5.1.9 正则

```sql
# regexp

select * from emp where name regexp '^j.*(n|y)$';
```

### 5.2 连表操作理论

* 准备表

```sql
#建表
create table dep(
id int,
name varchar(20) 
);

create table emp(
id int primary key auto_increment,
name varchar(20),
sex enum('male','female') not null default 'male',
age int,
dep_id int
);

#插入数据
insert into dep values
(200,'技术'),
(201,'人力资源'),
(202,'销售'),
(203,'运营');

insert into emp(name,sex,age,dep_id) values
('jason','male',18,200),
('egon','female',48,201),
('kevin','male',18,201),
('nick','male',28,202),
('owen','male',18,203),
('jerry','female',18,204);
```

#### 5.2.1 表查询

```sql
select * from dep,emp;  # 结果   笛卡尔积

select * from emp,dep where emp.dep_id = dep.id;
```

```sql
"""
MySQL在后面查询数据过程中
开设了拼表操作的对应的方法
	inner join  内连接
	left join   左连接
	right join  右连接
	union		全连接
"""
```

* inner join 内连接

```sql
select * from emp inner join dep on emp.dep_id = dep.id;
# 只拼接两张表中公有的数据部分
```

* left join 左连接

```sql
select * from emp left join dep on emp.dep_id = dep.id;
# 左表所有的数据都展示出来 右表没有对应的项就用NULL
```

* right join 右连接

```sql
select * from emp right join dep on emp.dep_id = dep.id;
# 右表所有的数据都展示出来 没有对应的项就用NULL
```

* union 全连接  

```sql
select * from emp left join dep on emp.dep_id = dep.id
union
select * from emp right join dep on emp.dep_id = dep.id;
# 左右两表所有的数据都展示出来, 即合并左连接和右连接
```

#### 5.2.2 子查询

```sql
# 将一个查询语句的结果当做另外一个查询语句的条件去用, 当作条件的时候，用()括起来
```

```sql
"""
查询部门是技术或者人力资源的员工信息
	1 先获取部门的id号
    2 再去员工表里面筛选出对应的员工
"""
select id from dep where name='技术' or name = '人力资源';
select name from emp where dep_id in (200,201);

select * from emp where 
	dep_id in (select id from dep where name='技术' or name = '人力资源');
```

#### 5.2.3 总结

```sql
表的查询结果可以作为其他表的查询条件
也可以通过起别名的方式把它作为一个张虚拟表和其他表关联

"""
多表查询就两种方式
	先拼接表再查询
	子查询 一步一步来
"""
```

## 6 navicat 

```sql
Navicat 软件

navicat 能够充当多个数据库的客户端

navicat 图形化界面有时候反应速度较慢 你可以选择刷新或者关闭当前窗口再次打开即可

当你有一些需求该软件无法满足的时候 你就自己动手写 sql
```

```sql
# 提示

1 MySQL是不区分大小写的
	验证码忽略大小写
		内部统一转大写或者小写比较即可
			upper
			lower

2 MySQL建议所有的关键字写大写

3 MySQL中的注释 有两种
	--
	#

4 在navicat中如何快速的注释和解注释
	ctrl + ？  加注释
	ctrl + ？  基于上述操作再来一次就是解开注释
	如果你的navicat版本不一致还有可能是
	ctrl + shift + ？解开注释
```

```sql
# 练习题
# 分布式解决，一步一步来

-- 1、查询所有的课程的名称以及对应的任课老师姓名
SELECT
	course.cname,
	teacher.tname 
FROM
	course
	INNER JOIN teacher ON course.teacher_id = teacher.tid;

-- 4、查询平均成绩大于八十分的同学的姓名和平均成绩
SELECT
	student.sname,
	t1.avg_num 
FROM
	student
	INNER JOIN (
	SELECT
		score.student_id,
		avg( num ) AS avg_num 
	FROM
		score
		INNER JOIN student ON score.student_id = student.sid 
	GROUP BY
		score.student_id 
	HAVING
		AVG( num ) > 80 
	) AS t1 ON student.sid = t1.student_id;


-- 7、 查询没有报李平老师课的学生姓名
# 分步操作
# 1 先找到李平老师教授的课程id
# 2 再找所有报了李平老师课程的学生id
# 3 之后去学生表里面取反 就可以获取到没有报李平老师课程的学生姓名
SELECT
	student.sname 
FROM
	student 
WHERE
	sid NOT IN (
	SELECT DISTINCT
		score.student_id 
	FROM
		score 
	WHERE
		score.course_id IN ( SELECT course.cid FROM teacher INNER JOIN course ON teacher.tid = course.teacher_id WHERE teacher.tname = '李平老师' ) 
	);

-- 8、 查询没有同时选修物理课程和体育课程的学生姓名
# (只要选了一门的 选了两门和没有选的都不要)
# 1 先查物理和体育课程的id
# 2 再去获取所有选了物理和体育的学生数据
# 3 按照学生分组 利用聚合函数count筛选出只选了一门的学生id
# 4 依旧id获取学生姓名
SELECT
	student.sname 
FROM
	student 
WHERE
	student.sid IN (
	SELECT
		score.student_id 
	FROM
		score 
	WHERE
		score.course_id IN ( SELECT course.cid FROM course WHERE course.cname IN ( '物理', '体育' ) ) 
	GROUP BY
		score.student_id 
	HAVING
		COUNT( score.course_id ) = 1 
	);

-- 9、 查询挂科超过两门(包括两门)的学生姓名和班级
# 1 先筛选出所有分数小于60的数据
# 2 按照学生分组 对数据进行计数获取大于等于2的数据
SELECT
	class.caption,
	student.sname 
FROM
	class
	INNER JOIN student ON class.cid = student.class_id 
WHERE
	student.sid IN (
	SELECT
		score.student_id 
	FROM
		score 
	WHERE
		score.num < 60 GROUP BY score.student_id HAVING COUNT( score.course_id ) >= 2 
	);
```

## 7 pymysql 模块

### 7.1 查询

```python
# 支持Python代码操作数据库mysql

import pymysql


conn = pymysql.connect(
        host = "127.0.0.1",
        port = 3306,
        user = "root",
        password = "123456",
        database = "db68",
        charset = "utf8"       # 编码不需要加 -
)

cursor = conn.cursor(cursor=pymysql.cursors.DictCursor)         # 产生一个游标对象

sql = "show databases;"
res = cursor.execute(sql)

print(res)                     # execute 返回的是当前sql语句所影响的行数，该返回值一般不用

print(cursor.fetchone())        # 拿一条
print(cursor.fetchmany(2))      # 指定拿几条
print(cursor.fetchall())        # 拿所有


# 读取数据类似于文件光标的移动
cursor.scroll(1, "relative")    # 相对于光标所在的位置继续往后移动一位

cursor.scroll(1, "absolute")    # 相对于数据的起始位置往后移动一位

```

* **sql 注入**

```sql
利用一些语法的特性 书写一些特点的语句实现固定的语法
MySQL利用的是MySQL的注释语法
select * from user where name='jason' -- jhsadklsajdkla' and password=''

select * from user where name='xxx' or 1=1 -- sakjdkljakldjasl' and password=''

# 敏感的数据不要自己做拼接 交给execute帮你拼接即可

```

```python
import pymysql

conn = pymysql.connect(
    host = "127.0.0.1",
    port = 3306,
    user = "root",
    password = "123456",
    database = "db68",
    charset = "utf8"
)

cursor = conn.cursor(pymysql.cursors.DictCursor)

username = input(">>>: ")
password = input(">>>: ")

sql = "select * from user_passwd where name='%s' and password='%s'" % (username, password) 

rows = cursor.execute(sql)
if rows:
    print("登录成功")
else:
    print("登录失败")
    
    
>>>: xx' or 1=1 -- fagaga
>>>:
    
```

```sql
select * from user_passwd where name='xx' or 1=1 -- fagaga' and password=''    
```

* 结合数据库完成一个用户的登录功能

```python
import pymysql


conn = pymysql.connect(
    host = '127.0.0.1',
    port = 3306,
    user = 'root',
    password = '123456',
    database = 'db68',
    charset = 'utf8'  # 编码千万不要加-
)  # 链接数据库

cursor = conn.cursor(cursor=pymysql.cursors.DictCursor)

username = input('>>>:')
password = input('>>>:')
sql = "select * from user where name=%s and password=%s"
# 不要手动拼接数据 先用%s占位 之后将需要拼接的数据直接交给execute方法即可
print(sql)
rows = cursor.execute(sql,(username,password))  # 自动识别sql里面的%s用后面元组里面的数据替换
if rows:
    print('登录成功')
    print(cursor.fetchall())
else:
    print('用户名密码错误')
```

### 7.2 pymysql 补充（增修删）

```python
"""
增删改查中
    增删改 它们的操作涉及到数据的修改
    需要二次确认
    conn.commit()       确认
"""

import pymysql


conn = pymysql.connect(
    host = "127.0.0.1",
    port = 3306,
    user = "root",
    password = "123456",
    database = "db68",
    charset = "utf8",
    autocommit=True     # 改成 True ,后续就不需要 conn.commit()
)


cursor = conn.cursor(pymysql.cursors.DictCursor)

# 增加
sql = "insert into user_passwd(name, password) values(%s, %s)"

rows = cursor.execute(sql, ("李七", "123"))

print(rows)
conn.commit()   # 确认

# 修改
sql = "update user_passwd set name='张三三' where id=1"
rows = cursor.execute(sql)
print(rows)
conn.commit()

# 删除
sql = "delete from user_passwd where id=1"
rows = cursor.execute(sql)
print(rows)
conn.commit()

# 查询
sql = "select * from user_passwd"
rows = cursor.execute(sql)
print(cursor.fetchall())
conn.commit()
```

```python
# 增加多行数据
cursor = conn.cursor(pymysql.cursors.DictCursor)

sql = "insert into user_passwd(name, password) values(%s, %s)"

rows = cursor.executemany(sql, [('123', 123), ('456', '235'), ('231', '231')])
```

## 8 数据库的其它知识

* 视图
* 触发器
* 事务(***)
* 存储过程
* 内置函数
* 流程控制
* 索引理论(**)

### 8.1 视图 

* 什么是视图

​			视图就是通过查询得到一张虚拟表，然后保存下来，下次可以直接使用，其实视图也是表

* 为什么用视图

​			如果要频繁的操作一张虚拟表(拼表组成的)，你就可以制作成视图 后续直接操作

* 如何操作

```sql
create view 表名 as 虚拟表的查询sql语句

# 具体操作
create view teacher2course as
select * from teacher INNER JOIN course
on teacher.tid = course.teacher_id
;
```

* 注意

​		1 创建视图在硬盘上只会有表结构 没有表数据(数据还是来自于之前的表)
​		2 视图一般只用来查询 里面的数据不要继续修改 可能会影响真正的表

### 8.2 触发器

​	在满足对表数据进行增、删、改的情况下，自动触发的功能

​	使用触发器可以帮助我们实现监控、日志...

​	触发器可以在六种情况下自动触发 增前 增后 删前删后 改前改后

* 基本语法结构

```sql
create trigger 触发器的名字  before/after insert/update/delete on 表名
for each row
begin
	sql语句
end

# 具体使用 针对触发器的名字，要做到见名知意
# 针对增
create trigger tri_before_insert_t1  before insert on t1
for each row
begin
	sql语句
end

create trigger tri_after_insert_t1  after insert on t1
for each row
begin
	sql语句
end
"""删除、修改 书写格式同上"""

ps: 修改MySQL默认的语句结束符  只作用于当前窗口
	delimiter $$  将默认的结束符号由 ; 改为 $$
    delimiter ;
```

```sql
# 案例
CREATE TABLE cmd (
    id INT PRIMARY KEY auto_increment,
    USER CHAR (32),
    priv CHAR (10),
    cmd CHAR (64),
    sub_time datetime, 			# 提交时间
    success enum ('yes', 'no') 	# 0代表执行失败
);

CREATE TABLE errlog (
    id INT PRIMARY KEY auto_increment,
    err_cmd CHAR (64),
    err_time datetime
);
"""
当cmd表中的记录succes字段是no那么就触发触发器的执行去errlog表中插入数据
NEW指代的就是一条条数据对象
"""
delimiter $$
create trigger tri_after_insert_cmd after insert on cmd 
for each row
begin
	if NEW.success = 'no' then
    	insert into errlog(err_cmd,err_time) values(NEW.cmd,NEW.sub_time);
    end if;
end $$
delimiter ;

# 朝cmd表插入数据
INSERT INTO cmd (
    USER,
    priv,
    cmd,
    sub_time,
    success
)
VALUES
    ('jason','0755','ls -l /etc',NOW(),'yes'),
    ('jason','0755','cat /etc/passwd',NOW(),'no'),
    ('jason','0755','useradd xxx',NOW(),'no'),
    ('jason','0755','ps aux',NOW(),'yes');

# # 删除触发器
drop trigger tri_after_insert_cmd;
```

### 8.3 事务

* 什么是事务

​		开启一个事务可以包含多条sql语句 这些sql语句要么同时成功
​		要么一个都别想成功 称之为事务的原子性	

* 事务的作用
  保证了对数据操作的安全性
  eg:	还钱的例子
      	egon用银行卡给我的支付宝转账1000
      		1 将egon银行卡账户的数据减1000块
          	2 将jason支付宝账户的数据加1000块
      你在操作多条数据的时候可能会出现某几条操作不成功的情况 

* 事务的四大特性

  ```sql
  ACID
  A:原子性
  	一个事务是一个不可分割的单位，事务中包含的诸多操作
  	要么同时成功要么同时失败
  C:一致性
  	事务必须是使数据库从一个一致性的状态变到另外一个一致性的状态
  	一致性跟原子性是密切相关的
  I:隔离性
  	一个事务的执行不能被其他事务干扰
  	（即一个事务内部的操作及使用到的数据对并发的其他事务是隔离的，并发执行的事务之间也是互相不干扰的）
  D:持久性
  	也叫"永久性"
  	一个事务一旦提交成功执行成功 那么它对数据库中数据的修改应该是永久的
  	接下来的其他操作或者故障不应该对其有任何的影响
  ```

* 如何使用事务

```sql
# 事务相关的关键字

# 1 开启事务
start transaction;

# 2 回滚(回到事务执行之前的状态)
rollback;

# 3 确认(确认之后就无法回滚了)
commit;

"""模拟转账功能"""
create table user(
	id int primary key auto_increment,
    name char(16),
    balance int
);
insert into user(name,balance) values
('jason',1000),
('egon',1000),
('tank',1000);

# 1 先开启事务
start transaction;

# 2 多条sql语句
update user set balance=900 where name='jason';
update user set balance=1010 where name='egon';
update user set balance=1090 where name='tank';

# 3 确认(确认之后就无法回滚了)
commit;

"""
总结
	当你想让多条sql语句保持一致性 要么同时成功要么同时失败 
	你就应该考虑使用事务
"""
```

### 8.4 存储过程

​	存储过程就类似于python中的自定义函数

​	它的内部包含了一系列可以执行的sql语句，存储过程存放于MySQL服务端中，你可以直接通过调用存储过程触发内部sql语句的执行

* 基本使用

```sql
create procedure 存储过程的名字(形参1,形参2,...)
begin
	sql代码
end

# 调用
call 存储过程的名字();
```

**三种开发模型**

```sql
1. 
应用程序:程序员写代码开发
MySQL:提前编写好存储过程，供应用程序调用

好处: 开发效率提升了 执行效率也上去了
缺点: 考虑到认为元素、跨部门沟通的问题  后续的存储过程的扩展性差

2.
应用程序:程序员写代码开发之外 设计到数据库操作也自己动手写

优点: 扩展性很高
缺点: 开发效率降低 编写sql语句太过繁琐 而且后续还需要考虑sql优化的问题
	
3.
应用程序:只写程序代码 不写sql语句 
基于别人写好的操作MySQL的python框架直接调用操作即可			ORM框架  

优点: 开发效率比上面两种情况都要高 
缺点: 语句的扩展性差 可能会出现效率低下的问题 

# 第一种基本不用。一般都是第三种，出现效率问题再动手写sql
```

* 存储过程具体演示

```sql
delimiter $$
create procedure p1(
	in m int,  # 只进不出  m不能返回出去
    in n int,
    out res int  # 该形参可以返回出去
)
begin
	select tname from teacher where tid>m and tid<n;
    set res=666;  # 将res变量修改 用来标识当前的存储过程代码确实执行了
end $$
delimiter ;
```

```sql
# 针对形参res 不能直接传数据 应该传一个变量名

# 定义变量
set @ret = 10;

# 查看变量对应的值
select @ret;
```

* pymysql 中调用存储过程

```python
import pymysql


conn = pymysql.connect(
    host = '127.0.0.1',
    port = 3306,
    user = 'root',
    passwd = '123456',
    db = 'db68',
    charset = 'utf8',
    autocommit = True
)

cursor = conn.cursor(pymysql.cursors.DictCursor)
# 调用存储过程
cursor.callproc('p1', (1, 5, 10))

"""
@_p1_0 = 1
@_p1_1 = 5
@_p1_2 = 10
"""
# print(cursor.fetchall())

cursor.execute('select @_p1_2;')
print(cursor.fetchall())
```

### 8.5 内置函数

```sql
# 跟存储过程是有区别的，存储过程是自定义函数，函数就类似于是内置函数

('jason','0755','ls -l /etc',NOW(),'yes')		# NOW() 内置函数

CREATE TABLE blog (
    id INT PRIMARY KEY auto_increment,
    NAME CHAR (32),
    sub_time datetime
);

INSERT INTO blog (NAME, sub_time)
VALUES
    ('第1篇','2015-03-01 11:31:21'),
    ('第2篇','2015-03-11 16:31:21'),
    ('第3篇','2016-07-01 10:21:31'),
    ('第4篇','2016-07-22 09:23:21'),
    ('第5篇','2016-07-23 10:11:11'),
    ('第6篇','2016-07-25 11:21:31'),
    ('第7篇','2017-03-01 15:33:21'),
    ('第8篇','2017-03-01 17:32:21'),
    ('第9篇','2017-03-01 18:31:21');

select date_format(sub_time,'%Y-%m'),count(id) from blog group by date_format(sub_time,'%Y-%m');

# data_format()		# 内置函数
```

### 8.6 流程控制

```sql
# if判断
delimiter //
CREATE PROCEDURE proc_if ()
BEGIN
    declare i int default 0;
    if i = 1 THEN
        SELECT 1;
    ELSEIF i = 2 THEN
        SELECT 2;
    ELSE
        SELECT 7;
    END IF;
END //
delimiter ;
```

```sql
# while循环
delimiter //
CREATE PROCEDURE proc_while ()
BEGIN
    DECLARE num INT ;
    SET num = 0 ;
    WHILE num < 10 DO
        SELECT
            num ;
        SET num = num + 1 ;
    END WHILE ;
delimiter ;    
```

### 8.7 索引理论

ps: 数据都是存在与硬盘上的，查询数据不可避免的需要进行IO操作

索引: 就是一种数据结构，类似于书的目录。

​		 意味着以后在查询数据的应该先找目录再找数据，而不是一页一页的翻书，从而提升查询速度降低IO操作

索引在MySQL中也叫“键”,是存储引擎用于快速查找记录的一种数据结构

* primary key
* unique key
* index key

注意foreign key不是用来加速查询用的，不在我们的而研究范围之内

上面的三种key，前面两种除了可以增加查询速度之外各自还具有约束条件，而最后一种index key没有任何的约束条件，只是用来帮助你快速查询数据

**本质**

通过不断的缩小想要的数据范围筛选出最终的结果，同时将随机事件(一页一页的翻)

变成顺序事件(先找目录、找数据)

也就是说有了索引机制，我们可以总是用一种固定的方式查找数据



一张表中可以有多个索引(多个目录)

索引虽然能够帮助你加快查询速度但是也有缺点

```sql
1 当表中有大量数据存在的前提下 创建索引速度会很慢
2 在索引创建完毕之后 对表的查询性能会大幅度的提升 但是写的性能也会大幅度的降低

索引不要随意的创建！！！
```

#### b+树

```sql
只有叶子节点存放的是真实的数据 其他节点存放的是虚拟数据 仅仅是用来指路的
树的层级越高查询数据所需要经历的步骤就越多(树有几层查询数据就需要几步)

一个磁盘块存储是有限制的
为什么建议你将id字段作为索引
	占得空间少 一个磁盘块能够存储的数据多
	那么久降低了树的高度 从而减少查询次数
```

#### 聚集索引(primary key)

```sql
聚集索引指的就是主键 
Innodb  只有两个文件  直接将主键存放在了idb表中 
MyIsam  三个文件  单独将索引存在一个文件
```

#### 辅助索引(unique, index)

```sql
查询数据的时候不可能一直使用到主键，也有可能会用到name, password等其他字段
那么这个时候你是没有办法利用聚集索引。这个时候你就可以根据情况给其他字段设置辅助索引(也是一个b+树）

叶子节点存放的是数据对应的主键值
	先按照辅助索引拿到数据的主键值
	之后还是需要去主键的聚集索引里面查询数据                                           
```

#### 覆盖索引

```sql
# 在辅助索引的叶子节点就已经拿到了需要的数据

# 给name设置辅助索引
select name from user where name='jason';

# 非覆盖索引
select age from user where name='jason';
```

#### 测试索引是否有效的代码

```sql
**准备**

# 1. 准备表
create table s1(
id int,
name varchar(20),
gender char(6),
email varchar(50)
);

# 2. 创建存储过程，实现批量插入记录
delimiter $$ 	#声明存储过程的结束符号为$$
create procedure auto_insert1()
BEGIN
    declare i int default 1;
    while(i<3000000)do
        insert into s1 values(i,'jason','male',concat('jason',i,'@oldboy'));
        set i=i+1;
    end while;
END$$ 			# $$结束
delimiter ; 	# 重新声明分号为结束符号

# 3. 查看存储过程
show create procedure auto_insert1\G 

# 4. 调用存储过程
call auto_insert1();
```

```sql
# 表没有任何索引的情况下
select * from s1 where id=30000;

# 避免打印带来的时间损耗
select count(id) from s1 where id = 30000;
select count(id) from s1 where id = 1;

# 给id做一个主键
alter table s1 add primary key(id);  # 速度很慢

select count(id) from s1 where id = 1;  # 速度相较于未建索引之前两者差着数量级
select count(id) from s1 where name = 'jason'  # 速度仍然很慢
```

```sql
"""
范围问题
"""
# 并不是加了索引，以后查询的时候按照这个字段速度就一定快   
select count(id) from s1 where id > 1;  # 速度相较于id = 1慢了很多
select count(id) from s1 where id >1 and id < 3;
select count(id) from s1 where id > 1 and id < 10000;
select count(id) from s1 where id != 3;

alter table s1 drop primary key;  # 删除主键 单独再来研究name字段
select count(id) from s1 where name = 'jason';  # 又慢了

create index idx_name on s1(name);  # 给s1表的name字段创建索引
select count(id) from s1 where name = 'jason'  # 仍然很慢！！！
```

```sql
"""
再来看b+树的原理，数据需要区分度比较高，而我们这张表全是jason，根本无法区分
那这个树其实就建成了“一根棍子”
"""
select count(id) from s1 where name = 'xxx';  
# 这个会很快，我就是一根棍，第一个不匹配直接不需要再往下走了
select count(id) from s1 where name like 'xxx';
select count(id) from s1 where name like 'xxx%';
select count(id) from s1 where name like '%xxx';  # 慢 最左匹配特性

# 区分度低的字段不能建索引
drop index idx_name on s1;

# 给id字段建普通的索引
create index idx_id on s1(id);
select count(id) from s1 where id = 3;  # 快了
select count(id) from s1 where id*12 = 3;  # 慢了  索引的字段一定不要参与计算

drop index idx_id on s1;
select count(id) from s1 where name='jason' and gender = 'male' and id = 3 and email = 'xxx';
# 针对上面这种连续多个and的操作，mysql会从左到右先找区分度比较高的索引字段，先将整体范围降下来再去比较其他条件
create index idx_name on s1(name);
select count(id) from s1 where name='jason' and gender = 'male' and id = 3 and email = 'xxx';  # 并没有加速

drop index idx_name on s1;
# 给name，gender这种区分度不高的字段加上索引并不难加快查询速度

create index idx_id on s1(id);
select count(id) from s1 where name='jason' and gender = 'male' and id = 3 and email = 'xxx';  # 快了  先通过id已经讲数据快速锁定成了一条了
select count(id) from s1 where name='jason' and gender = 'male' and id > 3 and email = 'xxx';  # 慢了  基于id查出来的数据仍然很多，然后还要去比较其他字段

drop index idx_id on s1

create index idx_email on s1(email);
select count(id) from s1 where name='jason' and gender = 'male' and id > 3 and email = 'xxx';  # 快 通过email字段一剑封喉 
```

```sql
#### 联合索引

select count(id) from s1 where name='jason' and gender = 'male' and id > 3 and email = 'xxx';  
# 如果上述四个字段区分度都很高，那给谁建都能加速查询
# 给email加然而不用email字段
select count(id) from s1 where name='jason' and gender = 'male' and id > 3; 
# 给name加然而不用name字段
select count(id) from s1 where gender = 'male' and id > 3; 
# 给gender加然而不用gender字段
select count(id) from s1 where id > 3; 

# 带来的问题是所有的字段都建了索引然而都没有用到，还需要花费四次建立的时间
create index idx_all on s1(email,name,gender,id);  # 最左匹配原则，区分度高的往左放
select count(id) from s1 where name='jason' and gender = 'male' and id > 3 and email = 'xxx';  # 速度变快
```

## 9 数据库相关查询题目

> https://www.cnblogs.com/Dominic-Ji/p/10875493.html
