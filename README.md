# Percona Toolkit 中文文档

Percona Toolkit 是一套高级命令行工具的集合，用于执行一系列过于困难或复杂而难以手动执行的MySQL和系统任务。

Percona Toolkit 是那些私有或一次性脚本的理想替代工具，因为它由专业人士开发，并经过了充分而严谨的测试和验证。安装快速简单，无需依赖任何其他的库。

Percona Toolkit 源自 Maatkit 和 Aspersa 这两个最着名的MySQL服务器管理工具包。

## 获取 Percona Toolkit

* [安装](Installation.md)

## 工具列表
|工具|功能|
|:---|:---|
|[pt-align](pt-align.md) | 列对齐输出 |
|[pt-archiver](pt-archiver.md) |归档表记录|
|[pt-config-diff](pt-config-diff.md)  | 比较MySQL配置文件和服务器参数|
|pt-deadlock-logger | 提取和记录MySQL死锁的相关信息|
|pt-diskstats | 为GUN/LINUX打印磁盘IO统计信息，|
|[pt-duplicate-key-checker](pt-duplicate-key-checker.md) | 从 MySQL 表中找出重复的索引和外键|
|pt-fifo-split | 模拟切割文件并通过管道传递给队列|
|pt-find |查找MySQL表并执行指定的命令|
|pt-fingerprint | 生成查询指纹|
|pt-fk-error-logger | 提取和记录MySQL外键错误信息|
|pt-heartbeat | 监控MySQL复制延迟|
|pt-index-usage |从Log文件中读取插叙语句，并用Explain分析他们是如何利用索引|
|pt-ioprofile | 对某个pid附加一个strace进程进行IO分|
|pt-kill | Kill掉符合指定条件 MySQL 语句|
|pt-mext | 并行查看 SHOW GLOBAL STATUS 的多个样本的信息|
|pt-MySQL-summary | 对MySQL服务器生成一份详细的配置情况以及sataus信息|
|[pt-online-schema-change](pt-online-schema-change.md) | 执行无锁的表结构变更|
|pt-pmp | 为查询程序执行聚合的 GDB 堆栈跟踪|
|pt-query-digest |分析查询执行日志，并产生一个查询报告 |
|pt-show-grants | 规范化和打印 MySQL 权限|
|pt-sift | 浏览pt-stalk生成的文件|
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

## 说明

本文档基于 [Percona Toolkit Documentation](https://www.percona.com/doc/percona-toolkit) 官方英文文档翻译，同时参考了[《高性能MySQL》](http://shop.oreilly.com/product/0636920022343.do)一书。该书的三位主要作者 Baron Schwartz、Peter Zaitsev、Vadim Tkachenko 在MySQL DBA 圈内耳熟能详，他们组建的Percona公司即是Percona Toolkit的开发和维护方。