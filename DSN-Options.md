
|KEY  |DSN|COPY  |含义|
|:---:|:---:|:---: |:---|
|A    |charset|yes   |设置默认字符集|
|D    |database|yes   |连接的数据库名|
|F    |mysql_read_default_file|yes   |从指定的文件中读取默认选项|
|L    | |yes   |Explicitly enable LOAD DATA LOCAL INFILE|
|P    |port|yes   |连接的端口号，默认为3306|
|S    |mysql_socket|yes   |连接的Socket文件|
|a    | |no    |Database to USE when executing queries|
|b    | |no    |如果为True，则将（临时）禁用二进制日志|
|h    |host|yes   |连接的服务器名|
|i    | |yes   |Index to use|
|m    | |no    |Plugin module name|
|p    |password|yes   |连接的密码，如果密码含有`,`则需要转义|
|t    |table |yes   |连接的数据表名|
|u    |user|yes   |连接的用户名|