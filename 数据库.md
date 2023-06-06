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

