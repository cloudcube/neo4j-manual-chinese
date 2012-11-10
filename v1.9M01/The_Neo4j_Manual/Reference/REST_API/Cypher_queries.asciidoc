Neo4j REST API允许你使用Cypher进行查询，查看" Chapter 15, Cypher Query Language".返回的结果将作为列表的一个字符串头（列),和一部分数据，包括一个列表的所有行，每一行由子段值列表组成-节点，关系，路径或者任何简单值像字符串。
提示
为了提高相同场景查询效率，尽量不要使用字符，使用参数代替，尽可能让服务其缓存查询计划，
更多详情查看Section 18.3.1, “Send queries with parameters”
18.3.1. 使用参数查询
Cypher 支持使用作为JSON map提交的参数的查询
START x  = node:node_auto_index(name={startName})
MATCH path = (x-[r]-friend)
WHERE friend.name = {name}
RETURN TYPE(r)
请求示例
POST http://localhost:7474/db/data/cypher
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
18.3.2. 发送查询
一个简单的查询返回连接到节点 1所有的结果，如果存在返回节点和节点名称，否则，返回null
START x  = node(3)
MATCH x -[r]-> n
RETURN type(r), n.name?, n.age?
请求示例
POST http://localhost:7474/db/data/cypher
Accept: application/json
Content-Type: application/json
{
  "query" : "start x  = node(3) match x -[r]-> n return type(r), n.name?, n.age?",
  "params" : {
  }
响应示例
200: OK
Content-Type: application/json
{
  "columns" : [ "type(r)", "n.name?", "n.age?" ],
  "data" : [ [ "know", "him", 25 ], [ "know", "you", null ] ]
}
18.3.3. 返回路径
路径可以连同其他指定返回类型被一起返回
START x  = node(10)
MATCH path = (x--friend)
RETURN path, friend.name
请求示例
POST http://localhost:7474/db/data/cypher
Accept: application/json
Content-Type: application/json
{
  "query" : "start x  = node(10) match path = (x--friend) return path, friend.name",
  "params" : {
  }
}
响应示例
200: OK
Content-Type: application/json
{
  "columns" : [ "path", "friend.name" ],
  "data" : [ [ {
    "start" : "http://localhost:7474/db/data/node/10",
    "nodes" : [ "http://localhost:7474/db/data/node/10", "http://localhost:7474/db/data/node/9" ],
    "length" : 1,
    "relationships" : [ "http://localhost:7474/db/data/relationship/5" ],
    "end" : "http://localhost:7474/db/data/node/9"
  }, "you" ] ]
}
18.3.4. 嵌套结果
当发送返回向list和maps的嵌套结果的查询，根据他们的类型，这些会序列化成嵌套的JSON。
START n = node(19,18)
RETURN collect(n.name)
请求示例
POST http://localhost:7474/db/data/cypher
Accept: application/json
Content-Type: application/json
{
  "query" : "start n = node(19,18) return collect(n.name)",
  "params" : {
  }
}
响应示例
200: OK
Content-Type: application/json
{
  "columns" : [ "collect(n.name)" ],
  "data" : [ [ [ "I", "you" ] ] ]
}

18.3.5. 服务器错误
服务器上的错误，会被报告为一个JSON格式的异常堆栈和消息。
START x = node(8)
RETURN x.dummy
请求示例

POST http://localhost:7474/db/data/cypher
Accept: application/json
Content-Type: application/json
{
  "query" : "start x = node(8) return x.dummy",
  "params" : {
  }
}
响应示例
400: Bad Request
Content-Type: application/json
{
  "message" : "The property 'dummy' does not exist on Node[8]",
  "exception" : "BadInputException",
  "stacktrace" : [ "org.neo4j.server.rest.repr.RepresentationExceptionHandlingIterable.exceptionOnHasNext(RepresentationExceptionHandlingIterable.java:51)", "org.neo4j.helpers.collection.ExceptionHandlingIterable$1.hasNext(ExceptionHandlingIterable.java:61)", "org.neo4j.helpers.collection.IteratorWrapper.hasNext(IteratorWrapper.java:42)", "org.neo4j.server.rest.repr.ListRepresentation.serialize(ListRepresentation.java:58)", "org.neo4j.server.rest.repr.Serializer.serialize(Serializer.java:75)", "org.neo4j.server.rest.repr.MappingSerializer.putList(MappingSerializer.java:61)", "org.neo4j.server.rest.repr.CypherResultRepresentation.serialize(CypherResultRepresentation.java:50)", "org.neo4j.server.rest.repr.MappingRepresentation.serialize(MappingRepresentation.java:42)", "org.neo4j.server.rest.repr.OutputFormat.format(OutputFormat.java:182)", "org.neo4j.server.rest.repr.OutputFormat.formatRepresentation(OutputFormat.java:132)", "org.neo4j.server.rest.repr.OutputFormat.response(OutputFormat.java:119)", "org.neo4j.server.rest.repr.OutputFormat.ok(OutputFormat.java:55)", "org.neo4j.server.rest.web.CypherService.cypher(CypherService.java:68)", "java.lang.reflect.Method.invoke(Method.java:597)" ]
}



