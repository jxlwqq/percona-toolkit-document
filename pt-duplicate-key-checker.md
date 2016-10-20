# pt-duplicate-key-checker

## 名称
 pt-duplicate-key-checker - 从 MySQL 表中找出重复的索引和外键



## 简介


### 用法
```
pt-duplicate-key-checker [OPTIONS] [DSN]
```
pt-duplicate-key-checker 会检查MySQL表中重复或冗余的索引和外键。连接选项 OPTIONS 从 MySQL 配置文件中读取。

### 示例
#### 创建示例表：
```
CREATE DATABASE IF NOT EXISTS percona;
USE percona;
CREATE TABLE IF NOT EXISTS test (
  id INT NOT NULL AUTO_INCREMENT,
  PRIMARY KEY (id),
  UNIQUE KEY idx_uk(id),
  KEY idx_k(id)
);
```
#### 命令示例：
```
pt-duplicate-key-checker --host=localhost --port=3306 --user=root --password=123456 --databases=percona --tables=test
```
#### 返回结果：
```
# ########################################################################
# percona.test
# ########################################################################

# idx_k is a duplicate of PRIMARY
# Key definitions:
#   KEY `idx_k` (`id`)
#   PRIMARY KEY (`id`),
# Column types:
#	  `id` int(11) not null auto_increment
# To remove this duplicate index, execute:
ALTER TABLE `percona`.`test` DROP INDEX `idx_k`;

# Uniqueness of idx_uk ignored because PRIMARY is a duplicate constraint
# idx_uk is a duplicate of PRIMARY
# Key definitions:
#   UNIQUE KEY `idx_uk` (`id`),
#   PRIMARY KEY (`id`),
# Column types:
#	  `id` int(11) not null auto_increment
# To remove this duplicate index, execute:
ALTER TABLE `percona`.`test` DROP INDEX `idx_uk`;

# ########################################################################
# Summary of indexes
# ########################################################################

# Size Duplicate Indexes   8
# Total Duplicate Indexes  2
# Total Indexes            3
```
这个工具会将重复的索引和外键都列出来，并生成了删除重复索引的语句。

## 风险

## 描述

## 选项

## DSN 选项