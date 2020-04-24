---
title: mysql命令行操作
date: 2020-04-24 22:29:29
tags: [mysql命令行操作]
---
# 前言

mysql命令行操作

<!--more-->


* ## 打开 MySQL 5.7 Command Line Client - Unicode

* ## 展示所有数据库：
show databases;
```javascript
mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| flasker            |
| mygame_db          |
| mygamedb           |
| mysql              |
| performance_schema |
| python_study       |
| sys                |
| world              |
| wyj_game_db        |
+--------------------+
```

* ## 创建数据库（create database <数据库名>）： 
CREATE DATABASE mytest;
```javascript
mysql> CREATE DATABASE mytest;
 Query OK, 1 row affected (0.00 sec) 
此时数据库存在该数据库：
mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| flasker            |
| mygame_db          |
| mygamedb           |
| mysql              |
| mytest             |
| performance_schema |
| python_study       |
| sys                |
| world              |
| wyj_game_db        |
+--------------------+
```

* ## 删除数据库(drop database <数据库名>) :
 DROP DATABASE myTest;
```javascript
mysql> DROP DATABASE myTest;
Query OK, 0 rows affected (0.00 sec)
此时：
mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| flasker            |
| mygame_db          |
| mygamedb           |
| mysql              |
| performance_schema |
| python_study       |
| sys                |
| world              |
| wyj_game_db        |
+--------------------+
10 rows in set (0.00 sec)
```


* ## 创建数据库： 
CREATE DATABASE mytest;
```javascript
mysql> CREATE DATABASE mytest;
Query OK, 1 row affected (0.00 sec)
```

* ## 删除数据库：drop database <数据库名>
drop database mytest;
```javascript
mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| mytest             |
| performance_schema |
| sakila             |
| sys                |
| world              |
+--------------------+
7 rows in set (0.00 sec)

mysql> drop database mytest;
Query OK, 0 rows affected (0.00 sec)

mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sakila             |
| sys                |
| world              |
+--------------------+
6 rows in set (0.00 sec)
```


* ## 打开某一数据库：
use mytest;
```javascript
mysql> use mytest;
Database changed
```


创建数据库表：
create table <表名> ( <字段名1> <类型1> [,..<字段名n> <类型n>])
CREATE TABLE `user` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `username` varchar(80) DEFAULT NULL,
  `email` varchar(120) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=16 DEFAULT CHARSET=utf8;
```javascript
mysql> CREATE TABLE `user` (
    ->   `id` int(11) NOT NULL AUTO_INCREMENT,
    ->   `username` varchar(80) DEFAULT NULL,
    ->   `email` varchar(120) DEFAULT NULL,
    ->   PRIMARY KEY (`id`)
    -> ) ENGINE=InnoDB AUTO_INCREMENT=16 DEFAULT CHARSET=utf8;
Query OK, 0 rows affected (0.02 sec)
```

* ## 删除数据库中的表：
DROP TABLE user;
```javascript
mysql> show tables;
+------------------+
| Tables_in_mytest |
+------------------+
| user             |
+------------------+
1 row in set (0.00 sec)

mysql> use mytest;
Database changed
mysql> drop table user;
Query OK, 0 rows affected (0.01 sec)

mysql> show tables;
Empty set (0.00 sec)
```


* ## 展示数据库中所有的表：
show tables;
```javascript
mysql> show tables;
+------------------+
| Tables_in_mytest |
+------------------+
| user             |
+------------------+
```

* ## 展示数据库表结构：
desc user;
```javascript
mysql> desc user;
+----------+--------------+------+-----+---------+----------------+
| Field    | Type         | Null | Key | Default | Extra          |
+----------+--------------+------+-----+---------+----------------+
| id       | int(11)      | NO   | PRI | NULL    | auto_increment |
| username | varchar(80)  | YES  |     | NULL    |                |
| email    | varchar(120) | YES  |     | NULL    |                |
+----------+--------------+------+-----+---------+----------------+
3 rows in set (0.01 sec)
```


* ## 插入一条数据：
insert into user(username,email) values('erming','erming@example.com');
（或者：insert into user values(3,'erming','erming@example.com');）
```javascript
mysql> insert into user(username,email) values('erming','erming@example.com');
Query OK, 1 row affected (0.00 sec)

mysql>  select * from user;
+----+----------+----------------------+
| id | username | email                |
+----+----------+----------------------+
|  1 | admin    | xiaoming@example.com |
| 16 | erming   | erming@example.com   |
+----+----------+----------------------+
```


* ## 删除一条数据： 
delete from user where username='xiaoming';
```javascript
mysql> delete from user where username='xiaoming';
Query OK, 1 row affected (0.00 sec)
```


* ## 查找表中某一条数据： 
select * from user where username='admin';
```javascript
mysql> select * from user where username='admin';
+----+----------+----------------------+
| id | username | email                |
+----+----------+----------------------+
|  1 | admin    | xiaoming@example.com |
+----+----------+----------------------+
1 row in set (0.00 sec)

```

* ## 查找表中所有数据：
select * from user;
```javascript
mysql> select * from user;

+----+----------+----------------------+
| id | username | email                |
+----+----------+----------------------+
|  1 | admin    | admin@example.com    |
|  2 | xiaoming | xiaoming@example.com |
+----+----------+----------------------+

mysql>  select * from user;
+----+----------+-------------------+
| id | username | email             |
+----+----------+-------------------+
|  1 | admin    | admin@example.com |
+----+----------+-------------------+
```

* ## 更新一条数据：
update user set email='xiaoming@example.com' where username='admin';
```javascript
mysql> update user set email='xiaoming@example.com' where username='admin';
Query OK, 1 row affected (0.00 sec)
Rows matched: 1  Changed: 1  Warnings: 0

mysql> select * from user;
+----+----------+----------------------+
| id | username | email                |
+----+----------+----------------------+
|  1 | admin    | xiaoming@example.com |
+----+----------+----------------------+
1 row in set (0.00 sec)
```javascript
