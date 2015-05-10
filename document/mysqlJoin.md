### 交叉连接（笛卡尔积）：
```mysql
mysql> select * from (select 1 num, 'a' str) t1 inner join (select 2 num, 23 age
) t2;
+-----+-----+-----+-----+
| num | str | num | age |
+-----+-----+-----+-----+
|   1 | a   |   2 |  23 |
+-----+-----+-----+-----+
1 row in set (0.01 sec)
```
内连接（基于一定条件进行交叉连接的结果）：
mysql> select * from (select 1 num, 'a' str) t1 inner join (select 2 num, 23 age
) t2 inner join (select 1 num, 'fe' job) t3 on t1.num = t3.num;
+-----+-----+-----+-----+-----+-----+
| num | str | num | age | num | job |
+-----+-----+-----+-----+-----+-----+
|   1 | a   |   2 |  23 |   1 | fe  |
+-----+-----+-----+-----+-----+-----+
1 row in set (0.00 sec)

mysql> select * from (select 1 num, 'a' str) t1 inner join (select 2 num, 23 age
) t2 inner join (select 1 num, 'fe' job) t3 on t1.num = t2.num;
Empty set (0.00 sec)

using的使用场景：
连接两个表的列名是相同的，那么可以只用using替代on子句

mysql> select * from (select 1 num, 'a' str) t1 inner join (select 1 num, 23 age
) t2 using (num);
+-----+-----+-----+
| num | str | age |
+-----+-----+-----+
|   1 | a   |  23 |
+-----+-----+-----+
1 row in set (0.00 sec)

mysql> select * from (select 1 num, 'a' str) t1 inner join (select 1 num, 23 age
) t2 on t1.num=t2.num;
+-----+-----+-----+-----+
| num | str | num | age |
+-----+-----+-----+-----+
|   1 | a   |   1 |  23 |
+-----+-----+-----+-----+
1 row in set (0.00 sec)

自连接：
自连接就是对表自身进行连接，乍看会觉得有些奇怪，但有时还是存在这么做的理由，比如employee表中有一列指向了雇员的主管id(除非该雇员属于领导层，这种情况该列为null)，使用自连接可以列出每个雇员姓名的同时列出主管的姓名，下面模拟下此情况：
mysql> select * from jo;
+----+--------+------+
| id | name   | par  |
+----+--------+------+
|  1 | xueer  | NULL |
|  2 | shirly |    1 |
|  3 | jinger |    1 |
|  4 | yaner  |    2 |
+----+--------+------+
4 rows in set (0.00 sec)

mysql> select * from jo a inner join jo b;
+----+--------+------+----+--------+------+
| id | name   | par  | id | name   | par  |
+----+--------+------+----+--------+------+
|  1 | xueer  | NULL |  1 | xueer  | NULL |
|  2 | shirly |    1 |  1 | xueer  | NULL |
|  3 | jinger |    1 |  1 | xueer  | NULL |
|  4 | yaner  |    2 |  1 | xueer  | NULL |
|  1 | xueer  | NULL |  2 | shirly |    1 |
|  2 | shirly |    1 |  2 | shirly |    1 |
|  3 | jinger |    1 |  2 | shirly |    1 |
|  4 | yaner  |    2 |  2 | shirly |    1 |
|  1 | xueer  | NULL |  3 | jinger |    1 |
|  2 | shirly |    1 |  3 | jinger |    1 |
|  3 | jinger |    1 |  3 | jinger |    1 |
|  4 | yaner  |    2 |  3 | jinger |    1 |
|  1 | xueer  | NULL |  4 | yaner  |    2 |
|  2 | shirly |    1 |  4 | yaner  |    2 |
|  3 | jinger |    1 |  4 | yaner  |    2 |
|  4 | yaner  |    2 |  4 | yaner  |    2 |
+----+--------+------+----+--------+------+
16 rows in set (0.00 sec)

mysql> select * from jo a inner join jo b on a.id=b.par;
+----+--------+------+----+--------+------+
| id | name   | par  | id | name   | par  |
+----+--------+------+----+--------+------+
|  1 | xueer  | NULL |  2 | shirly |    1 |
|  1 | xueer  | NULL |  3 | jinger |    1 |
|  2 | shirly |    1 |  4 | yaner  |    2 |
+----+--------+------+----+--------+------+
3 rows in set (0.01 sec)



mysql的集合操作：
union和union all可连接多个数据库，区别在于union对连接后的集合排序并去除重复项，而union all保留重复项，使用union all得到的最终数据集的行数总是等于所要连接的各集合的行数之和

使用集合连接的表需要有同样的列数据才行：
mysql> select * from xueer;
+------+
| i    |
+------+
|    1 |
+------+
1 row in set (0.00 sec)

mysql> select * from jo;
+----+--------+------+
| id | name   | par  |
+----+--------+------+
|  1 | xueer  | NULL |
|  2 | shirly |    1 |
|  3 | jinger |    1 |
|  4 | yaner  |    2 |
+----+--------+------+
4 rows in set (0.00 sec)

mysql> select * from xueer union all select * from jo;
ERROR 1222 (21000): The used SELECT statements have a different number of column
s
mysql> select 'a' a, 'b' b, i from xueer union all select * from jo;
+---+--------+------+
| a | b      | i    |
+---+--------+------+
| a | b      |    1 |
| 1 | xueer  | NULL |
| 2 | shirly |    1 |
| 3 | jinger |    1 |
| 4 | yaner  |    2 |
+---+--------+------+
5 rows in set (0.00 sec)

注意集合连接的顺序，顺序不同查询结果的列名也不同：
mysql> select * from jo union all select 'a' a, 'b' b, i from xueer;
+----+--------+------+
| id | name   | par  |
+----+--------+------+
| 1  | xueer  | NULL |
| 2  | shirly |    1 |
| 3  | jinger |    1 |
| 4  | yaner  |    2 |
| a  | b      |    1 |
+----+--------+------+
5 rows in set (0.00 sec)

union会去除重复列：
mysql> select * from jo union all select 'a' a, 'b' b, i from xueer union all se
lect 'a' c, 'b' d, i from xueer;
+----+--------+------+
| id | name   | par  |
+----+--------+------+
| 1  | xueer  | NULL |
| 2  | shirly |    1 |
| 3  | jinger |    1 |
| 4  | yaner  |    2 |
| a  | b      |    1 |
| a  | b      |    1 |
+----+--------+------+
6 rows in set (0.00 sec)

mysql> select * from jo union select 'a' a, 'b' b, i from xueer union select 'a'
 c, 'b' d, i from xueer;
+----+--------+------+
| id | name   | par  |
+----+--------+------+
| 1  | xueer  | NULL |
| 2  | shirly |    1 |
| 3  | jinger |    1 |
| 4  | yaner  |    2 |
| a  | b      |    1 |
+----+--------+------+
5 rows in set (0.00 sec)

外连接必须指定是左外连接或者右外连接，并且必须指定连接条件，否则会报错
mysql> select * from jo outer join xueer on jo.id=xueer.i;
ERROR 1064 (42000): You have an error in your SQL syntax; check the manual that
corresponds to your MySQL server version for the right syntax to use near 'outer
 join xueer on jo.id=xueer.i' at line 1

mysql> select * from jo left join xueer;
ERROR 1064 (42000): You have an error in your SQL syntax; check the manual that
corresponds to your MySQL server version for the right syntax to use near '' at
line 1

mysql> select * from jo left join xueer on jo.id=xueer.i;
+----+--------+------+------+
| id | name   | par  | i    |
+----+--------+------+------+
|  1 | xueer  | NULL |    1 |
|  2 | shirly |    1 | NULL |
|  3 | jinger |    1 | NULL |
|  4 | yaner  |    2 | NULL |
+----+--------+------+------+
4 rows in set (0.00 sec)

mysql> select * from jo right join xueer on jo.id=xueer.i;
+------+-------+------+------+
| id   | name  | par  | i    |
+------+-------+------+------+
|    1 | xueer | NULL |    1 |
+------+-------+------+------+
1 row in set (0.00 sec)

mysql> select * from jo a right join jo b on a.id=b.par;
+------+--------+------+----+--------+------+
| id   | name   | par  | id | name   | par  |
+------+--------+------+----+--------+------+
|    1 | xueer  | NULL |  2 | shirly |    1 |
|    1 | xueer  | NULL |  3 | jinger |    1 |
|    2 | shirly |    1 |  4 | yaner  |    2 |
| NULL | NULL   | NULL |  1 | xueer  | NULL |
+------+--------+------+----+--------+------+
4 rows in set (0.00 sec)

mysql> select * from jo a left join jo b on a.id=b.par;
+----+--------+------+------+--------+------+
| id | name   | par  | id   | name   | par  |
+----+--------+------+------+--------+------+
|  1 | xueer  | NULL |    2 | shirly |    1 |
|  1 | xueer  | NULL |    3 | jinger |    1 |
|  2 | shirly |    1 |    4 | yaner  |    2 |
|  3 | jinger |    1 | NULL | NULL   | NULL |
|  4 | yaner  |    2 | NULL | NULL   | NULL |
+----+--------+------+------+--------+------+
5 rows in set (0.00 sec)

交叉连接生成两个表的笛卡尔积：
mysql> select * from jo cross join xueer;
+----+--------+------+------+
| id | name   | par  | i    |
+----+--------+------+------+
|  1 | xueer  | NULL |    1 |
|  2 | shirly |    1 |    1 |
|  3 | jinger |    1 |    1 |
|  4 | yaner  |    2 |    1 |
+----+--------+------+------+
4 rows in set (0.00 sec)

关于 MySQL FULL JOIN 全连接
MySQL 没有提供 SQL 标准中的 FULL JOIN（全连接）：两个表记录都取出，而不管彼此是否有对应记录。要解决此问题，可以使用 UNION 关键字来合并 LEFT JOIN 与 RIGHT JOIN，达到模拟 FULL JOIN 的目的。

实际上，在 MySQL 中（仅限于 MySQL） CROSS JOIN 与 INNER JOIN 的表现是一样的，在不指定 ON 条件得到的结果都是笛卡尔积，反之取得两个表完全匹配的结果。
INNER JOIN 与 CROSS JOIN 可以省略 INNER 或 CROSS 关键字，因此下面的 SQL 效果是一样的：
... FROM table1 INNER JOIN table2
... FROM table1 CROSS JOIN table2
... FROM table1 JOIN table2