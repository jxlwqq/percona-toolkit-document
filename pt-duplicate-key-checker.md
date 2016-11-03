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
CREATE TABLE IF NOT EXISTS pt_duplicate_key_checker (
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
pt-duplicate-key-checker --host=localhost --port=3306 --user=root --password=123456 --charset=utf8 --databases=percona --tables=pt_duplicate_key_checker
```
#### 返回结果
```
# ########################################################################
# percona.pt_duplicate_key_checker
# ########################################################################

# Uniqueness of idx_i ignored because PRIMARY is a duplicate constraint
# idx_i is a duplicate of PRIMARY
# Key definitions:
#   UNIQUE KEY `idx_i` (`id`),
#   PRIMARY KEY (`id`),
# Column types:
#	  `id` int(11) not null auto_increment
# To remove this duplicate index, execute:
ALTER TABLE `percona`.`pt_duplicate_key_checker` DROP INDEX `idx_i`;

# idx_u is a left-prefix of idx_up
# Key definitions:
#   KEY `idx_u` (`username`),
#   KEY `idx_up` (`username`,`password`),
# Column types:
#	  `username` varchar(50) not null default '' comment '用户名'
#	  `password` char(32) not null default '' comment '密码'
# To remove this duplicate index, execute:
ALTER TABLE `percona`.`pt_duplicate_key_checker` DROP INDEX `idx_u`;

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
连接MySQL时提示输入密码。

### --charset
简写格式：-A
数据类型：字符串（string）

设置默认字符集。如果值是 utf8，首先需要将 Perl 语言的 binmode() 函数的 STDOUT（标准输出）设置为utf8，然后将 mysql_enable_utf8 选项传递给 DBD::mysql，最后在连接MySQL后，运行 SET NAMES UTF8 命令。其他非 utf8 值，在完成设置 binmode() 函数的 STDOUT，连接MySQL后，直接运行 SET NAMES 命令。

### --[no]clustered

### --config
类型：数组（Array）

读取以逗号分隔的配置信息;如果指定--config，则必须是命令行上的第一个选项。

### --databases
简写格式：-d
数据类型：哈希（hash）
仅检查给定的数据库，多个数据库用,分割。
 

### --defaults-file
简写格式：-F
数据类型: 字符串（string）
仅从指定的文件中读取MySQL选项。文件路径必须是一个绝对路径。

### --engines
简写格式: -e
数据类型: 哈希（hash）

 仅检查给定的数据库引擎类型的数据表，多个引擎类型用,分割。

### --help
显示工具的帮助信息。

### --host

简写格式：-h
数据类型：字符串（string）
连接 MySQL 服务器。

### --ignore-databases
数据类型: Hash
忽略给定的数据库，多个数据库以,分割。

### --ignore-engines
数据类型：哈希（Hash）
忽略给定的数据库引擎类型的数据表，多个引擎类型用,分割。

### --ignore-order
忽略联合索引中列的顺序，例如 KEY(a,b) 与 KEY(b,a) 是重复的。

### --ignore-tables
数据类型：哈希（hash）

忽略给定的数据表，多个数据表用,分割。表名前需指定库名。例如 数据库名.数据表名（db_name.table_name）。


### --key-types
数据类型：字符串（string）
默认值：fk

当值为f（指foreign keys 外键）时，仅检查重复外键。
当值为k（指keys 索引），仅检查重复或冗余索引。
当值为fk（只 foreign keys & keys 外键和索引），检查重复外键和索引。

### --password
简写格式：-p
数据类型：字符串（string）

连接数据库的密码。如果密码中包含`,`则需要用`\`转义。

### --pid
数据类型：字符串（string）

创建一个特定的PID文件。如果这个PID文件已经存在并且文件包含的PID不同于当前的PID，则本脚本将不会启用。如果这个PID文件已经存在并且文件包含的PID已不在运行中，则本脚本将用当前的PID覆盖这个PID文件。当脚本执行结束后，这个PID文件将被自动移除。

### --port

简写格式：-P
数据类型：整型（int）
连接MySQL时指定端口号。

### --set-vars

数据类型：数组（Array）

以逗号分隔的键值对列表格式设置MySQL变量。例如 wait_timeout 的默认值为：
```
wait_timeout=10000
```
在命令行上指定的变量将覆盖这些默认值。例如，指定`wait_timeout=500`将覆盖原来的默认值10000。

如果某个变量不能被设置，本脚本将打印警告信息并继续执行。

### --socket

简写格式：-S
数据类型：字符串（string）
连接MySQL时指定Socket文件。

### --[no]sql
默认值：yes
打印每个重复索引的 DROP KEY 的语句。默认情况下，在每个重复索引下面，打印出`ALTER TABLE DROP KEY`的语句，这样就可以通过拷贝粘贴来执行这些语句来删除重复的索引。

如果不想显示这些语句，可以指定 --no-sql。

### --[no]summary

默认值：yes
在输出端打印索引的概要。

### --tables
简写格式：-t
数据类型：哈希（hash）
仅检查给定的数据表，多个表用`,`分割。
表名前应该添加库名。例如`db_name.table_name`。

### --user

简写格式：-u
数据类型：字符串（string）
连接MySQL时的用户名。

### --verbose
简写格式：-v
打印所有的索引和外键，而不仅仅是重复或冗余的部分。

### --version
显示脚本的版本号。

### --[no]version-check

检查Percona Toolkit、MySQL以及其他程序的最新版本。

这个选项在标准的"检查并自动更新"的功能中，加入了两个额外的特性。

首先，该工具除了会检查它自己的版本，还会检查本地系统中其他相关程序的版本。例如，它会检查MySQL服务器、Peal、Peal模块DBD::mysql的版本。

其次，它会检查并警告对应版本的已知问题和bug。例如，MySQL 5.5.25 存在一个非常严重的bug，并且被以 新版本 5.5.25a 的形式重新发布。
 
在默认的结果输出中，任何更新信息或已知问题均会被打印出来。这些额外的特性不会干扰本工具正常的操作。

更新信息可访问：[https://www.percona.com/version-check](https://www.percona.com/version-check)


## DSN 选项

DSN 选项用于创建一个 DSN。每个选线以`option=value`的形式给出。这些选项是区分大小写的，所以`P`和`p`代表着不同的含义。在`=`之前或之后都不能都空格，如果值含有空格，则需要使用引号。DSN 选项之间用逗号分隔。
详细细节可参见 percona-toolkit 帮助页面。

 |KEY  |DSN|COPY  |含义|
|:---:|:---:|:---: |:---|
|A    |charset|yes   |设置默认字符集|
|D    |database|yes   |连接的数据库名|
|F    |mysql_read_default_file|yes   |从指定的文件中读取默认选项|
|P    |port|yes   |连接的端口号，默认为3306|
|S    |mysql_socket|yes   |连接的Socket文件|
|h    |host|yes   |连接的服务器名|
|p    |password|yes   |连接的密码，如果密码含有`,`则需要转义|
|u    |user|yes   |连接的用户名|

## 环境

环境变量 `PTDEBUG` 为 `1` 的话，可以打印出详细的调试信息。要开启调试并将结果输出到一个文件中，可以运行以下命令：
```
PTDEBUG=1 pt-duplicate-key-checker ... > FILE 2>&1
```
注意，调试信息可能会生成几兆字节的输出。

## 系统要求

Perl开发环境。

## Bugs

本工具目前已知的bug：[http://www.percona.com/bugs/pt-align](http://www.percona.com/bugs/pt-align)