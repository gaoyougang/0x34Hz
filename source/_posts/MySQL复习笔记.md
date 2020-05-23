---
title: MySQL复习笔记
date: 2020-05-19 19:10:51
author:
img:
top:
cover:
coverImg:
password:
toc:
mathjax:
summary:
categories: 学习
tags: mysql
reprintPolicy:
---
# MySQL复习笔记

![Mysql](https://encrypted-tbn0.gstatic.com/images?q=tbn%3AANd9GcT9kgd-ZtbiOdvuEOFzKTBBqA1bv2eMMrHynHlGSN13CuMd9iy9&usqp=CAU)

[MySQL](https://www.runoob.com/mysql/mysql-tutorial.html)详细教程

*数据*：描述事物的符号记录，可经过数字化存入计算机，形式多样。

*数据库*：存储数据的仓库，是长期存放在计算机内、有组织、可共享的大量数据的集合。

*特点*：

+ 数据结构化
+ 数据的共享性高，冗余度低，易扩充
+ 数据独立性高
+ 由DBMS统一管理和控制(安全性、完整性、并发控制、故障恢复)

*两种登录方式：*

**TCP/IP方式（远程、本地）：**`mysql -uroot -p123456 -h 192.168.1.36 -P3306`

**Socket方式（本地）**：`mysql -uroot -p123456 -S /tmp/mysql.sock`

## MySQL日常操作命令

```mysql
#增加(INSERT)
create database test_db charset=utf8; 	#创建名为test_db的数据库，字符设为utf-8
use test_db;	#进入test_db数据库
create table test01(id varchar(20),name varchar(20));	#创建名为test01表，并创建两个字段，id、name数据长度用(定义长度)
insert into test01 values (’001‘,‘kali’);	#向表中插入数据

#删除(DELETE)
drop database test_db;  #删了库，快跑路
drop table test01;  #删除test01表
truncate table test01; #删除test01表内容
delete from test01;  #清空表内容
delete from test01 where name='kali'; #删除指定你内容

#修改(UPDATE)
grant all on test_db.* to gt@'localhost' identified by "123456";  #修改数据库test_db内所有文件的权限给用户gt，允许其通过123456本地访问test_db数据库进行777
grant select on test_db.* to gt@'192.168.1.36' identified by "123456";  #给gt用户通过密码123456,以只读的权限通过192.158.1.36访问服务端内test_db数据库，权限有(select\insert\update\delete)
update test01 set name='TIMI' where name='kali'; #修改数据指定内容

#查询(SELECT)
select user,host from mysql.user;  #查看数据库所有用户和其登录设备
show databases;	#查看存在的数据库
show tables;	#查看数据库里有多少张表
show variables like "%char%";  #查看数据库字符集
show processlist;		#查看你进程列表
select * from test01;	#查看test01表中的数据内容
select * from test01 where id='001' and name='kali';  #多条件查找
select * from test01 where name like "%ka%" limit 1;  #匹配限制查询(匹配含有ka，且输出一条信息)
show create table test01;   #查看创建test01表用的语句和一些信息
desc test01;  #查看test01表的字段结构(desc等价于describe)

flush privileges;  #刷新权限
source test_db.sql;  #当前目录查找导入
quit; #或者exit;退出数据库
```

**导出数据库(备份）**：`mysqldump -uroot -p123456 test_db > ./test_db.sql`

**导入数据库**：`mysql -uroot -p123456 test_db < ./test_db.sql`

**修改mysql中root密码**：`mysqladmin -uroot -p123456 password newpassword`

## 用Shell编写简单的mySQL备份脚本

```shell
#！/bin/sh
# auto to backup mySQL
# date: 2020-5-19

# Define PATH 定义变量
BAKDIR=/data/backup/mysql/`date +%Y-%m-%d`
MYSQLDB=test_db
MYSQLPW=123456
MYSQLUSR=gt
MYSQLCMD=/usr/bin/mysqldump

#判断是否是root用户在运行，$UID系统变量
if
	[ $UID -ne 0 ];then
	echo "Must to be root can run this shell!"
	sleep 2
	exit 0
fi

#判断备份目录是否存在，不存在则新建
if
	[ ! -d $BAKDIR ];then
	mkdir -p $BAKDIR
	echo -e "\033[32mThe $BAKDIR create successfully!\033[0m"
else
	echo "The $BAKDIR exists...."
fi

#运行备份语句,可通过which mysqldump查找命令所在绝对路径
$MYSQLCMD -u$MYSQLUSR -p$MYSQLPW -d $MYSQLDB > $BAKDIR/$MYSQLDB.sql
if [ $? -eq 0 ];then
	echo  -e "\033[32mThe mysql backup $MYSQLDB successfully.\033[0m"
else
	echo "The mysql backup $MYSQLDB Failed, please check."
fi
```

## Mysql设置UTF-8和密码破解

1. *Mysql数据库中，插入中文乱码，何解？*

   **修改/etc/my.cnf**

   ```
   [client]字段里加入	default-character-set=utf8
   [mysqld]字段里加入	character-set-server=utf8
   [mysql] 字段里加入	default-character-set=utf8
   ```

   然后重启mysql服务即可！

2. *Mysql忘记密码，何解？*

   ```
   #关闭mysql服务，跳过权限启动
   /usr/bin/mysqld_safe --user=mysql --skip-grant-tables &
   mysql    #回车就可免密登录
   #在数据库里执行，将root密码改为654321
   use mysql
   update user set password=password("654321") where user='root'; 
   quit;
   /usr/bin/mysqld_safe --user=mysql &
   #查看mysql进程，杀死不必要的两个，留下--color=auto mysql
   ps -ef | grep mysql
   #重启mysql服务
   systemctl restart mysqld
   ```

   已知密码为654321，修改密码为123456

   ```
   mysqladmin -uroot -p password 123456
   ```

## Mysql建表约束

1. 办主键约束：能唯一确定一张表中的 一条记录，不重复且不为空

   ```mysql
   #单个字段创建主键
   create table user(
       id int primary key,  #表示id只能唯一，不能重复和为空
       name varchar(20),
       password varchar(20)
   );
   insert into user values (1,'root','123456');
   
   #联合主键
   create table user1(
       id int, 
       name varchar(20),
       password varchar(20),
       primary key(id,name)  #表示id name不能同时相同
   );
   insert into user1 values (1,'root','123456');
   insert into user1 values (1,'gt','123456');  #插入合法，只要id name不同时相
   ```

2. 自增约束：常与主键约束组合使用

   ```mysql
   create table user2(
   	id int primary key auto_increment,
       name varchar(20)
   );
   insert into user2 (name) values ('root');  #序号自增
   
   #创建表时忘记创建主键时
   create table user3(
   	id int,
       name varchar(20)
   );
   #方法一：
   alter table user3 add primary key(id);  #添加主键
   desc user3;  #查看字段与添加外键执行之后对比
   alter table user3 drop primary key; #删除主键
   
   #方法二：
   alter table user3 modify id int primary key; #通过修改字段的方式
   ```

3. 唯一约束：约束修饰字段的值不能重复

   ```mysql
   #添加方法一
   create table user4(
   	id int,
       name varchar(20)
   );
   #添加唯一约束
   alter table user4 add unique(name);
   #查看字段
   desc user4;
   
   #方法二
   create table user5(
   	id int,
       name varchar(20),
       unique(id，name)  #与联合主键相似，只要不同时相同
   );
   
   #删除唯一主键
   alter table user4 drop index name;
   
   #方法三(modify)
   alter table user4 modify name varchar(20) unique;
   ```

4. 非空约束：修饰的字段不能为空

   ```mysql
   create table user6(
   	id int,
       name varchar(20) not null
   );
   insert into user6 (id) values (1); #会报错'name' doesn't have a default value
   insert into user6 (name) values ('gt'); #不会报错
   ```

5. 默认约束：当插入字段未传值时，会默认使用默认约束的值

   ```mysql
   create table user7(
       id int,
       name varchar(20),
       age int default 10
   );
   insert into user7 (id,name) values (1,'kali'); #age会默认为10
   insert into user7 values (2,'gt',19); #age这里会是19
   #同样可以通过alter修改
   ```

6. 外键约束：涉及子父表的时候

   ```mysql
   create table classes(
   	id int primary key,
       name varchar(20)
   );
   
   #子表
   create table students(
   	id int primary key,
       name varchar(20),
       class_id int,
       foreign key(class_id) references classes(id)
   );
   
   insert into classes values(1,'class1');
   insert into classes values(2,'class2');
   insert into classes values(3,'class3');
   insert into classes values(4,'class4');
   
   insert into students values(1001,'kali',1);
   insert into students values(1002,'kali',2);
   insert into students values(1003,'kali',3);
   insert into students values(1004,'kali',4);  #这样插入不会报错
   
   insert into students values(1005,'kali',5); #会报错，因为父表内没有5班
   #父表中没有的数据值，再子表中不可使用;父表被子表引用的记录，不可被删除
   ```