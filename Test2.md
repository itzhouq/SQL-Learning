# 第三章 数据库管理
## 1. SQL Server中的表达式及运算符
- 表达式的数据

数据  | 说明
---|---
常量 | 包括数字0~9、字母
变量 | 与Java、C语言定义的变量相同，但必须满足SQL Server的命名规则
字段名 | 数据表的列名
函数 | 自定义函数和SQL Server系统函数

- 运算符包括一元运算符和二元运算符
- 一元运算符：只有一个数据参与的运算。
- 二元运算符：有两个数据组合执行的运算；



- 条件运算符：= > < >= <= <> !=
- 逻辑运算符：AND OR NOT 

## INSERT语句
### （1）一次添加一行数据

```
USE DATABASE_Test
GO	--创建表之前选择对应的数据库

CREATE TABLE Tb_Student
(
	StudentNo varchar(9) PRIMARY KEY, --约束主键
	StudentName varchar(30) NOT NULL, --非空约束
	--CHECK约束一般用于约束列的取值范围
	StudentAge int NOT NULL CHECK(StudentAge > 20 and StudentAge < 30),
	Country varchar(20) NOT NULL DEFAULT '中国',--默认值约束
	StuTime datetime,
	Tuition money
)

INSERT INTO Tb_Student(StudentNo,StudentName,StudentAge,Country,StuTime,Tuition) 
VALUES ('200709001','Tom',21,'美国','2009-09-02',2222.50)
```


- INTO可以省略
- 表中每列数据都添加值时字段名可以省略。

```
INSERT INTO Tb_Student
VALUES('200709003','SAHA',21,'印度','2007-09-02',1000.50)
```
- 默认约束的列不指定值时将添加默认值

```
INSERT INTO Tb_Student(StudentNo,StudentName,StudentAge,StuTime,Tuition) 
VALUES ('200709004','张小飞',21,'2007-09-02',2000.50)
```
### （2）一次添加多行数据

```
INSERT Tb_Student(StudentNo,StudentName,StudentAge,Country,StuTime,Tuition) 
SELECT '200709007','关羽',23,'中国','2006-09-02',20000.50 UNION
SELECT '200709008','JACK',21,'美国','2008-09-03',2222.50
```


## 3. DELETE/TRUNCATE语句
- 删除全部内容
```
DELETE FROM Tb_Student
```

- 删除部分内容

```
DELETE FROM Tb_Student WHERE StudentName = '关羽'
```


- TRUNCATE语句只能删除整张表的数据

```
TRUNCATE TABLE Tb_Student
```

语法 | 优点 | 缺点
---|---|---
DELETE | 选择性的删除数据，数据可恢复 | 当删除整张表的数据时效率较低
TRUNCATE | 只能删除整张表的数据，但效率高 | 不能选择性删除，数据可恢复

## 4. UPDATA

```
UPDATE Tb_name SET StuAge = 25 WHERE StuName='Tom'
```


## 5. 完整示例

```
-- 创建数据库StudentInfo
CREATE DATABASE StudentInfo
-- 利用use语句跳转到StudentInfo数据库中
USE StudentInfo
GO
-- 创建学生表Student,同时指明字段名和约束条件
CREATE TABLE Student
(
	StudentNo varchar(20) PRIMARY KEY,--主键约束（非空唯一）
	StudentName varchar(30) NOT NULL,--非空约束
	StudentAge int CHECK(StudentAge >= 20 and StudentAge <= 30),--CHECK约束取值范围
	Country varchar(20) NOT NULL DEFAULT '中国',--默认值约束
	Tuition money NOT NULL
)

-- 增加数据，使用INSERT语句
INSERT INTO Student(StudentNo,StudentName,StudentAge,Country,Tuition)
VALUES ('200709001', 'Tom', 21,'美国','2000.50')
-- 省略INTO
INSERT Student(StudentNo,StudentName,StudentAge,Country,Tuition)
VALUES ('200709002','Jack',22,'英国','20001.30')
-- 省略字段名，每一列都添加数据时
INSERT Student
VALUES ('200709003','Mary',23,'美国','20003.20')
-- 默认约束的列不指定时将添加默认值,此时字段名中不写相应字段名
INSERT Student(StudentNo,StudentName,StudentAge,Tuition)
VALUES ('200709004','可可',22,'200.10')

--一次添加多行数据
INSERT Student
SELECT '200709005','Lily',23,'美国','2000.33' UNION
SELECT '200709006','Lucy',25,'英国','3000.3'

-- 修改部分行数据的字段值
UPDATE Student SET Tuition = '2500.0' WHERE StudentName = 'Lily'
```




- 示例2：

```
create database Mstanford2
use Mstanford2
go
create table Tb_Student
(
	StudentNo int identity(200709002, 1) primary key,--主键，自增
	StudentName varchar(20) not null,
	StudentAge int check(StudentAge > 20 and StudentAge < 30),
	Country varchar(20) default '中国',
	StuTime datetime not null,
	Tuition money not null
)

create table Tb_Student_Course
(
	SCNO int identity(1, 1) primary key,
	StudentNo int references Tb_Student(StudentNo),
	CourseName varchar(30) unique not null,--非空唯一
	CourseTime int not null,
	StuTime datetime default '2018-09-01',
	Notes varchar(1000)
)

--添加数据
use Mstanford2
go
insert Tb_Student 
values('凯奇',26,'法国','2009-09-01','4000.00')
insert Tb_Student
select 'shha',26,'印度','2009-09-01','1000.50' union
select '张小飞',26,'中国','2009-09-01','2000.50' union
select '李刚',26,'中国','2009-09-01','7000.50'
```

