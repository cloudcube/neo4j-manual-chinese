18.8.1. Update relationship properties
18.8.2. Remove properties from a relationship
18.8.3. Remove property from a relationship
18.8.4. Remove non-existent property from a relationship
18.8.5. Remove properties from a non-existing relationship
18.8.6. Remove property from a non-existing relationship
18.8.1. 更新关系属性
请求示例
PUT http://localhost:7474/db/data/relationship/120/properties
Accept: application/json
Content-Type: application/json
{
  "jim" : "tobias"
}
响应示例
204: No Content
18.8.2. 从一个关系移除所有属性
请求示例
DELETE http://localhost:7474/db/data/relationship/0
Accept: application/json
响应示例
204: No Content

18.8.3. 从一个关系移除属性
看下面的请求示例
请求示例
DELETE http://localhost:7474/db/data/relationship/2/properties/cost
Accept: application/json
响应示例
204: No Content
18.8.4. 从一个关系移除不存在的属性
努力去移除一个不存在的属性，会有一个错误在结果中。
请求示例
DELETE http://localhost:7474/db/data/relationship/3/properties/non-existent
Accept: application/json
响应示例
404: Not Found
Content-Type: application/json
{
  "message" : "Relationship[3] does not have a property \"non-existent\"",
  "exception" : "NoSuchPropertyException",
  "stacktrace" : [ "org.neo4j.server.rest.web.DatabaseActions.removeRelationshipProperty(DatabaseActions.java:733)", "org.neo4j.server.rest.web.RestfulGraphDatabase.deleteRelationshipProperty(RestfulGraphDatabase.java:605)", "java.lang.reflect.Method.invoke(Method.java:597)" ]
}
18.8.5. 从一个不存在的关系中移除所有属性
努力从一个不存在的关系中移除所有属性，会有一个错误在结果中。
请求示例
DELETE http://localhost:7474/db/data/relationship/1234/properties
Accept: application/json
响应示例
404: Not Found
Content-Type: application/json
{
  "exception" : "RelationshipNotFoundException",
  "stacktrace" : [ "org.neo4j.server.rest.web.DatabaseActions.relationship(DatabaseActions.java:137)", "org.neo4j.server.rest.web.DatabaseActions.removeAllRelationshipProperties(DatabaseActions.java:711)", "org.neo4j.server.rest.web.RestfulGraphDatabase.deleteAllRelationshipProperties(RestfulGraphDatabase.java:589)", "java.lang.reflect.Method.invoke(Method.java:597)" ]
}
18.8.6. 从一个不存在的关系中移除一个属性
努力的从一个不存在的关系中移除一个属性，会由一个错误在结果中。
请求示例
DELETE http://localhost:7474/db/data/relationship/1234/properties/cost
Accept: application/json
响应示例
404: Not Found
Content-Type: application/json
{
  "exception" : "RelationshipNotFoundException",
  "stacktrace" : [ "org.neo4j.server.rest.web.DatabaseActions.relationship(DatabaseActions.java:137)", "org.neo4j.server.rest.web.DatabaseActions.removeRelationshipProperty(DatabaseActions.java:727)", "org.neo4j.server.rest.web.RestfulGraphDatabase.deleteRelationshipProperty(RestfulGraphDatabase.java:605)", "java.lang.reflect.Method.invoke(Method.java:597)" ]
}




