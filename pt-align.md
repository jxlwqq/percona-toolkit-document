# pt-align

## 名称

pt-align 格式化输出，对齐其他工具输出的列

## 简介

### 用法
```
pt-align [FILES]
```
pt-align 对齐其他工具输出的列。如果没有指定文件，将读取标准输入流(STDIN)：
```
iostat | pt-align
```

### 示例
#### 创建示例文件
```
vi test.txt
```
输入以下文字：
```
DATABASE TABLE   ROWS
foo      bar      100
long_db_name table  1
another  long_name 500
```
#### 命令示例
```
pt-align test.txt
```
#### 返回结果
```
TABASE       TABLE     ROWS
foo          bar        100
long_db_name table        1
another      long_name  500
```

## 风险

Percona Toolkit 是一套成熟的并经过充分与严格测试验证的工具，但是任何一个数据库工具都有可能对系统和数据库服务器造成风险。在使用这个工具之前，请
* 阅读本工具的文档
* 审查本工具已知的 Bug
* 在非生产环境测试本工具
* 备份生产环境并校验该备份

## 描述

pt-align 逐行读取并将它们按单词拆分。它会计算每一行的单词数量，如果某一个数量占优，那么这一数字就会被认定为每行要显示的单词数量。如果有些行低于或高于这一数字，这么这些行将被丢弃。此外，它还会基于这个单词是否是一个数字来决定对齐的方式（数字为右对齐，非数字为左对齐）。最后它计算每个列的最大宽度，然后打印出来。

## 选项

本工具接受额外的命令行参数。

### --help
显示帮助信息。

### --version
显示版本信息。

## 环境
本工具不使用任何环境变量。
## 系统要求

Perl开发环境。
## Bugs

本工具目前已知的bug：http://www.percona.com/bugs/pt-align