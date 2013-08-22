18.12.1. Create an auto index for nodes with specific configuration
18.12.2. Create an auto index for relationships with specific configuration
18.12.3. Get current status for autoindexing on nodes
18.12.4. Enable node autoindexing
18.12.5. Lookup list of properties being autoindexed
18.12.6. Add a property for autoindexing on nodes
18.12.7. Remove a property for autoindexing on nodes
开箱即用的自动索引支持精确匹配应为在你第一次访问他们的时候他们带有默认的配置被创建 (查看 Section 14.12, “Automatic Indexing”)。然而在任何索引被创建之前，通过改变他们的配置可以影响服务的整个生命周期。
警告
这种已经自动建立了自动索引的方法不能够用于数据库。改变自动索引的配置必须先删除现有索引，所以要小心。
小心
这种方法行之有效，但它不是非常愉快的。未来版本的Neo4j可能删除这一漏洞赞成一个更好的结构化特性来管理自动索引的配置。
我们配置或者创建他们之前自动索引必须通过配置使其可用。首先确定你已经添加了一些配置像这样到你的服务器配置文件"neo4j.properties"
node_auto_indexing=true
relationship_auto_indexing=true
node_keys_indexable=name,phone
relationship_keys_indexable=since
node_auto_indexing和relationship_auto_indexing设置为节点和关系打开自动索引。node_keys_indexable键允许你指定一个以逗号分隔的节点属性键索引。relationship_keys_indexable 也同样可以对关系属性键做同样的事情。
接下来像往常医药通过调用脚本启动服务器，相关描述在 Section 17.1, “Server Installation”.
接下来，我们必须预先建立一个自动索引，通过告诉服务器创建一个显然的有相同名称的手动索引作为节点（或者关系）的自动索引。例如，在这个例子中，我们创建了一个节点的自动索引，它的名字是 node_auto_index,就像这样：
18.12.1. 为指定配置的节点创建一个自动索引
请求示例
POST http://localhost:7474/db/data/index/node/
Accept: application/json
Content-Type: application/json
{
  "name" : "node_auto_index",
  "config" : {
    "type" : "fulltext",
    "provider" : "lucene"
  }
}
响应示例
201: Created
Content-Type: application/json
Location: http://localhost:7474/db/data/index/node/node_auto_index/
{
  "template" : "http://localhost:7474/db/data/index/node/node_auto_index/{key}/{value}",
  "type" : "fulltext",
  "provider" : "lucene"
}
如果你需要配置自动索引的关系，方法类似：
18.12.2. 为指定配置文件的关系创建自动索引
请求示例
POST http://localhost:7474/db/data/index/relationship/
Accept: application/json
Content-Type: application/json
{
  "name" : "relationship_auto_index",
  "config" : {
    "type" : "fulltext",
    "provider" : "lucene"
  }
}
响应示例
201: Created
Content-Type: application/json
Location: http://localhost:7474/db/data/index/relationship/relationship_auto_index/
{
  "template" : "http://localhost:7474/db/data/index/relationship/relationship_auto_index/{key}/{value}",
  "type" : "fulltext",
  "provider" : "lucene"
}
如果你好奇这是如何工作的，在服务器端触发器创建了一个索引，碰巧由相同的名称作为自动索引，数据库将自己创建。现在，当我们与数据库进行交互，该索引认为认为已经创建好了英次状态及忽略了这一步，就会进行日常的正常的自动索引。
小心
在任何正常的自动索引被创建之前，你必须在服务器生命周期早期做。
这里有少数的几个REST提供了调用AutoIndexer组件的接口。下面的结构调用一起功能，通过简单的改变各自URI的一部分支持节点和关系。
18.12.3. 为节点上的自动索引获取当前状态
请求示例
GET http://localhost:7474/db/data/index/auto/node/status
Accept: application/json
响应示例
200: OK
Content-Type: application/json
false
18.12.4. 使节点能够自动索引
请求示例
PUT http://localhost:7474/db/data/index/auto/node/status
Accept: application/json
Content-Type: application/json
true
响应示例
204: No Content
18.12.5. 查找正在被索引的属性列表
请求示例
GET http://localhost:7474/db/data/index/auto/node/properties
Accept: application/json
响应示例
200: OK
Content-Type: application/json
[ "some-property" ]
18.12.6. 为节点上的自动索引添加属性
请求示例
POST http://localhost:7474/db/data/index/auto/node/properties
Accept: application/json
Content-Type: application/json
myProperty1
响应示例
204: No Content
18.12.7. 为节点上的自动索引移除一个属性
请求示例
DELETE http://localhost:7474/db/data/index/auto/node/properties/myProperty1
Accept: application/json
响应示例
204: No Content


