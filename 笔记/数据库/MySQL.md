# MySQL入门

## MySQL服务启动

1. 手动
2. cmd--> services.msc 打开服务窗口
3. 管理员打开cmd
   1. net start mysql ：启动服务
   2. net stop mysql ：关闭服务

## MySQL登录

三种登录方式

1. 直接登录，命令`mysql -u用户名 -p密码`

2. 服务器登录，命令`mysql -hip -u用户名 -p链接目标密码` 

   

两种退出

1. exit
2. quit



# SQL

SQL：Structured Query Language ：结构化查询语言

定义了操作所有关系型数据库的规则

## SQL通用语法

1. SQL语句单行或者多行书写，封号结尾。
2. 不区分大小写，关键字建议使用大写。
3. 三种注释
   * 单行： --注释内容  或者 # 注释内容（mysql特有）
   * 多行注释 /* 内容 */



## SQL分类

* DDL：(Data Definition Language)数据定义语言，用来操作数据库和表。
* DML：(Data Manipulation Language)，数据操纵语言，用户通过它可以实现对数据库的基本操作。                   
* DQL：(Data Query Language)数据查询语言标准语法
* DCL：(Data Control Language) 数据控制语言，在SQL语言中，是一种可对数据访问权进行控制的指令



## DDL：操作数据库，表

### 操作数据库：CRUD

1. C(Create)：创建

   * `create database xx`创建数据库
   * `create database if not exists xxx ` 判断是否存在，不存在就创建
   * `create database xx character set 字符集名称`

   **练习：**`create database if not exists xx character set gkb` 判断+创建指定字符集

2. R(Retrieve)：查询

   * `show databases;`查询所有数据库名称
   * `show create database xxx(数据库名称)`查询数据库的字符集：查看某个数据库的创建语句
   * `select database()`挑出正在使用的数据库名称

3. U(Update)：修改

   * `alter database xx character set  gbk`//设置xx的字符集为gbk

4. D(Delete)：删除

   * `drop database xx`删除数据库

5. 使用数据库

   * `use xxx`使用数据库

   * 

### 操作表

1. C(Create)：创建

   * **语法：**

     ```
     	creat table 表名（
     		列名1 数据类型1，
     		列名2 数据类型2，
     		....
     		列名n,数据类型
     	）
     	
     	create table 表1 like 表2//创建表1，和表2一模一样。
     ```

     

   * 常用数据类型：

     1. int 整数类型
     2. double  **需要指定精度double (a,b)最多5位，保留2位有效小数，最大值就是999.99**
     3. data  日期类，只包含年月日。yyyy-MM-dd
     4. dataTime：日期，年月时分秒 yyyy-MM-dd HH:mm:ss
     5. timestamp：时间错类型，年月时分秒 yyyy-MM-dd HH:mm:ss；**如果不给这个字段赋值，或者赋值为null，就默认是当前系统时间**
     6. varchar 字符串，**只要指定最大字符 ： name varchar(20)最大2个字符**  ，一个汉字是一个字符。  

   示例：

   ```
    create table student(
   id int,
   name varchar(32),
   age int,
   score double(4,1),
   birthday date,
   insert_time timestamp);
   Query OK, 0 rows affected (0.03
   ```

   

2. R(Retrieve)：查询

   * `show tables`查询正在使用的数据库所有表的名称

   * `desc xx`  查询表结构

     

3. U(Update)：修改(**alter关键字**)

   * 修改表名

     `alter table 表名 rename to 新表名`

   * 修改字符集 

     `alter table 表名 character set 新字符集 `

   * 添加一列

     `alter table 表名 add 列名 数据类型`

   * 修改列 

     `alter table 表名 change 列名 新列名 新列的类型`

   * 只修改数据类型，**modify是修改而change是整个的改变。**

     `alter table 表名 modify 列名 新数据类型 `

   * 删除列

     `alter table 表名 drop 列名`
     
     sql server :`alter table book drop column a `

4. D(Delete)：删除

   * 删除表格

     `drop table 表名;`

## DML：增删表中的数据

1. 添加数据：
   * 语法：
     * insert into 表名(列名1,列名2,...列名n) value(值1,值2,...值n);
   * 注意：
     1. 列名和值要一一对应。
     2. 如果表名后，不定义列名，则默认给所有列添加值                                                                                                       nsert into 表名 values(值1,值2,...值n);
     3. 除了数字类型，其他类型需要使用引号(单双都可以)引起来
2. 删除数据：
   * 语法：
     * delete from 表名 [where 条件]
   * 注意：
     1. 如果不加条件，则删除表中所有记录。
     2. 如果要删除所有记录
        1. delete from 表名; -- 不推荐使用。有多少条记录就会执行多少次删除操作
        2. TRUNCATE TABLE 表名; -- 推荐使用，效率更高 先删除表，然后再创建一张一样的表。
3. 修改数据：
   * 语法：
     * update 表名 set 列名1 = 值1, 列名2 = 值2,... [where 条件];
   * 注意：如果不加任何条件，则会将表中所有记录全部修改。





## DQL：查询表中记录

### 语法：

select
	字段列表
from
	表名列表
where
	条件列表
group by
	分组字段
having
	分组之后的条件，如果不满足结果，这个组会就被忽略
order by
	排序
limit
	分页限定

例子:

* 



### 基础查询

* **多字段的查询**

select 字段1，字段2....  from 表名

* **注意**
  * 如果查询所有字段，用* 代替字段列表。**select *from 表名**
* 去重复
  * 在字段后面增加distinct，**去重只能去两个完全相同的**。
* **列相加**
  * 用字段名相加表示，如`math+score`
* **列相加，解决null的情况**
  * 如果有字段是null，他们相加的时候会变成null，因此可以用判断语句**，IFNULL(字段，值)**，如果这个字段是NULL，它就是后面的值，如果不是，它就是原本的值
* **给列起别名**
  * 用AS +别名  如`math+score AS 分数`
  * 也可以用 空格+别名`math+score 分数`





### 条件查询

#### 运算符

* 、< 、<= 、>= 、= 、<>

* BETWEEN...AND  在两数字之间

* IN( 数字1，数字2，数字3) 在多个数字之中

* **LIKE：模糊查询**

  * 占位符：

    * _:单个任意字符
    * %：多个任意字符，也可以是空字符
    * LIKE 后面要用单/双引号包括起来

    `WHERE NAME LIKE '%化%' `查询名字里有马的

* IS NULL   IS NOT NULL 

* and  或 &&

* or  或 || 

* not  或 !



### 进阶查询

#### 排序查询

* **语法： order by 排序字段1 排序方式1，排序字段2 排序方式2。** 多个排序条件，只有在前面一样的才会判断后面。
* 排序方式：
  * DESC 降序
  * ASC 升序



#### 聚合函数

1. count ：计算

   一般选择非空的列：**主键**

2. max：计算最大值

3. min：计算最小值

4. sum：计算和

5. avg：计算平均值



* 注意：聚合函数的计算，排除null值
  * 解决方案: IFNULL函数





### 分组查询

* 语法：

  * **GROUP BY 划分字段** （按划分字段进行划分）
  * HAVING 条件 ，分组后的条件，判断这个组是否能被显示出来。

* 注意，在划分后的两组里，字段显示的是整体，因此**个人的数据是毫无意义的**，因此分组后的字段一般分为2种

   1. 分组字段

   2. 聚合函数

      聚合函数会统计每一个组的，而不是所有数据的

* ```mysql
  SELECT sex,SUM(age) 年龄之和 FROM stu GROUP BY sex HAVING 年龄之和 >30;
  ```

### 分页查询

* 语法 ：LIMIT 开始页码，显示几条数据 

`SELECT * FROM stu LIMIT 0,3;--显示从0开始3条数据`

* 注意： LIMIT 是一个mysql的方言





## 约束

* 概念：对数据进行约定，保证数据的有效性。
* 分类：
  1. 主键约束 primary key 
  2. 非空约束 not null
  3. 唯一约束 unique
  4. 外键约束 foreign key

#### 非空约束NOT NULL

​	1. 在创建表格在数据类型的后面进行约束

```sql
	NAME VARCHAR(20) NOT NULL 
```

2. 利用alter modify添加非空约束

```sql
alter table stu modify varchar(20) not null
```

### 默认值 defualt

* 默认值，可以在数据类型后面增加默认值，在default 后面跟上默认值即可

```
create table st9 (
 address varchar(20) default '广州'
)		
```



#### 唯一约束 unique

* **添加唯一约束：**

1. 在创建表格在数据类型的后面进行约束
2. 用modify 给对应的列名添加唯一约束



* **删除唯一约束：**删除唯一约束，不能modify去修改，比较特殊,要用drop去删除索引。

```sql
ALTER TABLE stu DROP INDEX 列名
```

* 注意： 唯一约束可以允许有多个NULL



####  主键约束

* 主键：非空且唯一

  

* **添加主键约束：**

  1. 创建时约束

     1. `ALTER TABLE 表格 MODIFY id INT PRIMARY KEY `利用alter modify添加 

     因为主键约束是跟在数据类型后面一起的 。	

* **删除主键约束：**用ALTER + DROP 删除这个表的主键

  ```sql
  ALTER TABLE stu DROP PRIMARY KEY
  ```

  

#### 主键自动增长

* 自动增长：如果某一列是数值类型，用auto_increment 可以自动增长。**自增长只能用在主键上**

  

* **添加自动生长：**在数据类型后面增加就额可以了（也能在创建的时候生成，和上面一样）。

  ```sql
  ALTER TABLE stu MODIFY id INT AUTO_INCREMENT 
  ```



* **删除：** 

  ALTER TABLE 修改就可以 了







#### 外键约束 foreign key

* 外键约束： 让表与表之间产生关系，从而保证数据的正确性

* 主表： 用来约束别人的表

* 从表：被主表约束的表

* 添加方式：

  ```sql
  -- 添加方式1
  create table 表名{
  	外键列1
          constraint 外键名//自己起的 foreign key （外键列1） references 主表(主表字段名)
  }
  
  
  -- 添加方式2
  ALTER TABLE 表 ADD constraint 外键名 foreign KEY (外键列) references 主表(主表字段名)
  ```

  ![image-20210203212329628](C:\Users\37802\AppData\Roaming\Typora\typora-user-images\image-20210203212329628.png)

* 删除方式：

  ```SQL
  ALTER TABLE 表格 DROP FOREIGN KEY 外键名
  ```





#### 级联操作

* **级联更新：**链接外键约束，自动更新数据。
* **级联删除：**链接外键约束，外键删除，会自动自身删除数据。

* **添加操作：**

  ```sql
  ALTER TABLE 表 ADD constraint 外键名 foreign KEY (外键列) references 表2(表2的某个列名) 
  + 1. ON UPDATE CASCADE -- 自动更新
  + 2. ON DELETE CASCADE -- 自动删除
  ```


## 数据库的设计范式

## 数据库的备份和还原

*  **备份：** mysqldump  -u   -p   数据库名称 > 路径
* **还原：**首先使用数据库，然后用source + 路径，注入数据库，即可还原。

## 多表查询

### 内链接查询：只查询条件交集部分

查询A，B两表，出现的结果是A表的每一条数据和B表每一条数据的结合，即<A_i,B_i>，为两表的**笛卡尔积**。

1. 表起别名

   在多表查询的过程中，给表区别名会方便，注意：**起完别名之后，查询的字段也要用别名，不然会报错。**




* **隐式内连接查询：**

```sql

SELECT
	t2.`id`,
	t2.`name`,
FROM 
	dept t1,-- t1 是别名
	emp t2
WHERE
	t1.`id`=t2.`dept_id`
```



* **显式内连接查询：**

```sql
SELECT * FROM 表1 [INNER] JOIN 表格2 别名 ON 条件; 
```

### 外链接查询：查询一个表的所有记录和另外一个表与其交集的部分

1. 左外连接：查询左表所有记录，和交集的部分。也就是左边一定会完整的显示，即使有些数据是空（没有办法匹配）。

```sql
SELECT * FROM 表 left [outer] JOIN 表格2 别名 ON 条件; 
```

2. 右外连接，left改right ，查询右表以及交集部分。



* 命令总结：外连接在内连接的命令基础上就多了一个left/right

### 子查询

* 子查询：条件里的嵌套查询，里层的是子查询

* 格式：

  ```sql
  select 查询字段 FROM 表 where 字段 条件(=,>,<,<>)(子查询)
  ```

* 案例：查询工资最高的

  ```sql
  select 工资 FROM 表 where 字段 =(select max(工资) from 表)
  ```

**子查询多行：** IN(子查询1，子查询2....)



**子查询多行多列**：子查询里面可以放一个表格，就等于把两个表格硬套在一起。下面是查询工资小于平均工资的人的所有信息（包括另外一张市场表格)

```sql
SELECT 
	*
FROM 
	(SELECT id,NAME,salary,dept_id FROM emp WHERE salary<(SELECT AVG(salary) FROM emp)) e, -- 要注意给表格起名字
	dept d
WHERE 
	d.`id`=e.dept_id;
```





# 事务


1. 概念：
	*  如果一个包含多个步骤的业务操作，被事务管理，那么这些操作要么同时成功，要么同时失败。
	
2. 操作：
	1. 开启事务： start transaction;
	2. 回滚：rollback;
	3. 提交：commit;

* 事务提交的两种方式：
		* 自动提交：
			* mysql就是自动提交的
			* 一条DML(增删改)语句会自动提交一次事务。
		* 手动提交：
			* Oracle 数据库默认是手动提交事务
			* 需要先开启事务，再提交
	
	* 修改事务的默认提交方式：
		* 查看事务的默认提交方式：SELECT @@autocommit; -- 1 代表自动提交  0 代表手动提交
		* 修改默认提交方式： set @@autocommit = 0;



# DCL:管理用户，授权

* SQL分类：
	1. DDL：操作数据库和表
	2. DML：增删改表中数据
	3. DQL：查询表中数据
	4. DCL：管理用户，授权
* DBA：数据库管理员
* DCL：管理用户，授权
	1. 管理用户
		1. 添加用户：
			* 语法：CREATE USER '用户名'@'主机名' IDENTIFIED BY '密码';
		2. 删除用户：
			* 语法：DROP USER '用户名'@'主机名';
		3. 修改用户密码：

