# Options

### --ask-pass
连接 MySQL 时，提示输入密码。

### --charset
简写格式：-A；类型：字符串（string）

设置默认字符集。如果值是 utf8，首先需要将 Perl 语言的 binmode() 函数的 STDOUT（标准输出）设置为utf8，然后将 mysql_enable_utf8 选项传递给 DBD::mysql，最后在连接MySQL后，运行 SET NAMES UTF8 命令。其他非 utf8 值，在完成设置 binmode() 函数的 STDOUT，连接MySQL后，直接运行 SET NAMES 命令。
```
binmode(STDOUT, ':encoding(utf8)');
```

