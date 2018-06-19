# SQL

### 什么是SQL

>  SQL 是用于访问和处理关系型数据库的标准的计算机语言,需要注意的是对于不用数据库有个别的语法差别

### SQL运用

* SQL能够对数据库进行CRUD操作
* SQL可以在数据库中创建视图
* SQL可以在数据库中创建存储过程
* SQL可以在数据库中设置触发器

### 基本CRUD

#####  insert into

``` sql
INSERT INTO table_name (column1,column2,column3,...)  VALUES (value1,value2,value3,...);
```

##### update

``` sql
UPDATE table_name SET column1=value1,column2=value2,... WHERE some_column=some_value;
```

##### delete

``` sql
DELETE FROM table_name WHERE some_column=some_value;
```

##### select

``` sql
SELECT column_name,column_name FROM table_name;
```

###  DQL基本结构

> mysql语法

``` sql
select [distinct] 字段列表 [as 字段别名]
from 数据源
[where子句](筛选记录)
[group by 子句]
[having子句] (筛选组)
[order by 子句]
[limit子句];

说明：
1.数据源可以是一张表，也可以是某个查询的结果组成的二维表
2.having后面的条件字段一定是select后的某个字段
```

> 执行顺序

``` sql
1. 对from后的数据源进行合并(若数据源是某个查询组成的二维表)
2. 执行where子句，对数据源中记录进行过滤
3. 执行groupby子句，对过滤后的数据进行分组
4. 执行having子句，对分组后的数据进行筛选
5. 执行select子句，处理select产生的列表
6. 执行distinct子句，处理vt5中重复的数据
5. 执行orderby子句，对筛选后的数据进行排序
6. 执行limit子句，对排序后的进行分页,返回给调用者
```

### 查询关键字

##### distinct

> 去除重复数据

``` sql
SELECT DISTINCT column_name FROM table_name;
```

##### where

> 过滤满足条件的数据

``` sql
SELECT column_name,column_name FROM table_name WHERE column_name operator value;
```

##### 运算符（> |>=|<>|<|<=）

``` sql
SELECT * FROM table_name WHERE column_name operator value;
```

##### and|or

> 与或操作

``` sql
SELECT * FROM table_name WHERE column_name='lero' OR column_name1='hero';
SELECT * FROM table_name WHERE column_name='lero' and column_name1='hero';
SELECT * FROM table_name WHERE age > 18  AND (column_name='hero' OR column_name1='lero');
```

##### order by

> 对查询结果进行排序

``` sql
SELECT column_name,column_name1 FROM table_name ORDER BY column_name DESC,column_name1 ASC;
```

##### top&limit

> 取出指定范围内的数据

``` sql
SELECT TOP number column_name(s) FROM table_name;
SELECT column_name(s) FROM table_name LIMIT startIndex,number;
```

##### like

> 模糊匹配

``` sql
SELECT column_name(s) FROM table_name WHERE column_name LIKE pattern;

其中 pattern 可以为：
'%a'    //以a结尾的数据
'a%'    //以a开头的数据
'%a%'    //含有a的数据
'_a_'    //三位且中间字母是a的
'_a'    //两位且结尾字母是a的
'a_'    //两位且开头字母是a的
正则匹配：
MySQL 中使用 REGEXP 或 NOT REGEXP 运算符 (或 RLIKE 和 NOT RLIKE) 来操作正则表达式
取出name的以a到c匹配的结果
select * from info WHERE name REGEXP '^[a-c]';
```

##### between...and|not between...and

> between操作符用于选取介于两个值之间的数据范围内的值

> not between操作符用于选取介于两个值之间的数据范围外的值

``` sql
SELECT column_name(s) FROM table_name WHERE column_name BETWEEN value1 AND value2;
SELECT column_name(s) FROM table_name WHERE column_name not BETWEEN value1 AND value2;

请注意，在不同的数据库中，BETWEEN 操作符会产生不同的结果！
在某些数据库中，BETWEEN 选取介于两个值之间但不包括两个测试值的字段。
在某些数据库中，BETWEEN 选取介于两个值之间且包括两个测试值的字段。
在某些数据库中，BETWEEN 选取介于两个值之间且包括第一个测试值但不包括最后一个测试值的字段
```

##### in

> 常用于构造嵌套查询

``` sql
SELECT column_name(s) FROM table_name WHERE column_name IN (value1,value2,...);

SELECT column_name(s) FROM table_name WHERE column_name IN (select column_name form table_next);

IN 与 操作符= 的异同
相同点：均在WHERE中使用作为筛选条件之一、均是等于的含义
不同点：IN可以规定多个值，等于规定一个值
```

##### inner join|join

> INNER JOIN 与 JOIN 是相同的,用于两张表以上求交集

``` sql
SELECT column_name(s)
FROM table1
INNER JOIN table2
ON table1.column_name=table2.column_name;
```

##### left join|left outer join

> 从左表（table1）返回所有的行，即使右表（table2）中没有匹配，如果右表中没有匹配，则结果为 NULL

``` sql
SELECT column_name(s)
FROM table1
LEFT JOIN table2
ON table1.column_name=table2.column_name;
```

##### right join|right outer join

> 从右表（table2）返回所有的行，即使左表（table1）中没有匹配。如果左表中没有匹配，则结果为 NULL。

``` sql
SELECT column_name(s)
FROM table1
RIGHT JOIN table2
ON table1.column_name=table2.column_name;
```

##### full join|full outer join

> mysql不支持,FULL OUTER JOIN 关键字结合了 LEFT JOIN 和 RIGHT JOIN 的结果

##### union|union all

``` sql
#union的结果允许重复的值
SELECT column_name(s) FROM table1
UNION
SELECT column_name(s) FROM table2;

#union all的结果不允许重复的值
SELECT column_name(s) FROM table1
UNION ALL
SELECT column_name(s) FROM table2;

#注意：UNION 内部的每个 SELECT 语句必须拥有相同数量的列。列也必须拥有相似的数据类型。同时，每个 SELECT 语句中的列的顺序必须相同
```

##### is null| is not null

> 无法使用比较运算符来测试 NULL 值，比如 =、< 或 <>,只能用如上这两个关键字

``` sql
SELECT *  FROM  table1 WHERE column_name IS NULL
SELECT *  FROM  table1 WHERE column_name IS not NULL
```

##### group by

> 可结合一些聚合函数来使用

``` sql
SELECT column_name, aggregate_function(column_name)
FROM table_name
WHERE column_name operator value
GROUP BY column_name;
```

##### having

> 对聚合函数的条件过滤

``` sql
SELECT column_name, aggregate_function(column_name)
FROM table_name
WHERE column_name operator value
GROUP BY column_name
HAVING aggregate_function(column_name) operator value;
```

### 函数

##### IFNULL()|COALESCE()

> 对于空列的处理函数(mysql)

``` sql
P_Id	ProductName	 UnitPrice	UnitsInStock	UnitsOnOrder
1	    Jarlsberg	  10.45	      16	            15
2	    Mascarpone	  32.56	      23
3	    Gorgonzola	  15.67	      9	                 20

SELECT ProductName,UnitPrice*(UnitsInStock+IFNULL(UnitsOnOrder,0)) FROM Products;
SELECT ProductName,UnitPrice*(UnitsInStock+COALESCE(UnitsOnOrder,0))FROM Products;
```

##### NOW()|CURDATE()|CURTIME()

> 返回当前系统的日期和时间

``` sql
SELECT NOW() FROM table_name; //2008-11-11 12:45:34
SELECT CURDATE() FROM table_name; //2008-11-11
SELECT CURTIME() FROM table_name; //11:45:34
```

##### 聚合函数：AVG()|COUNT()|MAX()|MIN()|SUM()

> 返回数值列的平均值|总数|最大值|最小值|求和

``` SQL
SELECT AVG(column_name) FROM table_name
SELECT COUNT(column_name) FROM table_name
SELECT MAX(column_name) FROM table_name
SELECT MIN(column_name) FROM table_name
SELECT SUM(column_name) FROM table_name
```

##### UCASE()|LCASE()

``` sql
SELECT UCASE(column_name) FROM table_name //把字段对应的值转为大写
SELECT LCASE(column_name) FROM table_name //把字段对应的值转为小写
```

##### MID()

> 用于从文本字段中提取字符

``` sql
SELECT MID(column_name,start[,length]) FROM table_name
```

##### ROUND()

> 把数值字段舍入为指定的小数位数

``` sql
SELECT ROUND(column_name,decimals) FROM table_name  //decimals	必需。规定要返回的小数位数。
```

##### FORMAT()

> 对字段的显示进行格式化

``` sql
SELECT FORMAT(column_name,format) FROM table_name

eg:SELECT FORMAT(Now(),'YYYY-MM-DD') as nowDate FROM Table1
```

##### 字符串函数

> concat()字符串连接，concat_ws()使用指定的分隔符进行连接

``` sql
select CONCAT(goods_name,'temp')goods_name from goods;
select CONCAT_WS(',',goods_name,'temp')goods_name from goods;
```

> substring()截取字符串

``` sql
select substring(goods_name,3)goods_name from goods;
```

> trim()删除空格

``` sql
select trim(goods_name)goods_name from goods;
```

### 视图

> 视图是基于 SQL 语句的结果集的可视化的表,视图中的字段来自一个或多个数据库中的真实的表中的字段

> 目的是把常用的复杂查询简化出来，方便下次直接使用，而不用每次都进行复杂的查询

##### mysql语法

> 创建视图

``` sql
CREATE VIEW view_name AS
SELECT column_name(s)
FROM table_name
WHERE condition
```

> 查询视图

``` sql
SELECT * FROM view_name where condition
```

> 更新视图

``` sql
CREATE OR REPLACE VIEW view_name AS
SELECT column_name(s)
FROM table_name
WHERE condition
```

> 删除视图

``` sql
DROP VIEW view_name
```

### 存储过程

> 存储过程是一组为了完成特定功能的SQL语句集，经编译后存储在数据库中，用户通过指定存储过程的名字并给定参数（如果该存储过程带有参数）来调用执行它

##### 语法

> MySQL默认以";"为分隔符，如果没有声明分割符，则编译器会把存储过程当成SQL语句进行处理，因此编译过程会报错，所以要事先用“DELIMITER //”声明当前段分隔符，让编译器把两个"//"之间的内容当做存储过程的代码，不会执行这些代码；“DELIMITER ;”的意为把分隔符还原

``` sql
DELIMITER //
create procedure procedure_name(In|out|inout 参数名 数据类型，In|out|inout 参数名 数据类型)
begin
过程体
end
//
DELIMITER ;
```

##### 参数

> 存储过程根据需要可能会有输入、输出、输入输出参数，如果有多个参数用","分割开。MySQL存储过程的参数用在存储过程的定义，共有三种参数类型,IN,OUT,INOUT

``` sql
in:必须在调用存储过程时指定，在存储过程中修改该参数的值不能被返回，为默认值
out:该值可在存储过程内部被改变，并可返回
inout:调用时指定，并且可被改变和返回
```

##### in参数

``` sql
DELIMITER //
  CREATE PROCEDURE in_param(IN p_in int)
    BEGIN
    SELECT p_in;
    SET p_in=2;
    SELECT p_in;
    END;
//
DELIMITER ;
#调用
SET @p_in=1;
CALL in_param(@p_in); //1 2
SELECT @p_in; //1
#结论：p_in虽然在存储过程中被修改，但并不影响@p_id的值
```

##### out参数

``` sql
DELIMITER //
  CREATE PROCEDURE out_param(OUT p_out int)
    BEGIN
      SELECT p_out;
      SET p_out=2;
      SELECT p_out;
    END;
    //
DELIMITER ;
#调用
SET @p_out=1;
CALL out_param(@p_out); //1 2
SELECT @p_out; //2
#结论：该值可在存储过程内部被改变，并可返回
```

##### inout参数

``` sql
DELIMITER //
  CREATE PROCEDURE inout_param(INOUT p_inout int)
    BEGIN
      SELECT p_inout;
      SET p_inout=2;
      SELECT p_inout;
    END;
    //
DELIMITER ;
#调用
SET @p_inout=1;
CALL inout_param(@p_inout) ; // 1 2
SELECT @p_inout;//2
#结论：调用时指定，并且可被改变和返回
```

##### 变量

> 用户变量一般以@开头

``` sql
#变量定义
SET 变量名 = 变量值 [,变量名= 变量值 ...];
eg:
SELECT 'Hello World' into @x;
SELECT @x; //Hello world
SET @y='Goodbye Cruel World';
SELECT @y; //Goodbye Cruel World
SET @z=1+2+3;
SELECT @z;  //6

#在存储过程中使用用户变量
CREATE PROCEDURE GreetWorld() SELECT CONCAT(@greeting,' World');
SET @greeting='Hello';
CALL GreetWorld();

#在存储过程间传递全局范围的用户变量
CREATE PROCEDURE p1() SET @last_proc='p1';
CREATE PROCEDURE p2() SELECT CONCAT('Last procedure was ',@last_proc);
CALL p1();
CALL p2();
```

> 局部变量

``` sql
DELIMITER //
  CREATE PROCEDURE proc()
    BEGIN
      DECLARE x1 VARCHAR(5) DEFAULT 'outer';
        BEGIN
          DECLARE x1 VARCHAR(5) DEFAULT 'inner';
          SELECT x1;
        END;
      SELECT x1;
    END;
    //
DELIMITER ;
#调用
CALL proc();
```



##### 删除存储过程

``` sql
DROP PROCEDURE [过程1[,过程2…]]
```

##### 存储过程控制语言

> if-then-else

``` sql
#条件语句IF-THEN-ELSE
DROP PROCEDURE IF EXISTS proc3;
DELIMITER //
CREATE PROCEDURE proc3(IN parameter int)
  BEGIN
    DECLARE var int;
    SET var=parameter+1;
    IF var=0 THEN
      INSERT INTO t VALUES (17);
    END IF ;
    IF parameter=0 THEN
      UPDATE t SET s1=s1+1;
    ELSE
      UPDATE t SET s1=s1+2;
    END IF ;
  END ;
  //
DELIMITER ;
```

> CASE-WHEN-THEN-ELSE语句

``` sql
#CASE-WHEN-THEN-ELSE语句
DELIMITER //
  CREATE PROCEDURE proc4 (IN parameter INT)
    BEGIN
      DECLARE var INT;
      SET var=parameter+1;
      CASE var
        WHEN 0 THEN
          INSERT INTO t VALUES (17);
        WHEN 1 THEN
          INSERT INTO t VALUES (18);
        ELSE
          INSERT INTO t VALUES (19);
      END CASE ;
    END ;
  //
DELIMITER ;
```

> while-do...end-while循环

```sql
DELIMITER //
  CREATE PROCEDURE proc5()
    BEGIN
      DECLARE var INT;
      SET var=0;
      WHILE var<6 DO
        INSERT INTO t VALUES (var);
        SET var=var+1;
      END WHILE ;
    END;
  //
DELIMITER ;
```

> repeat..end repeat循环

``` sql
DELIMITER //
  CREATE PROCEDURE proc6 ()
    BEGIN
      DECLARE v INT;
      SET v=0;
      REPEAT
        INSERT INTO t VALUES(v);
        SET v=v+1;
        UNTIL v>=5
      END REPEAT;
    END;
  //
DELIMITER ;
```

> loop...end loop

``` sql
DELIMITER //
  CREATE PROCEDURE proc7 ()
    BEGIN
      DECLARE v INT;
      SET v=0;
      LOOP_LABLE:LOOP
        INSERT INTO t VALUES(v);
        SET v=v+1;
        IF v >=5 THEN
          LEAVE LOOP_LABLE;
        END IF;
      END LOOP;
    END;
  //
DELIMITER ;
```

### 触发器

> 触发器是一种与表操作有关的数据库对象，当触发器所在表上出现指定事件时，将调用该对象，即表的操作事件触发表上的触发器的执行,换言之，它是在对数据库表进行insert、update、delete的时候执行，自动执行，不能直接调用

#####  触发器四要素

*  监视地点（table）
*  监视事件（insert/update/delete）
*  触发时间（after/before）
*  触发事件（insert/update/delete）

##### 语法

``` sql
DELIMITER //
CREATE TRIGGER
trigger_name
trigger_time
trigger_event ON tbl_name
FOR EACH ROW
trigger_stmt
//
DELIMITER ;
说明：
trigger_name：触发器的名称
tirgger_time：触发时机，为BEFORE或者AFTER
trigger_event：触发事件，为INSERT、DELETE或者UPDATE
tb_name：表示建立触发器的表明，就是在哪张表上建立触发器
trigger_stmt：触发器的程序体，可以是一条SQL语句或者是用BEGIN和END包含的多条语句
FOR EACH ROW：表示任何一条记录上的操作满足触发事件都会触发该触发器
```

##### new与 old

> 用来表示触发器的所在表中，触发了触发器的那一行数据

 ``` sql
在 INSERT 型触发器中，NEW 用来表示将要（BEFORE）或已经（AFTER）插入的新数据；
在 UPDATE 型触发器中，OLD 用来表示将要或已经被修改的原数据，NEW 用来表示将要或已经修改为的新数据；
在 DELETE 型触发器中，OLD 用来表示将要或已经被删除的原数据；
 ```



##### 工作原理

> 初始化订单表order，不添加任何数据

``` sql
order_id(pk)        goods_id             order_num

```

> 初始化商品表goods，添加如下3条数据

```  sql
goods_id(pk)        goods_name           goods_num
1					小米手机	            20
2                     苹果手机                20
3                     锤子手机                20
```

> 编写购买商品后，商品库存数量减少的触发器

``` sql
DELIMITER //
CREATE TRIGGER t1
AFTER  INSERT ON `order`
FOR EACH ROW
BEGIN
UPDATE goods SET goods_num=goods_num-new.order_num WHERE goods_id=new.goods_id;
END;
//
DELIMITER ;

```

> 模拟购买商品，在订单表中插入一条数据，会触发上述触发器

``` sql
order_id(pk)        goods_id             order_num
 1                       1                  10
```

> 刷新goods表，发现goods_num自动减少10个，说明触发器生效

``` sql
goods_id(pk)        goods_name           goods_num
1					小米手机	            10
2                     苹果手机                20
3                     锤子手机                20
```

> 编写撤销订单的触发器

``` sql
DELIMITER //
DROP TRIGGER if EXISTS t1;
CREATE TRIGGER t1
AFTER DELETE ON `order`
FOR EACH ROW
BEGIN
UPDATE goods SET goods_num=goods_num+old.order_num WHERE goods_id=old.goods_id;
END;
//
DELIMITER ;
```

> 模拟撤销订单，在订单中删除刚才插入的数据，会触发撤销订单的触发器

``` sql
order_id(pk)        goods_id             order_num
```

> 撤销订单后，刷新goods表，发现goods_number自动还原为20个，说明撤销订单触发器生效

``` sql
goods_id(pk)        goods_name           goods_num
1					小米手机	            20
2                     苹果手机                20
3                     锤子手机                20
```

> 编写修改订单触发器,包括修改购买的数量以及购买的商品

``` sql
DELIMITER //
DROP TRIGGER if EXISTS t1;
CREATE TRIGGER t1
AFTER UPDATE ON `order`
FOR EACH ROW
BEGIN
-- 撤销订单
UPDATE goods SET goods_num=goods_num+old.order_num WHERE goods_id=old.goods_id;
-- 新增订单
UPDATE goods SET goods_num=goods_num-new.order_num WHERE goods_id=new.goods_id;
END;
//
DELIMITER ;
```

> 修改订单

``` sql
order_id(pk)        goods_id             order_num
 1                       1                  5
```

> 修改订单后的，执行了修改触发器

``` sql
goods_id(pk)        goods_name           goods_num
1					小米手机	            15
2                     苹果手机                20
3                     锤子手机                20
```

##### 删除触发器

``` sql
drop trigger  user_log;#删除触发器
```

### 常见面试题

##### 统计数据行列转换

> 常用于统计，报表

``` sql
-- 创建表  学生表
CREATE TABLE `student` (
    `stuid` VARCHAR(16) NOT NULL COMMENT '学号',
    `stunm` VARCHAR(20) NOT NULL COMMENT '学生姓名',
    PRIMARY KEY (`stuid`)
)
COLLATE='utf8_general_ci'
ENGINE=InnoDB;


-- 课程表

CREATE TABLE `courses` (
    `courseno` VARCHAR(20) NOT NULL,
    `coursenm` VARCHAR(100) NOT NULL,
    PRIMARY KEY (`courseno`)
)
COMMENT='课程表'
COLLATE='utf8_general_ci'
ENGINE=InnoDB;


-- 成绩表
CREATE TABLE `score` (
    `stuid` VARCHAR(16) NOT NULL,
    `courseno` VARCHAR(20) NOT NULL,
    `scores` FLOAT NULL DEFAULT NULL,
    PRIMARY KEY (`stuid`, `courseno`)
)
COLLATE='utf8_general_ci'
ENGINE=InnoDB;

-- 插入数据

-- 学生表数据

Insert Into student (stuid, stunm) Values('1001', '张三');
Insert Into student (stuid, stunm) Values('1002', '李四');
Insert Into student (stuid, stunm) Values('1003', '赵二');
Insert Into student (stuid, stunm) Values('1004', '王五');
Insert Into student (stuid, stunm) Values('1005', '刘青');
Insert Into student (stuid, stunm) Values('1006', '周明');

-- 课程表数据
Insert Into courses (courseno, coursenm) Values('C001', '大学语文');
Insert Into courses (courseno, coursenm) Values('C002', '新视野英语');
Insert Into courses (courseno, coursenm) Values('C003', '离散数学');
Insert Into courses (courseno, coursenm) Values('C004', '概率论与数理统计');
Insert Into courses (courseno, coursenm) Values('C005', '线性代数');
Insert Into courses (courseno, coursenm) Values('C006', '高等数学(一)');
Insert Into courses (courseno, coursenm) Values('C007', '高等数学(二)');

-- 成绩表数据

Insert Into score(stuid, courseno, scores) Values('1001', 'C001', 67);
Insert Into score(stuid, courseno, scores) Values('1002', 'C001', 68);
Insert Into score(stuid, courseno, scores) Values('1003', 'C001', 69);
Insert Into score(stuid, courseno, scores) Values('1004', 'C001', 70);
Insert Into score(stuid, courseno, scores) Values('1005', 'C001', 71);
Insert Into score(stuid, courseno, scores) Values('1006', 'C001', 72);
Insert Into score(stuid, courseno, scores) Values('1001', 'C002', 87);
Insert Into score(stuid, courseno, scores) Values('1002', 'C002', 88);
Insert Into score(stuid, courseno, scores) Values('1003', 'C002', 89);
Insert Into score(stuid, courseno, scores) Values('1004', 'C002', 90);
Insert Into score(stuid, courseno, scores) Values('1005', 'C002', 91);
Insert Into score(stuid, courseno, scores) Values('1006', 'C002', 92);
Insert Into score(stuid, courseno, scores) Values('1001', 'C003', 83);
Insert Into score(stuid, courseno, scores) Values('1002', 'C003', 84);
Insert Into score(stuid, courseno, scores) Values('1003', 'C003', 85);
Insert Into score(stuid, courseno, scores) Values('1004', 'C003', 86);
Insert Into score(stuid, courseno, scores) Values('1005', 'C003', 87);
Insert Into score(stuid, courseno, scores) Values('1006', 'C003', 88);
Insert Into score(stuid, courseno, scores) Values('1001', 'C004', 88);
Insert Into score(stuid, courseno, scores) Values('1002', 'C004', 89);
Insert Into score(stuid, courseno, scores) Values('1003', 'C004', 90);
Insert Into score(stuid, courseno, scores) Values('1004', 'C004', 91);
Insert Into score(stuid, courseno, scores) Values('1005', 'C004', 92);
Insert Into score(stuid, courseno, scores) Values('1006', 'C004', 93);
Insert Into score(stuid, courseno, scores) Values('1001', 'C005', 77);
Insert Into score(stuid, courseno, scores) Values('1002', 'C005', 78);
Insert Into score(stuid, courseno, scores) Values('1003', 'C005', 79);
```

> 查询每个学生的 每门课程与每门成绩

``` sql
select  st.stuid as 'ID' ,  st.stunm  as '姓名', cs.coursenm  as '课程名' ,sc.scores as '成绩'   from  student st, score sc ,courses cs
where st.stuid = sc.stuid and sc.courseno = cs.courseno
```

> 进行行转列

``` sql
select st.stuid as '编号' , st.stunm  as '姓名' ,
Max(case c.coursenm when '大学语文' then s.scores else 0 end ) as '大学语文',
max(case c.coursenm when '新视野英语' then IFNULL(s.scores,0)else 0 end) as  '新视野英语',
Max(case c.coursenm when '离散数学' then IFNULL(s.scores,0) ELSE 0 END)  as '离散数学',
MAX(case c.coursenm when '概率论与数理统计' then IFNULL(s.scores,0) else 0 end) as '概率论与数理统计',
MAX(case c.coursenm  when '线性代数' then IFNULL(s.scores,0) else 0 END) as  '线性代数',
MAX(case c.coursenm when '高等数学(一)' THEN IFNULL(s.scores,0) else 0 end) as '高等数学(一)',
MAX(case c.coursenm when '高等数学(二)' THEN IFNULL(s.scores,0) else 0 end) as '高等数学(二)'
from  student st
LEFT JOIN score s on st.stuid = s.stuid
LEFT JOIN courses c on c.courseno = s.courseno
GROUP BY st.stuid
```

> GROUP_CONCAT() 组连接，也非常有用

``` sql
select   s.stuid as  '编号' , GROUP_CONCAT(s.courseno) as '课程号' , GROUP_CONCAT(s.scores) as '成绩'  from score s GROUP BY  s.stuid
```







