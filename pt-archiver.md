# pt-archiver

## 名称

pt-archiver - 将mysql数据库中表的记录归档到另外一个表或者文件

## 简介

### 用法
```
pt-archiver [OPTIONS] --source DSN --dest DSN --where WHERE
```
-- source 和 --dest 选项使用DSN（数据源名称）语法(下表）。

  |KEY  |COPY  |MEANING|
  |:---:|:---: |:---|
  |A    |yes   |Default character set|
  |D    |yes   |Database that contains the table|
  |F    |yes   |Only read default options from the given file|
  |L    |yes   |Explicitly enable LOAD DATA LOCAL INFILE|
  |P    |yes   |Port number to use for connection|
  |S    |yes   |Socket file to use for connection|
  |a    |no    |Database to USE when executing queries|
  |b    |no    |If true, disable binlog with SQL_LOG_BIN|
  |h    |yes   |Connect to host|
  |i    |yes   |Index to use|
  |m    |no    |Plugin module name|
  |p    |yes   |Password to use when connecting|
  |t    |yes   |Table to archive from/to|
  |u    |yes   |User for login if not current user|
