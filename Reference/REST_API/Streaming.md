整个REST API可以作为数据流传输，在服务端的导致更好的性能和更低的内存。使用它，适合每一个请求头调用，详情参见下面实例:
警告:
这个特性很新，与传输一个大的响应的非数据流的API相比，你应该让自己很好的使用这个响应数据流的类型，自从它对于客户端和服务端更加有效的机制，所以在未来的特性中将支持默认可用的文件流。
请求示例
GET http://localhost:7474/db/data/
Accept: application/json
X-Stream: true
响应示例
200: OK
Content-Type: application/json; stream=true
{
  "extensions" : {
  },
  "node" : "http://localhost:7474/db/data/node",
  "reference_node" : "http://localhost:7474/db/data/node/103",
  "node_index" : "http://localhost:7474/db/data/index/node",
  "relationship_index" : "http://localhost:7474/db/data/index/relationship",
  "extensions_info" : "http://localhost:7474/db/data/ext",
  "relationship_types" : "http://localhost:7474/db/data/relationship/types",
  "batch" : "http://localhost:7474/db/data/batch",
  "cypher" : "http://localhost:7474/db/data/cypher",
  "neo4j_version" : "1.9.M01"
}
