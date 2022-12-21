### 通用语法

1. 可以单行或多行书写，以分号结尾

2. 可以使用空格、缩进来增强语句的可读性

3. 不区分大小写，关键字建议使用大写

4. 注释
	1. 单行注释 -- 或 #
	2. 多行注释 /* text \*/

### 分类

|分类|全称|说明|
|-----|----|-----|
|DDL|Data Definition Lanaguage|数据定义语言，用来定义数据库对象（数据库，表，字段）|
|DML|Data Manipulation Lanaguage|数据操作语言，用来对数据库表中的数据进行增删改查|
|DQL|Data Query Language|数据查询语言，用来查询数据库中表的记录|
|DCL|Data Control Language|数据控制语言，用来创建数据库用户，控制数据库的访问权限|


### DDL

**数据库操作**

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

**数据表操作**

- 查询所有表
```mysql
show tables;
```

- 查询表结构
```mysql
desc 表名;
```

- 查询只当表的建表语句
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

**数据类型**

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
5.年龄（不存在负数）
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

**修改表操作**

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

- 修改字段名和类型
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

### DML

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

注意：

>插入数据时，指定的字段顺序需要与值的顺序是一一对应的
>字符串和日期型数据应该包含在引号内
>插入的数据大小，应该在字段的规定范围内

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



### DQL

- 查询关键字
```mysql
SELECT
	字段列表
FROM
	表名列表
WHERE
	条件列表
GROUP BY
	分组字段列表
HAVING
	分组后条件列表
ORDER BY
	排序字段列表
LIMIT
	分页参数
```

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

**条件查询**

-  语法
```mysql
select 字段列表 from 表名 where 条件列表;
```

- 条件
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

**聚合函数**

> 将一列数据作为一个整体，进行纵向计算

常见聚合函数
|函数|功能|
|----|----|
|count|统计数量|
|max|最大值|
|min|最小值|
|avg|平均值|
|sum|求和|

语法
```mysql
select 聚合函数(字段列表) from 表名;
```

>所有null值不参与聚合函数计算

**分组查询**

```mysql
select 字段列表 from 表名 [where 条件] group by 分组字段名 [having 分组后过滤条件];
```

where 和 having 区别
>执行时机不同：where是分组之前进行过滤，不满足where条件，不参与分组；而having是分组之后对结果进行一个过滤
>判断条件不同：where不能对聚合函数进行判断，而having可以

注意
>执行顺序：where > 聚合函数 > having
>分组之后，查询的字段一般为聚合函数和分组字段，查询其他字段无意义

**排序查询**
```mysql
select 字段列表 from 表名 order by 字段1 排序方式1, 字段2 排序方式2;
```
排序方式：
asc：升序
desc：降序

> 如果是多个字段排序，当第一个字段值相同时，才会根据第二个字段进行排序


**分页查询**
```mysql
select 字段列表 from 表名 limit 起始索引, 查询记录数;
```
如：
```mysql
select * from employee limit 10,10;
```

注意：
起始索引从0开始，起始索引=(查询页码-1)\*每页显示记录数
分页查询是数据库的方言，不同的数据库有不同的实现，MySQL是Limit
如果查询的是第一页数据，起始索引可以省略，直接简写为 limit 10


### DCL

