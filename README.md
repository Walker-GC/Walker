<h1 align="center">quick4j</h1>


**quick4j** 是一个通用JavaWeb项目骨架， 
积极在探索使用Java、Web等一系列技术，搭建开发高性能、高可拓展性、高可维护性，高安全性的web项目；
以及Web前端模块化、组件化开发，与后台最佳的交互；以及探索使用NoSQL、与SQL等多数据库共存的解决方案；
提供大量模块参考，比如：权限管理模块。

作为一个 seed(种子) 项目，你可以基于它，快速搭建项目原型，并开发。


## 项目情况
> 此项目本是在3年前，开发新的项目时，采用ssm搭建的一个原型；完成新项目后，本着开源的精神，整理共享出来，帮助后人，这就是现在的quick4j。  
> 但无心插柳柳成荫，超过500多的forks，几百份求助邮件，每天几十Clones，让我倍感欣慰，为开源贡献了微薄的力量。  
> 前段时间，由于该项目中包含有IntelliJ IDEA软件的keygen，被GitHub Ban了。删除相关代码后，由于工作太忙，也没有申诉了。  
> 技术的更替，是值得鼓励的。我是一个喜欢挑战的人，我现在已经转战JavaScript，所以以后也没时间、没精力更新此项目，抱歉大家。  
> 当然，我喜欢开源软件，正是因为开源，集众人的智慧，技术才会快速发展，加快人类的对未知事物探索。让我们做的更好吧。


## 你可以使用 **quick4j**：
> * 快速搭建项目原型，基于Spring + Spring MVC + Mybatis，简单轻便、易于扩展的架构，适用于大多数项目
> * 封装了常用的CURD，配合mybatis-generator 自动生成dao、model、mapper层，减少重复劳动，提高生产力，实现快速、平稳的开发
> * 实现Mybatis的分页查询模块，支持MySQL、PostgreSQL、SQLServer等数据库分页查询
> * 通用的权限管理模块，基于Apache Shiro的 用户-角色-权限(RBAC)的细粒度权限控制
> * 大量配置示例，根据需求，自由优化、调整，达到最佳性能
> * 大量前端模块化开发示例，积极在探索前端最佳的架构，与后台最佳的交互，构建雄心勃勃的Application


## How to get/use it
> 
* 1、git clone https://github.com/Eliteams/quick4j.git
* 2、在MySQL中导入 quick4j/src/test/resources/quick4j.sql 脚本
* 3、更新 quick4j/src/main/resources/application.properties 中 JDBC Global Setting
* 4、cd quick4j
* 5、mvn war:war 
* 6、把 quick4j/target/quick4j.war 发布在App Server(Tomcat、JBOSS) 中


## 在IDE 中查看源码并运行
> #### 1 、在IntelliJ IDEA
* File -> Import Project -> select quick4j folder -> create project form existing sources -> ...

> #### 2、 在Eclipse
* File -> Import -> Existing Maven Projects -> ...


## If you have a better suggestion,Please share out,Let's do better.
> Author ：StarZou  
> Email  ：starzou@126.com


## License
> Copyright (c) 2016 StarZou.  
> Licensed under the MIT license.  

> Welcome to stars, forks, create new pull request.

2017/7/28 Walker Liu
Below is personal note about sql server:

create database test_lgc;
use COMPUPTD;
select * from dbo.ACCT;
--查看表结构
sp_help ASD;--alt+f1

--查看当前ServerName、DB Name
SELECT @@SERVERNAME, DB_NAME()

--字符串函数
/*字符串拼接
mysql:concat()
oracle:concat(),||
sql server:+
*/
select id, name from #student2
select (convert(varchar(100), id)+''+name) as 'id+name' from #student2
--select * from Results
--select report_id+''+field_desc from results

/*
字符串长度
oralce:length()
sql server:len()
*/
select name, len(name) from #student2

/*
大小写转换
sql server没有initcap函数（首字母大写）
*/
select upper(name), lower(name) from #student2

/*
lpad,rpad补位函数(使左对齐、右对齐)
sql server不支持，需使用replicate
*/
select right(replicate('0',10)+'123',10) 右对齐, '123'+replicate('0',10-len('123')) 左对齐

/*
截取字符串
oracle:substr(str,m,n)
sql server:substring(str,m,n),left(str,m),right(str,m)
*/
select substring(name,1,3) from #student2
select left(name,3) from #student2
select right(name,3) from #student2

/*
查看字符串位置
oracle:instr(str,str2,m,n)查找str2在str中从第m个字符开始，第n次出现的位置
sql server:charindex(str2,str)
*/
select charindex('s','Hauss') 

/*
数字函数
*/

/*
日期函数
*/
select distinct ACCTCODE from dbo.ACCT;
select distinct top 3 with ties ACCTCR from dbo.ACCT order by ACCTCR;
select 15%4 as Result;

select convert(varchar(20),getdate(),10) as Time;
select Date = convert(varchar(12),getdate(),23);

select * from dbo.ACCT;
select DateName(Year,GetDate()) as Year;
select DatePart(DayOfYear,GetDate()) as DayCount;
select DatePart(DW,GetDate()) as DayOfWeek;	--从Sunday开始

--select cast(char(10),getdate(),101) as datetime;

select
	DATEDIFF(yyyy,'1984/5/20',GETDATE()) as MarriedYears,
	DATEDIFF(dd,'1984/5/20',GETDATE()) as MarriedDays;
select DATEADD(hh,100,GETDATE()) as [100HoursFromNow ];
--convert()函数的用法
select GETDATE() as RawDate,
	CONVERT (nvarchar(25),getdate(),100) as Date100,
	CONVERT (nvarchar(25),getdate(),1) as Date1;
--Str(number,length,decimal):将数字转换成字符串
select STR(123,5,2) as [Str];

/*
分页查询
mysql:limit
oracle:rownum
sql server:
*/
--top方案：
--select * from #grade2 order by score desc
/*select top 10 * from #grade2
where id not in(select top 3 id from #grade2 order by score desc)
order by score desc

--max:
select top 10 * from #grade2
where id<(select max(score) from (select top 3 id, score from #grade2 order by score desc)tt)
order by score desc
*/

--从第6条开始，查询10条
select top 15 * from #grade2 order by score desc
select *
from (select row_number() over(order by score desc) page_number, *
		from (select top 15 * from #grade2 order by score desc) t
	 )tt
where page_number>5



/*
1、from子句组装来自不同数据源的数据
2、where子句基于指定的条件对记录进行筛选
3、group by子句将数据划分为多个分组
4、使用聚合函数进行计算
5、使用having子句筛选分组
6、计算所有的表达式
7、使用order by对结果集进行排序
*/

select *
from dbo.ACCT a
where a.ACCTDB=a.ACCTCR;

--事务
begin tran
create table GoldInventory(
InventoryId int primary key
)
--commit
rollback

--管理存储过程
--create、alter和drop
go
create procedure datalist
as
select * from dbo.ACCT;

--执行存储过程,返回结果集

exec datalist;

--存储过程的编译是自动进行的
--使用sp_recompile对存储过程（或触发器）标记，以便在下次执行时对其进行重新编译
exec sp_recompile datalist;

select cast(c.sql as char(35)) as StoredProcedure
from Master.dbo.SysDatabases c
order by StoredProcedure;

select * from dbo.tcomm1;
sp_help tcomm1;
sp_help;
select @@Connections as RequestCount;
select @@CPU_BUSY;

declare @column1 char,@column2 char,@column3 char,@column4 char,@column5 char;

--查询有多少符合条件的行,select count(1) as XXX from XXX where ...
select count(1) as cnt  from tmfplmstr  where plan_number = '10198';
select plan_number from tmfplmstr;
sp_help tmfplmstr;


create table T_Employee_Walker
(ID		int not null,
 NAME	varchar(10) not null,
 GENDER	char(1) not null,
 JOB	varchar(10),
 DEPNO	char(10)
)

drop table T_Employee_Walker

--序列Sequence
--6/13/2017
--create SEQUENCE SEQ_Employee_ID as bigint
--increment by 1
--start with 1
--nomaxvalue
--nocycle
--nocache

--CREATE SEQUENCE dbo.seq_test AS BIGINT
--START WITH 1
--INCREMENT BY 1

--触发器
--6/14/2017
/*
SQL Server 2000 支持两种类型的触发器：AFTER 触发器和INSTEAD OF 触发器。
其中AFTER触发器即为SQL Server 2000 版本以前所介绍的触发器。该类型触发器要求只有执行某一操作（INSERT UPDATE DELETE）之后，
触发器才被触发，且只能在表上定义。可以为针对表的同一操作定义多个触发器。对于AFTER 触发器，可以定义哪一个触发器被最先触发，
哪一个被最后触发，通常使用系统过程sp_settriggerorder 来完成此任务。 
INSTEAD OF 触发器表示并不执行其所定义的操作（INSERT、 UPDATE、 DELETE），而仅是执行触发器本身。既可在表上定义INSTEAD OF 触发器，
也可以在视图上定义INSTEAD OF 触发器，但对同一操作只能定义一个INSTEAD OF 触发器。
*/
/*
create trigger tri_xxx
on tbl_xxx
for before/after insert/delete/update/
as
*/

create table T_Employee_Walker
(ID		int not null,
 NAME	varchar(10) not null,
 GENDER	char(1) not null,
 JOB	varchar(10),
 DEPNO	char(10)
)


create trigger TGR_Employee_Walker
on T_Employee_Walker
--with encryption
for insert
as
select getdate()

select * from T_Employee_Walker
insert into T_Employee_Walker(ID, NAME, GENDER, JOB, DEPNO)
values('001', 'Jack', 'M', 'Programmer', 'C')

--identity用法
--低版本sql server不支持sequence
--Identity column must be of data type int, bigint, smallint, tinyint, or decimal or numeric with a scale of 0, and constrained to be nonnullable.
--6/14/2017
drop table T_Employee_Walker

create table T_Employee_Walker
(
	ID		int primary key identity(1, 1),
	NAME	varchar(10) not null,
	GENDER	char(1) not null,
	JOB	varchar(10),
	DEPNO	char(10),
	LASTUPDATESTMP datetime null
 )

insert into T_Employee_Walker(NAME, GENDER, JOB, DEPNO, LASTUPDATESTMP)
values('Hauss', 'M', 'Programmer', 'C', getdate())
insert into T_Employee_Walker(NAME, GENDER, JOB, DEPNO, LASTUPDATESTMP)
values('Joseph', 'M', 'Programmer', 'C', getdate())
insert into T_Employee_Walker(NAME, GENDER, JOB, DEPNO, LASTUPDATESTMP)
values('Linda', 'F', 'Programmer', 'C', getdate())

select convert(varchar(30), getdate(), 21)
select 'Current Datetime is :' + convert(varchar(30), getdate(), 20)
select 'Current Datetime is :' + convert(varchar(30), getdate(), 23)

DELETE FROM T_Employee_Walker
select * from T_Employee_Walker

--***********************************************************************************************
--test 4 trigger begin
select * from T_Employee_Walker
drop table T_Employee_Walker
drop trigger TGR_Employee_Walker

create trigger tri_employee_insert
on T_Employee_Walker
after insert
as
begin
select * from T_Employee_Walker
select getdate()
end

create trigger tri_employee_delete
on T_Employee_Walker
after delete
as
begin
if not exists (select * from dbo.sysobjects where name = 'T_Employee_Copy')
create table T_Employee_Copy
(ID				int,
 NAME			varchar(10) not null,
 GENDER			char(1) not null,
 JOB			varchar(10),
 DEPNO			char(10),
 LASTUPDATESTMP datetime null
)
insert into T_Employee_Copy
select * from DELETED 
end

delete from T_Employee_Walker where ID in (2, 3)
select * from T_Employee_Walker
select * from T_Employee_Copy

insert into T_Employee_Walker select NAME, GENDER, JOB, DEPNO, LASTUPDATESTMP from T_Employee_Copy

--test 4 trigger end
--***********************************************************************************************

--replicate函数:以指定的次数重复字符串
--sql server不支持lpad、rpad函数
--6/14/2017
--select LPAD('a', 10, '@')

declare @sql varchar(200), --需填充的字符串 
@char varchar(4), --填充使用的字符 
@len int --填充后的长度 
select @sql='abc' 
select @char='0' 
select @len=10 
select (right(replicate(@char,@len)+@sql,@len)) 右对齐 
,@sql+replicate(@char,@len-LEN(@sql)) 左对齐 

select right('abcdefghijklmn', 5)
select charindex('g', 'abcdefghijklmn')--7


insert into T_Employee_Walker(ID, NAME, GENDER, JOB, DEPNO, LASTUPDATESTMP)
values(right(replicate('0', 10)+'1', 10), 'Corn', 'M', 'Programmer', 'C', getdate())

select str(123.436,20), len(str(123.436,20))

--返回带有分隔符的 Unicode 字符串，分隔符的加入可使输入的字符串成为有效的 SQL Server 分隔标识符。
--quotename('character_string')  character_string 不得超过 128 个字符。超过 128 个字符的输入将返回 NULL。
select QUOTENAME('abcde', ''''), QUOTENAME('abcde','{}'), QUOTENAME('abcde', '[]')

--发音匹配度
select NAME, SOUNDEX(NAME) Sound_Match_Level from T_Employee_Walker

--DIFFERENCE(NAME, 'Jerry')
--返回一个0~4之间的值来反映两个字符串的发音相似度，这个值越大则表示两个字符串的发音相似度越大。
select NAME, SOUNDEX(NAME) as Sound_Match_Level, DIFFERENCE(NAME, 'Jerry') as Sound_Similarity from T_Employee_Walker

--6/14/2017
create table T_Student
(
	Student_ID	int primary key identity(1,1),
	Name		nvarchar(10) not null,
	Gender		char(1) not null,
	Age			int not null,
	Class_ID	int not null,
	Class_Name	nvarchar(10) not null,
	constraint chkAge check(Age between 0 and 100)
)

drop table T_Class
begin tran
create table T_Class
(
	Class_ID		int not null,
	Class_Name		nvarchar(10) not null,
	Student_Name	nvarchar(10) not null
)

insert into T_Class (Class_ID, Class_Name, Student_Name) values (401, N'四（1）班', N'张三')
insert into T_Class (Class_ID, Class_Name, Student_Name) values (401, N'四（1）班', N'李四')
insert into T_Class (Class_ID, Class_Name, Student_Name) values (401, N'四（1）班', N'王五')
insert into T_Class (Class_ID, Class_Name, Student_Name) values (401, N'四（1）班', N'陈六')
insert into T_Class (Class_ID, Class_Name, Student_Name) values (403, N'四（3）班', N'陈汉')
insert into T_Class (Class_ID, Class_Name, Student_Name) values (408, N'四（8）班', N'张飞')

select * from T_Class

rollback
commit

insert into T_Student (Name, Gender, Age, Class_ID, Class_Name) values (N'张三', 'M', 10, 401, N'四（1）班')
insert into T_Student (Name, Gender, Age, Class_ID, Class_Name) values (N'李四', 'M', 11, 402, N'四（2）班')
insert into T_Student (Name, Gender, Age, Class_ID, Class_Name) values (N'王五', 'M', 10, 403, N'四（3）班')
insert into T_Student (Name, Gender, Age, Class_ID, Class_Name) values (N'陈六', 'M',  9, 404, N'四（4）班')
insert into T_Student (Name, Gender, Age, Class_ID, Class_Name) values (N'徐磊', 'M', 10, 405, N'四（5）班')

select * from T_Student

--【内联接】：只连接匹配的行。inner join
--inner可省略。最常见的一种连接，也被称为普通连接，最早称之为自然连接。

--等值联接
select * from T_Student a, T_Class b where a.Class_ID = b.Class_ID
--等价于
select * from T_Student a inner join T_Class b on a.Class_ID = b.Class_ID
--不等联接(>,<,<>,>=,<=,!>,!<)
select * from T_Student a inner join T_Class b on a.Class_ID <> b.Class_ID

--【外联接】outer join
--outer可省略

--左外联接：包含左边表的全部行（不管右边的表中是否存在与它们匹配的行），以及右边表中全部匹配的行
select * from T_Student a left join T_Class b on a.Class_ID = b.Class_ID

--右外联接：包含右边表的全部行（不管左边的表中是否存在与它们匹配的行），以及左边表中全部匹配的行
select * from T_Student a right join T_Class b on a.Class_ID = b.Class_ID

--全(外)联接：包含左、右两个表的全部行，不管另外一边的表中是否存在与它们匹配的行。
select * from T_Student a full join T_Class b on a.Class_ID = b.Class_ID

--交叉联接，不带where
select * from T_Student a cross join T_Class b

--交叉联接，有where子句，往往会先生成两个表行数乘积的数据表，然后才根据where条件从中选择
select * from T_Student a cross join T_Class b
select * from T_Student a cross join T_Class b where a.Class_ID = b.Class_ID
select * from T_Student a, T_Class b
select * from T_Student a, T_Class b where a.Class_ID = b.Class_ID

drop table T_Student, T_Class

--主键

--约束

--ROW_NUMBER:连续不并列排名
SELECT AGE, ROW_NUMBER() OVER(ORDER BY AGE) AS ROW_NUMBER FROM T_Student

--RANK:做不连续排名，有并列
SELECT * FROM T_Student
SELECT Age, RANK() OVER(ORDER BY Age) AS RANK FROM T_Student 

--DENSE_RANK：连续排名，有并列
SELECT Age, DENSE_RANK() OVER(ORDER BY Age) AS RANK FROM T_Student

--group by 和 partition by
/*
	group by 是对检索结果的保留行进行单纯分组，一般总爱和聚合函数例如AVG(),COUNT(),MAX(),MIN()一块使用
	partition by虽然也具有分组功能，同时还具有其它功能
*/
CREATE TABLE A
(
	B CHAR(2),
	C CHAR(2),
	D INT
)
INSERT INTO A(B, C, D) VALUES('02', '02', 1)
INSERT INTO A VALUES('02', '03', 2)
INSERT INTO A VALUES('02', '04', 3)
INSERT INTO A VALUES('02', '05', 4)
INSERT INTO A VALUES('02', '01', 5)
INSERT INTO A VALUES('02', '06', 6)
INSERT INTO A VALUES('02', '07', 7)
INSERT INTO A VALUES('02', '03', 5)
INSERT INTO A VALUES('02', '02', 12)
INSERT INTO A VALUES('02', '01', 2)
INSERT INTO A VALUES('02', '01', 23)
SELECT * FROM A
drop table A

--BEGIN TEST
SELECT * FROM A
SELECT B, C, SUM(D) E FROM A GROUP BY B, C

--test4 group by
create table #test_fruitinfo
(
	FruitName varchar(20),
	ProductPlace varchar(20),
	Price smallmoney
)
insert into #test_fruitinfo
select FruitName = 'Apple', ProductPlace = 'China', Price = $1.1 union all
select FruitName = 'Apple', ProductPlace = 'Japan', Price = $2.1 union all
select FruitName = 'Apple', ProductPlace = 'USA', Price = $2.5 union all
select FruitName = 'Orange', ProductPlace = 'China', Price = $0.8 union all
select FruitName = 'Banana', ProductPlace = 'China', Price = $3.1 union all
select FruitName = 'Peach', ProductPlace = 'USA', Price = $3.0
select * from #test_fruitinfo

select ProductPlace, count(*)
from #test_fruitinfo
group by ProductPlace
having count(*) >1

drop table #test_fruitinfo

/*使用分析函数*/
SELECT * FROM A
SELECT B, C, SUM(D) E FROM A GROUP BY B, C
SELECT B, C, SUM(D) OVER(PARTITION BY B, C) E FROM A

SELECT B, C, D, SUM(D) OVER(PARTITION BY B, C) E FROM A
--SELECT B, C, D, SUM(D) OVER(PARTITION BY B, C ORDER BY D) E FROM A	--sql server不支持
SELECT * FROM A WHERE EXISTS (SELECT D FROM A WHERE D IN(0, 50))

/*Below is tested for Union/Union all/Except/Intersect.*/
CREATE TABLE AB
(
	B CHAR(2),
	C CHAR(2),
	D INT
)
INSERT INTO AB
SELECT '02', '03', 5 UNION ALL
SELECT '02', '02', 12 UNION ALL
SELECT '02', '01', 2 UNION ALL
SELECT '02', '01', 23 UNION ALL
SELECT '05', '01', 15 UNION ALL
SELECT '05', '08', 24 UNION ALL
SELECT '08', '10', 5
SELECT * FROM AB

--union:合并，过滤重复数据
select * from A
union select * from AB

--union all:不过滤重复数据
select * from A
union all select * from AB

--except:过滤掉前表中，两张表的交集
select B from A
select B from AB
select B from AB
except select B from A

--Intersect:取交集
select * from A
select * from AB
select * from AB
intersect select * from A
drop table A, AB
--END TEST

CREATE TABLE Employee  
(  
  ID int identity(1,1),  
  EmpName nvarchar(20),  
  EmpSalary varchar(10),  
  EmpDepartment nvarchar(20)   
);  
  
INSERT INTO Employee  
SELECT N'张三','5000',N'开发部' UNION ALL  
SELECT N'李四','2000',N'销售部'  UNION ALL  
SELECT N'王麻子','2500',N'销售部' UNION ALL  
SELECT N'张三表叔','8000',N'开发部' UNION ALL  
SELECT N'李四表叔','5000',N'开发部' UNION ALL  
SELECT N'王麻子表叔','5000',N'销售部'

SELECT * ,ROW_NUMBER() OVER(PARTITION BY EmpDepartment ORDER BY EmpSalary) EmpNumber FROM Employee
--取得最高工资的员工信息
--1.
SELECT *, MAX(EmpSalary) OVER(PARTITION BY EmpDepartment) MaxSalary FROM Employee
--2.
SELECT EmpDepartment, MAX(EmpSalary) FROM Employee GROUP BY EmpDepartment 

DROP TABLE Employee

/* select * from abc
	go
	select getdate() */

select dateadd(month,1,getdate())

--******************************************

/*
查询出:学生平均成绩第2、3的学生信息和课程信息
*/
create table #course2(
	id int primary key identity(1,1),
	name varchar(15)
)
select * from #course2
insert into #course2
select name = 'java' union all
select name = 'sql server' union all
select name = 'javascript' union all
select name = 'vb.net'

create table #student2(
	id int primary key identity(1,1),
	name varchar(10)
)
insert into #student2
select name = 'Hauss' union all
select name = 'David' union all
select name = 'Sean' union all
select name = 'Joseph' union all
select name = 'Hunter' union all
select name = 'Jerry' union all
select name = 'Leo' union all
select name = 'Harry' union all
select name = 'Corn' union all
select name = 'Jack'
select * from #student2

create table #grade2(
	id int primary key identity(1,1),
	c_id int,
	s_id int,
	score int
)
insert into #grade2
select c_id = 1, s_id = 1, score = 88 union all
select c_id = 1, s_id = 2, score = 98 union all
select c_id = 1, s_id = 3, score = 76 union all
select c_id = 1, s_id = 4, score = 95 union all
select c_id = 1, s_id = 5, score = 91 union all
select c_id = 1, s_id = 6, score = 82 union all
select c_id = 1, s_id = 7, score = 87 union all

select c_id = 2, s_id = 1, score = 96 union all
select c_id = 2, s_id = 2, score = 85 union all
select c_id = 2, s_id = 3, score = 78 union all
select c_id = 2, s_id = 4, score = 71 union all
select c_id = 2, s_id = 5, score = 92 union all
select c_id = 2, s_id = 6, score = 88 union all
select c_id = 2, s_id = 7, score = 71 union all
select c_id = 2, s_id = 8, score = 91 union all
select c_id = 2, s_id = 9, score = 82 union all

select c_id = 3, s_id = 1, score = 97 union all
select c_id = 3, s_id = 2, score = 83 union all
select c_id = 3, s_id = 3, score = 89 union all
select c_id = 3, s_id = 4, score = 78 union all
select c_id = 3, s_id = 5, score = 63 union all
select c_id = 3, s_id = 6, score = 77 union all
select c_id = 3, s_id = 7, score = 86 union all
select c_id = 3, s_id = 8, score = 71 union all

select c_id = 4, s_id = 1, score = 86 union all
select c_id = 4, s_id = 2, score = 89 union all
select c_id = 4, s_id = 3, score = 81 union all
select c_id = 4, s_id = 4, score = 74 union all
select c_id = 4, s_id = 5, score = 67 union all
select c_id = 4, s_id = 6, score = 83 union all
select c_id = 4, s_id = 7, score = 96 union all
select c_id = 4, s_id = 8, score = 91 union all
select c_id = 4, s_id = 9, score = 93 union all
select c_id = 4, s_id = 10, score = 64
select * from #grade2
drop table #grade2

SELECT *, ROW_NUMBER() OVER(PARTITION BY c_id ORDER BY score desc) RankNumber
FROM #grade2
group by c_id

SELECT DISTINCT c.name+',' FROM #course2 c FOR XML PATH('')

--answer 1
select distinct s.id stu_id, s.name stu_name, (SELECT DISTINCT c.name+'  ' FROM #course2 c FOR XML PATH('')) all_course, b.avg_score, b.RankNum
from (select a.*, ROW_NUMBER() OVER(order by avg_score desc) RankNum
	from (select s_id, avg(score) avg_score
		from #grade2 group by s_id) a) b,
	#course2 c, #student2 s
where b.RankNum in (2, 3) and b.s_id = s.id

--answer 2
select distinct tt.page_number, s.id stu_id, s.name stu_name, (select distinct name+'  ' from #course2 for xml path('')) all_courses, tt.avg_score
from (select row_number() over(order by avg_score desc) page_number, *
		from (select top 3 s_id, avg(score) avg_score from #grade2 group by s_id order by avg_score desc) t
	 )tt, #course2 c, #student2 s
where page_number>1 and s.id = tt.s_id

--3

--**************-********

--name	s_id	avg	cou_id	cou_name	score
--David	2	88	1	java	98

select r2.name, r2.s_id as stu_id, r3.course_id, r3.course_name, r3.score, avg_score
from
	(select top 2 r1.*
	from (select top 3 b.name, a.s_id, avg(a.score) as avg_score
		  from #grade2 a join #student2 b on a.s_id = b.id
		  group by b.name, a.s_id order by avg_score desc) r1
	order by avg_score asc) r2
join
	(select t1.id as course_id,t1.name as course_name,t2.id as stu_id,t2.name as stu_name ,t3.score from #course2 t1 , #student2 t2, #grade2 t3
	where t1.id = t3.c_id and t2.id = t3.s_id) r3
on r2.s_id = r3.stu_id
order by avg_score desc
--*************************

select * from #course2
select * from #student2
select * from #grade2
drop table #course2
drop table #student2
drop table #grade2

select b.name,a.s_id,avg(a.score) from #grade2 a
join #student2 b on a.s_id = b.id
group by b.name,a.s_id 

--Hauss
/*
select t1.*, t2.cou_id,t2.cou_name,t2.score from
(select top 2 t.* from (select top 3 b.name,a.s_id,avg(a.score) as avg from #grade2 a
join #student2 b on a.s_id = b.id
group by b.name,a.s_id) t order by avg asc) t1
join (select t1.id as cou_id,t1.name as cou_name,t2.id as stu_id,t2.name as stu_name ,t3.score from #course2 t1 , #student2 t2, #grade2 t3
where t1.id = t3.c_id and t2.id = t3.s_id) t2
on t1.s_id = t2.stu_id
order by s_id 

select t1.*, t2.cou_id,t2.cou_name,t2.score from
(select top 2 t.* from (select top 3 b.name,a.s_id,avg(a.score) as avg from #grade2 a
join #student2 b on a.s_id = b.id
group by b.name,a.s_id order by avg desc) t order by avg asc) t1
join (select t1.id as cou_id,t1.name as cou_name,t2.id as stu_id,t2.name as stu_name ,t3.score from #course2 t1 , #student2 t2, #grade2 t3
where t1.id = t3.c_id and t2.id = t3.s_id) t2
on t1.s_id = t2.stu_id
order by s_id 
*/

--exercise
create table  #student
(
	Sno		char(10) primary key,
	Sname	nvarchar(5),
	Ssex	nchar(1),
	Sage	int,
	Sdept	nvarchar(10)
)

insert into #student
select Sno = '9512101', Sname = N'李勇', Ssex = N'男', Sage = 19, Sdept = N'计算机系'
union all
select Sno = '9512102', Sname = N'刘晨', Ssex = N'男', Sage = 20, Sdept = N'计算机系'
union all
select Sno = '9512103', Sname = N'王敏', Ssex = N'女', Sage = 20, Sdept = N'计算机系'
union all
select Sno = '9521101', Sname = N'张立', Ssex = N'男', Sage = 22, Sdept = N'信息系'
union all
select Sno = '9521102', Sname = N'吴宾', Ssex = N'女', Sage = 21, Sdept = N'信息系'
union all
select Sno = '9521103', Sname = N'张海', Ssex = N'男', Sage = 20, Sdept = N'信息系'
union all
select Sno = '9531101', Sname = N'钱小力', Ssex = N'女', Sage = 18, Sdept = N'数学系'
union all
select Sno = '9531102', Sname = N'王大力', Ssex = N'男', Sage = 19, Sdept = N'数学系'

--select * from #student
--drop table #student

create table #course
(
	Cno		char(5) primary key,
	Cname	nvarchar(10),
	Hours	int
)

insert into #course
select Cno = 'C01', Cname = N'计算机文化学', Hours = 70
union all
select Cno = 'C02', Cname = 'VB', Hours = 90
union all
select Cno = 'C03', Cname = N'计算机网络', Hours = 80
union all
select Cno = 'C04', Cname = N'数据库基础', Hours = 108
union all
select Cno = 'C05', Cname = N'高等数学', Hours = 180
union all
select Cno = 'C06', Cname = N'数据结构', Hours = 72

--select * from #course

create table #sc
(
	Sno	char(10),
	Cno char(5),
	Grade int
)

insert into #sc
select Sno = '9512101', Cno = 'C01', Grade = 90
union all
select Sno = '9512101', Cno = 'C02', Grade = 86
union all
select Sno = '9512101', Cno = 'C06', Grade = NULL
union all
select Sno = '9512102', Cno = 'C02', Grade = 78
union all
select Sno = '9512102', Cno = 'C04', Grade = 66
union all
select Sno = '9521102', Cno = 'C01', Grade = 82
union all
select Sno = '9521102', Cno = 'C02', Grade = 75
union all
select Sno = '9521102', Cno = 'C04', Grade = 92
union all
select Sno = '9521102', Cno = 'C05', Grade = 50
union all
select Sno = '9521103', Cno = 'C02', Grade = 68
union all
select Sno = '9521102', Cno = 'C06', Grade = NULL
union all
select Sno = '9531101', Cno = 'C01', Grade = 80
union all
select Sno = '9531101', Cno = 'C05', Grade = 95
union all
select Sno = '9531102', Cno = 'C05', Grade = 85

--drop table #sc
--select * from #sc

--1.分别查询学生表和课程表的全部数据
select * from #student
select * from #course

--2.查询成绩在70－80的学生的学号、课程号和成绩
select Sno, Cno, Grade
from #sc
where Grade between 70 and 80

--3.查询C01课程成绩最高的分数
select max(Grade) from #sc where Cno = 'C01'
select top 1 Grade from #sc where Cno = 'C01' order by Grade desc

--4.查询学生都选修了哪些课程，列出课程号
select Cname, Cno
from #course
where Cno in (select distinct Cno from #sc)

--5.查询修了C02课程的所有学生的平均成绩、最高成绩和最低成绩
select avg(Grade) avg_grade, max(Grade) max_grade, min(Grade) min_grade
from #sc
where Cno = 'C02'

--6.统计每个系的学生人数
select Sdept, count(*) 人数 from #student group by Sdept

--7.统计每门课程的修课人数和考试最高分
select r.Cno, c.Cname 课程名称, r.修课人数, r.最高分
from #course c, 
	(select Cno, count(Sno) 修课人数, isNull(max(Grade),0) 最高分
	from #sc
	group by Cno) r
where c.Cno = r.Cno

--8.统计每个学生的选课数量，并按选课数量递增显示结果
select r.Sno, s.Sname, r.选课数量
from #student s, 
	(select Sno, count(Cno) 选课数量
	from #sc
	group by Sno) r
where s.Sno = r.Sno
order by r.选课数量 asc

--9.统计选修课的学生总数和考试的平均成绩
select count(distinct Sno) 修课学生总数, avg(Grade) 平均成绩 from #sc

--10.查询选课数量超过2的学生的平均成绩和选课数量
select r.Sno, s.Sname, r.平均成绩, r.选课数量
from #student s
join
(select Sno, avg(Grade) 平均成绩, count(Cno) 选课数量
from #sc
group by Sno having count(Cno)>2) r
on s.Sno = r.Sno

--11.列出总成绩超过200的学生，要求列出学号、总成绩
select Sno 学号, sum(Grade) 总成绩
from #sc
group by Sno having sum(Grade)>200

--12.查询选修了C02课程的学生的姓名和所在系
select s.Sname, s.Sdept, sc.Cno
from #student s
join #sc sc
on s.Sno = sc.Sno
where sc.Cno = 'C02'

select Sname, Sdept
from #student s
join
	(select Sno
	from #sc
	where Cno = 'C02') r
on s.Sno = r.Sno

--13.查询成绩80分以上的学生姓名、课程号和成绩，并按成绩降序显示
select s.Sname, sc.Cno, sc.Grade
from #student s
join #sc sc
on s.Sno = sc.Sno
where sc.Grade>80
order by sc.Grade desc

--14.查询计算机系男生修了“数据库基础”的姓名、性别、成绩
--C04
--select * from #student where Ssex = N'男' and Sdept = '计算机系'
--select * from #course
--select * from #sc
select s.Sname, s.Ssex, c.Cname, sc.Grade
from #student s
join #sc sc
on s.Sno = sc.Sno
join #course c
on c.Cno = sc.Cno
where s.Sdept = N'计算机系' and s.Ssex = N'男' and c.Cname = N'数据库基础'

--15.查询哪些学生的年龄相同,要求列出年龄相同的学生的姓名和年龄。
select a.Sname, a.Sage
from #student a
join #student b
on a.Sage in(select Sage from #student where a.Sage = b.Sage and a.Sname != b.Sname)
group by a.Sname, a.Sage

--16.查询哪些课程没有人选,要求列出课程号和课程名。
select Cno, Cname
from #course
where Cno not in(select distinct Cno from #sc)

--17.查询有考试成绩的所有学生的姓名、修课名称及考试成绩
--要求将查询结果放在一张新的临时表(假设新表名为#new-sc)中。
select Sname 有考试成绩的学生的姓名, Cname 修课名称, Grade  考试成绩
into #new_sc
from #student s, #course c, #sc sc
where sc.Grade != 0 and s.Sno = sc.Sno and C.Cno = sc.Cno
select * from #new_sc

--18.分别查询信息系和计算机系的学生的姓名、性别、修课名称、修课成绩
--并要求将这两个查询结果合并成一个结果集,
--并以系名、姓名、性别、修课名称、修课成绩的顺序显示各列。
--此题用到了并union查询
select s.Sdept 系名, s.Sname 姓名, s.Ssex 性别, c.Cname 修课名称, isNull(sc.Grade, 0) 修课成绩
from #student s, #course c, #sc sc
where s.Sdept in(N'信息系', N'计算机系') and s.Sno = sc.Sno and c.Cno = sc.Cno

--19.用子查询实现如下查询:
--(1) 查询选修了C01号课程的学生的姓名和所在系。
select Sname, Sdept
from #student
where Sno in(select Sno from #sc where Cno = 'C01')

--(2) 查询数学系成绩80分以上的学生的学号、姓名。
select Sno, Sname
from #student
where Sno in(select sc.Sno from #sc sc, #student s where s.Sdept = N'数学系' and sc.Grade > 80)

--(3) 查询计算机系学生所选的课程名.
select Cname
from #course
where Cno in(select Cno
			from #sc
			where Sno in(select Sno	from #student where Sdept = N'计算机系'))

--20.将计算机系成绩高于80分的学生的修课情况插入到另一张表中
--可以先建表再插数据，或者插数据的同时建表
select distinct s.Sno, s.Sname, sc.Cno, c.Cname
into walker_test
from #student s, #sc sc, #course c
where s.Sdept = N'计算机系' and s.Sno = sc.Sno and sc.Grade > 80 and sc.Cno = c.Cno

--select * from walker_test
--9512101   	李勇	C01  	计算机文化学
--9512101   	李勇	C02  	VB
--drop table walker_test

--select distinct Sno from #sc where Grade > 80
--intersect
--select distinct Sno from #student where Sdept = N'计算机系'

drop table #student, #sc, #course
