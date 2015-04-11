# MySQL

### mysql的相关概念

实体：数据库用户所关注的对象，如顾客、部门、地理位置等

表: 行的集合，既可以保存在内存中（未持久化），也可以保存在存储设备中（已持久化）

结果集： 未持久化表的另一个名字，一般为SQL查询结果

#### 字符型数据
    字符型数据可以使用定长或变长的字符串来实现，不同点在于固定长度的字符串使用空格向右填充，以保证同样的字节数；变长字符串不需要向右填充，并且使用字节数可变，定义字符列时，必须指定该列能存放字符串的最大长度，例如：

```
    char(20)
    varchar(20)
```

### mysql常用命令

调用命令行工具，同时指定用户名和要使用的数据库:

```
    shell> mysql -u username -p databasename
```

查看当前日期和时间:

```
    mysql> select now();
```

退出mysql命令行可以使用quit或exit

```
    mysql> exit;
```

show databases; 显示数据库   

create database name; 创建数据库   

use databasename; 选择数据库 

drop database name; 直接删除数据库，不提醒   

show tables; 显示表 

describe tablename; 显示具体的表结构  

mysqladmin -u root -p drop databasename; 删除数据库前，有提示

select version(),current_date; 显示当前mysql版本和当前日期      

修改mysql中root的密码： 

```javascript
    shell> mysql -h localhost -u root -p //登录 
    mysql> use mysql;  // 进入mysql数据库，user表中存放着所有的MYSQL用户信息
    mysql> update user set password=password("xueok654123") where user='root';   
    mysql> describe user; 显示表mysql数据库中user表的列信息);   
```

增加新用户并授予一定权限（格式：grant select on 数据库.* to 用户名@登陆主机 identified by '密码'）：

```javascript
    grant select, insert, update, delete on *.* to shirly@"%" identified by 'shirly' // 增加一个用户shirly密码为shirly，让她可以再任何主机上登陆，并对所有数据库有查询、插入、修改、删除的权限(前提是用root用户连入MYSQL)

    grant select,insert,update,delete on mydb.* to test2@localhost identified by “abc”; //增加一个用户test2密码为abc,让他只可以在localhost上登录，并可以对数据库mydb进行查询、插入、修改、删除的操作（localhost指本地主机，即MYSQL数据库所在的那台主机），这样用户即使有test2的密码，他也无法从internet上直接访问数据库，只能通过MYSQL主机上的web页来访问了。

    grant select,insert,update,delete on mydb.* to test2@localhost identified by “”; //不想test2有密码  
    
    grant all on *.* to 'someuser'@'somehost' identified by 'password';  //mysql默认的是本地主机是localhost,对应的IP地址就是127.0.0.1，所以你用你的IP地址登录会出错，如果你想用你的IP地址登录就要先用grant命令进行授权。

```

重命名表:
 
```javascript
   mysql> alter table t1 rename t2;   
```

mysqldump 备份数据库:

```javascript
    shell> mysqldump -h host -u root -p dbname >dbname_backup.sql 
```

恢复数据库:

```javascript
    shell> mysqladmin -h myhost -u root -p create dbname 
    shell> mysqldump -h host -u root -p dbname < dbname_backup.sql  
```

### 参考资料

* [mysql的安装配置使用教程](http://www.cnblogs.com/mr-wid/archive/2013/05/09/3068229.html)

* [mysql and node tutorial in production](http://codeforgeek.com/2015/01/nodejs-mysql-tutorial/)

* [优化网站性能之数据库架构篇](http://www.lovelucy.info/website-database-optimization.html)

* [《MongoDB实战》试读：第一章：为现代Web而生的数据库](http://book.douban.com/reading/21674153/)

* [MySQL索引背后的数据结构及算法原理](http://blog.codinglabs.org/articles/theory-of-mysql-index.html)

* [讲述mysql索引和优化的故事](http://database.51cto.com/art/201107/278040.htm)

**mysql读书列表**：

* [《SQL学习指南》](../pdf/SQL学习指南.pdf)
* [《MYSQL性能调优与架构设计》](../pdf/MySQL性能调优与架构设计.pdf)
* [《高可用MYSQL：构建健壮的数据中心》](../pdf/高可用MySQL：构建健壮的数据中心.pdf)
* [《SQL反模式》](../pdf/SQL反模式.pdf)
* [《大话存储-网络存储系统原理精解与最佳实践](../pdf/大话存储-网络存储系统原理精解与最佳实践.pdf)
* [《深入理解MYSQL核心技术》](../pdf/深入理解MySQL核心技术.pdf)
* [《高性能MYSQL》](../pdf/高性能MySQL第三版.pdf)
