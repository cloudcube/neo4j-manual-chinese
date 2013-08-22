18.17.1. Send a Query
18.17.2. Return paths
18.17.3. Send queries with parameters
18.17.4. Server errors
警告
这个功能是现在提供的核心REST API。插件将持续工作一段时间，但截至neo4j 1.6弃用。查看 Section 18.3, “Cypher queries” 的文档内置Cypher支持。
Neo4j插件可以使用 Chapter 15, Cypher Query Language查询。结果返回一个字符串头（columns）,和一部分data,组成一个列表的所有行，每一行都包括一个REST 表示的列表的字段值-Node,Relationship或者像String简单值。
18.17.1. 发送一个查询
一个简单查询，返回连接到node 1的所有的节点，返回这些节点和名称属性，如果存在的化。否则返回null:
START x  = node(3)
MATCH x -[r]-> n
RETURN type(r), n.name?, n.age?
请求示例
POST http://localhost:7474/db/data/ext/CypherPlugin/graphdb/execute_query
Accept: application/json
Content-Type: application/json
{
  "query" : "start x  = node(3) match x -[r]-> n return type(r), n.name?, n.age?",
  "params" : {
  }
}
响应示例
200: OK
Content-Type: application/json
{
  "columns" : [ "type(r)", "n.name?", "n.age?" ],
  "data" : [ [ "know", "him", 25 ], [ "know", "you", null ] ]
}
18.17.2. 返回路径
路径能够被一起返回使用指定的返回类型。
START x  = node(7)
MATCH path = (x--friend)
RETURN path, friend.name
请求示例
POST http://localhost:7474/db/data/ext/CypherPlugin/graphdb/execute_query
Accept: application/json
Content-Type: application/json
{
  "query" : "start x  = node(7) match path = (x--friend) return path, friend.name",
  "params" : {
  }
}
响应示例
200: OK
Content-Type: application/json
{
  "columns" : [ "path", "friend.name" ],
  "data" : [ [ {
    "start" : "http://localhost:7474/db/data/node/7",
    "nodes" : [ "http://localhost:7474/db/data/node/7", "http://localhost:7474/db/data/node/6" ],
    "length" : 1,
    "relationships" : [ "http://localhost:7474/db/data/relationship/3" ],
    "end" : "http://localhost:7474/db/data/node/6"
  }, "you" ] ]
}发送
18.17.3. 发送带有参数的查询
Cypher支持带有参数的查询，这些参数作为一个JSON映射被提交
START x  = node:node_auto_index(name={startName})
MATCH path = (x-[r]-friend)
WHERE friend.name = {name}
RETURN TYPE(r)
请求示例
POST http://localhost:7474/db/data/ext/CypherPlugin/graphdb/execute_query
Accept: application/json
Content-Type: application/json
{
  "query" : "start x  = node:node_auto_index(name={startName}) match path = (x-[r]-friend) where friend.name = {name} return TYPE(r)",
  "params" : {
    "startName" : "I",
    "name" : "you"
  }
}
响应示例
200: OK
Content-Type: application/json
{
  "columns" : [ "TYPE(r)" ],
  "data" : [ [ "know" ] ]
}
18.17.4. 服务器错误
服务器上的错误会被报告为一个JSON格式的异常堆栈和消息。
START x = node(5)
RETURN x.dummy
请求示例
POST http://localhost:7474/db/data/ext/CypherPlugin/graphdb/execute_query
Accept: application/json
Content-Type: application/json
{
  "query" : "start x = node(5) return x.dummy",
  "params" : {
  }
}
响应示例
400: Bad Request
Content-Type: application/json
{
  "message" : "The property 'dummy' does not exist on Node[5]",
  "exception" : "BadInputException",
  "stacktrace" : [ "org.neo4j.server.rest.repr.RepresentationExceptionHandlingIterable.exceptionOnHasNext(RepresentationExceptionHandlingIterable.java:51)", "org.neo4j.helpers.collection.ExceptionHandlingIterable$1.hasNext(ExceptionHandlingIterable.java:61)", "org.neo4j.helpers.collection.IteratorWrapper.hasNext(IteratorWrapper.java:42)", "org.neo4j.server.rest.repr.ListRepresentation.serialize(ListRepresentation.java:58)", "org.neo4j.server.rest.repr.Serializer.serialize(Serializer.java:75)", "org.neo4j.server.rest.repr.MappingSerializer.putList(MappingSerializer.java:61)", "org.neo4j.server.rest.repr.CypherResultRepresentation.serialize(CypherResultRepresentation.java:50)", "org.neo4j.server.rest.repr.MappingRepresentation.serialize(MappingRepresentation.java:42)", "org.neo4j.server.rest.repr.OutputFormat.format(OutputFormat.java:182)", "org.neo4j.server.rest.repr.OutputFormat.formatRepresentation(OutputFormat.java:132)", "org.neo4j.server.rest.repr.OutputFormat.response(OutputFormat.java:119)", "org.neo4j.server.rest.repr.OutputFormat.ok(OutputFormat.java:55)", "org.neo4j.server.rest.web.ExtensionService.invokeGraphDatabaseExtension(ExtensionService.java:122)", "java.lang.reflect.Method.invoke(Method.java:597)" ]
}
