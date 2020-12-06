---
title: 'MySQL'
date: 2019-03-15 13:52:09
tags:
	- 数据库
categories: 
	- 数据库
	- MySQL
---

# **MySql常用数据类型**

| **类型**                         | **描述**                                             |
| -------------------------------- | ---------------------------------------------------- |
| **Char**(长度)                   | **定长字符串，存储空间大小固定，适合作为主键或外键** |
| **Varchar**(长度)                | **变长字符串，存储空间等于实际数据空间**             |
| **double**(有效数字位数，小数位) | **数值型**                                           |
| **Float(**有效数字位数，小数位)  | **数值型**                                           |
| **Int**(**长度**)                | **整型**                                             |
| **bigint(**长度)                 | **长整型**                                           |
| **Date**                         | 日期型                                               |
| **BLOB**                         | **Binary Large OBject**（二进制大对象）              |
| **CLOB**                         | **Character Large OBject**（字符大对象）             |
| **其它**…………………                  |                                                      |

# 一条SQL查询语句是如何执行的？

{%asset_img 12.png%}

第二步：查询缓存；MySQL 拿到一个查询请求后，会先到查询缓存看看，之前是不是执行过这条语句。之前执行过的语句及其结果可能会以 key-value 对的形式。**但是大多数情况下我会建议你不要使用查询缓存，为什么呢？因为查询缓存往往弊大于利。**（因为只要表做了更新，查询缓存就会清空，对更新频繁的数据库来说，查询缓存命中率低。因此查询缓存适合业务是静态表，或者很长时间才更新）

需要注意的是，**MySQL 8.0 版本直接将查询缓存的整块功能删掉了**，也就是说 8.0 开始彻底没有这个功能了。

优化器：优化器是在表里面有多个索引的时候，决定使用哪个索引；或者在一个语句有多表关联（join）的时候，决定各个表的连接顺序。

# 一条SQL更新语句是如何执行的？

例如执行以下更新语句：

```sql
mysql> update T set c=c+1 where ID=2;
```

与查询语句的不同：
（1）在一个表上有更新的时候，跟这个表有关的查询缓存会失效，所以这条语句就会把表 T 上所有缓存结果都清空。这也就是我们一般不建议使用查询缓存的原因；

（2）更新流程还涉及两个重要的日志模块，它们正是我们今天要讨论的主角：redo log（重做日志）和 binlog（归档日志）。



# **存储引擎MyISAM、InnoDB、MEMORY**

**MyISAM:**它不支持事务，也不支持外键，尤其是**访问速度快**，对事务完整性没有要求或者以**SELECT、INSERT为主的应用**基本都可以使用这个引擎来创建表。

**InnoDB:**InnoDB是一个健壮的事务型存储引擎,他引入了行级锁和外键约束。一般来说，如果需要事务支持，并且有较高的并发读取频率，InnoDB是不错的选择。

**MEMORY：**使用MySQL Memory存储引擎的出发点是速度。为得到最快的响应时间，采用的逻辑存储介质是系统内存。

一般在以下几种情况下使用Memory存储引擎：1.目标数据较小，而且被非常频繁地访问。2.如果数据是临时的，而且要求必须立即可用，那么就可以存放在内存表中。3.存储在Memory表中的数据如果突然丢失，不会对应用服务产生实质的负面影响。

{%asset_img 08.png%}



# 查询性能优化

## 使用 Explain 进行分析

Explain 用来分析 SELECT 查询语句，开发人员可以通过分析 Explain 结果来优化查询语句。

{%asset_img 11.png%}

比较重要的字段有：

- select_type : 查询类型，有简单查询、联合查询、子查询等
- key : 使用的索引
- rows : 扫描的行数

## 优化数据访问

### 1. 减少请求的数据量

- 只返回必要的列：最好不要使用 SELECT * 语句。
- 只返回必要的行：使用 LIMIT 语句来限制返回的数据。
- 缓存重复查询的数据：使用缓存可以避免在数据库中进行查询，特别在要查询的数据经常被重复查询时，缓存带来的查询性能提升将会是非常明显的。

### 2. 减少服务器端扫描的行数

最有效的方式是使用索引来覆盖查询。

## 重构查询方式

### 1. 切分大查询

一个大查询如果一次性执行的话，可能一次锁住很多数据、占满整个事务日志、耗尽系统资源、阻塞很多小的但重要的查询。

```sql
DELETE FROM messages WHERE create < DATE_SUB(NOW(), INTERVAL 3 MONTH);
```

```sql
rows_affected = 0
do {
    rows_affected = do_query(
    "DELETE FROM messages WHERE create  < DATE_SUB(NOW(), INTERVAL 3 MONTH) LIMIT 10000")
} while rows_affected > 0
```

### 2. 分解大连接查询

将一个大连接查询分解成对每一个表进行一次单表查询，然后在应用程序中进行关联，这样做的好处有：

- 让缓存更高效。对于连接查询，如果其中一个表发生变化，那么整个查询缓存就无法使用。而分解后的多个查询，即使其中一个表发生变化，对其它表的查询缓存依然可以使用。
- 分解成多个单表查询，这些单表查询的缓存结果更可能被其它查询使用到，从而减少冗余记录的查询。
- 减少锁竞争；
- 在应用层进行连接，可以更容易对数据库进行拆分，从而更容易做到高性能和可伸缩。
- 查询本身效率也可能会有所提升。例如下面的例子中，使用 IN() 代替连接查询，可以让 MySQL 按照 ID 顺序进行查询，这可能比随机的连接要更高效。

```sql
SELECT * FROM tab
JOIN tag_post ON tag_post.tag_id=tag.id
JOIN post ON tag_post.post_id=post.id
WHERE tag.tag='mysql';
```

```sql
SELECT * FROM tag WHERE tag='mysql';
SELECT * FROM tag_post WHERE tag_id=1234;
SELECT * FROM post WHERE post.id IN (123,456,567,9098,8904);
```

# 切分

## 水平切分

水平切分又称为 Sharding，它是将同一个表中的记录拆分到多个结构相同的表中。

当一个表的数据不断增多时，Sharding 是必然的选择，它可以将数据分布到集群的不同节点上，从而缓存单个数据库的压力。

{%asset_img 04.jpg%}

## 垂直切分

垂直切分是将一张表按列切分成多个表，通常是按照列的关系密集程度进行切分，也可以利用垂直切分将经常被使用的列和不经常被使用的列切分到不同的表中。

在数据库的层面使用垂直切分将按数据库中表的密集程度部署到不同的库中，例如将原来的电商数据库垂直切分成商品数据库、用户数据库等。

{%asset_img 05.jpg%}

## Sharding 策略

- 哈希取模：hash(key) % N；
- 范围：可以是 ID 范围也可以是时间范围；
- 映射表：使用单独的一个数据库来存储映射关系。

## Sharding 存在的问题

### 1. 事务问题

使用分布式事务来解决，比如 XA 接口。

### 2. 连接

可以将原来的连接分解成多个单表查询，然后在用户程序中进行连接。

### 3. ID 唯一性

- 使用全局唯一 ID（GUID）
- 为每个分片指定一个 ID 范围
- 分布式 ID 生成器 (如 Twitter 的 Snowflake 算法)

# 复制

## 主从复制

主要涉及三个线程：binlog 线程、I/O 线程和 SQL 线程。

-  **binlog 线程** ：负责将主服务器上的数据更改写入二进制日志（Binary log）中。
-  **I/O 线程** ：负责从主服务器上读取二进制日志，并写入从服务器的中继日志（Relay log）。
-  **SQL 线程** ：负责读取中继日志，解析出主服务器已经执行的数据更改并在从服务器中重放（Replay）。

{%asset_img 06.png%}

## 读写分离

主服务器处理写操作以及实时性要求比较高的读操作，而从服务器处理读操作。

读写分离能提高性能的原因在于：

- 主从服务器负责各自的读和写，极大程度缓解了锁的争用；
- 从服务器可以使用 MyISAM，提升查询性能以及节约系统开销；
- 增加冗余，提高可用性。

读写分离常用代理方式来实现，代理服务器接收应用层传来的读写请求，然后决定转发到哪个服务器。

{%asset_img 07.png%}



# 锁

## 按锁的粒度分类：

Mysql为了解决并发、数据安全的问题，使用了锁机制。

可以按照锁的粒度把数据库锁分为表级锁和行级锁。

**表级锁**

Mysql中锁定 **粒度最大** 的一种锁，对当前操作的整张表加锁，实现简单 ，**资源消耗也比较少，加锁快，不会出现死锁** 。其锁定粒度最大，触发锁冲突的概率最高，并发度最低，MyISAM和 InnoDB引擎都支持表级锁。

**行级锁**

Mysql中锁定 **粒度最小** 的一种锁，只针对当前操作的行进行加锁。 行级锁能大大减少数据库操作的冲突。其加锁粒度最小，并发度高，但加锁的**开销也最大，加锁慢，会出现死锁**。 InnoDB支持的行级锁，包括如下几种。

**Record Lock:** 对索引项加锁，锁定符合条件的行。其他事务不能修改和删除加锁项；
**Gap Lock:** 对索引项之间的“间隙”加锁，锁定记录的范围（对第一条记录前的间隙或最后一条将记录后的间隙加锁），不包含索引项本身。其他事务不能在锁范围内插入数据，这样就防止了别的事务新增幻影行。
**Next-key Lock：** 锁定索引项本身和索引范围。即Record Lock和Gap Lock的结合。可解决幻读问题。

**虽然使用行级索具有粒度小、并发度高等特点，但是表级锁有时候也是非常必要的：**

事务更新大表中的大部分数据直接使用表级锁效率更高；

事务比较复杂，使用行级索很可能引起死锁导致回滚。



## 按锁的操作类型分类：

a.读锁（共享锁）：对同一个数据（衣服），多个读操作可以同时进行，互不干扰。

b.写锁（互斥锁）：如果当前写操作没有完毕（买衣服的一系列操作），则无法进行其他的读操作；

```sql
create table tablelock(
id int primary key auto_increment, 
name varchar(20)
) engine myisam; 
insert into tablelock(name)values('al');
insert into tablelock(name)values('a2');
insert into tablelock(name)values('a3');
insert into tablelock(name)values('a4');
insert into tablelock(name)values('a5'); 
commit;
```

增加锁的语法：
locak table   表1   read/write，表2 read/write，.…
查看加锁的表：
show open tables；

## 表锁介绍：

**加读锁实验：**

会话：session：每一个访问数据的dos命令行、数据库客户端工具都是一个会话加读锁：

会话0：

```sql
lock table tablelock read；

select * from tablelock；--读（查），可以
delete from tablelock where id=1；--写（增删改），不可以
select * from emp；--读，不可以
delete from tablelock where eno=1；--写，不可以
```

总结：如果给A表加了读锁，则当前会话只能对A表进行读操作，对其他表也不能进行读写操作。

会话1（其他会话）：

```sql
select * from tablelock；--读（查），可以
delete from tablelock where id =1；-写，会“等待”会话0将锁释放
select*from emp；-—读（查），可以
delete from emp where eno=1；--写，可以
```

总结：
会话0给A表加了锁；其他会话的操作：

a.可以对其他表（A表以外的表）进行读、写操作

b.对A表：读-可以；写-**需要等待释放锁。**

释放锁的语句：unlock tables;

### 加写锁实验：

会话0：
lock table tablelock write；

当前会话（会话0）可以对加了写锁的表进行任何操作（增删改查）：但是不能操作其他表。

会话1（其他会话）：

对会话0中加写锁的表可以进行增删改查的前提是：**等待会话0释放写锁**

### MySQL表级锁的锁模式

MyISAM在执行查询语句（SELECT）前，会自动给涉及的所有表加读锁，

在执行更新操作（DML）前，会自动给涉及的表加写锁。

所以对MyISAM表进行操作，会有以下情况：
a、对MyISAM表的读操作（加读锁），不会阻塞其他进程（会话）对同一表的读请求，但会阻塞对同一表的写请求。只有当读锁释放后，才会执行其它进程的写操作。
b、对MyISAM表的写操作（加写锁），会阻塞其他进程（会话）对同一表的读和写操作，只有当写锁释放后，才会执行其它进程的读写操作。

### 分析表锁定：

查看哪些表加了锁：show open tables；1代表被加了锁
分析表锁定的严重程度：show status like 'table%’；

Table locks immediate：即可能获取到的锁数
Tablelocks waited：需要等待的表锁数（如果该值越大，说明存在越大的锁竞争）

一般建议：
Tablelocks immediate/Table locks waited>5000，建议采用InnoDB引擎，否则MyISAM引擎

## 行锁介绍：

```sql
create table linelock(
	id int(5) primary key auto_increment, 
	name varchar(20)
)engine=innodb; 
insert into linelock(name)values'1');
insert into linelock(name)values('2');
insert into linelock(name)values('3');
insert into linelock(name)values('4');
insert into linelock(name)values('5'); 
--对于插入语句mysq1默认自动commit；oracle默认不会自动commit
--为了研究行锁，暂时将自动commit关闭；set autocommt=0；以后需要通过commit
```

行锁，分析操作同一条数据：（有影响）

会话0：写操作
insert into linelock values（'a6'）；

会话1：写操作同样的数据
update linelock set name='ax' where id=6；

对行锁情况：
1.如果会话x对某条数据a进行DML(增删改)操作（研究时：关闭了自动commit的情况下），则其他会话必须**等待（会产生阻塞）**会话x结束事务（commit/ro1lback）后才能对数据a进行操作。
2.表锁是通过unlock tables，也可以通过事务解锁；行锁是通过事务解锁。



行锁，操作不同数据（无影响）：
会话0：写操作
insert into linelock walues（8，'a8'）；

会话1：写操作，不同的数据

update linelock set name='ax' where id=5；

行锁，一次锁一行数据；因此如果操作的是不同数据，则不干扰。

### 行锁的注意事项：

【1】**如果没有索引，则行锁会转为表锁**
show index from linelock；

alter table linelock add index idx_linelock_name（name）；  --将name新增为一个索引

会话0：写操作

​			update linelock set name='ai'where name='3'；

会话1：写操作，不同的数据
			update linelock set name='aiX'where name='4'；

以上可以正常操作



会话0：写操作
			update linelock set name='ai'where name=3；

会话1：写操作，不同的数据
			update linelock set name='aiX'where name=4；
--可以发现，数据被阻塞了（加锁）
--原因：**如果索引类发生了类型转换，则索引失效。**因此此次操作，会从行锁转为表锁。

注意：如果使用针对InnoDB的表使用行锁，**被锁定字段不是主键，也没有针对它建立索引的话。行锁锁定的也是整张表。锁整张表会造成程序的执行效率会很低。**

补充：**当你创建或设置主键的时候，mysql会自动添加一个与主键对应的唯一索引，不需要再做额外的添加。**

【2】行锁的一种特殊情况：间隙锁：

值在范围内，但却不存在
--此时linelock表中没有id=7的数据

{%asset_img 10.png%}

update linelock set name ='x'where id>1 and id<9；--即在此where范围中，没有id=7的数间隙：Mysq1会自动给间隙加索-〉间隙锁。即本题会自动给id=7的数据加间隙锁（行锁）。

行锁：如果有where，则实际加索的范围就是where后面的范围（不是实际的值）


InnoDB默认采用行锁；

缺点：比表锁性能损耗大。

优点：并发能力强，效率高。
因此建议，高并发用InnoDB，否则用MyISAM。



**行锁分析：**
show status like '%innodb_row_lock%’
Innodb row_lock_current_waits：当前正在等待锁的数量

Innodb row_lock_time：等待总时长。从系统启到现在一共等待的时间

Innodb row lock time avg：平均等待时长。从系统启到现在平均等待的时间

Innodb row locktime max：最大等待时长。从系统启到现在最大一次等待的时间Innodb_row_lock_waits：等待次数。从系统启到现在一共等待的次数



**如果仅仅是查询数据，能否加锁？**

可以for update研究学习时，将自动提交关闭：

手动开启事务的三种方式：

```sql
set autocommit=0；
start transaction；
begin；
```

```sql
select*from linelock where id=2 for update；--通过for update对queFy语句进行加锁。
```



# 参考资料

- BaronScbwartz, PeterZaitsev, VadimTkacbenko, 等. 高性能 MySQL[M]. 电子工业出版社, 2013.

### 一条SQL语句在MySQL中如何执行的

[一条SQL语句在MySQL中如何执行的](https://mp.weixin.qq.com/s?__biz=Mzg2OTA0Njk0OA==&mid=2247485097&idx=1&sn=84c89da477b1338bdf3e9fcd65514ac1&chksm=cea24962f9d5c074d8d3ff1ab04ee8f0d6486e3d015cfd783503685986485c11738ccb542ba7&token=79317275&lang=zh_CN#rd)

### 一条SQL语句执行得很慢的原因有哪些？

[腾讯面试：一条SQL语句执行得很慢的原因有哪些？---不看后悔系列](https://mp.weixin.qq.com/s?__biz=Mzg2OTA0Njk0OA==&mid=2247485185&idx=1&sn=66ef08b4ab6af5757792223a83fc0d45&chksm=cea248caf9d5c1dc72ec8a281ec16aa3ec3e8066dbb252e27362438a26c33fbe842b0e0adf47&token=79317275&lang=zh_CN#rd)