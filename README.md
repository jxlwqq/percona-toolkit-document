# Percona Toolkit 中文文档

Percona Toolkit 是一套高级命令行工具的集合，用于执行一系列过于困难或复杂而难以手动执行的MySQL和系统任务。

Percona Toolkit 是那些私有或临时脚本的理想替代工具，因为它由专业人士开发，并经过了充分而严谨的测试和验证。安装快速简单，无需依赖任何其他的库。

Percona Toolkit 源自 Maatkit 和 Aspersa 这两个著名的MySQL服务器管理工具包。

## 获取 Percona Toolkit

* [安装](Installation.md)

## 工具列表
|工具|功能|
|:---|:---|
|[pt-align](pt-align.md) | 列对齐输出 |
|[pt-archiver](pt-archiver.md) |归档表记录|
|[pt-config-diff](pt-config-diff.md)  | 比较 MySQL 配置文件和服务器参数|
|pt-deadlock-logger | 提取和记录 MySQL 死锁的相关信息|
|pt-diskstats | 为 GUN/LINUX 打印磁盘 IO 统计信息，|
|[pt-duplicate-key-checker](pt-duplicate-key-checker.md) | 从 MySQL 表中找出重复的索引和外键|
|pt-fifo-split | 模拟切割文件并通过管道传递给队列|
|pt-find |查找 MySQL 表并执行指定的命令|
|[pt-fingerprint](pt-fingerprint.md) | 生成查询指纹|
|pt-fk-error-logger | 提取和记录 MySQL 外键错误信息|
|pt-heartbeat | 监控 MySQL 复制延迟|
|pt-index-usage |从 Log 文件中读取插叙语句，并用 Explain 分析他们是如何利用索引|
|pt-ioprofile | 对某个 pid 附加一个 strace 进程进行 IO 分|
|pt-kill | 杀死符合指定条件 MySQL 语句|
|pt-mext | 并行查看 SHOW GLOBAL STATUS 的多个样本的信息|
|pt-mysql-summary | 对 MySQL 服务器生成一份详细的配置情况以及 status 信息|
|[pt-online-schema-change](pt-online-schema-change.md) | 执行 status 无锁的表结构变更|
|pt-pmp | 为查询程序执行聚合的 GDB 堆栈跟踪|
|pt-query-digest |分析查询执行日志，并产生一个查询报告 |
|pt-show-grants | 规范化和打印 MySQL 权限|
|pt-sift | 浏览 pt-stalk 生成的文件|
|pt-slave-delay | 设置从服务器落后于主服务器指定时间|
|pt-slave-find | 查看所有从服务器复制层级关系|
|pt-slave-restart | 监视 MySQL 复制错误，并尝试重启 MySQL 复制当复制停止的时候|
|pt-stalk | 出现问题的时候收集 MySQL 的用于诊断的数据|
|pt-summary | 友好地收集和显示系统信息概况  |
|pt-table-checksum | 检查 MySQL 复制一致性|
|pt-table-sync | 高效同步 MySQL 表的数据|
|pt-table-usage | |
|pt-upgrade | 在多台服务器上执行查询，并比较差异|
|pt-variable-advisor | 分析 MySQL 的参数变量，并对可能存在的问题提出建议|
|pt-visual-explain | 格式化 explain 出来的执行计划按照 tree 方式输出|

## 选项
* [选项（Options）](Options.md)
* [DSN 选项（DSN Options）](DSN-Options.md)

## 说明

本文档基于 [Percona Toolkit Documentation](https://www.percona.com/doc/percona-toolkit) 官方英文文档翻译，同时参考了[《高性能MySQL》](http://shop.oreilly.com/product/0636920022343.do)一书。该书的三位主要作者 Baron Schwartz、Peter Zaitsev、Vadim Tkachenko 在MySQL DBA 圈内耳熟能详，他们组建的Percona公司即是Percona Toolkit的开发和维护方。

