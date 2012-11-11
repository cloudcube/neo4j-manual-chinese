18.9.1. Create node index
18.9.2. Create node index with configuration
18.9.3. Delete node index
18.9.4. List node indexes
18.9.5. Add node to index
18.9.6. Remove all entries with a given node from an index
18.9.7. Remove all entries with a given node and key from an index
18.9.8. Remove all entries with a given node, key and value from an index
18.9.9. Find node by exact match
18.9.10. Find node by query
一个索引可以包含节点或者关系。
注意
当创建一个带有默认配置的索引，通过添加节点/关系到它开始简单的使用它，然后，它会自动为你创建。
默认配置意味这取决你如何配置你的数据库。如果你还没有改变任何索引配置，这意味这索引将使用一个基于Lucene的后端。
下面的所有示例将会为你展示如何操作节点上的索引，但他们同样适用于关系索引，简单的将"node"部分修改为"relationship".
如果你想自定义索引设置，查看Section 18.9.2, “Create node index with configuration”.
18.9.1. 创建节点索引
注意
这个方式创建所以，你可以简单的开始使用它，它会被带有默认配置自动创建。

请求示例
POST http://localhost:7474/db/data/index/node/
Accept: application/json
Content-Type: application/json
{
  "name" : "favorites"
}
响应示例
201: Created
Content-Type: application/json
Location: http://localhost:7474/db/data/index/node/favorites/
{
  "template" : "http://localhost:7474/db/data/index/node/favorites/{key}/{value}"
}

18.9.2. 创建带有配置的节点索引
这种请求方式仅限于你向自定义索引设置，如果你喜欢默认配置，你就可以开始所以节点/关系，因为不存在的索引会自动为你创建，索引配置的更多信息查看Section 14.10, “Configuration and fulltext indexes”。
请求示例
POST http://localhost:7474/db/data/index/node/
Accept: application/json
Content-Type: application/json
{
  "name" : "fulltext",
  "config" : {
    "type" : "fulltext",
    "provider" : "lucene"
  }
}
响应示例
201: Created
Content-Type: application/json
Location: http://localhost:7474/db/data/index/node/fulltext/
{
  "template" : "http://localhost:7474/db/data/index/node/fulltext/{key}/{value}",
  "type" : "fulltext",
  "provider" : "lucene"
}
18.9.3. 删除节点索引
请求示例
DELETE http://localhost:7474/db/data/index/node/kvnode
Accept: application/json

响应示例
204: No Content
18.9.4. 列出节点索引
请求示例
GET http://localhost:7474/db/data/index/node/
Accept: application/json
响应示例
200: OK
Content-Type: application/json
{
  "node_auto_index" : {
    "template" : "http://localhost:7474/db/data/index/node/node_auto_index/{key}/{value}",
    "provider" : "lucene",
    "type" : "exact"
  },
  "favorites" : {
    "template" : "http://localhost:7474/db/data/index/node/favorites/{key}/{value}",
    "provider" : "lucene",
    "type" : "exact"
  }
}
18.9.5. 添加节点到索引
联合一个在指定索引中带有给定键/值对的节点
注意
分割符在URI必须被编码为%20
警告
这并不覆盖以前的条目。如果你索引相同的键/值对条目联合两次，两个索引条目将被创建。做更新类操作的时候，你需要先删除旧条目，在添加新的。
请求示例
POST http://localhost:7474/db/data/index/node/favorites
Accept: application/json
Content-Type: application/json
{
  "value" : "some value",
  "uri" : "http://localhost:7474/db/data/node/178",
  "key" : "some-key"
}
响应示例
201: Created
Content-Type: application/json
Location: http://localhost:7474/db/data/index/node/favorites/some-key/some%20value/178
{
  "extensions" : {
  },
  "paged_traverse" : "http://localhost:7474/db/data/node/178/paged/traverse/{returnType}{?pageSize,leaseTime}",
  "outgoing_relationships" : "http://localhost:7474/db/data/node/178/relationships/out",
  "traverse" : "http://localhost:7474/db/data/node/178/traverse/{returnType}",
  "all_typed_relationships" : "http://localhost:7474/db/data/node/178/relationships/all/{-list|&|types}",
  "property" : "http://localhost:7474/db/data/node/178/properties/{key}",
  "all_relationships" : "http://localhost:7474/db/data/node/178/relationships/all",
  "self" : "http://localhost:7474/db/data/node/178",
  "outgoing_typed_relationships" : "http://localhost:7474/db/data/node/178/relationships/out/{-list|&|types}",
  "properties" : "http://localhost:7474/db/data/node/178/properties",
  "incoming_relationships" : "http://localhost:7474/db/data/node/178/relationships/in",
  "incoming_typed_relationships" : "http://localhost:7474/db/data/node/178/relationships/in/{-list|&|types}",
  "create_relationship" : "http://localhost:7474/db/data/node/178/relationships",
  "data" : {
  },
  "indexed" : "http://localhost:7474/db/data/index/node/favorites/some-key/some%20value/178"
}
18.9.6. 移除带有从一个索引给定节点的所有条目
请求示例
DELETE http://localhost:7474/db/data/index/node/kvnode/188
Accept: application/json
响应示例
204: No Content

18.9.7. 移除一个给定节点和从一个索引键的所有条目
请求示例
DELETE http://localhost:7474/db/data/index/node/kvnode/kvkey2/189
Accept: application/json
响应示例
204: No Content
18.9.8. 移除一个给定节点，从一个索引的键和值的所有条目
请求示例
DELETE http://localhost:7474/db/data/index/node/kvnode/kvkey1/value1/190
Accept: application/json
响应示例
204: No Content
18.9.9. 通过精确匹配找到节点
注意
分隔符号在URI中必须被编码为"%20"
请求示例
GET http://localhost:7474/db/data/index/node/favorites/key/the%2520value
Accept: application/json
响应示例
200: OK
Content-Type: application/json
[ {
  "indexed" : "http://localhost:7474/db/data/index/node/favorites/key/the%2520value/179",
  "outgoing_relationships" : "http://localhost:7474/db/data/node/179/relationships/out",
  "data" : {
  },
  "traverse" : "http://localhost:7474/db/data/node/179/traverse/{returnType}",
  "all_typed_relationships" : "http://localhost:7474/db/data/node/179/relationships/all/{-list|&|types}",
  "property" : "http://localhost:7474/db/data/node/179/properties/{key}",
  "self" : "http://localhost:7474/db/data/node/179",
  "outgoing_typed_relationships" : "http://localhost:7474/db/data/node/179/relationships/out/{-list|&|types}",
  "properties" : "http://localhost:7474/db/data/node/179/properties",
  "incoming_relationships" : "http://localhost:7474/db/data/node/179/relationships/in",
  "extensions" : {
  },
  "create_relationship" : "http://localhost:7474/db/data/node/179/relationships",
  "paged_traverse" : "http://localhost:7474/db/data/node/179/paged/traverse/{returnType}{?pageSize,leaseTime}",
  "all_relationships" : "http://localhost:7474/db/data/node/179/relationships/all",
  "incoming_typed_relationships" : "http://localhost:7474/db/data/node/179/relationships/in/{-list|&|types}"
} ]
18.9.10. 通过查询查找节点
这里使用的查询语言取决于你是什么类型的索引查询。默认的索引类型是Lucene,在这种情况下，在这里你应该使用Lucene查询语言，下面的例子是模糊搜索多个键。
查看
http://lucene.apache.org/java/3_5_0/queryparsersyntax.html
得到一个带有预定义排序的查询需要添加参数。
order=ordering
在排序是索引、相关性、分数中的一个。在这种情况下，一个额外的字段将被添加到每个结果，持有分数报告的浮动值的查询结果。
请求示例
GET http://localhost:7474/db/data/index/node/bobTheIndex?query=Name:Build~0.1%20AND%20Gender:Male
Accept: application/json
响应示例
200: OK
Content-Type: application/json
[ {
  "outgoing_relationships" : "http://localhost:7474/db/data/node/180/relationships/out",
  "data" : {
    "Name" : "Builder"
  },
  "traverse" : "http://localhost:7474/db/data/node/180/traverse/{returnType}",
  "all_typed_relationships" : "http://localhost:7474/db/data/node/180/relationships/all/{-list|&|types}",
  "property" : "http://localhost:7474/db/data/node/180/properties/{key}",
  "self" : "http://localhost:7474/db/data/node/180",
  "outgoing_typed_relationships" : "http://localhost:7474/db/data/node/180/relationships/out/{-list|&|types}",
  "properties" : "http://localhost:7474/db/data/node/180/properties",
  "incoming_relationships" : "http://localhost:7474/db/data/node/180/relationships/in",
  "extensions" : {
  },
  "create_relationship" : "http://localhost:7474/db/data/node/180/relationships",
  "paged_traverse" : "http://localhost:7474/db/data/node/180/paged/traverse/{returnType}{?pageSize,leaseTime}",
  "all_relationships" : "http://localhost:7474/db/data/node/180/relationships/all",
  "incoming_typed_relationships" : "http://localhost:7474/db/data/node/180/relationships/in/{-list|&|types}"
} ]





