# pt-online-schema-change
## 名称
pt-online-schema-change -  执行无锁的表结构变更

## 简介

### 意义

MySQL的ALTER TABLE 是让人痛苦的操作，因为在大部分情况下，它都会锁表并且重建整张表。该操作的性能对大表来说是个大问题。

MySQL执行大部分修改表结构操作的方法是用新的结构创建一个空表，从旧表中查出所有数据插入新表，然后删除旧表。这样操作可能花费很长时间，如果内存不足而表又很大，而且还有很多索引的情况下尤其如此。

一般而言，大部分ALTER TABLE操作将导致MySQL服务中断。对常见的场景，能使用的技巧只有两种：

* 一种是现在一台不提供服务的机器上执行ALTER TABLE操作，然后和提供服务器的主库进行切换；
* 另一种技巧是"影子拷贝"。影子拷贝的技巧是用要求的表结果创建一张和源表无关的新表，然后通过重命名和删表操作交换两张表。

pt-online-schema-change 就是可以帮助完成影子拷贝工作的工具。

### 用法
```
pt-online-schema-change [OPTION] DSN
```

pt-online-schema-change 执行无锁的表结构变更。DSN选项可指定要操作的数据库和表。使用此工具前务必阅读它的文档，并仔细检查备份。

### 示例

#### 创建示例表：
```
CREATE DATABASE IF NOT EXISTS percona;
USE percona;
DROP TABLE IF EXISTS user;
CREATE TABLE IF NOT EXISTS user (
  id       INT         NOT NULL AUTO_INCREMENT,
  PRIMARY KEY (id)
);
```
#### 命令示例：
```
pt-online-schema-change --alter "ADD COLUMN c1 INT" D=percona,t=user
```

