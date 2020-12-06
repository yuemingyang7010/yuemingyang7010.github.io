---
title: '动力节点SQL34题'
date: 2019-03-18 13:52:09
tags:
	- 数据库
	- 数据库刷题
categories: 
	- 数据库
	- 数据库刷题
---

## 0 练习数据

sql脚本文件：

```sql
drop table if exists dept;
drop table if exists salgrade;
drop table if exists emp;
 
create table dept(
		deptno int(10) primary key,
		dname varchar(14),
		loc varchar(13)
		);
		
create table salgrade(
		grade int(11),
		losal int(11),
		hisal int(11)
		);
		
create table emp(
		empno int(4) primary key,
		ename varchar(10),
		job varchar(9),
		mgr int(4),
		hiredate date,
		sal double(7,2),
		comm double(7,2),
		deptno int(2)
		);
		
insert into dept(deptno,dname,loc) values(10,'ACCOUNTING','NEW YORK');
insert into dept(deptno,dname,loc) values(20,'RESEARCHING','DALLAS');
insert into dept(deptno,dname,loc) values(30,'SALES','CHICAGO');
insert into dept(deptno,dname,loc) values(40,'OPERATIONS','BOSTON');
 
insert into salgrade(grade,losal,hisal) values(1,700,1200);
insert into salgrade(grade,losal,hisal) values(2,1201,1400);
insert into salgrade(grade,losal,hisal) values(3,1401,2000);
insert into salgrade(grade,losal,hisal) values(4,2001,3000);
insert into salgrade(grade,losal,hisal) values(5,3001,5000);
 
insert into emp(empno,ename,job,mgr,hiredate,sal,comm,deptno)
	values(7369,'SIMITH','CLERK',7902,'1980-12-17',800,null,20);
insert into emp(empno,ename,job,mgr,hiredate,sal,comm,deptno)
	values(7499,'ALLEN','SALESMAN',7698,'1981-02-20',1600,300,30);
insert into emp(empno,ename,job,mgr,hiredate,sal,comm,deptno)
	values(7521,'WARD','SALESMAN',7698,'1981-02-22',1250,500,30);
insert into emp(empno,ename,job,mgr,hiredate,sal,comm,deptno)
	values(7566,'JONES','MANAGER',7839,'1981-04-02',2975,null,20);
insert into emp(empno,ename,job,mgr,hiredate,sal,comm,deptno)
	values(7654,'MARTIN','SALESMAN',7698,'1981-09-28',1250,1400,30);
insert into emp(empno,ename,job,mgr,hiredate,sal,comm,deptno)
	values(7698,'BLAKE','MANAGER',7839,'1981-05-01',2850,null,30);
insert into emp(empno,ename,job,mgr,hiredate,sal,comm,deptno)
	values(7782,'CLARK','MANAGER',7839,'1981-06-09',2450,null,10);
insert into emp(empno,ename,job,mgr,hiredate,sal,comm,deptno)
	values(7788,'SCOTT','ANALYST',7566,'1987-04-19',3000,null,20);
insert into emp(empno,ename,job,mgr,hiredate,sal,comm,deptno)
	values(7839,'KING','PRESIDENT',null,'1981-11-17',5000,null,10);
insert into emp(empno,ename,job,mgr,hiredate,sal,comm,deptno)
	values(7844,'TURNER','SALESMAN',7698,'1981-09-08',1500,null,30);
insert into emp(empno,ename,job,mgr,hiredate,sal,comm,deptno)
	values(7876,'ADAMS','CLERK',7788,'1987-05-23',1100,null,20);
insert into emp(empno,ename,job,mgr,hiredate,sal,comm,deptno)
	values(7900,'JAMES','CLERK',7698,'1981-12-03',950,null,30);
insert into emp(empno,ename,job,mgr,hiredate,sal,comm,deptno)
	values(7902,'FORD','ANALYST',7566,'1981-12-03',3000,null,20);
insert into emp(empno,ename,job,mgr,hiredate,sal,comm,deptno)
	values(7934,'MILLER','CLERK',7782,'1982-01-23',1300,null,10);
	
select * from dept;
select * from salgrade;
select * from emp;
```

dept表格

```sql
mysql> select * from dept;
+--------+-------------+----------+
| deptno | dname       | loc      |
+--------+-------------+----------+
|     10 | ACCOUNTING  | NEW YORK |
|     20 | RESEARCHING | DALLAS   |
|     30 | SALES       | CHICAGO  |
|     40 | OPERATIONS  | BOSTON   |
+--------+-------------+----------+
```

salgrade表格

```sql
mysql> select * from salgrade;
+-------+-------+-------+
| grade | losal | hisal |
+-------+-------+-------+
|     1 |   700 |  1200 |
|     2 |  1201 |  1400 |
|     3 |  1401 |  2000 |
|     4 |  2001 |  3000 |
|     5 |  3001 |  5000 |
+-------+-------+-------+
```

emp表格

```sql
mysql> select * from emp;
+-------+--------+-----------+------+------------+---------+---------+--------+
| empno | ename  | job       | mgr  | hiredate   | sal     | comm    | deptno |
+-------+--------+-----------+------+------------+---------+---------+--------+
|  7369 | SIMITH | CLERK     | 7902 | 1980-12-17 |  800.00 |    NULL |     20 |
|  7499 | ALLEN  | SALESMAN  | 7698 | 1981-02-20 | 1600.00 |  300.00 |     30 |
|  7521 | WARD   | SALESMAN  | 7698 | 1981-02-22 | 1250.00 |  500.00 |     30 |
|  7566 | JONES  | MANAGER   | 7839 | 1981-04-02 | 2975.00 |    NULL |     20 |
|  7654 | MARTIN | SALESMAN  | 7698 | 1981-09-28 | 1250.00 | 1400.00 |     30 |
|  7698 | BLAKE  | MANAGER   | 7839 | 1981-05-01 | 2850.00 |    NULL |     30 |
|  7782 | CLARK  | MANAGER   | 7839 | 1981-06-09 | 2450.00 |    NULL |     10 |
|  7788 | SCOTT  | ANALYST   | 7566 | 1987-04-19 | 3000.00 |    NULL |     20 |
|  7839 | KING   | PRESIDENT | NULL | 1981-11-17 | 5000.00 |    NULL |     10 |
|  7844 | TURNER | SALESMAN  | 7698 | 1981-09-08 | 1500.00 |    NULL |     30 |
|  7876 | ADAMS  | CLERK     | 7788 | 1987-05-23 | 1100.00 |    NULL |     20 |
|  7900 | JAMES  | CLERK     | 7698 | 1981-12-03 |  950.00 |    NULL |     30 |
|  7902 | FORD   | ANALYST   | 7566 | 1981-12-03 | 3000.00 |    NULL |     20 |
|  7934 | MILLER | CLERK     | 7782 | 1982-01-23 | 1300.00 |    NULL |     10 |
+-------+--------+-----------+------+------------+---------+---------+--------+
```

## 1 取得每个部门最高薪水的人员名称

先取得每个部门的最高薪水，然后找出和此薪水和部门编号相同的人。

```sql
mysql> select deptno,max(sal) as maxsal from emp group by deptno;
+--------+---------+
| deptno | maxsal  |
+--------+---------+
|     20 | 3000.00 |
|     30 | 2850.00 |
|     10 | 5000.00 |
+--------+---------+
```

```sql
mysql> select a.ename,a.deptno,a.sal from emp a join(select deptno,max(sal) as maxsal from emp group by deptno) b on a.sal=b.maxsal and a.deptno=b.deptno;
+-------+--------+---------+
| ename | deptno | sal     |
+-------+--------+---------+
| BLAKE |     30 | 2850.00 |
| SCOTT |     20 | 3000.00 |
| KING  |     10 | 5000.00 |
| FORD  |     20 | 3000.00 |
+-------+--------+---------+
```

## 2 哪些人的薪水在部门平均薪水之上

先求出每个部门的平均薪水

```sql
mysql> select deptno,avg(sal) from emp group by deptno;
+--------+-------------+
| deptno | avg(sal)    |
+--------+-------------+
|     20 | 2175.000000 |
|     30 | 1566.666667 |
|     10 | 2916.666667 |
+--------+-------------+
```

然后找出部门中大于其平均薪水的人

```sql
mysql> select a.deptno,a.ename,a.sal,b.avgsal from emp a join (select deptno,avg(sal) as avgsal from emp group by deptno) b on a.sal>b.avgsal and a.deptno=b.deptno;
+--------+-------+---------+-------------+
| deptno | ename | sal     | avgsal      |
+--------+-------+---------+-------------+
|     30 | ALLEN | 1600.00 | 1566.666667 |
|     20 | JONES | 2975.00 | 2175.000000 |
|     30 | BLAKE | 2850.00 | 1566.666667 |
|     20 | SCOTT | 3000.00 | 2175.000000 |
|     10 | KING  | 5000.00 | 2916.666667 |
|     20 | FORD  | 3000.00 | 2175.000000 |
+--------+-------+---------+-------------+
```

## 3 取得部门中所有人的平均的薪水等级

先计算每个人的薪水等级

```sql
mysql> select a.deptno,a.ename,a.sal,b.grade from emp a join salgrade b on a.sal between b.losal and b.hisal order by a.deptno;
+--------+--------+---------+-------+
| deptno | ename  | sal     | grade |
+--------+--------+---------+-------+
|     10 | MILLER | 1300.00 |     2 |
|     10 | CLARK  | 2450.00 |     4 |
|     10 | KING   | 5000.00 |     5 |
|     20 | SCOTT  | 3000.00 |     4 |
|     20 | SIMITH |  800.00 |     1 |
|     20 | ADAMS  | 1100.00 |     1 |
|     20 | JONES  | 2975.00 |     4 |
|     20 | FORD   | 3000.00 |     4 |
|     30 | BLAKE  | 2850.00 |     4 |
|     30 | ALLEN  | 1600.00 |     3 |
|     30 | TURNER | 1500.00 |     3 |
|     30 | WARD   | 1250.00 |     2 |
|     30 | JAMES  |  950.00 |     1 |
|     30 | MARTIN | 1250.00 |     2 |
+--------+--------+---------+-------+
```

然后根据部门分组，由于使用了分组函数，所以只能显示参加分组的字段和分组函数

```sql
mysql> select a.deptno,avg(grade) as AvgSalGrade from emp a join salgrade b on a.sal between b.losal and b.hisal group by a.deptno;
+--------+-------------+
| deptno | AvgSalGrade |
+--------+-------------+
|     20 |      2.8000 |
|     30 |      2.5000 |
|     10 |      3.6667 |
+--------+-------------+
```

## 4 不准使用组函数Max，给出最高薪水（给出两种解决方案）

方案一，按照薪水降序排列，取第一个

```sql
mysql> select ename,sal from emp order by sal desc limit 1;
+-------+---------+
| ename | sal     |
+-------+---------+
| KING  | 5000.00 |
+-------+---------+
```

方案二，使用自连接。

```sql
mysql> select ename,sal from emp where sal not in 
(select a.sal from emp a join emp b on a.sal<b.sal);#最大值不小于表中任何一个工资，所以不会出现在此临时表中
+-------+---------+
| ename | sal     |
+-------+---------+
| KING  | 5000.00 |
+-------+---------+
```

## 5 取得平均薪水最高的部门和部门编号

方案一：先取得每个部门的平均薪水

```sql
mysql> select deptno, avg(sal) as avgsal from emp group by deptno;
+--------+-------------+
| deptno | avgsal      |
+--------+-------------+
|     20 | 2175.000000 |
|     30 | 1566.666667 |
|     10 | 2916.666667 |
+--------+-------------+
```

再取得最高的平均薪水

```sql
mysql> select avg(sal) as avgsal from emp group by deptno order by avgsal desc limit 1; 
+-------------+
| avgsal      |
+-------------+
| 2916.666667 |
+-------------+
```

然后取得和最高平均薪水相同的部门（因为可能有多个部门并列最高）

```sql
mysql> select deptno,avg(sal) as avgsal from emp group by deptno having avgsal=
    -> (select avg(sal) as avgsal from emp group by deptno order by avgsal desc limit 1);
+--------+-------------+
| deptno | avgsal      |
+--------+-------------+
|     10 | 2916.666667 |
+--------+-------------+
```

方案二：与方案一类似，用max取得最高平均薪水

```sql
mysql> select deptno,avg(sal) as avgsal 
from emp group by deptno having avgsal=(select max(t.avgsal) from (select avg(sal) as avgsal from emp group by deptno) t);
+--------+-------------+
| deptno | avgsal      |
+--------+-------------+
|     10 | 2916.666667 |
+--------+-------------+
```

## 6 取得平均薪水最高的部门名称

类似上题

```sql
SELECT d.dname,t.avgsal FROM dept d JOIN (SELECT deptno,AVG(sal) AS avgsal FROM emp GROUP BY deptno ORDER BY avgsal DESC LIMIT 1) t ON d.deptno=t.deptno;
+--------+-------------+
| deptno | avgsal      |
+--------+-------------+
|     10 | 2916.666667 |
+--------+-------------+
```

## #7 求平均薪水的等级最高的部门的部门名称

先求各部门平均薪水的等级

```sql
select a.deptno,a.avgsal,b.grade from (select deptno,avg(sal) as avgsal from emp group by deptno) a join salgrade b on avgsal between losal and hisal;
+--------+-------------+-------+
| deptno | avgsal      | grade |
+--------+-------------+-------+
|     20 | 2175.000000 |     4 |
|     30 | 1566.666667 |     3 |
|     10 | 2916.666667 |     4 |
+--------+-------------+-------+
```

取得最高的等级

```sql
mysql> select b.grade from (select deptno,avg(sal) as avgsal from emp group by deptno) a join salgrade b on avgsal between losal and hisal order by b.grade desc limit 1;
+-------+
| grade |
+-------+
|     4 |
+-------+
```

显示平均薪水等级等于最高等级的部门名称

```sql
mysql> select d.dname,c.avgsal,c.grade
from
	(select a.deptno,a.avgsal,b.grade
     	from
     		(select deptno,avg(sal) as avgsal from emp group by deptno) a
     	join
     		salgrade b
     	on
     		avgsal between losal and hisal) c
join
	dept d
on
	c.deptno=d.deptno and c.grade=
	#最高等级为4
		(select b.grade from (select deptno,avg(sal) as avgsal from emp group by deptno) a join salgrade b on avgsal between losal and hisal order by b.grade desc limit 1);
+-------------+-------------+-------+
| dname       | avgsal      | grade |
+-------------+-------------+-------+
| ACCOUNTING  | 2916.666667 |     4 |
| RESEARCHING | 2175.000000 |     4 |
+-------------+-------------+-------+
```

## ##8 取得比普通员工（员工代码没在mgr字段出现的）最高薪水更高的领导人姓名

先取出普通员工的最高薪水，注意mgr字段里有null，不能直接使用not in 语句

```sql
mysql> select max(sal) as cmsal from emp where empno not in (select distinct mgr from emp where mgr is not null);
+---------+
| cmsal   |
+---------+
| 1600.00 |
+---------+
```

取得所有领导人的姓名和薪水

```sql
mysql> select ename,sal from emp where empno in (select distinct mgr from emp);
+-------+---------+
| ename | sal     |
+-------+---------+
| FORD  | 3000.00 |
| BLAKE | 2850.00 |
| KING  | 5000.00 |
| JONES | 2975.00 |
| SCOTT | 3000.00 |
| CLARK | 2450.00 |
+-------+---------+
```

然后取得结果

```sql
mysql> select ename,sal from emp where empno in (select distinct mgr from emp) and sal>(select max(sal) as cmsal from emp where empno not in (select distinct mgr from emp where mgr is not null));
+-------+---------+
| ename | sal     |
+-------+---------+
| JONES | 2975.00 |
| BLAKE | 2850.00 |
| CLARK | 2450.00 |
| SCOTT | 3000.00 |
| KING  | 5000.00 |
| FORD  | 3000.00 |
+-------+---------+
```

​	

## 13 面试题

三张表：

- 学生表S：学号SNO，姓名SNAME
- 课程表C：课号CNO，课程名CNAME，课程老师CTEACHER
- 选课表SC：学号SNO，课号CNO，分数SCGRADE

1.显示没选”黎明“老师课的学生

```sql
#”黎明“老师的课号
select cno from c where cteacher='黎明';
#没选其课学生的学号
select distinct sno from sc where sno not in (select cno from c where cteacher='黎明');
#没选课学生的姓名
select sname from s where sno in (select distinct sno from sc where sno not in (select cno from c where cteacher='黎明'));
```

2.列出2门以上（含2门）不及格学生姓名及平均成绩

```sql
#2门及以上不及格学生的学号
select sno from sc where scgrade<60 group by sno having count(*)>=2;
#不及格学生姓名
select sname,sno from s where sno in (select sno from sc where scgrade<60 group by sno having count(*)>=2);
#学生的平均成绩
select sno,avg(scgrade) avggrade from sc group by sno;
#联合姓名和平均成绩
select a.sno,a.sname,b.avggrade 
from (select sname,sno from s where sno in (select sno from sc where scgrade<60 group by sno having count(*)>=2)) a 
join (select sno,avg(scgrade) avggrade from sc group by sno) b 
on a.sno=b.sno;
```

3.学过1号课程和2号课程的所有学生的姓名

```sql
#选过1号课程和2号课程的学生学号
select distinct sno from sc where cno=1 or cno=2;
#从s表中取出其姓名
select sno,sname from s where sno in (select distinct sno from sc where cno=1 or cno=2);
```

最后补充根据视频数据编写的sql文件

```sql
create table c(
	cno int(2) primary key auto_increment,
	cname varchar(32),
	cteacher varchar(16)
	);
	
create table s(
	sno int(2) primary key auto_increment,
	sname varchar(16)
	);

create table sc(
	sno int(2),
	cno int(2),
	scgrade int(3),
	primary key(sno,cno)
	);
	
insert into c(cname,cteacher)values('语文','张老师');
insert into c(cname,cteacher)values('政治','王老师');
insert into c(cname,cteacher)values('英语','李老师');
insert into c(cname,cteacher)values('数学','赵老师');
insert into c(cname,cteacher)values('物理','黎明');

insert into s(sname)values('学生1');
insert into s(sname)values('学生2');
insert into s(sname)values('学生3');
insert into s(sname)values('学生4');

insert into sc(sno,cno,scgrade)values(1,1,40);
insert into sc(sno,cno,scgrade)values(1,2,30);
insert into sc(sno,cno,scgrade)values(1,3,20);
insert into sc(sno,cno,scgrade)values(1,4,80);
insert into sc(sno,cno,scgrade)values(1,5,60);
insert into sc(sno,cno,scgrade)values(2,1,60);
insert into sc(sno,cno,scgrade)values(2,2,60);
insert into sc(sno,cno,scgrade)values(2,3,60);
insert into sc(sno,cno,scgrade)values(2,4,60);
insert into sc(sno,cno,scgrade)values(2,5,40);
insert into sc(sno,cno,scgrade)values(3,1,60);
insert into sc(sno,cno,scgrade)values(3,2,80);
```

## 14 列出所有员工及领导的姓名

最高的领导的领导为null，因此需要使用左连接

```sql
mysql> select a.ename as empname,b.ename as leadname from emp a left join emp b on a.mgr=b.empno;
+---------+----------+
| empname | leadname |
+---------+----------+
| SIMITH  | FORD     |
| ALLEN   | BLAKE    |
| WARD    | BLAKE    |
| JONES   | KING     |
| MARTIN  | BLAKE    |
| BLAKE   | KING     |
| CLARK   | KING     |
| SCOTT   | JONES    |
| KING    | NULL     |
| TURNER  | BLAKE    |
| ADAMS   | SCOTT    |
| JAMES   | BLAKE    |
| FORD    | JONES    |
| MILLER  | CLARK    |
+---------+----------+
```



## 15 列出受雇日期早于其上级的所有员工的编号、姓名、部门名称

```sql
mysql> select e.empno,e.ename,e.hiredate,l.ename,l.hiredate from emp e join emp l on e.mgr=l.empno and e.hiredate<l.hiredate;
+-------+--------+------------+-------+------------+
| empno | ename  | hiredate   | ename | hiredate   |
+-------+--------+------------+-------+------------+
|  7369 | SIMITH | 1980-12-17 | FORD  | 1981-12-03 |
|  7499 | ALLEN  | 1981-02-20 | BLAKE | 1981-05-01 |
|  7521 | WARD   | 1981-02-22 | BLAKE | 1981-05-01 |
|  7566 | JONES  | 1981-04-02 | KING  | 1981-11-17 |
|  7698 | BLAKE  | 1981-05-01 | KING  | 1981-11-17 |
|  7782 | CLARK  | 1981-06-09 | KING  | 1981-11-17 |
+-------+--------+------------+-------+------------+
```



## 16 列出部门名称和这些部门的员工信息，同时列出没有员工的部门

有部门没有员工，所以需要使用左连接或右连接

```sql
mysql> select d.dname,e.* from dept d left join emp e on d.deptno=e.deptno;
+-------------+-------+--------+-----------+------+------------+---------+---------+--------+
| dname       | empno | ename  | job       | mgr  | hiredate   | sal     | comm    | deptno |
+-------------+-------+--------+-----------+------+------------+---------+---------+--------+
| RESEARCHING |  7369 | SIMITH | CLERK     | 7902 | 1980-12-17 |  800.00 |    NULL |     20 |
| SALES       |  7499 | ALLEN  | SALESMAN  | 7698 | 1981-02-20 | 1600.00 |  300.00 |     30 |
| SALES       |  7521 | WARD   | SALESMAN  | 7698 | 1981-02-22 | 1250.00 |  500.00 |     30 |
| RESEARCHING |  7566 | JONES  | MANAGER   | 7839 | 1981-04-02 | 2975.00 |    NULL |     20 |
| SALES       |  7654 | MARTIN | SALESMAN  | 7698 | 1981-09-28 | 1250.00 | 1400.00 |     30 |
| SALES       |  7698 | BLAKE  | MANAGER   | 7839 | 1981-05-01 | 2850.00 |    NULL |     30 |
| ACCOUNTING  |  7782 | CLARK  | MANAGER   | 7839 | 1981-06-09 | 2450.00 |    NULL |     10 |
| RESEARCHING |  7788 | SCOTT  | ANALYST   | 7566 | 1987-04-19 | 3000.00 |    NULL |     20 |
| ACCOUNTING  |  7839 | KING   | PRESIDENT | NULL | 1981-11-17 | 5000.00 |    NULL |     10 |
| SALES       |  7844 | TURNER | SALESMAN  | 7698 | 1981-09-08 | 1500.00 |    NULL |     30 |
| RESEARCHING |  7876 | ADAMS  | CLERK     | 7788 | 1987-05-23 | 1100.00 |    NULL |     20 |
| SALES       |  7900 | JAMES  | CLERK     | 7698 | 1981-12-03 |  950.00 |    NULL |     30 |
| RESEARCHING |  7902 | FORD   | ANALYST   | 7566 | 1981-12-03 | 3000.00 |    NULL |     20 |
| ACCOUNTING  |  7934 | MILLER | CLERK     | 7782 | 1982-01-23 | 1300.00 |    NULL |     10 |
| OPERATIONS  |  NULL | NULL   | NULL      | NULL | NULL       |    NULL |    NULL |   NULL |
+-------------+-------+--------+-----------+------+------------+---------+---------+--------+
```



## 17 列出至少有5名员工的所有部门

```sql
mysql> select d.dname,count(e.empno) as number from dept d join emp e on d.deptno=e.deptno group by d.deptno having number>=5;
+-------------+--------+
| dname       | number |
+-------------+--------+
| RESEARCHING |      5 |
| SALES       |      6 |
+-------------+--------+
```



## 18 列出薪水比simith多的所有员工信息

```sql
mysql> select * from emp where sal>(select sal from emp where ename='simith');
+-------+--------+-----------+------+------------+---------+---------+--------+
| empno | ename  | job       | mgr  | hiredate   | sal     | comm    | deptno |
+-------+--------+-----------+------+------------+---------+---------+--------+
|  7499 | ALLEN  | SALESMAN  | 7698 | 1981-02-20 | 1600.00 |  300.00 |     30 |
|  7521 | WARD   | SALESMAN  | 7698 | 1981-02-22 | 1250.00 |  500.00 |     30 |
|  7566 | JONES  | MANAGER   | 7839 | 1981-04-02 | 2975.00 |    NULL |     20 |
|  7654 | MARTIN | SALESMAN  | 7698 | 1981-09-28 | 1250.00 | 1400.00 |     30 |
|  7698 | BLAKE  | MANAGER   | 7839 | 1981-05-01 | 2850.00 |    NULL |     30 |
|  7782 | CLARK  | MANAGER   | 7839 | 1981-06-09 | 2450.00 |    NULL |     10 |
|  7788 | SCOTT  | ANALYST   | 7566 | 1987-04-19 | 3000.00 |    NULL |     20 |
|  7839 | KING   | PRESIDENT | NULL | 1981-11-17 | 5000.00 |    NULL |     10 |
|  7844 | TURNER | SALESMAN  | 7698 | 1981-09-08 | 1500.00 |    NULL |     30 |
|  7876 | ADAMS  | CLERK     | 7788 | 1987-05-23 | 1100.00 |    NULL |     20 |
|  7900 | JAMES  | CLERK     | 7698 | 1981-12-03 |  950.00 |    NULL |     30 |
|  7902 | FORD   | ANALYST   | 7566 | 1981-12-03 | 3000.00 |    NULL |     20 |
|  7934 | MILLER | CLERK     | 7782 | 1982-01-23 | 1300.00 |    NULL |     10 |
+-------+--------+-----------+------+------------+---------+---------+--------+
```

​	

## 20 列出最低薪水大于1500的各种工作及从事此工作的全部雇员人数

```sql
SELECT job,COUNT(empno) AS num, MIN(sal) AS minsal FROM emp GROUP BY job HAVING minsal>1500;
+-----------+-----+---------+
| job       | num | minsal  |
+-----------+-----+---------+
| MANAGER   |   3 | 2450.00 |
| ANALYST   |   2 | 3000.00 |
| PRESIDENT |   1 | 5000.00 |
+-----------+-----+---------+
```

## 21 列出在部门sales工作的员工的姓名，假定不知道销售部的部门编号

```sql
mysql> select e.ename,d.dname from emp e join dept d on e.deptno=d.deptno and d.dname='sales';
+--------+-------+
| ename  | dname |
+--------+-------+
| ALLEN  | SALES |
| WARD   | SALES |
| MARTIN | SALES |
| BLAKE  | SALES |
| TURNER | SALES |
| JAMES  | SALES |
+--------+-------+
```

## ##22 列出薪水高于公司平均薪水的所有员工，所在部门，上级领导，薪水等级

需要使用多表连接，将三张表连接在一起。

先取出公司平均薪水

```sql
mysql> select avg(sal) as avgsal from emp;
+-------------+
| avgsal      |
+-------------+
| 2073.214286 |
+-------------+
```

然后多表连接

```sql
SELECT e.ename,e.sal,d.dname,l.ename AS leadername,s.grade
	FROM emp e
	JOIN dept d ON e.deptno=d.deptno
	LEFT JOIN emp l ON e.mgr=l.empno  #易错！！！一定要用外连接保证king也能显示出来
	JOIN salgrade s ON e.sal BETWEEN s.losal AND s.hisal
	HAVING sal>(SELECT AVG(sal) AS avgsal FROM emp);
+-------+---------+-------------+--------------+-------+
| ename | sal     | dname       |  leadername  | grade |
+-------+---------+-------------+--------------+-------+
| JONES | 2975.00 | RESEARCHING |    KING      |     4 |
| BLAKE | 2850.00 | SALES       |    KING      |     4 |
| CLARK | 2450.00 | ACCOUNTING  |    KING      |     4 |
| SCOTT | 3000.00 | RESEARCHING |    JONES     |     4 |
| KING  | 5000.00 | ACCOUNTING  |    (null)    |     5 |
| FORD  | 3000.00 | RESEARCHING |    JONES     |     4 |
+-------+---------+-------------+--------------+-------+
```

## #23 列出与scott从事相同工作的所有员工及部门名称

先找出scott的岗位

```sql
mysql> select job from emp where ename='scott';
+---------+
| job     |
+---------+
| ANALYST |
+---------+
```

然后找出从事此工作的员工并与部门表连接显示部门名，注意排除scott本人

```sql
SELECT e.ename,d.dname 
	FROM emp e 
	JOIN dept d ON e.deptno=d.deptno 
		AND e.job=(SELECT job FROM emp WHERE ename='scott') 
		AND e.ename!='scott';
+-------+-------------+
| ename | dname       |
+-------+-------------+
| FORD  | RESEARCHING |
+-------+-------------+
```

## 24 列出薪水等于部门30中员工的薪水的其他员工的姓名和薪水

取出部门30的各个员工的薪水

```sql
mysql> select sal from emp where deptno=30;
+---------+
| sal     |
+---------+
| 1600.00 |
| 1250.00 |
| 1250.00 |
| 2850.00 |
| 1500.00 |
|  950.00 |
+---------+
```

然后找出其他部门中是否有员工工资与上表中数据相同

```sql
mysql> select ename,sal from emp where sal in (select sal from emp where deptno=30) and deptno<>30;
Empty set (0.00 sec)
```

## 25 列出薪水高于部门30的全部员工的员工姓名，薪水，部门名称

先取出部门30的最高薪水

```sql
mysql> select sal from emp where deptno=30 order by sal desc limit 1;
+---------+
| sal     |
+---------+
| 2850.00 |
+---------+
```

然后连接员工表和部门表

```sql
mysql> select e.ename,e.sal,d.dname from emp e join dept d where e.deptno=d.deptno and e.sal>(select sal from emp where deptno=30 order by sal desc limit 1);
+-------+---------+-------------+
| ename | sal     | dname       |
+-------+---------+-------------+
| JONES | 2975.00 | RESEARCHING |
| SCOTT | 3000.00 | RESEARCHING |
| KING  | 5000.00 | ACCOUNTING  |
| FORD  | 3000.00 | RESEARCHING |
+-------+---------+-------------+
```

## #26 列出在每个部门工作的员工数量，平均工资和平均服务期限

**！！！错误解答,少了没有员工的部门40**

```sql
SELECT deptno,COUNT(empno) AS number,AVG(sal) AS avgsal,
	IFNULL(AVG((TO_DAYS(NOW())-TO_DAYS(emp.hiredate))/365),0) AS avgtime
	FROM emp
	GROUP BY deptno;
+--------+--------+-------------+-------------+
| deptno | number | avgsal      | avgtime     |
+--------+--------+-------------+-------------+
|     20 |      5 | 2175.000000 | 35.57588000 |
|     30 |      6 | 1566.666667 | 37.84750000 |
|     10 |      3 | 2916.666667 | 37.54886667 |
+--------+--------+-------------+-------------+
```



**注意：以下select 后第一个字段必须为b.deptno,若为a.deptno则没有deptno为40的信息。**

```sql
SELECT b.deptno,COUNT(a.empno) AS number,AVG(a.sal) AS avgsal,IFNULL(AVG((TO_DAYS(NOW())-TO_DAYS(a.hiredate))/365),0) AS avgtime
	FROM emp a
	RIGHT JOIN dept b ON a.deptno=b.deptno	
	GROUP BY a.deptno;
+--------+--------+-------------+-------------+
| deptno | number | avgsal      | avgtime     |
+--------+--------+-------------+-------------+
|     20 |      5 | 2175.000000 | 35.57588000 |
|     30 |      6 | 1566.666667 | 37.84750000 |
|     10 |      3 | 2916.666667 | 37.54886667 |
|     40 |      0 |    0.000000 |  0.00000000 |
+--------+--------+-------------+-------------+
```

## 27 列出所有员工的姓名、部门名称、工资

```sql
mysql> select e.ename,d.dname,e.sal from emp e join dept d on d.deptno=e.deptno;
+--------+-------------+---------+
| ename  | dname       | sal     |
+--------+-------------+---------+
| SIMITH | RESEARCHING |  800.00 |
| ALLEN  | SALES       | 1600.00 |
| WARD   | SALES       | 1250.00 |
| JONES  | RESEARCHING | 2975.00 |
| MARTIN | SALES       | 1250.00 |
| BLAKE  | SALES       | 2850.00 |
| CLARK  | ACCOUNTING  | 2450.00 |
| SCOTT  | RESEARCHING | 3000.00 |
| KING   | ACCOUNTING  | 5000.00 |
| TURNER | SALES       | 1500.00 |
| ADAMS  | RESEARCHING | 1100.00 |
| JAMES  | SALES       |  950.00 |
| FORD   | RESEARCHING | 3000.00 |
| MILLER | ACCOUNTING  | 1300.00 |
+--------+-------------+---------+
```

## 28 列出所有部门的详细信息和人数

```sql
mysql> select d.*,count(e.ename) as totalemp from dept d left join emp e on d.deptno=e.deptno group by d.deptno;
+--------+-------------+----------+----------+
| deptno | dname       | loc      | totalemp |
+--------+-------------+----------+----------+
|     20 | RESEARCHING | DALLAS   |        5 |
|     30 | SALES       | CHICAGO  |        6 |
|     10 | ACCOUNTING  | NEW YORK |        3 |
|     40 | OPERATIONS  | BOSTON   |        0 |
+--------+-------------+----------+----------+
```

## 29 列出各种工作的最低工资以及从事此工作的雇员姓名

先取出各种工作的最低工资

```sql
mysql> select job,min(sal) as minsal from emp group by job;
+-----------+---------+
| job       | minsal  |
+-----------+---------+
| CLERK     |  800.00 |
| SALESMAN  | 1250.00 |
| MANAGER   | 2450.00 |
| ANALYST   | 3000.00 |
| PRESIDENT | 5000.00 |
+-----------+---------+
```

然后从员工表emp中找出岗位和工资与上表相同的员工

```sql
mysql> select e.ename,t.job,t.minsal from emp e join (select job,min(sal) as minsal from emp group by job) t on e.sal=t.minsal and e.job=t.job;
+--------+-----------+---------+
| ename  | job       | minsal  |
+--------+-----------+---------+
| SIMITH | CLERK     |  800.00 |
| WARD   | SALESMAN  | 1250.00 |
| MARTIN | SALESMAN  | 1250.00 |
| CLARK  | MANAGER   | 2450.00 |
| SCOTT  | ANALYST   | 3000.00 |
| KING   | PRESIDENT | 5000.00 |
| FORD   | ANALYST   | 3000.00 |
+--------+-----------+---------+
```

## 30 列出各个部门的manager的最低薪水

```sql
mysql> select min(sal),deptno from emp where job='manager' group by deptno;
+----------+--------+
| min(sal) | deptno |
+----------+--------+
|  2975.00 |     20 |
|  2850.00 |     30 |
|  2450.00 |     10 |
+----------+--------+
```

显示姓名和职业的话，可以再与员工表emp进行表连接

```sql
SELECT e.ename,e.job,t.minsal,t.deptno
	FROM emp e 
	JOIN (SELECT deptno,MIN(sal) AS minsal FROM emp WHERE job='manager' GROUP BY deptno) t
	ON e.sal=t.minsal AND e.job = 'manager';
+-------+---------+---------+--------+
| ename | job     | minsal  | deptno |
+-------+---------+---------+--------+
| JONES | MANAGER | 2975.00 |     20 |
| BLAKE | MANAGER | 2850.00 |     30 |
| CLARK | MANAGER | 2450.00 |     10 |
+-------+---------+---------+--------+
```

## 31 列出员工的年工资，按年薪从高到低排序

```sql
mysql> select ename,sal*12 as yearsal from emp order by sal desc;
+--------+----------+
| ename  | yearsal  |
+--------+----------+
| KING   | 60000.00 |
| SCOTT  | 36000.00 |
| FORD   | 36000.00 |
| JONES  | 35700.00 |
| BLAKE  | 34200.00 |
| CLARK  | 29400.00 |
| ALLEN  | 19200.00 |
| TURNER | 18000.00 |
| MILLER | 15600.00 |
| WARD   | 15000.00 |
| MARTIN | 15000.00 |
| ADAMS  | 13200.00 |
| JAMES  | 11400.00 |
| SIMITH |  9600.00 |
+--------+----------+
```

## 32 求出员工领导的薪水超过3000的员工姓名和领导姓名

```sql
mysql> select a.ename empname,b.ename leadname,b.sal from emp a join emp b on a.mgr=b.empno and b.sal>3000;
+---------+----------+---------+
| empname | leadname | sal     |
+---------+----------+---------+
| JONES   | KING     | 5000.00 |
| BLAKE   | KING     | 5000.00 |
| CLARK   | KING     | 5000.00 |
+---------+----------+---------+
```

## #33 求出部门名称中，带有‘s’字符的部门员工的工资合计，部门人数

**解答1:**

```sql
SELECT t.deptno,t.dname,IFNULL(SUM(e.sal),0) AS sumsal,IFNULL(COUNT(e.deptno),0) AS totalemp 
	FROM emp e 
	RIGHT JOIN (SELECT deptno,dname FROM dept WHERE dname LIKE '%s%') t ON e.deptno=t.deptno
	GROUP BY e.deptno;
+--------+-------------+----------+----------+
| deptno | dname       | sumsal   | totalemp |
+--------+-------------+----------+----------+
|     20 | RESEARCHING | 10875.00 |        5 |
|     30 | SALES       |  9400.00 |        6 |
|     40 | OPERATIONS  |     0.00 |        0 |
+--------+-------------+----------+----------+
```

**解答2：**

```sql
mysql> select d.deptno,d.dname,ifnull(sum(e.sal),0) as sumsal,ifnull(count(e.ename),0) as totalemp from dept d left join emp e on d.deptno=e.deptno where d.dname like '%s%' group by d.deptno;
+--------+-------------+----------+----------+
| deptno | dname       | sumsal   | totalemp |
+--------+-------------+----------+----------+
|     20 | RESEARCHING | 10875.00 |        5 |
|     30 | SALES       |  9400.00 |        6 |
|     40 | OPERATIONS  |     0.00 |        0 |
+--------+-------------+----------+----------+
```

## 34 给任职时间超过30年的员工加薪10%

```sql
update emp set sal=sal*1.1 where (to_days(now())-to_days(hiredate))/365>30;
```

