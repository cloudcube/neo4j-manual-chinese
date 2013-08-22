名称
neo4j-backup-Neo4j备份工具
大纲
neo4j-backup {-full-incremental}-从源地址URI到目录
描述
一个工具，通过网络从一个正在运行的Neo4j的图形数据库进行实时备份到本地
文件系统。备份可以是完全备份或增量。第一次备份必须是一个完整的备份，之后，
可以进行增量备份。
源（S）是在一个特殊的格式的URI，目标是一个文件系统的位置。
备份类别
-full
拷贝整个数据库到一个目录
-incremental
拷贝变化的，从最后的完全或者正两备份从已经存在的备份存储。
源URI
在下面的给是备份源被指出。
<运行模式>://<主机>[:<端口>][,<主机>[:<端口>]]...
注意多个主机将被定义
running mode
'single'或者'ha'.'ha'是一个在高可用模式的一个实例.'single'是独立的数据库
host
在独立模式中，服务器的原备份数据库备份服务。在高可用模式中，集群地址的集群成员。值的注意当使用高可用模式的时候多个主机将被指定。
port
在独立模式中，端口的原备份数据库备份服务。在高可用模式中，端口的集群实例。如果没有给出，默认值为6362将在独立模式中被使用，5001是高可用模式的。
重要
将数据库的配置参数设置为enable_online_backup=true，备份才能够被执行。备份服务在默认端口(6362)才有效。如果要是使用不同端口备份需要配置nable_online_backup=port=9999
替代
windows用法
Neo4jBackuo.bat脚本和Linux是一样的用法
例子
# 一个完整备份
neo4j-backup -full -from single://192.168.1.34 -to /mnt/backup/neo4j-backup
#一个增量备份
neo4j-backup -incremental -from single://freja -to /mnt/backup/neo4j-backup
#自定义端口实现增量备份
neo4j-backup -incremental -from single://freja:9999 -to /mnt/backup/neo4j-backup
#集群完全备份，指定两个集群成员
./neo4j-backup -full -from ha://oden:5001,loke:5002 -to /mnt/backup/neo4j-backup
#集群增量备份，自号定一个集群成员
./neo4j-backup -incremental -from ha://oden:5002 -to /mnt/backup/neo4j-backup
从一个备份恢复
Neo4j的备份功能齐全的数据库。要使用备份，使用备份替换你的数据库目录。



