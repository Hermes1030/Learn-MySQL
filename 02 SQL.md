### 通用语法

1. 可以单行或多行书写，以<font color=blue>分号结尾</font>

2. 可以使用<font color=blue>空格</font>、缩进来增强语句的可读性

3. <font color=blue>不区分</font>大小写，关键字建议使用大写

4. 注释
	1. 单行注释 -- 或 #
	2. 多行注释 /* text \*/

### SQL 分类


|分类|全称|说明|
|-----|----|-----|
|DDL|Data Definition Lanaguage|数据定义语言，用来定义数据库对象（数据库，表，字段）|
|DML|Data Manipulation Lanaguage|数据操作语言，用来对数据库表中的数据进行增删改查|
|DQL|Data Query Language|数据查询语言，用来查询数据库中表的记录|
|DCL|Data Control Language|数据控制语言，用来创建数据库用户，控制数据库的访问权限|


### DDL 数据定义语言

#### 1）数据库操作

- 查询
```mysql
# 查询所有数据库
show databases;
```

```mysql
# 查询当前数据库
select database();
```

- 创建
```mysql
create database [if not exists] 数据库名 [default charset 字符集][collate 排序规则];
```

- 删除
```mysql
drop database [if exists] 数据库名;
```

- 使用
```mysql
use 数据库名;
```

#### 2）数据表操作

- 查询所有表
```mysql
show tables;
```

- 查询表结构
```mysql
desc 表名;
```

- 查询指定表的建表语句
```mysql
show create table 表名;
```

- 创建表
```mysql
create table 表名(
    字段1 字段1类型[comment 字段1注释],
	字段2 字段2类型[comment 字段2注释],
	字段3 字段3类型[comment 字段3注释]
)[comment 表注释];
```

##### 数据类型

|类型|有符号范围(signed)|无符号范围(unsigned)|描述|
|----|------------|------------|-----|
|tinyint|(-128, 127)|(0, 255)|小整数值|
|smallint|(-32768, 32767)|(0, 65535)|大整数值|
|int或integer|(-2147483648, 2147483647)|(0, 4294967295)|大整数值|
|bigint|(-2^63, 2^63-1)|(0, 2^64-1)|极大整数值|
|float|(-3.402823466 E+38, 3.402823466351 E+38)|0和(1.175494351 E-38, 3.402823466 E+38)|单精度浮点数值|
|double|(-1.7976931348623157 E+308, 1.7976931348623157 E+308)|0和(2.2250738585072014 E-308, 1.7976931348623157 E+308)|双精度浮点数值|
|decimal|依赖于M(精度)和D(标度)的值|依赖于M(精度)和D(标度)的值|小数值(精确定点数)|

|类型|大小|描述|
|----|----|-----|
|char|0-255 bytes|定长字符串|
|varchar|0-65535 bytes|变长字符串|
|tinyblob|0-255 bytes|不超过255个字符的二进制数据|
|tinytext|0-255 bytes|短文本字符串|
|blob|0-65535 bytes|二进制形式的文本数据|
|text|0-65535 bytes|长文本数据|
|mediumblob|0-16777215 bytes|二进制形式的中等长度文本数据|
|mediumtext|0-16777215 bytes|中等长度文本数据|
|longblob|0-4294967295 bytes|二进制形式的极大文本数据|
|longtext|0-4294967295 bytes|极大文本数据|

|类型|大小|范围|格式|描述|
|----|----|------|----|-----|
|date|3|1000-01-01 至 9999-12-31|YYYY-MM-DD|日期值|
|time|3|-838：59：59 至 838：59：59|HH：MM：SS|时间值或持续时间|
|year|1|1901 至 2155|YYYY|年份值|
|datetime|8|1000-01-01 00：00：00 至 9999-12-31 23：59：59|YYYY-MM-DD HH：MM：SS|混合日期和时间|
|timestamp|4|1970-01-01 00：00：01 至 2038-01-19 03：14：07|YYYY-MM-DD HH：MM：SS|混合日期和时间值，时间戳|

```mysql
# 案例

1.编号（纯数字）
2.员工工号（字符串类型，10位）
3.员工姓名（字符串类型，10位）
4.性别（男/女）
5.年龄（不存在负数，无符号）
6.身份证号（18位，考虑X结尾）
7.入职时间（取年月日）

create table emp(
    id int comment '编号',
    workno varchar(10) comment '工号',
    name varchar(10) comment '姓名',
    gender char(1) comment '性别',
    age tinyint unsigned comment '年龄',
    idcard char(18) comment '身份证号',
    entrydate date comment '入职时间'
) comment '员工表';
```

#### 3）修改表操作

- 添加字段
```mysql
alter table 表名 add 字段名 类型(长度) [comment 注释] [约束];
```

```mysql
# 为emp表增加一个新的字段"昵称"为nickname，类型为varchar(20)
alter table emp add nickname varchar(20) comment '昵称';
```

- 修改数据类型
```mysql
alter table 表名 modify 字段名 新数据类型(长度);
```

- 改变字段名和类型
```mysql
alter table 表名 change 旧字段名 新字段名 类型(长度) [comment 注释] [约束];
```

- 删除字段
```mysql
alter table 表名 drop 字段名;
```

- 修改表名
```mysql
alter table 表名 rename to 新表名;
```

- 删除表
```mysql
drop table [if exists] 表名;
```

- 删除指定表，并重新创建
```mysql
truncate table 表名;
```


### DML 数据操作语言

- 添加数据
```mysql
# 给指定字段添加数据
insert into 表名(字段名1， 字段名2,...) values (值1, 值2, ...);
```

```mysql
# 给全部字段添加数据
insert into 表名 values (值1,值2, ... );
```

```mysql
# 批量添加数据
insert into 表名 (字段名1, 字段名2, ... ) values (值1,值2, ...),(值1,值2, ...),(值1,值2, ...);
insert into 表名 values(值1,值2, ...),(值1,值2, ...),(值1,值2, ...);
```

<font color=red>注意：</font>

>	插入数据时，指定的字段顺序需要与值的顺序是一一对应的
>	字符串和日期型数据应该包含在<font color=blue>引号</frot>内
>	插入的数据大小，应该在字段的规定范围内

- 修改数据
```mysql
update 表名 set 字段名1=值1, 字段2=值2, ...[where 条件];
```

>修改语句的条件可以有，也可以没有，如果没有条件，则会修改整张表的所有数据

- 删除数据
```mysql
delete from 表名 [where 条件];
```

> delete 语句的条件可以有，也可以没有，如果没有，则会删除整张表的所有数据
> delete 语句不能删除某一个字段的值（可以只用update）


### DQL 数据查询语言

- 查询关键字
```mysql
SELECT	字段列表 FROM	表名列表 WHERE	条件列表 GROUP BY	分组字段列表 HAVING	分组后条件列表 ORDER BY 排序字段列表 LIMIT 分页参数;
```

#### 1）非条件查询

- 查询多个字段
```mysql
select 字段1, 字段2, 字段3 ...from 表名;
select * from 表名;
```

- 设置别名
```mysql
select 字段1 [as 别名1],字段2 [as 别名2] ... from 表名;
```

- 去除重复记录
```mysql
select distinct 字段列表 from 表名;
```

#### 2）条件查询

-  语法
```mysql
select 字段列表 from 表名 where 条件列表;
```

##### 比较运算符

|比较运算符|功能|
|-----------|-----|
|>|大于|
|>=|大于等于|
|<|小于|
|<=|小于等于|
|=|等于|
|<> 或 !=|不等于|
|between...and...|在某个范围之内|
|in(...)|在in之后的列表中的值，多选一|
|like 占位符|模糊匹配|
|is null|是NUll|
|and 或 &&|并且|
|or 或 \|\| |或者|
|not 或 ！|非，不是|

##### 聚合函数

> 将一列数据作为一个整体，进行纵向计算

常见聚合函数
|函数|功能|
|----|----|
|count|统计数量|
|max|最大值|
|min|最小值|
|avg|平均值|
|sum|求和|

- 语法
```mysql
select 聚合函数(字段列表) from 表名;
```

>所有null值不参与聚合函数计算

#### 3）分组查询

```mysql
select 字段列表 from 表名 [where 条件] group by 分组字段名 [having 分组后过滤条件];
```

where 和 having 区别：

>1. 执行时机不同：where是分组之前进行过滤，不满足where条件，不参与分组；而having是分组之后对结果进行一个过滤
>2. 判断条件不同：where不能对聚合函数进行判断，而having可以

<font color=red>注意：</font>

>	执行顺序：where > 聚合函数 > having
>	分组后，查询的字段一般为聚合函数和分组字段，查询其他字段无意义

#### 4）排序查询

- 语法
```mysql
select 字段列表 from 表名 order by 字段1 排序方式1, 字段2 排序方式2;
```

排序方式：
asc：升序
desc：降序

> 如果是多个字段排序，当第一个<font color=blue>字段值相同</font>时，才会根据第二个字段进行排序

#### 5）分页查询

- 语法
```mysql
select 字段列表 from 表名 limit 起始索引, 查询记录数;
```

```mysql
select * from employee limit 10,10;
```

<font color=red>注意：</font>

> 	起始索引从0开始，起始索引=(查询页码-1)\*每页显示记录数
> 	分页查询是数据库的方言，不同的数据库有不同的实现，MySQL是Limit
> 	如果查询的是第一页数据，起始索引可以省略，直接简写为 limit 10

案例
```mysql
# 1.查询年龄为20，21，22，23岁的女性员工的信息
select * from employee where gender = '女' and age in(20,21,22,23);

# 2.查询性别为男，并且年龄在20-40岁（含）以内的姓名为三个字的员工
select * from employee where gender = '男' and age between 20 and 40  and name like '___';

# 3.统计员工表中，年龄小于60岁的男性员工和女性员工的人数
select gender,count(*) from employee where age < 60 group by gender; 

# 4.查询所有年龄小于等于35岁员工的姓名和年龄，并对查询结果按年龄升序排序，如果年龄相同按入职时间降序排序
select name,age from employee where age <= 35 order by age asc,workentry desc;

# 5.查询性别为男，且年龄在20-40岁（含）以内的前5个员工信息，对查询的结果按年龄升序排序，年龄相同按入职时间升序排序
select * from employee where gender = '男' and age between 20 and 40 order by age asc,workentry asc limit 5;
```

**执行顺序**

> 编写顺序：select > from > where > group by > aving > order by > limit
> 执行顺序：from > where > group by > having > select > order by > limit

### DCL 数据控制语言

#### 用户管理

- 创建用户
```mysql
create user '用户名'@'登录域' identified by '密码';
```

- 修改用户密码
```mysql
alter user '用户名'@'登录域' identified with mysql_native_password by '新密码';
```

- 删除密码
```mysql
drop user '用户名'@'登录域';
```

案例
```mysql
# 创建用户 gao 只能在当前主机localhost访问，密码123456
CREATE user 'gao'@'localhost' IDENTIFIED BY '123456';

# 创建用户 heima，可以在任意主机访问该数据库，密码5821156
CREATE user 'heima'@'%' IDENTIFIED BY '5821156';

# 修改用户heima的访问密码 ‘12345678’
ALTER user 'heima'@'%' IDENTIFIED WITH mysql_native_password by '12345678';

# 删除heima用户
drop user 'heima'@'%';
```

<font color=red>注意：</font>
>	登录域可以使用%通配
>	这类SQL开发人员操作的比较少，主要是（DBA，Database Administrator 数据库管理员）使用。

#### 权限控制

##### 权限说明

|权限|说明|
|------|------|
|ALL，ALL PRIVILEGES|所有权限|
|SELECT|查询数据|
|INSERT|插入数据|
|UPDATE|修改数据|
|DELETE|删除数据|
|ALTER|修改表|
|DROP|删除数据库/表/视图|
|CREATE|创建数据库/表|


- 查询权限
```mysql
show grants for '用户名'@'登录域';
```

- 授予权限
```mysql
grant 权限列表 on 数据库名.表名 to '用户'@'登录域';
```

- 撤销权限
```mysql
revoke 权限列表 on 数据库名.表名 from '用户名'@'登录域';
```

案例
```mysql
# 查询用户权限
show grants for 'gao'@'localhost';

# 授予权限
grant all on itcast.* to 'gao'@'localhost';

# 撤销权限
REVOKE all ON itcast.* FROM 'gao'@'localhost';
```

<font color=red>注意：</font>
>	多个权限之间，使用逗号分隔
>	授权时，数据库名和表名可以使用\*进行统配，代表所有

