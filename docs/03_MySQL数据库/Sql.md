

##  创建数据库表

```mysql
/*
  创建数据表: 关键字 create table
  格式:
    create table 表名(
       列名1 数据类型[长度] [约束],
       列名2 数据类型[长度] [约束],
       列名3 数据类型[长度] [约束]
    );
    
   细节: 可以使用关键字作为名字,但是为了规范,避开关键字
   使用关键字作为名字,请你添加反引号 ``	  ''  ""
   
   创建学生信息表
   编号,姓名,年龄
*/
CREATE TABLE student(
	id INT,
	NAME VARCHAR(10),
	age INT
);

#需求：创建雇员表，包含雇员的姓名，密码，性别, 生日信息。
CREATE TABLE employee(
	NAME VARCHAR(50),
	PASSWORD VARCHAR(50),
	gender CHAR(1),
	birthday DATE
);




```

## 非空约束

```mysql
/*
  非空约束:
    约束的是这个列数据不能是 null
    关键字 not null
*/
DROP TABLE persons;


# 创建非空约束的方式1: 创建表直接创建 
CREATE TABLE persons (
	  id INT PRIMARY KEY AUTO_INCREMENT,
	  firstname VARCHAR(255) NOT NULL,
	  lastname VARCHAR(255),
	  address VARCHAR(255)
);

INSERT INTO persons VALUES(NULL,'郭','富成','香港');

# 下面程序是错误的,写不进去
INSERT INTO persons VALUES(NULL,NULL,'富成','香港');




INSERT INTO persons VALUES(NULL,'','家驹','香港');
INSERT INTO persons (id,lastname,address)VALUES(3,'富成','香港');

# 创建非空约束的方式2: 修改表结构 alter table 
CREATE TABLE persons (
	  id INT PRIMARY KEY AUTO_INCREMENT,
	  firstname VARCHAR(255)  ,
	  lastname VARCHAR(255),
	  address VARCHAR(255)
);

ALTER TABLE persons MODIFY firstname VARCHAR(255) NOT NULL;

# 删除非空约束
ALTER TABLE persons MODIFY firstname VARCHAR(255)

```

## 分页查询

```mysql
/*
   分页查询
   (当前页-1)*每页显示条数
*/

SELECT * FROM product LIMIT 0,5;

SELECT * FROM product LIMIT 5,5;

SELECT * FROM product LIMIT 10,5;
```

## 分组查询

```mysql
/*
   分组查询
   举例:
     80人,分成10个组,每个组8人
     统计数据,每个小组统计
     1组: 成绩100
     2组: 成绩99
     
   一个列的相同值,进行分组
   关键字 group by 列名
*/

# 按照商品分类,进行价格求和
SELECT SUM(price),category_id FROM product GROUP BY category_id;

# 需求追加, 不想看到求和后价格低于1000的
# 在分组求和后的数据基础之上,再次条件过滤
# where 条件过滤在表中过滤(真实的数据表)
# 查询后的结果,过滤条件,使用关键字 having

SELECT SUM(price) price,category_id FROM product GROUP BY category_id
HAVING price > 1000;


# 统计各个分类商品的个数
SELECT COUNT(pid),category_id FROM product GROUP BY category_id;

# 统计各个分类商品的个数,且只显示个数大于1的信息
SELECT COUNT(pid) pid,category_id FROM product GROUP BY category_id HAVING pid>1;

# 统计价格>200元的 各个分类商品的个数,且只显示个数大于1的信息
SELECT COUNT(pid) pid,category_id FROM product WHERE price > 200 GROUP BY category_id HAVING pid>1;



```

## 更新数据

```mysql
  更新数据
  对原有数据进行修改
  没有确定,改了就是改了
  关键字 update  set where
  注意:
    修改数据一定要进行条件的筛选
    没有条件筛选,表中的所有数据,都会修改
*/
# update 表名 set 列=值,列=值,列=值 where 条件
# 修改王五,名字,年龄,描述
UPDATE student SET NAME='王五五',age=25,description='一般般' 
WHERE id=3;
```

## 基本查询

```mysql

/*
   产品表,商品表 product
   pid 主键 
   pname 商品名字
   price 价格
   category_id 分类
   c001 家电
   c002 服装
   c003 化妆品
   c004 食品
   c005 饮料
*/
CREATE TABLE product(
    pid INT PRIMARY KEY AUTO_INCREMENT,
    pname VARCHAR(20),
    price DOUBLE,
    category_id VARCHAR(32)
);
INSERT INTO product(pid,pname,price,category_id) VALUES(1,'联想',5000,'c001');
INSERT INTO product(pid,pname,price,category_id) VALUES(2,'海尔',3000,'c001');
INSERT INTO product(pid,pname,price,category_id) VALUES(3,'雷神',5000,'c001');
INSERT INTO product(pid,pname,price,category_id) VALUES(4,'JACK JONES',800,'c002');
INSERT INTO product(pid,pname,price,category_id) VALUES(5,'真维斯',200,'c002');
INSERT INTO product(pid,pname,price,category_id) VALUES(6,'花花公子',440,'c002');
INSERT INTO product(pid,pname,price,category_id) VALUES(7,'劲霸',2000,'c002');
INSERT INTO product(pid,pname,price,category_id) VALUES(8,'香奈儿',800,'c003');
INSERT INTO product(pid,pname,price,category_id) VALUES(9,'相宜本草',200,'c003');
INSERT INTO product(pid,pname,price,category_id) VALUES(10,'面霸',5,'c003');
INSERT INTO product(pid,pname,price,category_id) VALUES(11,'好想你枣',56,'c004');
INSERT INTO product(pid,pname,price,category_id) VALUES(12,'香飘飘奶茶',1,'c005');
INSERT INTO product(pid,pname,price,category_id) VALUES(13,'果9',1,NULL);


/*
  数据的基本查询
  拼接语句  
  关键字 
    select  选择,查询
    from    来自哪里,哪个表
    where   条件
    as      重命名
    distinct 去重
    
    基本语法:
      select 列名,列名 from 表名 查询指定的列
      select * from 表名 查询所有列 
 
*/

# 查询数据表,显示商品名和价格
SELECT pname,price FROM product;

# 查询全部的商品数据
SELECT * FROM product;


# 查询数据表,查询价格,去掉重复的
SELECT DISTINCT(price) FROM product;

# 查询数据表,所有的商品价格上调100元

SELECT pname,price+100 FROM product;

# 查询数据表,所有的商品价格上调100元 对查询的结果集进行列的重命名
SELECT pname,price+100 AS'price' FROM product;

# 查询数据表,所有的商品价格上调100元 对查询的结果集进行列的重命名,简化书写
# 去掉as,直接空格,写上新列名
SELECT pname,price+100 price FROM product;



 
```

## 聚合函数

```mysql
/*
  聚合函数
   sum(列名) 列的所有值求和
   min(列名) 查询列中的最小值
   max(列名) 查询列中的最大值
   avg(列名) 列的所有值求和/列数,平均值
   count(列名) 统计该列共有多少条数据
*/

#查询商品的总条数
SELECT COUNT(*) FROM product;
SELECT COUNT(pid) FROM product;

#查询价格大于200商品的总条数
SELECT COUNT(pid)  FROM product WHERE price > 200;

#查询所有商品的价格总和
SELECT SUM(price) FROM product;

#查询分类为'c001'的所有商品的价格总和
SELECT SUM(price) FROM product WHERE category_id = 'c001';

#查询分类为'c002'所有商品的平均价格
SELECT AVG(price) FROM product WHERE category_id = 'c002';

#查询商品的最大价格和最小价格

SELECT MAX(price),MIN(price) FROM product;


```

## 默认约束

```mysql
/*
  默认约束:
   对一个列,数据默认
   关键字 default
*/
DROP TABLE persons;
# 创建默认约束方式1: 创建表直接添加
CREATE TABLE persons (
   id INT PRIMARY KEY AUTO_INCREMENT,
   firstname VARCHAR(255),
   lastname VARCHAR(255),
   address VARCHAR(255) DEFAULT '北京市'
);

INSERT INTO persons VALUE(NULL,'张','三丰',NULL);

INSERT INTO persons(id,firstname,lastname) VALUE(NULL,'宋','无极');

# 创建默认约束方式2: 修改表结构

ALTER TABLE persons MODIFY address VARCHAR(255) DEFAULT '天津市';
```

## 排序查询

```mysql
/*
  排序查询:
    对查询的结果集排序
    关键字 order by
    升序: asc  默认的,可以不写
    降序: desc
    排序写在SQL语句的最后
*/

#使用价格排序(降序)
SELECT * FROM product ORDER BY price DESC;

#在价格排序(降序)的基础上，以分类排序(降序)

SELECT * FROM product ORDER BY category_id DESC;


#显示商品的价格(去重复)，并排序(降序)
SELECT DISTINCT (price) FROM product ORDER BY price DESC;
```

## 数据库命令

```mysql
/*
  创建数据库
  关键字 create database
  格式: create database 数据库名字
  
  删除数据
  关键字 drop  database
  格式: drop database 数据库名字
*/

DROP DATABASE mydatabase;

SHOW DATABASES;

CREATE DATABASE mydatabase;

USE mydatabase;



```

## 添加数据

```mysql
/*
  数据表添加数据
  关键字 insert  into  values
*/

# 添加数据的格式1:
# insert [into] 表名(列名1,列名2,列名3)values(值1,值2,值3)
# 数据库中,值的引号问题,数字可以不写任何符号,其他数据类型加引号,单引号
INSERT INTO student(id,NAME,age,description)VALUES(1,'张三',20,'学习努力');

# 添加数据的格式2:
# insert [into]表名 values(全部的值)
INSERT INTO student VALUES(2,'李四',21,'不好好学习');

# 添加数据的格式3: 批量数据
# insert [into]表名 values(全部的值),(全部的值),(全部的值)
INSERT INTO student VALUES
(3,'王五',24,'还是可以'),
(4,'赵柳',30,'天天扫雷'),
(5,'冯琦',19,'天天空档接龙')




```

## 条件查询

```mysql
/*
  条件查询
*/

#查询商品名称为“花花公子”的商品所有信息：
SELECT * FROM product WHERE pname = '花花公子';


#查询价格为800商品
SELECT * FROM product WHERE price = 800;


#查询价格不是800的所有商品
SELECT * FROM product WHERE price <> 800;
SELECT * FROM product WHERE NOT(price = 800);


#查询商品价格大于60元的所有商品信息
SELECT * FROM product WHERE price > 60;

#查询商品价格在200到1000之间所有商品
SELECT * FROM product WHERE price >= 200 AND price <= 1000;
# BETWEEN 值 and 值 区间范围查询 ,前面的值必须小于后面的
# BETWEEN 值 and 值 值是日期,前面的日期小, 2019-5-5   and 2019-10-6
SELECT * FROM product WHERE price BETWEEN 200 AND 1000;

#查询商品价格是200或800的所有商品
SELECT * FROM product WHERE price =200 OR price = 800;
# 推荐使用in查询,包含即可  in(数据无顺序关系)
SELECT * FROM product WHERE price IN(200,800,1);



#查询商品名称含有'霸'字的所有商品
# 使用通配符 % 零个或者多个
SELECT * FROM product WHERE pname LIKE '%霸%';


#查询商品名称以'香'开头的所有商品
SELECT * FROM product WHERE pname LIKE '香%';

#查询商品名称第二个字为'想'的所有商品
SELECT * FROM product WHERE pname LIKE '_想%'


#商品没有分类id的商品
#查询列 category_id 是空值
SELECT * FROM product WHERE category_id IS NULL;

#查询有分类id的商品
#查询列 category_id 不是空值
SELECT * FROM product WHERE category_id IS NOT NULL;
```

## 唯一约束

```mysql
/*
   唯一约束:
   设置唯一约束的这个列,数据必须唯一性
   
   主键约束也具有唯一性
   唯一约束也具有唯一性
   
   主键约束,数据不能是真实的业务数据
   关键字 unique
   
*/

DROP TABLE persons;

# 创建唯一约束的方式1: 创建表的同时,直接创建  推荐方式
CREATE TABLE persons (
	  id INT PRIMARY KEY AUTO_INCREMENT,
	  firstname VARCHAR(255)  UNIQUE,
	  lastname VARCHAR(255),
	  address VARCHAR(255)
);


INSERT INTO persons VALUES(NULL,'张','三丰','中国');

# 数据添加失败,唯一约束
INSERT INTO persons VALUES(NULL,'张','无忌','中国');


INSERT INTO persons VALUES(NULL,'','三丰','中国');


# 创建唯一约束的方式2: constraint区域添加
CREATE TABLE persons (
	  id INT PRIMARY KEY AUTO_INCREMENT,
	  firstname VARCHAR(255) ,
	  lastname VARCHAR(255),
	  address VARCHAR(255),
	  CONSTRAINT UNIQUE qk_firstname(firstname)
);

# 创建唯一约束的方式3: 修改表结构 alter table
CREATE TABLE persons (
	  id INT PRIMARY KEY AUTO_INCREMENT,
	  firstname VARCHAR(255) ,
	  lastname VARCHAR(255),
	  address VARCHAR(255)

);
ALTER TABLE persons MODIFY firstname VARCHAR(255) UNIQUE;

# 删除唯一约束

/*
   添加唯一约束后: 数据库MySQL认为,这个列会经常被查询
   会为这个列自动添加 索引(提高数据查询效率 )
   删除的是这个索引
*/

ALTER TABLE persons DROP INDEX firstname;
```

## 限制查询

```mysql
/*
   限制条数查询
   关键字 limit (SQL语句的最末尾)
   语法: limit m,n
   m: 数据表的开始索引0开始
   n: 限制显示多少条
*/
SELECT  * FROM product LIMIT 1,10;

SELECT * FROM product LIMIT 0, 1000;
```

## 修改表结构

```mysql
/*
  修改表结构
  在已经创建好的数据表中,修改他的结构
  操作具有风险性(数据丢失)
  关键字  alter  table
*/

# 数据表添加列 列名 desc 可变字符255
# alter table 表名 add 列名 数据类型[长度] [约束]
ALTER TABLE student ADD `desc` VARCHAR(255);

# 修改列的数据类型 desc列,修改为int
# alter table 表名 modify 列名 新数据类型[长度] [约束]
ALTER TABLE student MODIFY `desc` INT ;

# 修改列名 desc列名改为 description
# alter table 表明 change 旧列名 新列名 新数据类型[长度] [约束]
ALTER TABLE student CHANGE `desc` `description` INT;

# 修改表名 student 修改为 student_table 
# rename TABLE 旧表名 to 新表明

RENAME TABLE student TO student_table 




```

## 主键约束

```mysql
/*
   约束: 限制
   数据表的数据,不能随意,需要做限制
   主键约束 primary key
   约束内容:
     被设置为主键约束的列,数据在表中具有唯一性
     这个主键列的数据,不能是空(null)
     每个表只能设置一个主键,每个数据表都应该有一个主键
*/

# 创建主键约束的方式1: 创建表的同时,直接创建,推荐使用
CREATE TABLE persons(
	id INT PRIMARY KEY,
	firstname VARCHAR(50),
	lastname VARCHAR(50),
	address VARCHAR(50)
);
INSERT INTO persons(id,firstname,lastname,address)VALUES
(1,'刘','德华','香港');

INSERT INTO persons(id,firstname,lastname,address)VALUES
(2,'张','学友','香港');

# 创建主键约束的方式2: 在创建表的约束区域创建
# 关键字 constraint,可以指定约束的名字 约束字母缩写_列名
DROP TABLE persons;

CREATE TABLE persons(
	id INT ,
	firstname VARCHAR(50),
	lastname VARCHAR(50),
	address VARCHAR(50),
	CONSTRAINT PRIMARY KEY pk_id (id)
);

# 创建主键约束的方式3: 先建好表,修改alter table

CREATE TABLE persons(
	id INT ,
	firstname VARCHAR(50),
	lastname VARCHAR(50),
	address VARCHAR(50)
);

ALTER TABLE persons ADD CONSTRAINT PRIMARY KEY pk_id (id);

# 联合主键,多个列一起,实现一个主键

CREATE TABLE persons(
	id INT ,
	firstname VARCHAR(50),
	lastname VARCHAR(50),
	address VARCHAR(50),
	CONSTRAINT PRIMARY KEY pk(firstname,lastname)
);

/*
  数据的自动增长列 (免维护)
  设置为自动增长列:
    必须是整数
    必须是主键
    关键字 : auto_increment
*/
CREATE TABLE persons(
	id INT PRIMARY KEY AUTO_INCREMENT,
	firstname VARCHAR(50),
	lastname VARCHAR(50),
	address VARCHAR(50)
);
INSERT INTO persons(firstname,lastname,address)VALUES('刘','德华','香港');
INSERT INTO persons(firstname,lastname,address)VALUES('张','学友','香港');

# 添加语句,可以简化书写
# 主键列,写值是null,自动增长
INSERT INTO persons VALUES(NULL,'黎','明','香港');

# 删除主键是3的数据
DELETE FROM persons WHERE id = 3;

DELETE FROM persons

INSERT INTO persons VALUES(NULL,'周','星驰','香港');


# 摧毁表,并重建,主键数据从1开始
TRUNCATE persons;



```

## 查询错误

```mysql
# 分类表
CREATE TABLE category (
  cid VARCHAR(32) PRIMARY KEY ,
  cname VARCHAR(50)
);

#商品表
CREATE TABLE products(
  pid VARCHAR(32) PRIMARY KEY ,
  pname VARCHAR(50),
  price INT,
  flag VARCHAR(2), #是否上架标记为：1表示上架、0表示下架
  category_id VARCHAR(32),
  CONSTRAINT products_category_fk FOREIGN KEY (category_id) REFERENCES category (cid)
);

#分类
INSERT INTO category(cid,cname) VALUES('c001','家电');
INSERT INTO category(cid,cname) VALUES('c002','服饰');
INSERT INTO category(cid,cname) VALUES('c003','化妆品');
#商品
INSERT INTO products(pid, pname,price,flag,category_id) VALUES('p001','联想',5000,'1','c001');
INSERT INTO products(pid, pname,price,flag,category_id) VALUES('p002','海尔',3000,'1','c001');
INSERT INTO products(pid, pname,price,flag,category_id) VALUES('p003','雷神',5000,'1','c001');

INSERT INTO products (pid, pname,price,flag,category_id) VALUES('p004','JACK JONES',800,'1','c002');
INSERT INTO products (pid, pname,price,flag,category_id) VALUES('p005','真维斯',200,'1','c002');
INSERT INTO products (pid, pname,price,flag,category_id) VALUES('p006','花花公子',440,'1','c002');
INSERT INTO products (pid, pname,price,flag,category_id) VALUES('p007','劲霸',2000,'1','c002');

INSERT INTO products (pid, pname,price,flag,category_id) VALUES('p008','香奈儿',800,'1','c003');
INSERT INTO products (pid, pname,price,flag,category_id) VALUES('p009','相宜本草',200,'1','c003');



/*
   多表数据查询
   同时查询分类和商品表
   语法:
    select 列名 from 表名1,表名2
*/

# 两个表的乘积效果,数据上是错误的
# 笛卡尔,集合的时候,发现了两个集合中数据的乘积,错误的结果,笛卡尔积
SELECT * FROM category,products;


SELECT COUNT(*) FROM category,products;





```

## 多堆垛用户角色案例

```mysql
/*
  用户和角色:多对多
*/

-- 用户表
CREATE TABLE `user` (
	uid VARCHAR(32) PRIMARY KEY,
	username VARCHAR(32)
);

-- 角色表
CREATE TABLE role (
	rid VARCHAR(32) PRIMARY KEY,
	rname VARCHAR(32)
);

-- 中间表,都是外键
CREATE TABLE user_role(
	uid VARCHAR(32),
	rid VARCHAR(32)
);
/*
  中间表创建外键约束: 省略约束名
  uid外键,user表的uid主键
  rid外键,role表的rid主键
*/
ALTER TABLE user_role ADD CONSTRAINT FOREIGN KEY (uid)
REFERENCES `user` (uid);

ALTER TABLE user_role ADD CONSTRAINT FOREIGN KEY (rid)
REFERENCES role (rid);

# 中间表添加数据,用户是唐国强,角色皇帝
INSERT INTO user_role VALUES(1,2);

# 中间表添加数据,用户是唐国强,角色大臣
INSERT INTO user_role VALUES(1,1);

# 中间表添加数据,用户是刘德华,角色大臣
INSERT INTO user_role VALUES(2,1);

# 中间表添加数据,用户是刘德华,角色歌手
INSERT INTO user_role VALUES(2,3);
```

## 内连接

```mysql
/*
  多表查询: 内连接 隐式
  查询语法:
   select 列名 from 表A,表B
   where条件 主表的主键=从表的外键
*/
SELECT * FROM category,products WHERE
cid = category_id;

/*
  假如,主表的主键和从表的外键的列名,相同
  表名.列名
*/
SELECT * FROM category,products WHERE
category.cid = products.category_id;

/*
  可以使用 as 重命名
*/
SELECT * FROM category c ,products p 
WHERE c.cid = p.category_id;

/*
  内连接: 显示内连接
  关键字 inner join
  条件过滤,关键字 on
*/
SELECT * FROM category c INNER JOIN products p
ON c.cid = p.category_id;


```

## 数据关系-主外键

```mysql
CREATE DATABASE day18;

/*
  商品的分类表
  cid主键自动增长
  cname 商品分类名称
*/
CREATE TABLE category(
	cid INT PRIMARY KEY AUTO_INCREMENT,
	cname VARCHAR(32)
);
/*
  商品表
  pid主键自动增长
  category_id 列,是一个外键列
  列的数据,必须以category表的cid列数据为基准
*/

CREATE TABLE product(
	pid INT PRIMARY KEY AUTO_INCREMENT,
	pname VARCHAR(32),
	price INT,
	category_id INT
);

/*
  创建外键约束
  category表的cid列数据 控制  product表的category_id列中的数据
  语法:
    alter table 从表名 add constraint foreign key [约束名] (从表的外键的列名)
    references 主表名(主键列名)
*/
ALTER TABLE product ADD CONSTRAINT FOREIGN KEY fk_category_id (category_id)
REFERENCES category(cid);

# 商品分类表添加测试数据
INSERT INTO category VALUES(NULL,'家电');
INSERT INTO category VALUES(NULL,'服装');

# 商品信息表添加测试数据,注意category_id列的数据,参照主表的主键编写
INSERT INTO product VALUES(NULL,'联想笔记本',5999,1);
INSERT INTO product VALUES(NULL,'花花公子',50,2);

# 添加测试失败的数据
# 主表中没有的数据,从表中添加失败
INSERT INTO product VALUES(NULL,'饼干',99,3);

/*
  创建订单表 orders
  主键,订单总金额
*/
CREATE TABLE orders(
	oid INT PRIMARY KEY AUTO_INCREMENT,
	total INT
);

# 订单表添加测试数据
# 1号订单,联想,Jack
INSERT INTO orders VALUES(NULL,6199);

# 2订单,联想,雷神,Jack
INSERT INTO orders VALUES(NULL,12199);

/*
  商品和订单是多对多关系,中间表,表示关系
  2个列,都是外键,一个关联商品主键,关联订单主键
*/
CREATE TABLE ordersitem(
	pid INT,
	oid INT
);

/*
  中间表创建外键约束
  中间表的pid列外键,主键是商品表pid
*/
ALTER TABLE ordersitem ADD CONSTRAINT FOREIGN KEY fk_pid (pid)
REFERENCES product(pid);

/*
  中间表创建外键约束
  中间表的oid列外键,主键是订单表oid
*/
ALTER TABLE ordersitem ADD CONSTRAINT FOREIGN KEY fk_oid (oid)
REFERENCES orders(oid);

/*
  中间表添加数据
  # 1号订单,联想,Jack
*/
INSERT INTO ordersitem VALUES(1,1);
INSERT INTO ordersitem VALUES(6,1);

/*
  中间表添加数据
  2订单,联想,雷神,Jack
*/
INSERT INTO ordersitem VALUES(1,2);
INSERT INTO ordersitem VALUES(4,2);
INSERT INTO ordersitem VALUES(6,2);
```

## 索引

```mysql
/*
   索引 index
   海量数据的年代,高效
   MySQL数据库,查询数据的时候,全部数据的扫描,找出你要的数据
   索引技术,提升查询效率
   
   MySQL数据的存储引擎: 数据在硬盘中的存储方式 (文件 010101)
     InnoDB:文件
         支持事务
         支持关系,主外键
         数据增删改慢,查询一般
     MyISAM:文件
         不支持事务
         不支持关系,主外键
         数据增删改,快,查询一般
     
     Memory:存储在内存中,关机,丢失
     
     
        1） 单值索引 ：即一个索引只包含单个列，一个表可以有多个单列索引

	2） 唯一索引 ：索引列的值必须唯一，但允许有空值
	   约束,唯一约束 unique, 自动创建索引
	   

	3） 复合索引 ：即一个索引包含多个列
*/

# 价格上创建索引
CREATE INDEX index_price ON  products(price);
# 查询表中的索引
SHOW INDEX  FROM  products;

# 删除索引
DROP  INDEX  index_price  ON  products;

```

## 一对一省市案例

```mysql
/*
   一对多练习
   省和市
*/

# 创建省份表,主键,名字
CREATE TABLE province(
	pid INT PRIMARY KEY AUTO_INCREMENT,
	pname VARCHAR(20)
);

# 创建城市表,主键,名字,外键
CREATE TABLE city(
	cid  INT PRIMARY KEY AUTO_INCREMENT,
	cname VARCHAR(20),
	pid INT
);

# 城市表添加外键约束,外键是pid,主键是省份表的pid
ALTER TABLE city ADD CONSTRAINT FOREIGN KEY fk_city_pid (pid)
REFERENCES province(pid);

# 省份表添加测试数据
INSERT INTO province VALUES(NULL,'北京市'),(NULL,'河北省'),(NULL,'辽宁省');

# 城市表添加数据,参考省份表的主键!
INSERT INTO city VALUES(NULL,'昌平区',1);
INSERT INTO city VALUES(NULL,'海淀区',1);
INSERT INTO city VALUES(NULL,'朝阳区',1);


INSERT INTO city VALUES(NULL,'石家庄',2);
INSERT INTO city VALUES(NULL,'保定市',2);

INSERT INTO city VALUES(NULL,'沈阳市',3);
INSERT INTO city VALUES(NULL,'葫芦岛市',3);

INSERT INTO city VALUES(NULL,'东莞市',4);

```

## 做外链接

```mysql
/*
  外连接查询:
    左外: left outer join
    右外: right outer join
*/


/*
  左外连接:参照点就是左边的表!!左表的数据必须都显示.
  右边的表,没有数据,必须显示null!!
*/
SELECT * FROM category c LEFT OUTER JOIN products p
ON c.cid = p.category_id;

/*
 右外连接:参照点就是右边的表!!右边表的数据都要显示
 左边的表多余数据,不能显示
*/
SELECT * FROM category c RIGHT OUTER JOIN products p
ON c.cid = p.category_id;
```

## 隔离级别

```mysql
/*
   MySQL隔离级别设置为最低
   read uncommitted 发生脏读
     一个事务,看到了另一个没有提交事务 
     
     
   隔离级别设置到 read committed  脏读不会出现
   但是会出现 重复读
   
   一个事务,看到了另一个事务已经提交的数据 update
   
   
   隔离级别设置到 repeatable read  脏读,重复读 看不到
   虚读: 一个事务,看到了另一个事务已经提交的数据 insert 语句
   
   
*/
SET SESSION TRANSACTION ISOLATION LEVEL 

```

