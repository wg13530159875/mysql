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