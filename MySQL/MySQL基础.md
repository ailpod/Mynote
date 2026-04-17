---
date: 2025-05-09
---


---

# **MySQL数据库**

>数据模型： 数据库服务器 --> 数据库 --> 表 

---

## **关系型数据库（RDBMS）**

==员工表==

| id  | ==name== | ==job== | ==dept_id== |
| :-: | :------: | :-----: | :---------: |
|  1  |    张三    |   总裁    |      3      |
|  2  |    李四    |   经理    |      1      |
|  3  |    王五    |   开发    |      2      |

==部门表==（与员工表相关联）

| ==id==  | ==name== |
| :-: | :--: |
|  1  | 研发部  |
|  2  | 总经部  |
|  3  | 财务部  |

-  概念：建立在关系模型基础上，由多张相互连接的二维表组成的数据库

-  特点：
	- 使用表存储数据，格式统一，便于维护
	- 使用SQL语言操作，标准统一，使用方便


---

# **SQL**

## **SQL通用语法**

-  SQL语句可以单行或多行书写，以分号结尾 

-  SQL语句可以使用空格/缩进来增强语句可读性

-  MySQL数据库中的SQL语句不区分大小写  ==关键字建议大写==

-  单行注释： \-\- 或\# 注释（MySQL特有）    多行注释： /* 注释内容  \*/

---

## **SQL分类**

---
### **DDL**

>DDL（Data Definition Language）
>
>数据定义语言，用来定义数据库对象（数据库，表，字段）

---

#### **DDL--数据库操作**

==命令末尾记得加分号==

- 查询所有数据库： show databases ==；==

- 查询当前数据库： select database==() ；==

- 创建：

    create database \[if  not exists ] 数据库名 \[default charset 字符集]  \[collect排序规则] ；

==第一个可选项表示无该库则创建，字符集常用 utf8mb4==

- 删除：drop database \[if  exists ]数据库名 ；

- 使用：use 数据库名；

---

#### **DDL--表操作**

##### **表--创建与查询**

查询：

- 查询当前数据库所有表： show tables；

- 查询表结构：  desc ( describe) 表名  ，==用于查看创建好的表内容==

- 查询指定表的建表语句： show create table 表名； ==会显示创建表时写的语句==

创建：

```
Create table 表名 (
					字段1  字段1类型  约束 [ comment  字段1注释 ] ，
					字段2  字段2类型  约束 [ comment  字段2注释 ] ，		
					字段3  字段3类型  约束 [ comment  字段3注释 ] ，
					……
				    字段n  字段n类型 [ comment  字段n注释 ] 
)  [ comment  表注释 ] ;
```

==注意，最后一个字段后无逗号，其余均需逗号分隔==

##### **表--删除与修改**

修改：

- 添加字段： alter  table 表名 add 字段名 类型(长度)  \[comment 注释] \[约束] ；

- 修改数据类型： alter table 表名 modify 字段名 新数据类型（长度）；

- 修改字段名和类型： 
	- alter table 表名 change 旧字段名 新字段名 类型(长度)  \[comment 注释] \[约束] ；

- 修改表名：alter table 表名 rename to 新表名；

删除：

- 删除字段：alter table 表名 drop 字段名；

- 删除表： drop table if exists 表名；

- 删除并重新创建（清空）表： truncate table 表名；


---
### **DML**

>DML（Data Manipulation Language）
>
>数据操作语言，用来对数据库表中的内容进行增删改

---

#### **DML--添加数据**

-  给表中指定字段添加数据：
	- insert  into  表名 (字段1，字段2……)  values (值1，值2……)；

-  给表中所有字段添加数据：
	-  insert into  表名  values (值1，值2，……)；

-  批量添加数据
	- insert  into  表名 (字段1，字段2……)  values (值1，值2…)，(值1，值2…)，；
	- insert into  表名  values (值1，值2，…)，(值1，值2，…)，(值1，值2，…)；

>[!tip]+ 注意    
>
>插入数据时，字段和值要一一对应；
>
>字符串和时间型数据需要包含在引号内；
>
>插入数据的大小需要在字段规定范围内；

---

#### **DML--修改删除**

修改：

-  update 表名 set 字段名1=值1 ，字段名2=值2，…… \[where 条件] ；

删除：

- delete from  表名 \[where 条件]；

     ==delete语句不能删除某一字段的值，若需要使用update更新值为None==

---
### **DQL**

>DQL（Data Query Language）
>
>数据查询语言，用于查询数据库中表的记录

语法：==（编写顺序）==

```
SELECT
			字段列表---------------------------------  
FROM                                                         | -----------  基本查询
			表名列表---------------------------------       

WHERE
			条件列表--------------------------------- | -----------  条件查询
			
GROUP BY
			分组字段列表----------------------------
HAVING                                                      | -----------  分组查询
			分组后条件列表-------------------------
			
ORDER BY
			排序字段列表--------------------------- | -----------  排序查询
			
LIMIT
			分页参数--------------------------------- | -----------  分页查询
```

==执行顺序==： **from --> where --> group by --> having --> select --> order by --> limit**

---

####  **DQL--基本查询**

-  查询多个字段： select  字段1，字段2…  from 表名； /  select \* from 表名；

-  设置别名： select  字段1 \[As 别名1]，字段2 \[As 别名2]  … from  表名； ==as可以省略==

-  去除重复元素： select  distinct  字段列表 from 表名；

---

#### **DQL--条件查询**

[[MySQL数据类型|条件类型]]

-  select 字段 from 表名 where 条件 

==条件查询过滤空值： where 字段 is not null  /  !=' null '==

---

#### **DQL--分组查询**

[[MySQL数据类型|聚合函数]]

-  select  字段列表 from 表名  \[where 条件]  group by  分组字段名  \[having 分组后条件]

	==执行顺序： where --> 聚合函数 --> having==

---

#### **DQL--排序查询**

- select  字段列表  from  表名  order by 字段1 排序方式1 ，字段2 排序方式2 ；

- 排序方式 ：   ASC 升序     DESC 降序 

---

#### **DQL--分页查询**

-  select  字段列表   from  表名  limit  起始索引，查询记录数；

-  起始索引从0开始，起始索引 = ( 查询页码 - 1) \* 每页显示记录数 ；

-  如果查询的是第一页数据，则起始索引可以省略

---
### **DCL**

>DCL（Data Control Language）
>
>数据控制语言，用来创建数据库用户，控制数据库的访问权限

---

#### **DCL--用户管理**

- 查询用户：  USE mysql   --->   select  \*  from user

- 创建用户：create  user  '用户名'@'主机名' identified by  ' 密码 '  

	==用户名和主机名@是一个整体，不允许有空格，此处主机名可以替换成%，表示任一主机都可以登录==

- 修改用户密码：
	- alter  user  '用户名'@'主机名'  identified  with mysql_native_password  by  ' 新密 '  

- 删除用户： drop user ‘用户名’@‘主机名’  


---

#### **DCL--权限控制**

>[!example]- 常用权限
>ALL                ------ 所有权限
>SELECT          ------ 查询数据
>INSERT          ------ 插入数据
>UPDATE        ------ 修改数据
>DELETE         ------ 删除数据
>ALTER           ------ 修改表
>DROP           ------ 删除数据库/表/视图
>CREATE         ------ 创建数据库、表

-  查询权限 ：  show  grants  for ‘用户名’@‘主机名’  

-  授予权限 ： grant  权限列表  on  数据库名 .表名  to  ‘用户名’@‘主机名’

-  撤销权限 ： revoke  权限列表  on  数据库名 .表名  from  ‘用户名’@‘主机名’

	==授权时，数据库名和表名都可以用 \* 来表示所有， 多个权限间用逗号分隔==


---

# **函数**

>MySQL中内置有许多函数，本节主要罗列出常用函数

---

## **字符串函数**

-  **concat ( S1, S2, S3 ...Sn)** : 将字符串拼接成一个新字符串

-  **Lower / Upper (str )** : 将字符串全部转为小写/大写

-  **Lpad / Rpad (str, n, pad )** : 用字符串**pad**对**str**的左/右边填充，达到n个字符串长度 

-  **trim (str )** : 去掉字符串首尾的空格

-  **substring (str, start ,len)** : 返回字符串 **str** 从 **start** 起的长度为 **len** 的子链

	==常常和update搭配使用：update 表 set 字段 = 函数（）；==

---

## **数值函数**

-  **ceil(x ) / floor(x )** : 向上/向下取整 

-  **mod(x , y)** : 返回 x/y 的模

-  **rand( )** : 返回0-1内的随机数 

-  **round(x , y)** : 求参数x四舍五入的值 ，保留 y 位小数


---

## **日期函数**

- **datediff (date1 , date2)** : 返回date1和date2之间的天数

- **curdate( ) / curtime( )** : 返回当前日期/时间

- **now ( )** : 返回当前日期+时间

- **year / mouth / day (date )** ：获取输入日期的年 /月 /日

- **date_add / sub (date , interval expr type)** ：返回一个日期 / 时间 加上/减去间隔 expr 后的时间值 


---

## **流程控制函数**

-  **IF (value , t , f** )：如果value的值为true则返回 t ，否则返回 f 

-  **IFNULL ( value1 , value2 )** ：如果value1不为空则返回value1，否则返回value2

-  **CASE WHEN \[val1] THEN \[res1]  ... ELSE \[default] END**：如果val1为true 返回res1，否则返回default默认值

-  **CASE \[expr] WHEN \[val1] THEN \[res1]  ... ELSE \[default] END**：若 expr 的值等于 val1 返回res1 否则返回default默认值


---

# **约束**

>约束是作用在表中字段上的规则，用于限制存储在表中的数据
>
>目的是保证数据库中数据的正确、有效性和完整性

---

## **约束分类**

| 约束   |     | 描述                         |     | 关键字         |
| :--- | :-- | :------------------------- | :-- | ----------- |
| 非空约束 |     | 限制该字段的数据不能为null            |     | NOT NULL    |
| 唯一约束 |     | 保证该字段所有数据都是唯一的             |     | UNIQUE      |
| 主键约束 |     | 主键是一行数据的唯一标识，要求非空且唯一       |     | PRIMARY KEY |
| 默认约束 |     | 保存数据时，若未指定该字段值，则为默认值       |     | DEFAULT     |
| 检查约束 |     | 保证字段值满足某个条件                |     | CHECK (条件)  |
| 外键约束 |     | 让两张表的数据之间建立链接，保证数据的完整性和一致性 |     | FOREIGN KEY |

- 语法： 字段  数据类型  **约束**  comment ；

---

## **外键约束**

-  添加外键
```
create table 表名(
		字段名 数据类型，
		……
		[constraint ]  [外键名称]  foreign key(外键字段名)  references 主表(主表列名)
) ;
```

- [x] alter table 表名 add constraint 外键名 foreign key(字段名) references 主表(主表列名) ;

- 删除外键： alter table 表名 drop foreign key 外键名

- 删除、更新行为

>cascade ：删除/更新父表对应记录时，检查是否有对应外键，有的话一起删了
>
>set null：删除父表对应记录时，检查是否有对应外键，有则设为null

==set null 删除的字段要允许设置NULL值==

- [x]  后加上 ON UPDATE ___ ON DELETE ___  , 下划线上填行为即可 


---

# **多表查询**

>笛卡尔积：两个集合所有的组合情况，在多表查询中需要消除无效的笛卡尔积
>
>多表查询是建立在外键约束基础上的

---
## **多表关系**

>由于业务间相互关联，对应的表结构也存在各种联系，常分为三种：一对多(多对一)、多对多、一对一，因此衍生出多表关系这一概念


-  一对多：部门和员工间的关系 ，实现：**在多的一方建立外键，指向1的一方的主键**

-  多对多：学生和课程的关系 ，实现：**建立第三张表，包含至少两个外键，分别关联主键**

-  一对一：用于单表拆分、显示分类信息 ，实现：**在任意一方加入关联另一方主键的外键**

	==此时，该外键需要加入约束**UNIQUE**来确保一对一的关系==

---
### **连接**

>内外连接区别在于显示数据的多寡

---

#### **内连接**

-  隐式内连接：select  字段列表  from  表1、表2  where  条件 …；

-  显式内连接：select  字段列表  from 表1 \[inner ] join 表2 on 连接条件；

	==现常用显式内连接==
---

#### **外连接**

- 左外连接：select 字段列表 from 表1 **left** \[outer] join 表2 on 连接条件；

- 右外连接：select 字段列表 from 表1 **right** \[outer] join 表2 on 连接条件；

	==相当于查询表1(left) / 表2(right)中的所有数据，包含其交集部分==

>LEFT JOIN 的意思是：保留左表 (link_staff) 所有记录
>
>如果右表 (link_status) 中有匹配行（如 a.status = b.id and a.age < 30），则显示对应数据
>
>如果没有匹配行，则来自右表的所有字段（如 b.link_sta）会显示为 NULL

---

#### **自连接**

-  查询语法：select  字段列表  from  表1 别名1  join 表1 别名2  on  条件

-  同样包含有隐式、显式连接 ，注意要给表起别名

---

### **联合查询**

>联合查询就是将两个表的查询结果合并

```
select  字段1  from   表1...

union  [ all ]

select  字段2  from   表2...
```

-  若要去重，则用 union

- 联合查询的字段列表长度内容应一致

---

### **嵌套查询**

>SQL语句中嵌套select语句，称为子查询或嵌套查询
>
>外部SQL语句可以是增删改查的任意一个语句

---

 **标量子查询**

-  子查询的结果是单个值（数字，字符串，日期）等最简单的形式，称为标量子查询 : 

select * from link_staff where status = (select ==id== from link_status where link_sta = '总经办');


 **列子查询**

-  [[MySQL数据类型]]

- 子查询返回的结果是一列（多行）
```
select * from link_staff where status in 

			(select id from link_status where link_sta = '总经办' or link_sta = '运维') ;
```

**行子查询**

-  子查询返回结果为一行

-  例如： select  ==name, salary==  from table where id = 1  此时返回结果为一行

	==行子查询检索都符合要求的项，如示例中的name, salary都符合才会检索==

```
select name from link_staff where 

       (manageid,enterdate) = (select manageid,enterdate from link_staff where name = '阿烨');
```

**表子查询**

-  子查询返回结果是多行多列 ， 常用 IN 操作符 

---

# **事务**

>   一组操作的集合，事务会把所有操作作为一个整体一起向系统提交或撤销操作请求，即这些请求要么同时成功，要么同时失败

---

## **事务操作**

- 查看事务的自动提交方式： select @@autocommit  返回值为1代表自动，为0代表手动

-  提交事务： commit

-  回滚事务： rollback

-  开启事务：start transaction / begin

## **事务特性**

- 原子性(==A==tomicity)：事务是不可分割的最小操作单元，要么全部成功，要么全部失败

- 一致性(==C==onsistency)：事务完成时，必须使所有数据都保持一致状态

- 隔离性(==I==solation)：保证事务不受外部并发操作影响，独立运行

- 持久性(==D==urability)：事务完成后，数据将被永久修改

## **并发事务**

==引发的问题==

-  脏读：一个事务读取到另一个事务还未提交的数据
![[脏读.png]]
-  不可重复读：一个事务先后读取同一条记录，读取到的数据不同
![[不可重复读.png]]
-  幻读：一个事务按条件没查询到数据对应数据行后，插入数据，发现又有了该数据
![[幻读.png]]

## **隔离级别**

![[隔离级别.png]]

-  查看当前事务隔离级别：select @@transaction_isolation；

-  设置隔离级别：set  \[session/global] transaction isolation lever \[隔离级别]；

---

