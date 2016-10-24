# pt-archiver

## 名称

pt-archiver - 将MySQL数据库中表的记录归档到另外一个表或者文件

## 简介

### 用法
```
pt-archiver [OPTIONS] --source DSN --dest DSN --where WHERE
```
-- source 和 --dest 选项使用 DSN（数据源名称）语法（下表）。

|KEY  |COPY  |含义|
|:---:|:---: |:---|
|A    |yes   |设置默认字符集|
|D    |yes   |连接的数据库名|
|F    |yes   |从指定的文件中读取默认选项|
|L    |yes   |Explicitly enable LOAD DATA LOCAL INFILE|
|P    |yes   |连接的端口号，默认为3306|
|S    |yes   |连接的Socket文件|
|a    |no    |Database to USE when executing queries|
|b    |no    |如果为True，则将（临时）禁用二进制日志|
|h    |yes   |连接的服务器名|
|i    |yes   |Index to use|
|m    |no    |Plugin module name|
|p    |yes   |连接的密码|
|t    |yes   |连接的数据表名|
|u    |yes   |连接的用户名|

 如果 COPY 值是 yes, –dest 的值在未指定的情况下，默认等于 –source 给定的值.

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