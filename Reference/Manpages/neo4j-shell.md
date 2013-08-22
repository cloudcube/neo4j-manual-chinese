名称
neo4j-shell-搜索和管理的图形数据库的命令行工具
大纲
neo4j-shell [远程参数]
neo4j-shell [本地参数]
描述
Neo4j命令解释器是一个为浏览图的命令行工具，非常像Unix命令行，比如cd,ls和pwd将被使用浏览本地的文件。该终端能够直接连接一个数据库在文件系统上。能够被其他进程连接，并且使用只读模式。
远程参数
-port 端口
连接到主机端口(默认:1337)
-host 主机
连接到主机的域名或者ip（默认:localhost）
-name 名称
RMI 名称。比如:rmi://<主机>:<端口>/<名称>（默认:shell）
-readonly
授权数据库为只读模式
本地参数
-path 路径
数据库的路径目录。如果在本地不存在数据库，新的一个将被创建。
-pid 进程ID
进程标识连接
-readonly
授权数据库为只读模式
-c 命令
命令行执行，执行后退出shell
-config 配置
Neo4j配置文件的路径将被使用
例子
远程例子
neo4j-shell
neo4j-shell -port 1337
neo4j-shell -host 192.168.1.234 -port 1337 -name shell
neo4j-shell -host localhost -readonly
本地例子
neo4j-shell -path /path/to/db
neo4j-shell -path /path/to/db -config /path/to/neo4j.config
neo4j-shell -path /path/to/db -readonly

