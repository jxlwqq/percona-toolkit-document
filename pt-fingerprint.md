# pt-fingerprint

## 名称

**pt-fingerprint** - 生成查询指纹（预处理语句）

## 简介

### 用法

```
pt-fingerprint [OPTIONS] [FILES]
```

**pt-fingerprint** 用于生成查询指纹，通过 `--query` 选项，将选项值转换为指纹。如果没有选项，该命令会将命令行参数视为 FILE 路径，并从FILE读取并转换以分号分隔的查询语句。如果 FILE 是`-`，他将读取标准输入值。

生成一个简单的查询指纹：
```
pt-fingerprint --query "select a, b, c from users where id >= 500 order by id desc limit 1, 100"
```
```
select a, b, c from users where id >= ? order by id desc limit ?
```
从 FILE 文件中读取查询语句并生成查询指纹：
```
pt-fingerprint /path/to/file.txt
```
## 风险

Percona Toolkit 是一套成熟的并经过充分与严格测试验证的工具，但是任何一个数据库工具都有可能对系统和数据库服务器造成风险。在使用这个工具之前，请
* 阅读本工具的文档
* 审查本工具已知的 Bug
* 在非生产环境测试本工具
* 备份生产环境并校验该备份

## 描述

查询指纹是查询的抽象形式，这使得可以将类似查询分组在一起。抽象查询将删除文字值，规范空格等。例如，下面这两个查询：
```
SELECT name, password FROM user WHERE id='12823';
select name,   password from user
   where id=5;
```
这两个查询语句都指向同一个指纹：
```
select name, password from user where id=?
```
一旦确定查询指纹，我们就能可以讨论单独一个查询，因为它代表了所有其他类似的查询。

查询指纹识别适用于许多特殊情况，这在现实中已被证明是必要的。