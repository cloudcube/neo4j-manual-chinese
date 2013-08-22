大纲
neo4j <命令>
描述
Neo4j是图形数据库，适合处理高度连接数据
命令
console 作为一个应用程序启动服务器，作为前台程序运行，使用ctrl-c停止服务器。
start 作为守护进程启动服务器，作为后台程序运行。
stop 停止服务器后台守护进程。
restart 重启服务器
status 当前服务器运行状态。
install 作为一个平台适当的系统服务安装服务器。
remove 卸载系统服务。
info 显示配置信息，例如当前NEO4J——HOME和CLASSPATH
windows使用方法
双击Neo4j的批处理将本将会在当前窗口启动一个服务，在当前窗口按control-c退出。
Neo4j批处理安装/移除
Neo4j可以可以作为移向windows服务安装和运行，并且运行在一个没有控制台的窗口。你需要使用管理员权限运行该脚本。只要使用带有适当参数的Neo4j脚本。
Neo4j.bat install-作为windows服务安装
将安装服务
Neo4j.bat remove-删除Neo4j服务
将同志和删除Neo4j服务
Neo4j.bat start-将启动Neo4j服务
如果已经安装或者一个终端启动Neo4j服务。
其他会话。
Neo4j.bat stop-如果运行将停止Neo4j服务
Neo4j.bat restart-如果安装将重新启动Neo4j服务
Neo4j.bat status-报告Neo4j的服务状态
返回运行，停止或者已经安装。
文件
conf/neo4j-server.properties
服务器配置文件
conf/neo4j-wrapper.conf
服务包装器配置文件
conf/neo4j.properties
调优配置数据库


