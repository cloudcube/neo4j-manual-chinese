18.14.1. Find all shortest paths
18.14.2. Find one of the shortest paths between nodes
18.14.3. Execute a Dijkstra algorithm with similar weights on relationships
18.14.4. Execute a Dijkstra algorithm with weights on relationships
Neo4j带有一些内置的图形算法。他们从开始节点被执行。通过URI和带有发送请求的内容控制遍历。
algorithm
该算法选择，如果没有设置，默认是shortestPath,algorithm其中的一个。
shortestPath
allSimplePaths
allPaths
dijkstra(带有cost_property和default_const参数的可选项)
max_depth
作为一个证书的俄最大省督为ShortestPath一样，在适用的，默认值是1.
18.14.1. 查找所有最短路径
shortestPath算法能够发现相关节点的之间的多条路径，就像这个例子。
请求示例
POST http://localhost:7474/db/data/node/55/paths
Accept: application/json
Content-Type: application/json
{
  "to" : "http://localhost:7474/db/data/node/50",
  "max_depth" : 3,
  "relationships" : {
    "type" : "to",
    "direction" : "out"
  },
  "algorithm" : "shortestPath"
}
响应示例
200: OK
Content-Type: application/json
[ {
  "start" : "http://localhost:7474/db/data/node/55",
  "nodes" : [ "http://localhost:7474/db/data/node/55", "http://localhost:7474/db/data/node/51", "http://localhost:7474/db/data/node/50" ],
  "length" : 2,
  "relationships" : [ "http://localhost:7474/db/data/relationship/14", "http://localhost:7474/db/data/relationship/20" ],
  "end" : "http://localhost:7474/db/data/node/50"
}, {
  "start" : "http://localhost:7474/db/data/node/55",
  "nodes" : [ "http://localhost:7474/db/data/node/55", "http://localhost:7474/db/data/node/54", "http://localhost:7474/db/data/node/50" ],
  "length" : 2,
  "relationships" : [ "http://localhost:7474/db/data/relationship/13", "http://localhost:7474/db/data/relationship/22" ],
  "end" : "http://localhost:7474/db/data/node/50"
} ]

18.14.2. 找到两个节点间的最短路径
如果没有路径算法被指定，带有最大深度为1的ShortestPath算法将被选择。在这个例子中，max_depth设置为3为了找到3个连接节点的最短路径。
请求示例
POST http://localhost:7474/db/data/node/62/path
Accept: application/json
Content-Type: application/json
{
  "to" : "http://localhost:7474/db/data/node/57",
  "max_depth" : 3,
  "relationships" : {
    "type" : "to",
    "direction" : "out"
  },
  "algorithm" : "shortestPath"
}
响应示例
200: OK
Content-Type: application/json
{
  "start" : "http://localhost:7474/db/data/node/62",
  "nodes" : [ "http://localhost:7474/db/data/node/62", "http://localhost:7474/db/data/node/58", "http://localhost:7474/db/data/node/57" ],
  "length" : 2,
  "relationships" : [ "http://localhost:7474/db/data/relationship/24", "http://localhost:7474/db/data/relationship/30" ],
  "end" : "http://localhost:7474/db/data/node/57"
}
18.14.3. 在关系上执行一个具有相同砝码的Dijkstra算法
请求示例
POST http://localhost:7474/db/data/node/77/path
Accept: application/json
Content-Type: application/json
{
  "to" : "http://localhost:7474/db/data/node/80",
  "cost_property" : "cost",
  "relationships" : {
    "type" : "to",
    "direction" : "out"
  },
  "algorithm" : "dijkstra"
}
响应示例
200: OK
Content-Type: application/json
{
  "weight" : 2.0,
  "start" : "http://localhost:7474/db/data/node/77",
  "nodes" : [ "http://localhost:7474/db/data/node/77", "http://localhost:7474/db/data/node/78", "http://localhost:7474/db/data/node/80" ],
  "length" : 2,
  "relationships" : [ "http://localhost:7474/db/data/relationship/46", "http://localhost:7474/db/data/relationship/47" ],
  "end" : "http://localhost:7474/db/data/node/80"
}
18.14.4. 在关系上执行一个具有重量级的Dijkstra算法
请求示例
POST http://localhost:7474/db/data/node/68/path
Accept: application/json
Content-Type: application/json
{
  "to" : "http://localhost:7474/db/data/node/71",
  "cost_property" : "cost",
  "relationships" : {
    "type" : "to",
    "direction" : "out"
  },
  "algorithm" : "dijkstra"
}
响应示例
200: OK
Content-Type: application/json
{
  "weight" : 6.0,
  "start" : "http://localhost:7474/db/data/node/68",
  "nodes" : [ "http://localhost:7474/db/data/node/68", "http://localhost:7474/db/data/node/69", "http://localhost:7474/db/data/node/66", "http://localhost:7474/db/data/node/67", "http://localhost:7474/db/data/node/64", "http://localhost:7474/db/data/node/65", "http://localhost:7474/db/data/node/71" ],
  "length" : 6,
  "relationships" : [ "http://localhost:7474/db/data/relationship/33", "http://localhost:7474/db/data/relationship/35", "http://localhost:7474/db/data/relationship/37", "http://localhost:7474/db/data/relationship/40", "http://localhost:7474/db/data/relationship/42", "http://localhost:7474/db/data/relationship/43" ],
  "end" : "http://localhost:7474/db/data/node/71"
}

