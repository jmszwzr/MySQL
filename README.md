# MySQL 学习

## 一、数据库概述

### 1、关于数据库的基本概念

#### 数据库管理技术的发展阶段

>1. 人工管理阶段
>2. 文件系统阶段
>3. 数据库系统阶段

#### 数据库系统阶段涉及的概念

> 1. 数据库（DataBase，DB）是指长期保存在计算机的存储设备上，按照一定规则组织起来，可以被各种用户或应用共享的数据集合。
> 2. 数据库管理系统（DataBase Management System，DBMS）是一种操作和管理数据库的大型软件， 于建立、使用和维护数据库，对数据库进行统一管理和控制，以保证数据库的安全性和完整性。用户通过数据库管理系统访问数据库中的数据。当前比较流行的和常用的数据库管理系统有 Oracle、MySQL、SQL Server和DB2等。
> 3. 数据库系统（DataBase System，DBS）是指在计算机系统中引入数据库后的系统，通常由计算机硬件、软件、数据库管理系统和数据管理员组成。

#### 数据库技术经历的阶段

> 在数据库系统管理数据阶段，随着时间的推移，又经历了三个计数阶段，分别为：层次和网状数据库技术阶段。关系数据库技术阶段和后关系数据库技术阶段。

#### 数据库管理系统提供的功能

>1. 数据定义语言（Data Definition Language，DDL）：数据库管理系统提供了数据定义语言定义数据库涉及各种对象，定义数据的完整性约束、保密限制等约束。
>2. 数据操作语言（Data Manipulation Language，DML）：数据库管理系统提供了数据操作语言实现对数据的操作。基本的数据操作有两类：检索（查询）和更新（插入、删除和更新）。
>3. 数据控制语言（Data Control Language，DCL）：数据库管理系统提供了数据控制语言实现对数据库的控制，包含数据完整性控制。数据安全性控制和数据库的恢复等。

### 2、MySQL数据库管理系统

#### MySQL与开源文化

> 开源项目有：操作系统Linux、Apache服务器、Perl程序语言、MySQL数据库、Mozilla浏览器等

#### MySQL发展历史

> MySQL是一款免费开源、小型、关系型数据库管理系统。随着该数据库功能的不断完善、性能的不断提高，可靠性不断增强。

- 2000年4月，MySQL对旧的存储引擎进行了整理，命名为MyISAM。
- 2001年，支持事务处理和行级锁存储引擎InnoDB被集成到MySQL发行版中，该版本集成了MyISAM与InnoDB存储引擎，MySQL与InnoDB的正式结合版是4.0.
- 2004年10月，发布了经典的4.1版本。
- 2005年10月，发布了里程碑的一个版本，MySQL 5.0，在5.0中加入了游标，存储过程，触发器，视图和事务的支持。在5.0之后的版本里，MySQL明确的表现出迈出高性能数据库的发展步伐。
- MySQL公司与2008年1月16日被SUN公司收购，而在2009年SUN又被Oracle收购。

#### 为什么使用MySQL数据库

> 在许多数据库管理系统提供的功能特性，只有40%的功能被使用。而MySQL在性能与标准的取舍上，一直坚持性能优先的原则，成为了互联网行业非常流行的数据库软件之一。

## 二、MySQL安装与下载

### 1、首先确定MySQL数据文件存储目录及启动时监听的端口

```
// 存储目录
C:\MySQL\dbdata3307\conf\my.ini		#配置文件
C:\MySQL\dbdata3307					#数据文件

// my.ini
[mysqld]
port=3307
datadir=c:\mysql\dbdata3307\
```

### 2、启动

#### 命令行启动

```
// 3306默认启动
"D:\Program Files\MySQL Server 5.6\mysql-5.6.14-winx64\bin\mysqld" MySQL
// 自定义3307启动my.ini
mysqld --defaults-file="C:\MySQL\dbdata3307\conf\my.ini"
```

> - 查看`--defaults-file`命令： `mysqld --verbose --help > C:\MySQL\dbdata3307\conf\help.txt`
>   - `mysqld --help`	#查看MySQL版本的简略信息
>   - mysqld --verbose --help`		#查看详细信息
>   - mysql --help
> - 缺少的mysql文件夹从系统安装mysql的路径下\dbdata3307\下复制
> - 随时随地查看错误日志
> - 查看指定端口`netstat -an | findstr "3307"`

#### 脚本启动

```
// c:\mysql\dbdata3307\bin\start.bat
start mysqld --defaults-file="C:\MySQL\dbdata3307\conf\my.ini"
```



### 3、停止

```
mysqladmin -uroot -P3307 shutdown
```

## 三、MySQL数据库基本操作

> 帮助文档使用

### 命令行下的操作

#### 1、创建数据库

> mysql -uroot -p`报错`ERROR 1045 (28000): Access denied for user 'root'@'localhost' (using password: YES)`：`mysql -u root`

```
show grants; 	#查看当前用户权限
create database testdb;	#创建testdb数据库
create database if not exists testdb ;	#如果没有testdb数据库，则创建
create database if not exists testdb DEFAULT CHARACTER SET utf8;	#默认字符集为utf8
show warnings \G;	#规范化显示警告信息
show create database testdb;	#可以查看到已创建的数据库所属字符集
alter database testdb DEFAULT CHARACTER SET utf8;	#修改数据库的字符集

?	#查看mysql命令
help alter database;	#查看alter database详细的帮助说明
help show databases;	#查看show database详细的帮助说明
```

####  2、查看数据库 

```
show databases;
```

#### 3、选择数据库

```
help use;	#查看use的帮助说明
use mysql;	#选择数据库
select * from user;	#查看数据库中的表
select * from mysql.user;	#在任意数据库下查询另一个数据库中的表
```

#### 4、删除数据库

```
help drop database;	#查看drop database的帮助说明
drop database testdb;	#删除数据库，不可恢复
```

### 客户端工具下的数据库操作

#### 1、 创建数据库

#### 2、查看数据库

#### 3、选择数据库

#### 4、删除数据库



















































