18.11.1. Find node by exact match from an automatic index
18.11.2. Find node by query from an automatic index
在neo4j中要启用自动索引，设置数据库，参考 Section 14.12.1, “Configuration”.启用该功能后，你可以在这些索引中索引和查询节点。
18.11.1. 从一个自动索引通过精确匹配查找节点
自动索引节点可以使用普通的索引REST语法进行精确查找。
请求示例
GET http://localhost:7474/db/data/index/auto/node/name/I
Accept: application/json
响应示例
200: OK
Content-Type: application/json
[ {
  "outgoing_relationships" : "http://localhost:7474/db/data/node/2/relationships/out",
  "data" : {
    "name" : "I"
  },
  "traverse" : "http://localhost:7474/db/data/node/2/traverse/{returnType}",
  "all_typed_relationships" : "http://localhost:7474/db/data/node/2/relationships/all/{-list|&|types}",
  "property" : "http://localhost:7474/db/data/node/2/properties/{key}",
  "self" : "http://localhost:7474/db/data/node/2",
  "outgoing_typed_relationships" : "http://localhost:7474/db/data/node/2/relationships/out/{-list|&|types}",
  "properties" : "http://localhost:7474/db/data/node/2/properties",
  "incoming_relationships" : "http://localhost:7474/db/data/node/2/relationships/in",
  "extensions" : {
  },
  "create_relationship" : "http://localhost:7474/db/data/node/2/relationships",
  "paged_traverse" : "http://localhost:7474/db/data/node/2/paged/traverse/{returnType}{?pageSize,leaseTime}",
  "all_relationships" : "http://localhost:7474/db/data/node/2/relationships/all",
  "incoming_typed_relationships" : "http://localhost:7474/db/data/node/2/relationships/in/{-list|&|types}"
} ]
18.11.2. 从一个自动索引通过查询查找节点
查看通过查询找到的节点适用于真实的查询语法。
请求示例
GET http://localhost:7474/db/data/index/auto/node/?query=name:I
Accept: application/json
响应示例
200: OK
Content-Type: application/json
[ {
  "outgoing_relationships" : "http://localhost:7474/db/data/node/1/relationships/out",
  "data" : {
    "name" : "I"
  },
  "traverse" : "http://localhost:7474/db/data/node/1/traverse/{returnType}",
  "all_typed_relationships" : "http://localhost:7474/db/data/node/1/relationships/all/{-list|&|types}",
  "property" : "http://localhost:7474/db/data/node/1/properties/{key}",
  "self" : "http://localhost:7474/db/data/node/1",
  "outgoing_typed_relationships" : "http://localhost:7474/db/data/node/1/relationships/out/{-list|&|types}",
  "properties" : "http://localhost:7474/db/data/node/1/properties",
  "incoming_relationships" : "http://localhost:7474/db/data/node/1/relationships/in",
  "extensions" : {
  },
  "create_relationship" : "http://localhost:7474/db/data/node/1/relationships",
  "paged_traverse" : "http://localhost:7474/db/data/node/1/paged/traverse/{returnType}{?pageSize,leaseTime}",
  "all_relationships" : "http://localhost:7474/db/data/node/1/relationships/all",
  "incoming_typed_relationships" : "http://localhost:7474/db/data/node/1/relationships/in/{-list|&|types}"
} ]


