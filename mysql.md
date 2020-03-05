# day01

### windows下面安装mysql

- 1.双击安装msi软件

- 2.我接受

- 3.custom

- 4.mysql+  选择64位的把它扔到右边安装框中

- 大部分选择默认，只有在设置密码的时候一定要记住

- 下一步下一步操作即可


### ubuntu下面安装mysql

- 安装：sudo apt-get install mysql-server
  - 需要设置root用户的密码,后面登录的时候需要使用
  - 安装完以后，会自动启动，若没有启动可以手动操作：sudo service  mysql  start
  - ubuntu18.0.4在安装mysql的时候是没有提示输入密码的，密码文件/etc/mysql/debain.cnf
  - 链接数据库：mysql -h localhost  -u root -p 

### 数据库的简介

- 用途：用于存储生活中的一切数据，，如身份证，住宿，票务，网站，银行
- 分类：
  - 关系型数据库：MySQL,oracle,SQLserver  , sqlite...   ....
  - 非关系型数据库：Rides,MongoDB

- 概念：数据库的服务器，数据库，数据表，字段,数据
- SQL:structured query   language,结构化查询语言
- 命令：
  - 数据定义语言（DDL）：创建，删除，修改
  - 数据操作语言（DML）:增， 删， 改
  - 数据查询语言（DQL）:查
  - 数据控制语言（DCL）:授权，取消授权
  - 数据事务语言（DTL）:开启事务，提交事务，回滚

- ### 数据定义语言

- 查看库：show databases;, 会显示连接的数据库的服务器上面的所有的数据库

- 创建数据库：create database test（库名）;,创建一个叫test的数据库

- 删除数据库：drop database goudan(库名)；，删除goudan这个数据库（在使用的时候想好）

- 选择数据库：use test;,选择一个叫test的数据库

  - 查看当前的数据库
    - show tables;   显示数据库里面的所有的表；


- 创建表：create table  user(username varchar(20), password char(32));

- 查看表的结构：desc   user;

- 查看服务器的字符集和存储引擎

  - 存储引擎：innodb,myisam

  - 查看当前的字符集：show variables like "character%";(utf8)       #%后面忽略取前面

  - 查看存储引擎：show variables like "%storage%";(InnoDB)

  - ubuntu 下面的修改配置文件:/etc/mysql/mysql.conf.d/mysqld.cnf

    ```ini
    ....
    [mysqld]
    
    #字符集
    character-set-server = utf8
    #存储引擎
    default-storage-engine = InnoDB
    
    #重启才能生效
    ```


- C查看创建语言

  - 查看库：show create database test;(几乎不用)

  - 查看表的创建语言： show create table user;(几乎不用 )


- 删除表：drop table user;

- 修改表结构（alter）

  - 修改字段类型：alter table user modify useranme varchar(20);
  - 修改字段名字：alter table user change password pwd char(32);
  - 添加新的字段：
    - 默认在末尾添加：alter table user add age int;
    - 在指定的字段后面添加：alter table user add email varchar(64) after pwd;
    - 在开头进行字段添加：alter table user add id  int first;
    - after和first也适用于modify和change


  - 删除指定的字段：alter table user drop age;
  - 修改表的名字:alter table user rename new_user;

注意：在数据库的命令中一定要加英文的半角的分号

### 数据类型

- 整型

  | 类型        | 说明    |
  | ----------- | ------- |
  | tinyint     | 1字节   |
  | smallint    | 2个字节 |
  | mediumint   | 3个字节 |
  | int（常用） | 4个字节 |
  | bigint      | 8个字节 |


- 浮点类型

  | 类型          | 说明                                                         |
  | ------------- | ------------------------------------------------------------ |
  | float（m, d） | 单精度浮点数，4个字节，m表示总位数，d表示的是小数点后面的几位 |
  | double(m, d)  | 双精度浮点数，8个字节，同上                                  |
  | decimal(m, d) | 以字符串类型的形式存储浮点数，多用于金融行业                 |


- 字符串类型

  | 类型    | 说明                           |
  | ------- | ------------------------------ |
  | varchar | 变长的字符串，0-65535字节      |
  | char    | 定长的字符串，0-255字节        |
  | text    | 文本类型比如：博客项目中的博文 |


- 时间类型

  | 类型      | 说明                                            |
  | --------- | ----------------------------------------------- |
  | date      | 日期，格式：“2019-08-12”，3个字节               |
  | time      | 时间：格式：“15:01:37”，3个字节                 |
  | datetime  | 日期时间，格式：“2019-08-12 15:01:37 ”，8个字节 |
  | timestamp | 时间戳，4个字节，1970-现在                      |
  | year      | 年，只占1个字节                                 |


- 字段的修饰

| 类型           | 说明                                        |
| -------------- | ------------------------------------------- |
| unsigned       | 无符号                                      |
| zerofill       | 高位填充0，可以防止负数的出现               |
| default        | 默认值                                      |
| not null       | 不能为空                                    |
| auto_increment | 自动增加1，用于整形的字段，常和主键一起使用 |

- 字符集：

  - 查看系统的字符集：show character set;


- 存储引擎

  - 查看系统支持的引擎：show engines;

  - 常用的存储引擎有：

    | 存储引擎 | 说明                                |
    | -------- | ----------------------------------- |
    | InnoDB   | 适合多写的操作 ，支持的事务的(常用) |
    | MyISAM   | 适合多读的操作                      |


- 索引管理（重要）

  - 说明：简单理解就是一本书的目录，可以提高读取效率，但是也不是越多越好

  - 分类：7

    | 索引     | 说明                                                         |
    | -------- | ------------------------------------------------------------ |
    | 普通索引 | index，最基本的索引                                          |
    | 唯一索引 | unique,修饰的字段不能重复                                    |
    | 主键索引 | primary key,是一个特殊的唯一索引，，一张表只能有一个主键（很常用） |
    | 全文索引 | fulltext，对全局的数据添加索引（很少用）                     |


  - 添加索引：alter table new_user add index(email);

  - 删除索引：alter table new_user drop index email;

  - 创建表的时候，最标准写法：

    ```mysql
    create table user(
    	id int auto_increment,
    	name varchar(20),
    	primary key(id),
    	unique(name)
    	)engine=InnoDB default charset=UTF8;
    ```


退出mysql: Ctrl+Z



# day02

### 数据操作语言（DML）Data Manipulation Language

- 说明：在大多数操作数据库中都是增删改

- 准备：一张用于测试的表

  ```sql
   create table star(
   id int auto_increment,
   name varchar(20) not null,
   money float not null,
   province varchar(20) default null,
   age tinyint unsigned not null,
   sex tinyint not null,
   primary key(id)
   )engine=innodb default charset=utf8;
  ```


- 插入数据

  - 方式1：不指定字段,添加数据的时候需要指定所有的字段值，

    ```sql
    insert into star values(1,'蒋龙',2556783,'湖南',22,0)
    ```

    [^可以一次性插入多个，每条数据都必须使用()扩起来，使用之间使用逗号隔开]: 

    [^]: 

  - 方式2：指定字段，只需要传递指定字段值，通常使用这个方式，如：

    ```sql
    insert into star (name, money, age, sex, province)
    values("周杰伦", 78278278, 42, 0, "台湾"),
    ("胡歌", 72636633,39,0,"上海");
    ```

    注释：插入数据字段的顺序，与前面指定一定要一致，与数据库里面的字顺序无关。

  - 说明：插入数据不用传值的字段

    - 自增的字段
    - 有默认值
    - 可以为空

- 修改数据：

  - 示例：update star set money=888 where id=3;
  - 警告：修改的时候，一定不要忘了添加条件，否则后果自负

- 删除数据：

  - 示例：delete from star where name="胡歌";
  - 警告：删除操作一定不要了忘了加条件，后果很严重
  - 说明：在真实的项目中，不会真的删除，大多使用逻辑删除

### 数据查询语言（DQL）

- 基本查询：select * from star;

- 指定字段查询：select name, money  from star;

- 过滤重复的记录：select distinct province from star;

  - 说明：使用distinct 指定的字段不能重复，指定多个字段

- 条件查询：

  - 条件

    | 条件                   | 说明                    |
    | ---------------------- | ----------------------- |
    | >                      | 大于                    |
    | <                      | 小于                    |
    | >=                     | 大于等于                |
    | <=                     | 小于等于                |
    | =                      | 等于                    |
    | ！=或 <>               | 不等于                  |
    | and                    | 并且                    |
    | or                     | 或者                    |
    | [not]  between m and n | [不]在[m,n]的区间       |
    | [not] in ()            | [不]在指定的集合中      |
    | [not] like 条件        | 模糊匹配，%表示任意字符 |
    | is  [not]  NULL        | 是否为空                |

  - 示例：

    ```sql
    select * from star where id > 2;
    select * from star where id >5 and age >30;
    select * from star where age between 30 and 40;
    select * from star where id in (2,4,6);
    select * from star where province like "湖%";
    select * from star where province is null;
    ```

- 结果集排序（order by）

  ```sql
  select name, money from star order by money desc;
  select name, money, age from star order by age asc,money desc;
  ```

  - 说明:
    - 默认是升序asc，降序desc
    - 多个字段进行排序的时候，先安排第一个排序，相同的话再按照第二个进行排序

- 限制结果集（limit）

  ```sql
   select * from star limit 3;#提取3条数据
   select * from star limit 3 offset 2;#跳过两条提取3条数据
   select * from star limit 2,3;#同上
  ```

  - 分页：

    ```sql
    每页 pagesize=10,page表示页码数，请写出对应的查询语句
    第一页：limit 0,10
    d第二页：limit 10,10
    第三页：limit 20,10
    第4页：limit 30,10

    page页：limit (page - 1)*pagesize,pagesize
    ```

- 常用的聚合函数

  | 函数  | 说明     |
  | ----- | -------- |
  | count | 统计个数 |
  | sum   | 求和     |
  | max   | 最大值   |
  | min   | 最小值   |
  | avg   | 平均值   |
  |       |          |

  - 示例：

    ```
    select count(*)  as c from star;    #可以给查询的字段起别名 用as 来起
    select max(money) as max_money from star;
    ```

- 分组及过滤（重要）（group  by）

  ```sql
  select sex from star group by sex;    #以性别进行分组
  select count(*) as c, sex from star group by sex;#分组以后并统计
  select count(*) as c, province from star group by province having c=1;#分组统计以后过滤

  ```

### - ### 多表联合查询,,-

```
https://blog.csdn.net/huaxiawudi/article/details/82288044      作业     
```

- 创建两张表

  ```sql
  create table user (
  id int(11) not null auto_increment,
  username varchar(20) default null,
  gid int(11) ,
  primary key(id));


  create table goods (
  id int(11) not null auto_increment,
  name varchar(40) default null,
  price float default null,
  category varchar(20) default null,
  primary key(id));
  ```

- 隐式内连接

  - 说明：查询哪个用户购买了什么商品
  - 示例： select username,name from user,goods where user.gid =goods.id;

- 显示内连接

  - 说明：功能和隐式内连接一样，使用的关键字是join，一定要注意不是where，是on。
  - 示例：select username,name from user join goods on user.gid=goods.id;

- 左外连接

  - 说明：以左边的表为主，主要显示左边的表，右边的表有对应的数据就显示，没有话就显示null
  - 示例：select username,name from user left join goods on user.gid=goods.id;

- 右外连接

  - 说明：会显示右边的所有数据，左表有对应的数据的话就显示，没有的话就是null
  - 示例：select username,name from user right join goods on user.gid=goods.id;

- 记录联合

  - 格式：select 语句1 union select 语句2

  - 示例：select username,name from user right join goods on user.gid=goods.id union

    select username,name from user left join goods on user.gid=goods.id;

  - union all:将两边的查询结果直接拼接到一起

  - union:去重之后进行拼接


- 联合更新
  - 示例： update user u, goods g set u.gid=3,g.price=g.price+0.1 where u.gid=g.id and u.id=2;
- 子（嵌套）查询(面试考平时用的少)
  - select * from user where gid in (select id from goods);
  - 说明：条件是一个sql语句

### 数据事务语言（DTL）

- 说明：用于测试的表他的存储引擎必须是InnoDB

- K开启事务：禁止自动提交

  ```sql
  set autocommit = 0;

  ```

  sql

- 提交事务：整个事务过程没有出现问题

  ```sql
  commit;
  ```

- 操作回滚：事务出现了问题，然后进行回滚，回事务开始之前的状态

  ```sql
  rollback;
  ```

- **代码**

  ```
  #开启事务
  try:
  #执行相关的操作
  except  Exception as e:
  #操作回滚
  else:
  #提交事务务
  ```

#### 拓展:1,在cmd中进入Msql软件

mysql -u root -p



# day03

### 数据控制语言（DCL）

- 查看授权
  - 格式：show grants [for 'user'@'localhost']
  - 示例：show grants [for 'root'@'localhost']
  - 说明：查看指定的用户权限，若不指定用户，则查看的是当前登录的用户
- 创建用户
  - 格式：create user 'user'@'host' identified by 'password'
  - 示例：create user 'user'@'10.20.159.%'  identified by '123456'
  - 说明：%是通配符
- 用户授权
  - 格式：grant   权限  privilages  on 库.表 to "user"@'host' identified by 'password'
  - 示例： grant all privileges on test.user to 'user'@'10.20.159.%' identified by '123456‘
  - 说明：
    - 权限可以是：insert,delete,update,select, all是所有的权限
    - %表示的是任意主机
    - *表示的是所有的库和表
- 刷新权限：flush privileges
- 取消权限
  - 格式：revoke  权限 privileges on 库.表 from 'user'@'host'
  - 示例：revoke all privileges on test.* from 'user'@'10.20.159.%';


- 删除用户
  - 格式：drop  user 'user'@'localhost'
  - 示例：drop user 'user'@'10.20.159.%';

### 备份和恢复

首先要在mysql的bin目录下打开cmd窗口在窗口里输入下面代码.(自己电脑MySQL路径:D:\PHPTutorial\MySQL\bin)

- 备份：

  - 将数据库中的数据保存到文件中
  - 示例：mysqldump   -u root -p test >路径\ test.sql
  - navicate:右键->转储sql文件的

- 恢复：

  - 从保存的sql、文件中，解析执行sql语句

  - 示例：mysql -uroot -p test2 < 路径\test.sql

  - 重点:要记住恢复时test2这个库要先创建好

    就是练习

  cmd控制台：
  先进入mysql所在的bin目录下，如：cd C:\Program Files\MySQL\MySQL Server 5.5\bin
  mysqldump -u root -p 数据库 [表名1 表名2..]  > 文件路径
  比如: 把datacenter数据库备份到 c:\datacenter.sql
  mysqldump -u root -p datacenter> c:\datacenter.sql
  如果你希望备份是，数据库的某几张表
  mysqldump -uroot -p datacenter user > a.sql

### python操作mysql数据库

```python
import pymysql
#pip install pymysql  ()

#1.连接数据库服务器   mysql -h localhost -u root -p 123456
db = pymysql.connect(host="localhost", user="root", password="123456")
print(db)

#2.选择数据库 use test

db.select_db("test")
#3。设置字符集
db.set_charset("utf8")

#3.1创建游标对象，
cursor = db.cursor(cursor=pymysql.cursors.DictCursor)
#4.准备sql语句
sql = 'select id,name,age from star'
#5.执行sql语句
rowcount = cursor.execute(sql)
#结果集中的数量
print(rowcount)
print(cursor.rowcount)
print(cursor.rownumber)
#获取所有的数据(默认的是元祖)
#print(cursor.fetchall())

#获取一条数据
#print(cursor.fetchone())
print(cursor.fetchmany(1))


#关闭游标对象
cursor.close()
#关闭数据库的链接
db.close()
```

- 事务代码

  ```python
  import pymysql

  db = pymysql.connect(host="localhost", user="root", password="123456")

  db.select_db("test")

  db.set_charset("utf8")

  cursor = db.cursor()

  #开启事务
  db.begin()
  try:
      sql = "insert into user (username) values('goudan')"
      #对于DML:返回一个受影响的行数
      rowcount = cursor.execute(sql)
  except Exception as e:
      print(e)
      #操作回滚
      db.rollback()

  else:
      #开始真正的提交事务
      db.commit()
      print(rowcount)
      #最后插入数据的自增的id
      print(cursor.lastrowid)

  cursor.close()
  db.close()
  ```

### Redis简介

- 百科：Redis是一个开源的使用ANSI [C语言](https://baike.baidu.com/item/C%E8%AF%AD%E8%A8%80)编写、支持网络、可基于内存亦可持久化的日志型、Key-Value[数据库](https://baike.baidu.com/item/%E6%95%B0%E6%8D%AE%E5%BA%93/103728)，并提供多种语言的API。从2010年3月15日起，Redis的开发工作由VMware主持。从2013年5月开始，Redis的开发由Pivotal赞助。 
- 说明：是一个非关系型数据库，经常会用作缓存，消息中间件的操作,session的存储.
- 网址：www.redis.cn   中文网站
- 优势（面试）
  - 存储既可以在内存中，也可以持久化存储
  - 数据类型非常丰富，字符串，哈希，列表,集合，有序集合等
- 端口：6379        MYSQL 3306     HTTP:80 HTTPS:443  SSH:22  FTP:21

### redis 安装

- 命令安装：sudo apt-get install redis-server,版本比较低，客户端没有提示信息

- 源码安装：可以选择哪一个版本

  ```
  https://www.cnblogs.com/mayyan/p/7717994.html

  ```

- windows下面启动redis

  ```
  redis-server redis.windows.conf#启动redis服务器
  ```

- 使用

  ```
  redis-cli.exe#启动客户端
  ```


- 给大家推荐一个可视化工具

  ```
  redisplus
  ```

### redis常用命令

- 管理命令

  | 命令      | 说明                                     |
  | --------- | ---------------------------------------- |
  | ping      | 测试连接情况，默认回复一个pong           |
  | exit/quit | 退出客户端                               |
  | auth      | 身份认证                                 |
  | config    | 配置命令，用来查看选项或者设置相关配置的 |
  | info      | 查看redis服务器相关信息                  |
  | command   | 查看可用的命令                           |
  | select    | 选择库0-15,默认的是在0数据库下面         |
  | flushdb   | 清空当前的库，慎用                       |
  | flushall  | 清空所有的库，特别慎用                   |
  | save      | 前台执行持久化的时候使用(会阻塞)         |
  | bgsave    | 后台执行持久化操作（不会阻塞）           |
  |           |                                          |

- keys（键）

  | 命令    | 说明                                                         |
  | ------- | ------------------------------------------------------------ |
  | exists  | 判断指定的键是否存在，如果存在返回1，不存在返回0             |
  | keys    | 查看指定格式的键，*表示模糊匹配                              |
  | del     | 删除指定的键                                                 |
  | ttl     | 查看指定的键剩余的有效时间，-1表示永久有效，-2表示键不存在，单位为秒 |
  | expire  | 设置指定的有效时间，单位是秒（重要）                         |
  | persist | 删除指定键的有效时间，之后变成永久的                         |
  | move    | 将指定的键移动到指定的库                                     |
  | rename  | 修改指定键的名字                                             |

- 字符串

  | 命令   | 说明                                 |
  | ------ | ------------------------------------ |
  | set    | 设置键值对的数据                     |
  | get    | 通过键值获取值                       |
  | mset   | 设置多对的键值对                     |
  | mget   | 获取多对的键值对                     |
  | getset | 设置键值对，并返回原来的值           |
  | setex  | 设置键并设置过期时间                 |
  | append | 键值对追加内容，如果没有的话就设置值 |
  | strlen | 返回指定字符串的长度                 |
  | incr   | 数字值加1                            |
  | decr   | 数字值减1                            |
  | incrby | 数字值加上一个指定的数               |
  | decrby | 数字值减去一个指定的数               |





### mysql想要在windows黑打开需要设置环境变量

- 1.要找到mysql.exe的应用程序（也就是安装好的mysql），复制这个应用程序的路径

  例如（C:\Program Files\MySQL\MySQL Server 5.7\bin）

- 2.右键-》系统属性-》高级系统设置-》环境变量-》系统变量-》path在后面追加上面复制的路径

- 3.重新打开终端即可。

### 数据的结构的说明

- 结构化数据：就是表数据
- 半结构化数据：xml和json(键值对形式的)
- xml(用于微信小程序) 
- 非结构化数据：图片，音频，视频





# day05

git init    #初始化仓库

git add *  #向仓库中添加你要上传的文件

git remote add origin https://github.com/wangboyaqian/wb.git  #链接远程仓库

git push -u origin master #推送到本地的分支

git clone https://github.com/wangboyaqian/wb  克隆你的远程主机上面的项目



https://www.liaoxuefeng.com/wiki/896043488029600/896202815778784

git分支操作

git checkout -b dev

# 创建一个叫dev的分支，git checkout命令加上 -b参数，表示创建并切换

git branch 

# 查看当前的分支，会列出来所有的分支， 当前分支前面会有*号

# 切换到dev分支

# 在dev分支开始提交咱们的文件

git add *
git commit -m "笔记"

# 现在，dev分支的工作已经做完了， 切换到master分支

git checkout master

git merge dev

# 将dev合并到master分支上面

# 合并完成以后就可以放心的删除

git branch -d dev

# 查看有几个分支

git branch

# 然后进行提交

git push -u  origin master 

冲突的解决
场景：一个写了一个settings.py文件   内容  1111
通过自己的分支commit
第二个人 修改了settings.py文件  22222
通过自己的分支提交commmit（这个会出现问题）冲突了

git checkout -b bug #创建一个bug的分支
git add 1.txt#添加了一个文件
git commit -m "bssbn"#提交

git checkout master #回到主分支
git add 1.txt#添加了一个文件(修改之后的文件)
git commit -m "dshdjsjk"#提交
git merge bug #出错了

手动解决
git add 1.txt#添加了一个文件
git commit -m "ssnn"
git push -u origin master