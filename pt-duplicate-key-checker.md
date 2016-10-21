# pt-duplicate-key-checker

## 名称
pt-duplicate-key-checker - 从 MySQL 表中找出重复的索引和外键

## 简介

### 意义

重复索引是指在相同的列上按照相同的顺序创建的相同类型的索引。

冗余索引和重复索引有一些不同。如果创建了索引(A, B)，再在创建索引(A)就是冗余索引，因为这只是前一个索引的前缀索引。因此索引(A,B)也可以当做索引(A)来使用（这种冗余只是针对B-Tree索引来说的）。但如果在创建索引(B,A)则不是冗余索引，索引(B)也不是，因为B不是索引(A,B)的最左前缀列。另外，其他不同类型的索引（例如哈希索引或者全文索引）也不会是B-Tree索引的冗余索引，而无论覆盖的索引列是什么。

应该避免这样创建重复索引或冗余索引，发现以后也应该立即移除。MySQL虽然允许在相同列上创建多个索引，但需要单独维护重复的索引，并且优化器在优化查询时也需要逐个地进行考虑，这会影响性能。

### 用法
```
pt-duplicate-key-checker [OPTIONS] [DSN]
```
pt-duplicate-key-checker 会检查MySQL表中重复或冗余的索引和外键。连接选项 OPTIONS 从 MySQL 配置文件中读取。

### 示例
#### 创建示例表
```
CREATE DATABASE IF NOT EXISTS percona;
USE percona;
CREATE TABLE IF NOT EXISTS user (
  id       INT         NOT NULL AUTO_INCREMENT,
  username VARCHAR(50) NOT NULL DEFAULT '' COMMENT '用户名',
  email    VARCHAR(50) NOT NULL DEFAULT '' COMMENT '邮箱',
  password CHAR(32)    NOT NULL DEFAULT '' COMMENT '密码',
  PRIMARY KEY (id),
  UNIQUE KEY idx_i(id),
  KEY idx_u(username),
  KEY idx_up(username, password),
  KEY idx_e(email),
  FULLTEXT KEY (email)
);
```
#### 命令示例
```
pt-duplicate-key-checker --host=localhost --port=3306 --user=root --password=123456 --charset=utf8 --databases=percona --tables=user
```
#### 返回结果
```
# ########################################################################
# percona.user
# ########################################################################

# Uniqueness of idx_i ignored because PRIMARY is a duplicate constraint
# idx_i is a duplicate of PRIMARY
# Key definitions:
#   UNIQUE KEY `idx_i` (`id`),
#   PRIMARY KEY (`id`),
# Column types:
#	  `id` int(11) not null auto_increment
# To remove this duplicate index, execute:
ALTER TABLE `percona`.`user` DROP INDEX `idx_i`;

# idx_u is a left-prefix of idx_up
# Key definitions:
#   KEY `idx_u` (`username`),
#   KEY `idx_up` (`username`,`password`),
# Column types:
#	  `username` varchar(50) not null default '' comment '用户名'
#	  `password` char(32) not null default '' comment '密码'
# To remove this duplicate index, execute:
ALTER TABLE `percona`.`user` DROP INDEX `idx_u`;

# ########################################################################
# Summary of indexes
# ########################################################################

# Size Duplicate Indexes   156
# Total Duplicate Indexes  2
# Total Indexes            6
```
这个工具会将重复的索引和外键都列出来，并生成了删除重复索引的语句。

## 风险

Percona Toolkit 是一套成熟的并经过充分与严格测试验证的工具，但是任何一个数据库工具都有可能对系统和数据库服务器造成风险。在使用这个工具之前，请
* 阅读本工具的文档
* 审查本工具已知的 Bug
* 在非生产环境测试本工具
* 备份生产环境并校验该备份

## 描述

本工具会检查在MySQL表上执行 SHOW CREATE TABLE 命令的输出，并打印出重复索引和冗余索引。此外本工具还能检出重复的外键。重复外键是指那些覆盖同一表的相同列，且引用相同父表的外键。

本工具会输出一个简短的摘要，包括重复索引使用的总大小的估计值（以字节为单位）。该值是通过将索引长度乘以它们各自表中的行数计算得出的。

## 选项

本工具接受一些额外的命令行参数。

### --all-structs

比较不同类型的索引（例如B-Tree、Hash等）。
默认为False，因为即使覆盖的索引列完全相同，由于索引的类型不同，所以不能被认为是重复索引。

### --ask-pass
连接MySQL时提示输入密码

### --charset
设置默认的字符集。简写形式为: -A。

## DSN 选项