# pt-archiver

## 名称

pt-archiver - 将MySQL数据库中表的记录归档到另外一个表或者文件

## 简介

### 用法
```
pt-archiver [OPTIONS] --source DSN --dest DSN --where WHERE
```
-- source 和 --dest 选项使用 DSN（数据源名称）语法。如果 COPY 值是 yes, –dest 的值在未指定的情况下，默认等于 –source 给定的值.

### 示例

#### 创建示例表
```
CREATE DATABASE IF NOT EXISTS percona;
USE percona;
CREATE TABLE pt_archiver (
  id INT         NOT NULL AUTO_INCREMENT,
  c1 VARCHAR(50) NOT NULL DEFAULT '',
  c2 VARCHAR(50) NOT NULL DEFAULT '',
  c3 VARCHAR(50) NOT NULL DEFAULT '',
  PRIMARY KEY (id)
);
INSERT INTO pt_archiver (c1, c2, c3) VALUES ('fake_data', rand(), rand());
```
#### 命令示例
```

```