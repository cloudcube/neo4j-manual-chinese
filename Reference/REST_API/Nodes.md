18.4.1. Create Node
18.4.2. Create Node with properties
18.4.3. Get node
18.4.4. Get non-existent node
18.4.5. Delete node
18.4.6. Nodes with relationships can not be deleted
18.4.1. 创建节点
请求示例
POST http://localhost:7474/db/data/node
Accept: application/json
响应示例
201: Created
Content-Type: application/json
Location: http://localhost:7474/db/data/node/34
{
  "extensions" : {
  },
  "paged_traverse" : "http://localhost:7474/db/data/node/34/paged/traverse/{returnType}{?pageSize,leaseTime}",
  "outgoing_relationships" : "http://localhost:7474/db/data/node/34/relationships/out",
  "traverse" : "http://localhost:7474/db/data/node/34/traverse/{returnType}",
  "all_typed_relationships" : "http://localhost:7474/db/data/node/34/relationships/all/{-list|&|types}",
  "property" : "http://localhost:7474/db/data/node/34/properties/{key}",
  "all_relationships" : "http://localhost:7474/db/data/node/34/relationships/all",
  "self" : "http://localhost:7474/db/data/node/34",
  "outgoing_typed_relationships" : "http://localhost:7474/db/data/node/34/relationships/out/{-list|&|types}",
  "properties" : "http://localhost:7474/db/data/node/34/properties",
  "incoming_relationships" : "http://localhost:7474/db/data/node/34/relationships/in",
  "incoming_typed_relationships" : "http://localhost:7474/db/data/node/34/relationships/in/{-list|&|types}",
  "create_relationship" : "http://localhost:7474/db/data/node/34/relationships",
  "data" : {
  }
}
18.4.2. 创建带属性的节点
请求示例
POST http://localhost:7474/db/data/node
Accept: application/json
Content-Type: application/json
{
  "foo" : "bar"
}

响应示例
201: Created
Content-Length: 1108
Content-Type: application/json
Location: http://localhost:7474/db/data/node/35
{
  "extensions" : {
  },
  "paged_traverse" : "http://localhost:7474/db/data/node/35/paged/traverse/{returnType}{?pageSize,leaseTime}",
  "outgoing_relationships" : "http://localhost:7474/db/data/node/35/relationships/out",
  "traverse" : "http://localhost:7474/db/data/node/35/traverse/{returnType}",
  "all_typed_relationships" : "http://localhost:7474/db/data/node/35/relationships/all/{-list|&|types}",
  "property" : "http://localhost:7474/db/data/node/35/properties/{key}",
  "all_relationships" : "http://localhost:7474/db/data/node/35/relationships/all",
  "self" : "http://localhost:7474/db/data/node/35",
  "outgoing_typed_relationships" : "http://localhost:7474/db/data/node/35/relationships/out/{-list|&|types}",
  "properties" : "http://localhost:7474/db/data/node/35/properties",
  "incoming_relationships" : "http://localhost:7474/db/data/node/35/relationships/in",
  "incoming_typed_relationships" : "http://localhost:7474/db/data/node/35/relationships/in/{-list|&|types}",
  "create_relationship" : "http://localhost:7474/db/data/node/35/relationships",
  "data" : {
    "foo" : "bar"
  }
}
18.4.3. 获取节点
该节点包括获取属性和关系的可用操作的URI/templates.
请求示例
GET http://localhost:7474/db/data/node/3
Accept: application/json
响应示例
200: OK
Content-Type: application/json
{
  "extensions" : {
  },
  "paged_traverse" : "http://localhost:7474/db/data/node/3/paged/traverse/{returnType}{?pageSize,leaseTime}",
  "outgoing_relationships" : "http://localhost:7474/db/data/node/3/relationships/out",
  "traverse" : "http://localhost:7474/db/data/node/3/traverse/{returnType}",
  "all_typed_relationships" : "http://localhost:7474/db/data/node/3/relationships/all/{-list|&|types}",
  "property" : "http://localhost:7474/db/data/node/3/properties/{key}",
  "all_relationships" : "http://localhost:7474/db/data/node/3/relationships/all",
  "self" : "http://localhost:7474/db/data/node/3",
  "outgoing_typed_relationships" : "http://localhost:7474/db/data/node/3/relationships/out/{-list|&|types}",
  "properties" : "http://localhost:7474/db/data/node/3/properties",
  "incoming_relationships" : "http://localhost:7474/db/data/node/3/relationships/in",
  "incoming_typed_relationships" : "http://localhost:7474/db/data/node/3/relationships/in/{-list|&|types}",
  "create_relationship" : "http://localhost:7474/db/data/node/3/relationships",
  "data" : {
  }
}

18.4.4. 获取不存在的节点
请求示例
GET http://localhost:7474/db/data/node/800000
Accept: application/json
响应示例
404: Not Found
Content-Type: application/json
{
  "message" : "Cannot find node with id [800000] in database.",
  "exception" : "NodeNotFoundException",
  "stacktrace" : [ "org.neo4j.server.rest.web.DatabaseActions.node(DatabaseActions.java:123)", "org.neo4j.server.rest.web.DatabaseActions.getNode(DatabaseActions.java:234)", "org.neo4j.server.rest.web.RestfulGraphDatabase.getNode(RestfulGraphDatabase.java:235)", "java.lang.reflect.Method.invoke(Method.java:597)" ]
}
18.4.5. 删除节点
请求示例
DELETE http://localhost:7474/db/data/node/43
Accept: application/json
响应示例
204: No Content
18.4.6. 带有关系的节点不能够被删除
删除节点之前，要删除该结点的上的关系。
请求示例
DELETE http://localhost:7474/db/data/node/44
Accept: application/json
响应示例
409: Conflict
Content-Type: application/json
{
  "message" : "The node with id 44 cannot be deleted. Check that the node is orphaned before deletion.",
  "exception" : "OperationFailureException",
  "stacktrace" : [ "org.neo4j.server.rest.web.DatabaseActions.deleteNode(DatabaseActions.java:255)", "org.neo4j.server.rest.web.RestfulGraphDatabase.deleteNode(RestfulGraphDatabase.java:249)", "java.lang.reflect.Method.invoke(Method.java:597)" ]
}




