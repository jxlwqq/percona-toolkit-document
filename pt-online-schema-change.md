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
pt-online-schema-change h=localhost,A=utf8,D=percona,t=user,u=root,p=123456 --alter="add column c1 int not null" --execute
```

#### 返回结果：
```
No slaves found.  See --recursion-method if host web-phpdeMac-mini.local has slaves.
Not checking slave lag because no slaves were found and --check-slave-lag was not specified.
Operation, tries, wait:
  analyze_table, 10, 1
  copy_rows, 10, 0.25
  create_triggers, 10, 1
  drop_triggers, 10, 1
  swap_tables, 10, 1
  update_foreign_keys, 10, 1
Altering `percona`.`user`...
Creating new table...
Created new table percona._user_new OK.
Altering new table...
Altered `percona`.`_user_new` OK.
2016-10-21T18:02:21 Creating triggers...
2016-10-21T18:02:22 Created triggers OK.
2016-10-21T18:02:22 Copying approximately 1 rows...
2016-10-21T18:02:22 Copied rows OK.
2016-10-21T18:02:22 Analyzing new table...
2016-10-21T18:02:22 Swapping tables...
2016-10-21T18:02:23 Swapped original and new tables OK.
2016-10-21T18:02:23 Dropping old table...
2016-10-21T18:02:23 Dropped old table `percona`.`_user_old` OK.
2016-10-21T18:02:23 Dropping triggers...
2016-10-21T18:02:23 Dropped triggers OK.
Successfully altered `percona`.`user`.
```

## 风险

Percona Toolkit 是一套成熟的并经过充分与严格测试验证的工具，但是任何一个数据库工具都有可能对系统和数据库服务器造成风险。在使用这个工具之前，请
* 阅读本工具的文档
* 审查本工具已知的 Bug
* 在非生产环境测试本工具
* 备份生产环境并校验该备份

## 描述
pt-online-schema-change 能够模拟MySQL ALTER TABLE 的内部流程，它作用于原始表的一个副本。这意味着，原始表未被锁定，客户端可继续读取并更改数据。

pt-online-schema-change 的工作原理是创建一个和你要执行 ALTER 操作的表一样的空表结构，执行表结构修改，然后从原始表中拷贝原始数据到表结构修改后的表，当数据拷贝完成以后就会将原表移走，用新表代替原始表，默认动作是将原始表删除(DROP)。

在拷贝数据的过程中，任何在原始表的更新操作都会更新到新表，因为这个工具在会在原始表上创建一个触发器，这个触发器会将在原表上更新的内容更新到新表上。如果表中已经定义了触发器这个工具将无法工作。

当这个工具完成将数据复制到新表中时，它使用原子操作RENAME TABLE同时重命名原始表和新表。 完成后，该工具将删除原始表。