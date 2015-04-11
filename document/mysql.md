# MySQL

### mysql常用命令

***show databases;*** 显示数据库   

***create database name;*** 创建数据库   

***use databasename;*** 选择数据库 

***drop database name;*** 直接删除数据库，不提醒   

show tables; 显示表 

describe tablename; 显示具体的表结构  

mysqladmin -u root -p drop databasename; 删除数据库前，有提示
 
select version(),current_date; 显示当前mysql版本和当前日期      

修改mysql中root的密码： 

```mysql
    shell>mysql -h localhost -u root -p //登录 
    mysql>use mysql;  // 进入mysql数据库，user表中存放着所有的MYSQL用户信息
    mysql> update user set password=password("xueok654123") where user='root';   
    mysql>describe user; 显示表mysql数据库中user表的列信息);   
```

增加新用户并授予一定权限（格式：grant select on 数据库.* to 用户名@登陆主机 identified by '密码'）：

```mysql
    grant select, insert, update, delete on *.* to shirly@"%" identified by 'shirly' // 增加一个用户shirly密码为shirly，让她可以再任何主机上登陆，并对所有数据库有查询、插入、修改、删除的权限(前提是用root用户连入MYSQL)

    grant select,insert,update,delete on mydb.* to test2@localhost identified by “abc”; //增加一个用户test2密码为abc,让他只可以在localhost上登录，并可以对数据库mydb进行查询、插入、修改、删除的操作（localhost指本地主机，即MYSQL数据库所在的那台主机），这样用户即使有test2的密码，他也无法从internet上直接访问数据库，只能通过MYSQL主机上的web页来访问了。

    grant select,insert,update,delete on mydb.* to test2@localhost identified by “”; //不想test2有密码  
    
    grant all on *.* to 'someuser'@'somehost' identified by 'password';  //mysql默认的是本地主机是localhost,对应的IP地址就是127.0.0.1，所以你用你的IP地址登录会出错，如果你想用你的IP地址登录就要先用grant命令进行授权。

```

重命名表: 
```mysql
  mysql > alter table t1 rename t2;   
```

mysqldump 备份数据库:
```mysql
    shell> mysqldump -h host -u root -p dbname >dbname_backup.sql 
``` 

恢复数据库:
```mysql

``` 
  shell> mysqladmin -h myhost -u root -p create dbname 
  shell> mysqldump -h host -u root -p dbname < dbname_backup.sql   如果只想卸出建表指令，则命令如下： 
  shell> mysqladmin -u root -p -d databasename > a.sql 
  如果只想卸出插入数据的sql命令，而不需要建表命令，则命令如下：   shell> mysqladmin -u root -p -t databasename > a.sql 
  那么如果我只想要数据，而不想要什么sql命令时，应该如何操作呢?   mysqldump -T./ phptest driver 



  <!-- FROM orders 
  Where orderDate>#1/1/96# AND orderDate<#1/30/96#   注意： 
  Mcirosoft JET SQL 中，日期用'#'定界。日期也可以用Datevalue()函数来代替。   在比较字符型的数据时，要加上单引号''，尾空格在比较中被忽略。   例： 
  Where orderDate>#96-1-1#   也可以表示为： 
  Where orderDate>Datevalue('1/1/96')   使用 NOT 表达式求反。 
  例：查看96年1月1日以后的定单   Where Not orderDate<=#1/1/96#   2 范围(BETWEEN 和 NOT BETWEEN) 
  BETWEEN …AND…运算符指定了要搜索的一个闭区间。   例：返回96年1月到96年2月的定单。   Where orderDate Between #1/1/96# And #2/1/96#   3 列表(IN ，NOT IN) 
  IN 运算符用来匹配列表中的任何一个值。IN子句可以代替用OR子句连接的一连串   的条件。 
  例：要找出住在 London、Paris或Berlin的所有客户   Select CustomerID, CompanyName, ContactName, City   FROM Customers 
  Where City In('London',' Paris',' Berlin')   4 模式匹配(LIKE) 
  LIKE运算符检验一个包含字符串数据的字段值是否匹配一指定模式。   LIKE运算符里使用的通配符   通配符 含义 
  ? 任何一个单一的字符   * 任意长度的字符   # 0~9之间的单一数字 
  [字符列表] 在字符列表里的任一值   [!字符列表] 不在字符列表里的任一值   - 指定字符范围，两边的值分别为其上下限 
  例：返回邮政编码在(171)555-0000到(171)555-9999之间的客户   Select CustomerID ,CompanyName,City,Phone   FROM Customers 
  Where Phone Like '(171)555-####' 

  LIKE运算符的一些样式及含义   样式 含义 不符合 
  LIKE 'A*' A后跟任意长度的字符 Bc,c255   LIKE'5[*]' 5*5 555 
  LIKE'5?5' 5与5之间有任意一个字符 55,5wer5   LIKE'5##5' 5235，5005 5kd5,5346   LIKE'[a-z]' a-z间的任意一个字符 5,%   LIKE'[!0-9]' 非0-9间的任意一个字符 0,1   LIKE'[[]' 1,* 
  三 .用ORDER BY子句排序结果 
  orDER子句按一个或多个(最多16个)字段排序查询结果，可以是升序(ASC)也可   以是降序(DESC)，缺省是升序。ORDER子句通常放在SQL语句的最后。   orDER子句中定义了多个字段，则按照字段的先后顺序排序。   例： 
  Select ProductName,UnitPrice, UnitInStock   FROM Products 
  orDER BY UnitInStock DESC , UnitPrice DESC, ProductName 
  orDER BY 子句中可以用字段在选择列表中的位置号代替字段名，可以混合字段名   和位置号。 
  例：下面的语句产生与上列相同的效果。   Select ProductName,UnitPrice, UnitInStock   FROM Products 
  orDER BY 1 DESC , 2 DESC,3   四 .运用连接关系实现多表查询 
  例：找出同一个城市中供应商和客户的名字 
  Select Customers.CompanyName, Suppliers.ComPany.Name   FROM Customers, Suppliers 
  Where Customers.City=Suppliers.City 
  例：找出产品库存量大于同一种产品的定单的数量的产品和定单   Select ProductName,OrderID, UnitInStock, Quantity   FROM Products, [Order Deails] 
  Where Product.productID=[Order Details].ProductID   AND UnitsInStock>Quantity 
  另一种方法是用 Microsof JET SQL 独有的 JNNER JOIN   语法： 
  FROM table1 INNER JOIN table2   ON table1.field1 comparision table2.field2 

  其中comparision 就是前面Where子句用到的比较运算符。   Select FirstName,lastName,OrderID,CustomerID,OrderDate   FROM Employees 
  INNER JOIN orders ON Employees.EmployeeID=Orders.EmployeeID   注意： 
  INNER JOIN不能连接Memo OLE Object Single Double 数据类型字段。   在一个JOIN语句中连接多个ON子句   语法：   Select fields 
  FROM table1 INNER JOIN table2 
  ON table1.field1 compopr table2.field1 AND   ON table1.field2 compopr table2.field2 or   ON table1.field3 compopr table2.field3   也可以   Select fields 
  FROM table1 INNER JOIN   (table2 INNER JOIN [( ]table3   [INNER JOER] [( ]tablex[INNER JOIN]   ON table1.field1 compopr table2.field1   ON table1.field2 compopr table2.field2   ON table1.field3 compopr table2.field3 
  外部连接返回更多记录，在结果中保留不匹配的记录，不管存不存在满足条件的记   录都要返回另一侧的所有记录。   FROM table [LEFT|RIGHT]JOIN table2   ON table1.field1comparision table.field2 
  用左连接来建立外部连接，在表达式的左边的表会显示其所有的数据   例：不管有没有定货量，返回所有商品   Select ProductName ,OrderID   FROM Products 
  LEFT JOIN orders ON Products.PrductsID=Orders.ProductID 
  右连接与左连接的差别在于：不管左侧表里有没有匹配的记录，它都从左侧表中返   回所有记录。 
  例：如果想了解客户的信息，并统计各个地区的客户分布，这时可以用一个右连接   ，即使某个地区没有客户，也要返回客户信息。 
  空值不会相互匹配，可以通过外连接才能测试被连接的某个表的字段是否有空值。   Select *   FROM talbe1

 select 中加上distinct去除重复字段 

```mysql

```

  其中，只有指定了-T参数才可以卸出纯文本文件，表示卸出数据的目录，./表示当前目录，即与mysqldump同一目录。如果不指定driver 表，则将卸出整个数据库的数据。每个表会生成两个文件，一个为.sql文件，包含建表执行。另一个为.txt文件，只包含数据，且没有sql指令。 
  5、可将查询存储在一个文件中并告诉mysql从文件中读取查询而不是等待键盘输入。可利用外壳程序键入重定向实用程序来完成这项工作。例如，如果在文件my_file.sql 中存放有查   询，可如下执行这些查询： 
  例如，如果您想将建表语句提前写在sql.txt中:   mysql > mysql -h myhost -u root -p database < sql.txt   // 启动服务   mysqld --console   // 停止服务 
  mysqladmin -u root shutdown   // 登录后使用数据库 mysql   mysql -u root -p mysql 
  mysql -u root -p -h 11.11.11.11 database   // 创建数据库 
  create database db_name [default character set=gbk]   // 设置数据库默认字符集 
  alter databse db_name default character set gbk   // 更换数据库 use database test after log on   use test 
  // 创建一个带图像字段的表 create a table mypic to store picture   create table mypic (picid int, picname varchar(20), content blob);   // 显示表的结构 describe table mypic   desc mypic 
  // 显示当前表的建表语句   show create table table_name   // 更改表类型 
  alter table table_name engine innodb|myisam|memory   // 插入一条记录 insert a record 
  insert into mypic values (1, '第二章', 0x2134545);   // 显示当前用户 show current user   select user(); 
  // 显示当前用户密码 show current password   select password('root'); 
  // 显示当前日期 show current date   select now(); 


  // 更改用户密码 change user password 
  update user set password=password('xxx') where user='root';   // 分配用户权限 grant 
  grant all privileges on *.* toroot@localhost 
  grant select,insert,delete,update,alter,create,drop on lybbs.* mailto:tolybbs@"" identified by "lybbs"; 
  grant select,insert,delete,update,alter,create,drop on lybbs.* tolybbs@localhostidentified by "lybbs"; 
  // 在不重启的情况下刷新用户权限 flush privileges   flush privileges 
  // 向表中增加一个主键 add primary key   alter table mypic add primary key (picid) 
  // 修改表结构增加一个新的字段 add a new column userid after picid   alter table mypic add column userid int after picid 
  // 更改列类型,当存储图像过大时，使用默认blob超不过100k   alter table userpic change image image longblob;   alter table userpic modify image longblob;   // 设置默认字符集为gb2312 
  mysqld --default-character-set=gb2312   // 显示详细信息，包括字符集编码   show full columns from userpic;   // 改变表的编码 
  Alter TABLE userpic CHARACTER SET gb2312;   // mysql jdbc连接url 使用中文 
  jdbc:mysql://localhost/test?useUnicode=true&characterEncoding=gb2312   // 执行外部脚本   source  
  MySQL是最受欢迎的开源SQL数据库管理系统，由MySQL AB开发、发布和支持。MySQL AB是一家基于MySQL开发人员的商业公司，是一家使用了一种成功的商业模式来结合开源价值和方****的第二代开源公司。MySQL是MySQL AB的注册商标。 
  MySQL是一个快速的、多线程、多用户和健壮的SQL数据库服务器。MySQL服务器支持关键任务、重负载生产系统的使用，也可以将它嵌入到一个大配置(mass-deployed)的软件中去。 -->



### 参考资料

[mysql的安装配置使用教程](http://www.cnblogs.com/mr-wid/archive/2013/05/09/3068229.html)

[mysql and node tutorial in production](http://codeforgeek.com/2015/01/nodejs-mysql-tutorial/)

[优化网站性能之数据库架构篇](http://www.lovelucy.info/website-database-optimization.html)

[《MongoDB实战》试读：第一章：为现代Web而生的数据库](http://book.douban.com/reading/21674153/)

[MySQL索引背后的数据结构及算法原理](http://blog.codinglabs.org/articles/theory-of-mysql-index.html)

[讲述mysql索引和优化的故事](http://database.51cto.com/art/201107/278040.htm)

**mysql读书列表**：

* [《SQL学习指南》](../pdf/SQL学习指南.pdf)
* [《MYSQL性能调优与架构设计》](../pdf/MySQL性能调优与架构设计.pdf)
* [《高可用MYSQL：构建健壮的数据中心》](../pdf/高可用MySQL：构建健壮的数据中心.pdf)
* [《SQL反模式》](../pdf/SQL反模式.pdf)
* [《大话存储-网络存储系统原理精解与最佳实践](../pdf/大话存储-网络存储系统原理精解与最佳实践.pdf)
* [《深入理解MYSQL核心技术》](../pdf/深入理解MySQL核心技术.pdf)
* [《高性能MYSQL》](../pdf/高性能MySQL第三版.pdf)
