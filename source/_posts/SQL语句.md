---
title: 'SQL语句'
date: 2019-03-17 13:52:09
tags:
	- 数据库
	- SQL语句
categories: 
	- 数据库
	- SQL语句
---

# ●基础

1.连接数据库

```mysql
mysql -uroot -p
```

然后输入密码

2。创建“bjpowernode”数据库

```
mysql> create database bjpowernode;
```

3.选择数据库

```
mysql> use bjpowernode
```

4.导入数据

```
mysql>source  D:\ bjpowernode.sql
```



5. 删除数据库

```
mysql> drop database bjpowernode;
```



# **●** **常用SQL语句（DDL,DML,DCL,TCL）**

数据查询语言(DQL-Data Query Language) 

代表关键字:select

数据操纵语言(DML-Data Manipulation Language)   **增删改，针对表的数据**

代表关键字:insert,delete,update

数据定义语言(DDL-Data Definition Language)  **增删改，针对表的结构**

代表关键字:create ,drop,alter,

事务控制语言(TCL-Transactional Control Language)    **提交，回滚**

代表关键字:commit ,rollback;

数据控制语言(DCL-Data Control Language)    

代表关键字:grant,revoke.

## **●数据定义语句(create、drop、alter)**

**1.定义基本表**

SQL 语言使用CREATETABLE语句定义基本表，其基本格式如下：

{%asset_img 01.png%}

建立一个“学生信息”表Student：

{%asset_img 02.png%}

**2.修改基本表**

随着应用环境和应用需求的变化，有时需要修改已建立好的基本表，SQL 语言用ALTER TABLE语句修改基本表，其一般格式为：

{%asset_img 03.png%}

其中，<表名>是要修改的表，**ADD子句用于增加新列和新的完整性约束条件**，DROP子句用于删除指定的完整性约束条件，**MODIFY COLUMN子句用于修改原有的列定义，包括修改列名和数据类型。**

**例1：**向Student表增加“入学时间”列，其数据类型为日期型。

```sql
ALTER TABLE Student ADD S entrance DATE； 
```

上述代码不论基本表中原来是否已有数据，新增加的列一律为空值。

**例2：**要求将年龄的数据类型由字符型（假设原来的数据类型是字符型）改为整数。

```sql
ALTER TABLE Student MODIFY COLUMN Sage INT； 
```

**例3：**增加Student表Sname必须取唯一值的约束条件：

```sql
ALTER TABLE Student ADD UNIQUE（Sname）； 
```

**3.删除基本表**

当某个基本表不再需要时，可以使用DROPTABLE语句删除它。其一般格式为：

```sql
DROP TABLE<表名> [RESTRICT ICASCADE]； 
```

若选择RESTRICT，则该表的删除是有限制条件的：欲删除的基本表不能被其他表的约束所引用（如check，foreign key等约束），不能有视图，不能有触发器，不能有存储过程或函数等。如果存在这些依赖该表的对象，则此表不能被删除。

若选择CASCADE，则该表的删除没有限制条件。在删除该表的同时，相关的依赖对象，例如视图，都将被一起删除。删除Student表：

```sql
DROP TABLE Student CASCADE； 
```



## **●数据查询(select)**

一个完整的select语句格式如下:

```sql
select      #字段
from       #表名
where     # …….
group by   #……..
having     #……. (就是为了过滤分组后的数据而存在的—不可以单独的出现)
order by   #……..
```

以上语句的执行顺序

1. **首先执行where语句过滤原始数据**

2. **执行group by进行分组**

3. **执行having对分组数据进行操作**

4. **执行select选出数据**

5. **执行order by排序** 

原则：能在where中过滤的数据，尽量在where中过滤，效率较高。having的过滤是专门对分组之后的数据进行过滤的。

**1.简单查询**

假设在表Student（1.2.1节中已定义）中，查询名为BilGates的学生信息，你可以使用下面的查询：

```sql
SELECT*from Student WHERE Sname='Bill Gates'； 
```

假设在表Student中，查询名字中有Bill的学生信息，你可以使用下面的查询：

```sql
SELECT*from Student WHERE Sname like'$Bill%'； 
```

上述%是通配符，代表任意长度（可为0）的字符串。例如a%b表示以a开头，以b结尾的任意长度的字符串。除此之外，（下画线）代表任意单词字符。

假设在表Student中查询年龄在20~23岁（包括20岁与23岁）之间的学生的信息：

```sql
SELECT*FROM Student WHERE Sage BETWEEN 20 AND 23； 
```

与BETWEEN…AND…相对的谓词是NOT BETWEEN·…AND…。

假设在表Student中查询计算机系（CS）、信息系（MA）和数学系（IS）学生的姓名和性别：

```sql
SELECT Sname，Ssex FROM Student WHERE Sdept IN（'CS'，'IS'，'MA'）；
```

与IN相对的谓词是NOTIN，用于查找属性值不属于指定集合的元组。

假设在表Student中查询没有年龄信息的学生：

```sql
SELECT * FROM Student WHERE Sage IS NULL； 
```

**注意这里的“IS”不能被等号代替。**

#### **2.分组函数/聚合函数/多行处理函数**

| count | 取得记录数 |
| ----- | ---------- |
| sum   | 求和       |
| avg   | 取平均     |
| max   | 取最大的数 |
| min   | 取最小的数 |

**注意：分组函数自动忽略空值，不需要手动的加where****条件排除空值。**

**select count(\*) from emp where xxx;**                   **符合条件的所有记录总数。**

**select count(comm) from emp;         comm****这个字段中不为空的元素总数。**

注意：分组函数不能直接使用在where关键字后面。

```sql
mysql> select ename,sal from emp where sal > avg(sal);
ERROR 1111 (HY000): Invalid use of group function
```

例1：在SQL中，用于聚集查询的函数有哪些？（2012·搜狗）

解答：count，sum，avg，max，min。

#### **3.分组查询**

**3.1 group by**

GROUP BY子句**根据一个或多个属性的值来对元组进行分组，值相等的为一组。**

对查询结果分组的目的是为了细化聚集函数的作用对象，分组后聚集函数将作用于每一个组，即每一组都有一个函数值，如下列语句为查询Student 表中具有相同年龄的每个组的人数：

```sql
select Sage，count（*）from Student group by Sage； 
```

**如果使用了order by，order by必须放到group by后面**

**易错注意：！！！**

```sql
mysql> select empno,deptno,avg(sal) from emp group by deptno;

+-------+--------+-------------+
| empno | deptno | avg(sal)    |
+-------+--------+-------------+
|  7782 |     10 | 2916.666667 |
|  7369 |     20 | 2175.000000 |
|  7499 |     30 | 1566.666667 |
+-------+--------+-------------+
```

以上SQL语句在Oracle数据库中无法执行，执行报错。

以上SQL语句在Mysql数据库中可以执行，但是执行结果矛盾。

在SQL语句中若有group by 语句，那么在select语句后面只能跟**分组函数（分组函数参数不一定是参与分组的字段）**+**参与分组的字段**。

**3.2 having**

如果想对分组数据再进行过滤需要使用having子句，例如：

```sql
select Sage，count（*）from Student group by Sage having count（*）>1； 
```

#### **4.ORDER BY子句**

用户可以用order by子句对查询的结果按照**一个或多个属性**列的升序（ASC）或降序（DESC）排列，默认值为升序。

例如在表Student中，按学生的年龄值升序检索出全部学生的信息：

```sql
SELECT * FROM Student ORDER BY Sage； 
```

在表Student中，先按专业升序排序，然后同一专业的学生再按年龄降序排序，并输出全部学生信息：

```sql
SELECT * FROM Student ORDER BY Sdept，Sage desc； 
```

#### **5.LIMIT子句**

LIMIT主要是用于查询之后要显示返回的前几条或者中间某几行数据。

```sql
SELECT * FROM table LIMIT[offset，]rows I rows OFFSET offset  
```

LIMIT子句可以被用于强制SELECT 语句返回指定的记录数。LIMIT接受一个或两个数字参数。参数必须是一个整数常量。如果给定两个参数，第一个参数指定第一个返回记录行的偏移量，第二个参数指定返回记录行的最大数目。**初始记录行的偏移量是0（而不是1）。**如：

```sql
SELECT * FROM Student LIMIT 5，10；//检索记录行6-15 
```

如果只给定一个参数，它表示返回最前面的记录行数目：

```sql
SELECT*FROM Student LIMIT 5；//检索前5个记录行 
```

换句话说，LIMITn等价于LIMIT0，n。

例1：如下SQL语句是需要列出一个论坛版面第一页（每页显示20个）的帖子（post）标题（title），并按照发布时间（create_time）降序排列：（2012·腾讯）

```sql
SELECT title FROM post ? create_time DESC ? 0，20； 
```

解答：order by，limit。

#### **6.数据处理函数/单行处理函数**

| **Lower**   | 转换小写                                                     |
| ----------- | ------------------------------------------------------------ |
| upper       | 转换大写                                                     |
| **substr**  | 取子串（**substr(****被截取的字符串,****起始位置（从1****开始）,****截取的长度)**） |
| length      | 取长度                                                       |
| **trim**    | 去空格                                                       |
| str_to_date | 将字符串转换成日期                                           |
| date_format | 格式化日期                                                   |
| format      | 设置千分位                                                   |
| round       | 四舍五入                                                     |
| rand()      | 生成随机数                                                   |
| **Ifnull**  | 可以将null转换成一个具体值                                   |

**substr**  

查询姓名以M开头所有的员工

```sql
select * from emp where substr(ename, 1, 1)=upper('m');
```

**trim**

trim会去首尾空格，不会去除中间的空格

取得工作岗位为manager的所有员工

```sql
select * from emp where job=trim(upper('manager  '));
```



#### <u>**7.连接查询**</u>

连接查询：也可以叫跨表查询，需要关联多个表进行查询：

**内连接：**

```sql
SELECT Student.*，SC.* FROM Student，SC  WHERE Student.Sno=SC.Sno； 
```

在以上的连接操作中，只有满足条件的元组才能作为结果输出。若表Student中某些学生没有选课，则在SC表中没有相应的元组，造成最终结果中舍弃掉了这些学生的信息。

上述连接称为自然连接、**内连接**。

**外连接：**

有时想以Student表为主体列出每个学生的基本情况及其选课情况。若某个学生没有选课，依然将其保存到结果中（在SC表的属性上填空值），这时就需要使用**外连接**。

```sql
SELECT Student.*，SC.* FROM Student LEFT JOIN SC ON（Student.Sno=SC.Sno）; 
```

**以上是左外连接，左外连接列出左边表（本例为Student表）中的所有元组，右外连接列出右表关系中所有的元组。**



## **●**数据操纵(insert,delete,update)

数据操纵操作有3种：向表中添加若干行数据、修改表中的数据和删除表中的若干行数据。

**1.插入元组**

插入元组的INSERT语句的格式为：

```sql
INSERT  INTO tablel（fieldl，field2..） 
VALUES（value1，value2..）; 
```

其功能是将新元组插入到指定表中。其中新元组的fieldl的值为valuel，field2的值为value2…

如果INTO语句中没有指定任何属性列名，则新插入的元组必须在每个属性列上均有值。

将一个新学生元组（学号：201009013；姓名：王明；性别：男；所在系：CS；年龄：23）插入到Student表中的语句为：

```sql
INSERT INTO Student（Sno，Sname，Ssex，Sdept，Sage） 
VALUES（'201009013'，'王明’，‘M'，'CS'，23）； 
```

**2.修改数据**

修改数据又称为更新操作，其语句的一般格式为：

```sql
UPDATE table1 
SET fieldl=valuel，field2=value2 
WHERE范围； 
```

其功能是修改指定表中满足WHERE子句条件的元组。其中SET子句给出的value值用于取代相应的属性列值。如果省略WHERE子句，则表示要修改表中的所有元组。

将学生201009013的年龄改为22岁：

```sql
UPDATE Student  
SET Sage=22 
WHERE Sno='201009013'； 
```

**3.删除数据**

删除语句的一般格式为：

```sql
DELETE  
FROM table1 
WHERE范围； 
```

DELETE语句的功能是从指定表中删除满足WHERE子句条件的所有元组。如果省略 WHERE子句，表示删除表中全部元组，但表仍存在。删除学号为201009013的学生记录：

```sql
DELETE  
FROM Student  
WHERE Sno='201009013'； 
```

# ●SQL编程（触发器、游标等）

## ●触发器

### 应用场合

（1）当向一张表中**添加或删除记录时，需要在相关表中进行同步操作。**
比如，当一个订单产生时，订单所购的商品的库存量相应减少。
（2）当表上**某列数据的值与其他表中的数据有联系时**。
比如，当某客户进行欠款消费，可以在生成订单时通过设计触发器判断该客户的累计欠款是否超出了最大限度。
（3）当需要对某张表进行跟踪时。
比如，当有新订单产生时，需要及时通知相关人员进行处理；此时可以在订单表上设计添加触发器加以实现。

### 触发器创建语法之4要素

监视地点    table

监视事件    insert/update/delete
触发时间    after/before
触发事件    insert/update/delete

### 触发器创建

```sql
·创建触发器的语法
create trigger  触发器名称
after/befor（触发时间）insert/update/delete（监视事件）
on 表名（监视地址）
for each row 
begin
sql1；
sqlN；
end
```

```
查看已有triggers: show triggers;
删除已有triggers:drop trigger triggername;
```

举例：

**需求：**当下1个订单时，对应的商品要相应减少（买几个商品就少几个库存）
商品表：goods

订单表：ord

**分析：**
监视谁：ord

监视动作：insert

触发时间：after

触发事件：update

实例：

第一步：新建goods表、orde表，在goods表中插入数据

```sql
create table goods(
	gid int,
	gname varchar(20),
	num smallint
);
create table orde(
	oid int,
	gid int,
	much smallint
);
insert into goods values
(1,'cat',34),
(2,'dog',65),
(3,'pig',21);
```

```
mysql> select * from goods;
+------+-------+------+
| gid  | gname | num  |
+------+-------+------+
|    1 | cat   |   34 |
|    2 | dog   |   65 |
|    3 | pig   |   21 |
+------+-------+------+
```

第二步：**创建一个触发器**后，在orde表中插入一行数据

```sql
create trigger t1
after insert
on orde
for each row
update goods set num = num-1 where gid=1;
```

```
insert into orde values (1,1,5);
```

```
mysql> select * from goods;
+------+-------+------+
| gid  | gname | num  |
+------+-------+------+
|    1 | cat   |   33 |
|    2 | dog   |   65 |
|    3 | pig   |   21 |
+------+-------+------+
3 rows in set (0.00 sec)

mysql> select * from orde;
+------+------+------+
| oid  | gid  | much |
+------+------+------+
|    1 |    1 |    5 |
+------+------+------+
1 row in set (0.00 sec)
```

可以观察到，执行插入操作后，自动执行了触发器定义的内容。

**思考一：若想引用被监视的语句，怎么做？**

```sql
#在插入订单操作中，new代表orde中插入的一行
create trigger t_insert
after insert
on orde
for each row
update goods set num = num-new.much where gid=new.gid;

#在删除订单操作中，old代表orde中删除的一行
create trigger t_delete
after delete
on orde
for each row
update goods set num = num+old.much where gid=old.gid;

#在修改订单操作中，old代表之前的，new代表之后
create trigger t_update
before update
on orde
for each row
update goods set num = num+old.much-new.much where gid=new.gid;
```

插入操作

```
mysql> insert into orde values (2,2,5);
Query OK, 1 row affected (0.01 sec)

mysql> select * from goods;
+------+-------+------+
| gid  | gname | num  |
+------+-------+------+
|    1 | cat   |   33 |
|    2 | dog   |   60 |
|    3 | pig   |   21 |
+------+-------+------+
3 rows in set (0.00 sec)

mysql> select * from orde;
+------+------+------+
| oid  | gid  | much |
+------+------+------+
|    1 |    1 |    5 |
|    2 |    2 |    5 |
+------+------+------+
2 rows in set (0.00 sec)
```

删除操作

```
mysql> delete  from orde where oid=2;
Query OK, 1 row affected (0.01 sec)

mysql> select * from orde;
+------+------+------+
| oid  | gid  | much |
+------+------+------+
|    1 |    1 |    5 |
+------+------+------+
1 row in set (0.00 sec)

mysql> select * from goods;
+------+-------+------+
| gid  | gname | num  |
+------+-------+------+
|    1 | cat   |   33 |
|    2 | dog   |   65 |
|    3 | pig   |   21 |
+------+-------+------+
3 rows in set (0.00 sec)
```

更改操作：

```
mysql> update orde set much=2 where oid=1;
Query OK, 1 row affected (0.00 sec)
Rows matched: 1  Changed: 1  Warnings: 0

mysql> select * from orde;
+------+------+------+
| oid  | gid  | much |
+------+------+------+
|    1 |    1 |    2 |
+------+------+------+
1 row in set (0.00 sec)

mysql> select * from goods;
+------+-------+------+
| gid  | gname | num  |
+------+-------+------+
|    1 | cat   |   36 |
|    2 | dog   |   65 |
|    3 | pig   |   21 |
+------+-------+------+
3 rows in set (0.00 sec)
```

思考二：before目前似乎没有看出和after的区别？

再思考：如果剩余3头猪，但客户买了10头猪，发生什么情况？

能否预防能否在购买量much>库存量num时，把much自动改为num

提示：before适合

