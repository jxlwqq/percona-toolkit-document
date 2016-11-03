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

查询指纹识别适用于许多特殊情况，这在现实中已被证明是必要的。例如，具有5个值的 IN 列表实际上等于具有 4 个值的IN列表：
```
select * from user where user_id in(1,2,3,4);
select * from user where user_id in(1,2,3,4,5);
```
这两个查询语句都指向同一个指纹：
```
select * from user where user_id in(?+)
```
以下是在查询指纹识别期间的转换逻辑：
* 将来自`mysqldump`的所有SELECT查询分组在一起，即使它们针对不同的表。
* 将多值`INSERT`语句缩短为单个`VALUES()`列表
* 删除注释
* 抽象`USE`语句中的数据库，因此所有`USE`语句被分组在一起
* 替换所有值，例如带引号的字符串。为了效率，数字替换可能会有些表现不是那么完美，有些数字型的值可能会被错误的替换掉。例如嵌入在标识符中的数字也被替换，因此类似命名的表将被指纹到相同的值（例如，users_2009 和 users_2010 指纹将相同）。此外 十六进制数值也被替换。`NULL`也被视为一个值将被替换。
* 将多个连续空格压缩到一个空格中。
* 小写整个查询
* 用一个占位符替换`IN()`和`VALUES()`列表中的所值，而不考虑具体数量
* 将多个相同的UNION查询合并为一个
 
## 选项

本工具接受额外的命令行参数。

### --config

数据类型：数组（Array）

读取以逗号分隔的配置文件列表。如果指定，这必须是命令行上的第一个选项。

### --help
显示帮助信息。



### match-embedded-numbers

匹配嵌入在字符串中的数字，并替换为单个值。此选项使工具在匹配那些嵌入在单词中的数字时更加小心。例如加上这个选项，`catch22` 将不会被替换。相反默认不加这个选项，`catch22`会被替换成`catch?`。

如果数据库或表名包含数字，加上这个参数将非常有用。

### --match-md5-checksums
匹配md5校验和，并替换为单个值。此选项是工具在匹配md5校验和时更加小心。例如 `fbc5e685a5d3d45aa1d0347fdb7c4d35` 将会被
整体被`?`替换。而默认不带这个参数，这个校验和将会被替换为`fbc?`。

### --query
数据类型：字符串（string）
需要生成指纹的查询语句。


### --version
显示版本信息。

## 环境

环境变量 `PTDEBUG` 为 `1` 的话，可以打印出详细的调试信息。要开启调试并将结果输出到一个文件中，可以运行以下命令：
```
PTDEBUG=1 pt-fingerprint ... > FILE 2>&1
```
注意，调试信息可能会生成几兆字节的输出。

## 系统要求

Perl 开发环境。

## Bugs

本工具目前已知的 bug：http://www.percona.com/bugs/pt-fingerprint

