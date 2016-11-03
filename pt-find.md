# pt-find

## 名称

**pt-find** - 查找 MySQL 表并执行指定的命令，类似 GNU/Linux 的 find 命令。

## 简介

### 用法
```
pt-find [OPTIONS] [DATABASES]
```
**pt-find** 查找 mysql 表并执行指定的命令，和 gnu 的 find 命令类似。默认动作是打印数据库名和表名。

查找并打印出那些创建时间大于一天前，且数据库引擎是 MyISAM 的所有表：
```
pt-find --ctime +1 --engine MyISAM
```
查找 InnoDB 表并将它们的引擎改为 MyISAM：
```
pt-find --engine InnoDB --exec "ALTER TABLE %D.%N ENGINE=MyISAM"
```
