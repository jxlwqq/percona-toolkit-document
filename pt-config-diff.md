# pt-config-diff

## 名称

pt-config-diff 比较 MySQL 配置文件和服务器参数

## 简介

### 用法
```
pt-config-diff [OPTIONS] CONFIG CONFIG [CONFIG...]
```

pt-config-diff 用于比较 N(N>=2) 个MySQL 配置文件和服务器参数之间的差异。CONFIG 参数可以是DSN，也可以是指定的文件名。如果没有差异则返回结果没空。

### 示例
#### 命令示例
```
pt-config-diff h=localhost,u=root,p=123456 h=test.test.com,u=root,p=123456

pt-config-diff /etc/my.cnf h=localhost,u=root,p=123456

pt-config-diff /etc/my.cnf /etc/my.cnf.bak
```
#### 返回结果
```
26 config differences
Variable                  bochs-mac.local           localhost.localdomain
========================= ========================= =========================
basedir                   /usr/local/Cellar/mysq... /usr/local/mysql
bind_address              127.0.0.1                 *
character_sets_dir        /usr/local/Cellar/mysq... /usr/local/mysql/share...
datadir                   /usr/local/var/mysql/     /usr/local/mysql/data/
general_log_file          /usr/local/var/mysql/b... /usr/local/mysql/data/...
have_openssl              YES                       DISABLED
have_ssl                  YES                       DISABLED
hostname                  bochs-mac.local           localhost.localdomain
lc_messages_dir           /usr/local/Cellar/mysq... /usr/local/mysql/share/
log_error                 /usr/local/var/mysql/b... /usr/local/mysql/data/...
lower_case_file_system    ON                        OFF
lower_case_table_names    2                         0
pid_file                  /usr/local/var/mysql/b... /usr/local/mysql/data/...
plugin_dir                /usr/local/Cellar/mysq... /usr/local/mysql/lib/p...
server_uuid               4500395a-369f-11e6-923... e1ea23b0-2cb0-11e6-a9c...
slave_load_tmpdir         /var/folders/6h/jd2_5v... /tmp
slow_query_log_file       /usr/local/var/mysql/b... /usr/local/mysql/data/...
socket                    /tmp/mysql.sock           /var/lib/mysql/mysql.sock
ssl_ca                    ca.pem
ssl_cert                  server-cert.pem
ssl_key                   server-key.pem
system_time_zone          CST                       EDT
tls_version               TLSv1,TLSv1.1,TLSv1.2     TLSv1,TLSv1.1
tmpdir                    /var/folders/6h/jd2_5v... /tmp
version_comment           Homebrew                  Source distribution
version_compile_os        osx10.11                  Linux
```

## 风险

Percona Toolkit 是一套成熟的并经过充分与严格测试验证的工具，但是任何一个数据库工具都有可能对系统和数据库服务器造成风险。在使用这个工具之前，请
* 阅读本工具的文档
* 审查本工具已知的 Bug
* 在非生产环境测试本工具
* 备份生产环境并校验该备份

## 描述

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
本工具目前已知的bug：http://www.percona.com/bugs/pt-config-diff