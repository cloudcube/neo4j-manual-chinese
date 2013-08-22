18.7.1. Set property on node
18.7.2. Update node properties
18.7.3. Get properties for node
18.7.4. Property values can not be null
18.7.5. Property values can not be nested
18.7.6. Delete all properties from node
18.7.7. Delete a named property from a node
18.7.1. 节点上设置属性
在已存在的节点上设置不同的属性。注意单独的一个值被提交，不是作为一个map,仅仅作为一个值（是有效的JOSN）就像在下面的例子。
请求示例
PUT http://localhost:7474/db/data/node/56/properties/foo
Accept: application/json
Content-Type: application/json
"bar"
响应示例
204: No Content
18.7.2. 更新节点属性
在节点上，新的属性被设置将会替换现存的节点属性。
请求示例
PUT http://localhost:7474/db/data/node/50/properties
Accept: application/json
Content-Type: application/json
{
  "age" : "18"
}
响应示例
204: No Content
18.7.3. 为节点获取属性
请求示例
GET http://localhost:7474/db/data/node/4/properties
Accept: application/json
响应示例
200: OK
Content-Type: application/json
{
  "foo" : "bar"
}
18.7.4. 属性值不能为空
这个示例显示了当你尝试设置空的属性值后的响应。
请求示例
POST http://localhost:7474/db/data/node
Accept: application/json
Content-Type: application/json
{
  "foo" : null
}
响应示例
400: Bad Request
Content-Type: application/json
{
  "message" : "Could not set property \"foo\", unsupported type: null",
  "exception" : "PropertyValueException",
  "stacktrace" : [ "org.neo4j.server.rest.web.DatabaseActions.set(DatabaseActions.java:155)", "org.neo4j.server.rest.web.DatabaseActions.createNode(DatabaseActions.java:213)", "org.neo4j.server.rest.web.RestfulGraphDatabase.createNode(RestfulGraphDatabase.java:205)", "java.lang.reflect.Method.invoke(Method.java:597)" ]
}

18.7.5. 属性值不能够被嵌套
嵌套属性不能够被支持。你可以为下面示例存储一个字符串而不是嵌套的JSON
请求示例
POST http://localhost:7474/db/data/node/
Accept: application/json
Content-Type: application/json
{
  "foo" : {
    "bar" : "baz"
  }
}
响应示例
400: Bad Request
Content-Type: application/json
{
  "message" : "Could not set property \"foo\", unsupported type: {bar=baz}",
  "exception" : "PropertyValueException",
  "stacktrace" : [ "org.neo4j.server.rest.web.DatabaseActions.set(DatabaseActions.java:155)", "org.neo4j.server.rest.web.DatabaseActions.createNode(DatabaseActions.java:213)", "org.neo4j.server.rest.web.RestfulGraphDatabase.createNode(RestfulGraphDatabase.java:205)", "java.lang.reflect.Method.invoke(Method.java:597)" ]
}
18.7.6. 从节点删除属性
请求示例
DELETE http://localhost:7474/db/data/node/258/properties
Accept: application/json
响应示例
204: No Content
18.7.7. 从一个节点删除命名属性
要从一个节点删除一个单独的属性，清看下面的例子。
请求示例
DELETE http://localhost:7474/db/data/node/259/properties/name
Accept: application/json
请求示例
204: No Content



