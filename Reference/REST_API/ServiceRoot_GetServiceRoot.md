服务根是你发现REST API的一个起始点。它包括数据库的基本起始点，和一些版本和扩展信息，如果有一个参考节点集并且该节点实际存在与数据库中，reference_node才会出现。
请求例子:
GET http://localhost:7474/db/data/
Accept: application/json
响应实例
200: OK
Content-Type: application/json
{
  "extensions" : {
  },
  "node" : "http://localhost:7474/db/data/node",
  "reference_node" : "http://localhost:7474/db/data/node/101",
  "node_index" : "http://localhost:7474/db/data/index/node",
  "relationship_index" : "http://localhost:7474/db/data/index/relationship",
  "extensions_info" : "http://localhost:7474/db/data/ext",
  "relationship_types" : "http://localhost:7474/db/data/relationship/types",
  "batch" : "http://localhost:7474/db/data/batch",
  "cypher" : "http://localhost:7474/db/data/cypher",
  "neo4j_version" : "1.9.M01"
}
