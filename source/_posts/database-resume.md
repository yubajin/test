---
title: database-resume
date: 2019-04-08 10:02:05
tags:
    - 数据库
    - 面试清单

categories: 后端
---

# 数据库及其框架
## IN

## mysql 引擎

myisam 与innodb 

- MYISAM（默认） 不支持事务，不支持外键，表锁，插入数据时，锁定整个表，查表总行数时，不需要全表扫描 
- INNODB 支持事务，支持外键，行锁，查表总行数时，全表扫描

## 索引
- 我们对某一字段增加索引，查询时就会先去索引列表中一次定位到特定值的行数，大大减少遍历匹配的行数，所以能明显增加查询的速度
- 底层是B+树实现
- 加快数据检索速度
- 维护索引表
- 消耗一定空间
- 建立索引
```mssql
CREATE INDEX index_name
ON table_name (column_name)
```
***B-树(B树)和B+树***
- 索引建在磁盘上，用索引检索数据库时需要从磁盘中将索引加载到内存中
- 用二叉排序树的话，磁盘IO次数最坏为二叉树的高度
- 一个m阶的B树具有如下几个特征：
   1. 根结点至少有两个子女。
   2. 每个中间节点都包含k-1个元素和k个孩子，其中 m/2 <= k <= m
   3. 每一个叶子节点都包含k-1个元素，其中 m/2 <= k <= m
   4. 所有的叶子结点都位于同一层。
- B树因为矮胖，IO次数少，提升性能；虽然总的比较次数差不多，但内存中的比较耗时几乎可以省略

## 视图
虚拟表

由一个或一些基表联合查询出来

- 定制数据：将用户关心的数据放置在视图中，表与表的连接操作对用户来说隐藏了
- 视图是用户提供多种角度看待同一数据
- 安全：对不同的用户定义不同的视图，是数据不出现在不应该看到的用户视图上 

```mssql
CREATE VIEW Salary_F(emp_name,Sex ,salary)
AS
SELECT a.emp_name
	   ,a.Sex
	   ,a.salary
FROM xsgl.dbo.员工人事表employee a
WHERE Sex = 'F'
```
## 触发器
达到某个条件，触发对表执行一些语句 是一个特殊的存储过程

oracle规定触发器里面不能有事务操作

针对employee表写一个UPDATE触发器，当插入的薪水的值小于1000时，自动修改为1000。（提示：可用上变量和inserted、deleted表） 
```mssql
use [xsgl(完整性约束)] go 
create trigger updateEmployeeTrigger on 员工人事表employee after update,insert 
as begin 
update 员工人事表employee set salary = 1000 where salary < 1000 end;
```

从sales表中删除一个订单信息的时候，自动完成sale_item表中对应的明细的删除。
```mssql
use [xsgl(完整性约束)]
go
create trigger t4 on 销售主表sales after delete
as begin
	declare @orderNo int
	select @orderNo =order_no  from deleted
	delete 销货明细表sale_item where order_no = @orderNo
	print '修改sale_item表成功'
	end

delete from 销售主表sales where order_no = 10001;
```
## 游标
可以看做指向结果集的一个指针，通过使用游标，应用程序可以逐行访问并处理结果集。
```mssql
use [xsgl(数据库完整性)]
go
begin
	
declare select_cursor cursor for select *
								 from 员工人事表employee
								 where Sex = 'F' and title = '职员'
								 
open select_cursor 
fetch next from select_cursor 

while(@@FETCH_STATUS = 0)
	fetch next from select_cursor 
close select_cursor
```

## 存储过程
```mssql
--建立存储过程
create procedure CaculateSumPrice @order_no int,@sale_price numeric(9,2) output
as begin
declare @tot_amt numeric(9,2)
select @tot_amt = sum(distinct Qty*Unit_price)
from 销货明细表sale_item where order_no = @order_no
group by order_no
set @sale_price=@tot_amt
end

--执行存储过程
declare @return_saleprice numeric(9,2)
exec CaculateSumPrice 10001 ,@return_saleprice output

select @return_saleprice sale_price 
```
预先用SQL语句写好并用一个指定的名称存储起来,
只需调用execute,就可以实现存储过程里面定义好的服务。
其实也就相当于将数据库的复习操作用函数封装，用来调用

优点:
1. 存储过程可以重复使用,可减少数据库开发人员的工作量
2. 存储过程只在创造时进行编译，以后每次执行存储过程都不需再重新编译，而一般SQL语句每执行一次就编译一次,所以使用存储过程可提高数据库执行速度。
3. 安全性高,可设定只有某此用户才具有对指定存储过程的使用权

## 事务

### 事务的特性:

原子性:原子性是指事务包含的所有操作要么全部成功，要么全部失败回滚

一致性:
- 在事务开始之前和事务结束以后，数据库的完整性约束没有被破坏。
- 拿转账来说，假设用户A和用户B两者的钱加起来一共是5000，那么不管A和B之间如何转账，转几次账，事务结束后两个用户的钱相加起来应该还得是5000，这就是事务的一致性。

隔离性：隔离性是当多个用户并发访问数据库时，比如操作同一张表时，数据库为每一个用户开启的事务，不能被其他事务的操作所干扰，多个并发事务之间要相互隔离

持久性：在事务完成以后，该事务所对数据库所作的更改便持久的保存在数据库之中，并不会被回滚。

### 事务的并发问题:

脏读，不可重复读，幻读

1. 脏读：事务A读取了事务B更新的数据，然后B回滚操作，那么A读取到的数据是脏数据
2. 不可重复读：事务 A 多次读取同一数据，事务 B 在事务A多次读取的过程中，对数据作了更新并提交，导致事务A多次读取同一数据时，结果 不一致。
3. 幻读：系统管理员A将数据库中所有学生的成绩从具体分数改为ABCDE等级，但是系统管理员B就在这个时候插入了一条具体分数的记录，当系统管理员A改结束后发现还有一条记录没有改过来，就好像发生了幻觉一样，这就叫幻读。

### 事务隔离级别:
| 事务隔离级别|        脏读        |        不可重复读       |   幻读      |
| ------ | :----------------: |:----------------: |:----------------: |
|读未提交（read-uncommitted） |      是      |      是      |      是      |
| 不可重复读（read-committed |       否      |      是      |      是      |
| 可重复读（repeatable-read）|       否      |      否      |      是      |
| 串行化（serializable|       否      |      否      |      否      |	

## 什么是JDBC

连接数据库，执行SQL查询，存储过程

### 创建一个JDBC连接

- 注册并加载驱动
- 用DriverManager获取连接对象

### PreparedStatement

- PreparedStatement是预编译的，通过它可以将对应的SQL语句高效的执行多次
- 有助于防止SQL注入
- 进行动态查询
- 执行更快

## hibernate是什么

是一个持久化的orm框架

ORM object relation mapping(对象关系映射)

面向对象的概念		关系的概念
类		 			表
对象				行（记录）
属性				表的类（字段）

orm框架:把对数据库的操作转化为对对象的操作

## redis

key value的内存数据库


支持的数据类型
Strings
Lists
Sets 求交集、并集
Sorted Set 
hashes

## mybatis

### \#和$区别

- \#传入的数据都当成一个字符串，自动加双引号
- $传入的数据直接显示在sql
- \# 可以防sql注入

mybatis和hibernate有什么不同

- ，SQL写在XML里，解除sql与程序代码的耦合，便于统一管理；提供XML标签，支持编写动态SQL语句，并可重用。
- mybatis可以通过xml和注解的方式灵活配置运行sql，并将java对象和sql语句生成最终执行的sql

- Mybatis学习门槛低，简单易学，程序员直接编写原生态sql 

### mybatis配置



### 缓存

- 一级缓存

  Mybatis首先去缓存中查询结果集，如果没有则查询数据库，如果有则从缓存取出返回结果集就不走数据库。
  
  Mybatis内部存储缓存使用一个HashMap，key为hashCode+sqlId+Sql语句。value为从查询出来映射生成的java对象 

- Mybatis的二级缓存

  - 开启二级缓存后，CacheExecutor会饰Executor，进入一级缓存之前，先在CacheExecutor进行二级缓存的查询
  - 二级缓存是可以跨SqlSession的。 
 

### 分页


### 对jdbc的封装


