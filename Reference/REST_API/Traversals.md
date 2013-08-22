18.13.1. Traversal using a return filter
18.13.2. Return relationships from a traversal
18.13.3. Return paths from a traversal
18.13.4. Traversal returning nodes below a certain depth
18.13.5. Creating a paged traverser
18.13.6. Paging through the results of a paged traverser
18.13.7. Paged traverser page size
18.13.8. Paged traverser timeout
警告
在作为评价这定义下执行任意的Groovy代码遍历REST端点。在托管和开发环境中，这可能构成安全风险。在这些情况下，可以考虑使用声明的方法像 Chapter 15, Cypher Query Language，或者编写自己的服务端插件使用Java API执行你感兴趣的便利(查看 Section 10.1, “Server Plugins”),或者确保你的服务器安全查看Section 24.1, “Securing access to the Neo4j Server”.
遍历从开始节点执行。遍历由URI和带有发送请求的内容控制。
returnType
这种对象在响应中被在URL中的traverse/{returnType}检测。returnType可以有下面值的一个：
node
relationship
path:包含了完整陈述的开始和结束节点，其余的URI。
fullpath：包含所有节点和关系的完成陈述。
决定如何遍历图应该是你可以在请求的主题中使用这些参数:
order
以便访问节点的决定。
可能值：
breadth_first:查看Breadth-first search.
depth_first:查看Depth-first search
relationships
决定关系类型和应该遵循的方向。这个方向是：
all
in
out
uniqueness
决定应该如何计算唯一性。有关不同的唯一值查看 Java API on Uniqueness.可能的值：
node_global
none
relationship_global
node_path
relationship_path
prune_evaluator
决定是否应该沿着这条道路一直遍历或如果需要它调整，以便遍历不会继续沿着这条道路。你可以写你自己的修剪辨别器（查看Section 18.13.1, “Traversal using a return filter”）或者使用built-in none 修剪辨别器。
return_filter
决定当前位置应该包含在结果中。你需要为此写代码（查看see Section 18.13.1, “Traversal using a return filter”），或者使用其中一个内置的过滤器。
all
all_but_start_node
max_depth
是一种简单的方式指定一个具体的省督的修改辨别器。如果不指定一个深度为1的max和prene_evaluator指定max_depth代替，则没有最大深度限制。
position对象在return_filter和preune_evaluator的内容是一个路径对象，代表路径从开始节点遍历到当前位置。
开箱即用，在过滤和评估中REST API 支持JavaScript代码。在一个取得所有Neo4j Java API权限Java上下文中，脚本语言将被执行。
18.13.1. 遍历使用一个返回过滤
在这里例子中，none过滤器被使用和返回过滤器提供为了返回所有名称包含"t".结果是返回作为节点和最大深度为3.
请求示例
POST http://localhost:7474/db/data/node/244/traverse/node
Accept: application/json
Content-Type: application/json
{
  "order" : "breadth_first",
  "return_filter" : {
    "body" : "position.endNode().getProperty('name').toLowerCase().contains('t')",
    "language" : "javascript"
  },
  "prune_evaluator" : {
    "body" : "position.length() > 10",
    "language" : "javascript"
  },
  "uniqueness" : "node_global",
  "relationships" : [ {
    "direction" : "all",
    "type" : "knows"
  }, {
    "direction" : "all",
    "type" : "loves"
  } ],
  "max_depth" : 3
}
响应示例
200: OK
Content-Type: application/json
[ {
  "outgoing_relationships" : "http://localhost:7474/db/data/node/244/relationships/out",
  "data" : {
    "name" : "Root"
  },
  "traverse" : "http://localhost:7474/db/data/node/244/traverse/{returnType}",
  "all_typed_relationships" : "http://localhost:7474/db/data/node/244/relationships/all/{-list|&|types}",
  "property" : "http://localhost:7474/db/data/node/244/properties/{key}",
  "self" : "http://localhost:7474/db/data/node/244",
  "outgoing_typed_relationships" : "http://localhost:7474/db/data/node/244/relationships/out/{-list|&|types}",
  "properties" : "http://localhost:7474/db/data/node/244/properties",
  "incoming_relationships" : "http://localhost:7474/db/data/node/244/relationships/in",
  "extensions" : {
  },
  "create_relationship" : "http://localhost:7474/db/data/node/244/relationships",
  "paged_traverse" : "http://localhost:7474/db/data/node/244/paged/traverse/{returnType}{?pageSize,leaseTime}",
  "all_relationships" : "http://localhost:7474/db/data/node/244/relationships/all",
  "incoming_typed_relationships" : "http://localhost:7474/db/data/node/244/relationships/in/{-list|&|types}"
}, {
  "outgoing_relationships" : "http://localhost:7474/db/data/node/247/relationships/out",
  "data" : {
    "name" : "Mattias"
  },
  "traverse" : "http://localhost:7474/db/data/node/247/traverse/{returnType}",
  "all_typed_relationships" : "http://localhost:7474/db/data/node/247/relationships/all/{-list|&|types}",
  "property" : "http://localhost:7474/db/data/node/247/properties/{key}",
  "self" : "http://localhost:7474/db/data/node/247",
  "outgoing_typed_relationships" : "http://localhost:7474/db/data/node/247/relationships/out/{-list|&|types}",
  "properties" : "http://localhost:7474/db/data/node/247/properties",
  "incoming_relationships" : "http://localhost:7474/db/data/node/247/relationships/in",
  "extensions" : {
  },
  "create_relationship" : "http://localhost:7474/db/data/node/247/relationships",
  "paged_traverse" : "http://localhost:7474/db/data/node/247/paged/traverse/{returnType}{?pageSize,leaseTime}",
  "all_relationships" : "http://localhost:7474/db/data/node/247/relationships/all",
  "incoming_typed_relationships" : "http://localhost:7474/db/data/node/247/relationships/in/{-list|&|types}"
}, {
  "outgoing_relationships" : "http://localhost:7474/db/data/node/246/relationships/out",
  "data" : {
    "name" : "Peter"
  },
  "traverse" : "http://localhost:7474/db/data/node/246/traverse/{returnType}",
  "all_typed_relationships" : "http://localhost:7474/db/data/node/246/relationships/all/{-list|&|types}",
  "property" : "http://localhost:7474/db/data/node/246/properties/{key}",
  "self" : "http://localhost:7474/db/data/node/246",
  "outgoing_typed_relationships" : "http://localhost:7474/db/data/node/246/relationships/out/{-list|&|types}",
  "properties" : "http://localhost:7474/db/data/node/246/properties",
  "incoming_relationships" : "http://localhost:7474/db/data/node/246/relationships/in",
  "extensions" : {
  },
  "create_relationship" : "http://localhost:7474/db/data/node/246/relationships",
  "paged_traverse" : "http://localhost:7474/db/data/node/246/paged/traverse/{returnType}{?pageSize,leaseTime}",
  "all_relationships" : "http://localhost:7474/db/data/node/246/relationships/all",
  "incoming_typed_relationships" : "http://localhost:7474/db/data/node/246/relationships/in/{-list|&|types}"
}, {
  "outgoing_relationships" : "http://localhost:7474/db/data/node/245/relationships/out",
  "data" : {
    "name" : "Tobias"
  },
  "traverse" : "http://localhost:7474/db/data/node/245/traverse/{returnType}",
  "all_typed_relationships" : "http://localhost:7474/db/data/node/245/relationships/all/{-list|&|types}",
  "property" : "http://localhost:7474/db/data/node/245/properties/{key}",
  "self" : "http://localhost:7474/db/data/node/245",
  "outgoing_typed_relationships" : "http://localhost:7474/db/data/node/245/relationships/out/{-list|&|types}",
  "properties" : "http://localhost:7474/db/data/node/245/properties",
  "incoming_relationships" : "http://localhost:7474/db/data/node/245/relationships/in",
  "extensions" : {
  },
  "create_relationship" : "http://localhost:7474/db/data/node/245/relationships",
  "paged_traverse" : "http://localhost:7474/db/data/node/245/paged/traverse/{returnType}{?pageSize,leaseTime}",
  "all_relationships" : "http://localhost:7474/db/data/node/245/relationships/all",
  "incoming_typed_relationships" : "http://localhost:7474/db/data/node/245/relationships/in/{-list|&|types}"
} ]

18.13.2. 从一个遍历返回关系

请求示例
POST http://localhost:7474/db/data/node/235/traverse/relationship
Accept: application/json
Content-Type: application/json
{
  "order" : "breadth_first",
  "uniqueness" : "none",
  "return_filter" : {
    "language" : "builtin",
    "name" : "all"
  }
}
响应示例
200: OK
Content-Type: application/json
[ {
  "start" : "http://localhost:7474/db/data/node/235",
  "data" : {
  },
  "self" : "http://localhost:7474/db/data/relationship/128",
  "property" : "http://localhost:7474/db/data/relationship/128/properties/{key}",
  "properties" : "http://localhost:7474/db/data/relationship/128/properties",
  "type" : "know",
  "extensions" : {
  },
  "end" : "http://localhost:7474/db/data/node/234"
}, {
  "start" : "http://localhost:7474/db/data/node/235",
  "data" : {
  },
  "self" : "http://localhost:7474/db/data/relationship/129",
  "property" : "http://localhost:7474/db/data/relationship/129/properties/{key}",
  "properties" : "http://localhost:7474/db/data/relationship/129/properties",
  "type" : "own",
  "extensions" : {
  },
  "end" : "http://localhost:7474/db/data/node/233"
} ]
18.13.3. 从一个遍历返回路径
请求示例：
POST http://localhost:7474/db/data/node/238/traverse/path
Accept: application/json
Content-Type: application/json
{
  "order" : "breadth_first",
  "uniqueness" : "none",
  "return_filter" : {
    "language" : "builtin",
    "name" : "all"
  }
}
响应示例
200: OK
Content-Type: application/json
[ {
  "start" : "http://localhost:7474/db/data/node/238",
  "nodes" : [ "http://localhost:7474/db/data/node/238" ],
  "length" : 0,
  "relationships" : [ ],
  "end" : "http://localhost:7474/db/data/node/238"
}, {
  "start" : "http://localhost:7474/db/data/node/238",
  "nodes" : [ "http://localhost:7474/db/data/node/238", "http://localhost:7474/db/data/node/237" ],
  "length" : 1,
  "relationships" : [ "http://localhost:7474/db/data/relationship/130" ],
  "end" : "http://localhost:7474/db/data/node/237"
}, {
  "start" : "http://localhost:7474/db/data/node/238",
  "nodes" : [ "http://localhost:7474/db/data/node/238", "http://localhost:7474/db/data/node/236" ],
  "length" : 1,
  "relationships" : [ "http://localhost:7474/db/data/relationship/131" ],
  "end" : "http://localhost:7474/db/data/node/236"
} ]

18.13.4. 遍历返回低于一定深度的节点
在这里，所有的节点遍历深度低于3返回。
请求示例
POST http://localhost:7474/db/data/node/251/traverse/node
Accept: application/json
Content-Type: application/json
{
  "return_filter" : {
    "body" : "position.length()<3;",
    "language" : "javascript"
  },
  "prune_evaluator" : {
    "name" : "none",
    "language" : "builtin"
  }
}
响应示例
200: OK
Content-Type: application/json
[ {
  "outgoing_relationships" : "http://localhost:7474/db/data/node/251/relationships/out",
  "data" : {
    "name" : "Root"
  },
  "traverse" : "http://localhost:7474/db/data/node/251/traverse/{returnType}",
  "all_typed_relationships" : "http://localhost:7474/db/data/node/251/relationships/all/{-list|&|types}",
  "property" : "http://localhost:7474/db/data/node/251/properties/{key}",
  "self" : "http://localhost:7474/db/data/node/251",
  "outgoing_typed_relationships" : "http://localhost:7474/db/data/node/251/relationships/out/{-list|&|types}",
  "properties" : "http://localhost:7474/db/data/node/251/properties",
  "incoming_relationships" : "http://localhost:7474/db/data/node/251/relationships/in",
  "extensions" : {
  },
  "create_relationship" : "http://localhost:7474/db/data/node/251/relationships",
  "paged_traverse" : "http://localhost:7474/db/data/node/251/paged/traverse/{returnType}{?pageSize,leaseTime}",
  "all_relationships" : "http://localhost:7474/db/data/node/251/relationships/all",
  "incoming_typed_relationships" : "http://localhost:7474/db/data/node/251/relationships/in/{-list|&|types}"
}, {
  "outgoing_relationships" : "http://localhost:7474/db/data/node/254/relationships/out",
  "data" : {
    "name" : "Mattias"
  },
  "traverse" : "http://localhost:7474/db/data/node/254/traverse/{returnType}",
  "all_typed_relationships" : "http://localhost:7474/db/data/node/254/relationships/all/{-list|&|types}",
  "property" : "http://localhost:7474/db/data/node/254/properties/{key}",
  "self" : "http://localhost:7474/db/data/node/254",
  "outgoing_typed_relationships" : "http://localhost:7474/db/data/node/254/relationships/out/{-list|&|types}",
  "properties" : "http://localhost:7474/db/data/node/254/properties",
  "incoming_relationships" : "http://localhost:7474/db/data/node/254/relationships/in",
  "extensions" : {
  },
  "create_relationship" : "http://localhost:7474/db/data/node/254/relationships",
  "paged_traverse" : "http://localhost:7474/db/data/node/254/paged/traverse/{returnType}{?pageSize,leaseTime}",
  "all_relationships" : "http://localhost:7474/db/data/node/254/relationships/all",
  "incoming_typed_relationships" : "http://localhost:7474/db/data/node/254/relationships/in/{-list|&|types}"
}, {
  "outgoing_relationships" : "http://localhost:7474/db/data/node/249/relationships/out",
  "data" : {
    "name" : "Johan"
  },
  "traverse" : "http://localhost:7474/db/data/node/249/traverse/{returnType}",
  "all_typed_relationships" : "http://localhost:7474/db/data/node/249/relationships/all/{-list|&|types}",
  "property" : "http://localhost:7474/db/data/node/249/properties/{key}",
  "self" : "http://localhost:7474/db/data/node/249",
  "outgoing_typed_relationships" : "http://localhost:7474/db/data/node/249/relationships/out/{-list|&|types}",
  "properties" : "http://localhost:7474/db/data/node/249/properties",
  "incoming_relationships" : "http://localhost:7474/db/data/node/249/relationships/in",
  "extensions" : {
  },
  "create_relationship" : "http://localhost:7474/db/data/node/249/relationships",
  "paged_traverse" : "http://localhost:7474/db/data/node/249/paged/traverse/{returnType}{?pageSize,leaseTime}",
  "all_relationships" : "http://localhost:7474/db/data/node/249/relationships/all",
  "incoming_typed_relationships" : "http://localhost:7474/db/data/node/249/relationships/in/{-list|&|types}"
}, {
  "outgoing_relationships" : "http://localhost:7474/db/data/node/250/relationships/out",
  "data" : {
    "name" : "Emil"
  },
  "traverse" : "http://localhost:7474/db/data/node/250/traverse/{returnType}",
  "all_typed_relationships" : "http://localhost:7474/db/data/node/250/relationships/all/{-list|&|types}",
  "property" : "http://localhost:7474/db/data/node/250/properties/{key}",
  "self" : "http://localhost:7474/db/data/node/250",
  "outgoing_typed_relationships" : "http://localhost:7474/db/data/node/250/relationships/out/{-list|&|types}",
  "properties" : "http://localhost:7474/db/data/node/250/properties",
  "incoming_relationships" : "http://localhost:7474/db/data/node/250/relationships/in",
  "extensions" : {
  },
  "create_relationship" : "http://localhost:7474/db/data/node/250/relationships",
  "paged_traverse" : "http://localhost:7474/db/data/node/250/paged/traverse/{returnType}{?pageSize,leaseTime}",
  "all_relationships" : "http://localhost:7474/db/data/node/250/relationships/all",
  "incoming_typed_relationships" : "http://localhost:7474/db/data/node/250/relationships/in/{-list|&|types}"
} ]
18.13.5. 创建一个分页的遍历
分页遍历由POST-ing 一个节点描述被page_traverser键在一个节点描述连接标志创建。当创建一个分页遍历的时候，相同选项作为常规遍历，这意味着node,path或者fullpath,可以被针对。
请求示例
POST http://localhost:7474/db/data/node/34/paged/traverse/node
Accept: application/json
Content-Type: application/json
{
  "prune_evaluator" : {
    "language" : "builtin",
    "name" : "none"
  },
  "return_filter" : {
    "language" : "javascript",
    "body" : "position.endNode().getProperty('name').contains('1');"
  },
  "order" : "depth_first",
  "relationships" : {
    "type" : "NEXT",
    "direction" : "out"
  }
}
响应示例
201: Created
Content-Type: application/json
Location: http://localhost:7474/db/data/node/34/paged/traverse/node/6dba95aebc734a578b9f48e9a2596a86
[ {
  "outgoing_relationships" : "http://localhost:7474/db/data/node/35/relationships/out",
  "data" : {
    "name" : "1"
  },
  "traverse" : "http://localhost:7474/db/data/node/35/traverse/{returnType}",
  "all_typed_relationships" : "http://localhost:7474/db/data/node/35/relationships/all/{-list|&|types}",
  "property" : "http://localhost:7474/db/data/node/35/properties/{key}",
  "self" : "http://localhost:7474/db/data/node/35",
  "outgoing_typed_relationships" : "http://localhost:7474/db/data/node/35/relationships/out/{-list|&|types}",
  "properties" : "http://localhost:7474/db/data/node/35/properties",
  "incoming_relationships" : "http://localhost:7474/db/data/node/35/relationships/in",
  "extensions" : {
  },
  "create_relationship" : "http://localhost:7474/db/data/node/35/relationships",
  "paged_traverse" : "http://localhost:7474/db/data/node/35/paged/traverse/{returnType}{?pageSize,leaseTime}",
  "all_relationships" : "http://localhost:7474/db/data/node/35/relationships/all",
  "incoming_typed_relationships" : "http://localhost:7474/db/data/node/35/relationships/in/{-list|&|types}"
}, {
  "outgoing_relationships" : "http://localhost:7474/db/data/node/44/relationships/out",
  "data" : {
    "name" : "10"
  },
  "traverse" : "http://localhost:7474/db/data/node/44/traverse/{returnType}",
  "all_typed_relationships" : "http://localhost:7474/db/data/node/44/relationships/all/{-list|&|types}",
  "property" : "http://localhost:7474/db/data/node/44/properties/{key}",
  "self" : "http://localhost:7474/db/data/node/44",
  "outgoing_typed_relationships" : "http://localhost:7474/db/data/node/44/relationships/out/{-list|&|types}",
  "properties" : "http://localhost:7474/db/data/node/44/properties",
  "incoming_relationships" : "http://localhost:7474/db/data/node/44/relationships/in",
  "extensions" : {
  },
  "create_relationship" : "http://localhost:7474/db/data/node/44/relationships",
  "paged_traverse" : "http://localhost:7474/db/data/node/44/paged/traverse/{returnType}{?pageSize,leaseTime}",
  "all_relationships" : "http://localhost:7474/db/data/node/44/relationships/all",
  "incoming_typed_relationships" : "http://localhost:7474/db/data/node/44/relationships/in/{-list|&|types}"
}, {
  "outgoing_relationships" : "http://localhost:7474/db/data/node/45/relationships/out",
  "data" : {
    "name" : "11"
  },
  "traverse" : "http://localhost:7474/db/data/node/45/traverse/{returnType}",
  "all_typed_relationships" : "http://localhost:7474/db/data/node/45/relationships/all/{-list|&|types}",
  "property" : "http://localhost:7474/db/data/node/45/properties/{key}",
  "self" : "http://localhost:7474/db/data/node/45",
  "outgoing_typed_relationships" : "http://localhost:7474/db/data/node/45/relationships/out/{-list|&|types}",
  "properties" : "http://localhost:7474/db/data/node/45/properties",
  "incoming_relationships" : "http://localhost:7474/db/data/node/45/relationships/in",
  "extensions" : {
  },
  "create_relationship" : "http://localhost:7474/db/data/node/45/relationships",
  "paged_traverse" : "http://localhost:7474/db/data/node/45/paged/traverse/{returnType}{?pageSize,leaseTime}",
  "all_relationships" : "http://localhost:7474/db/data/node/45/relationships/all",
  "incoming_typed_relationships" : "http://localhost:7474/db/data/node/45/relationships/in/{-list|&|types}"
}, {
  "outgoing_relationships" : "http://localhost:7474/db/data/node/46/relationships/out",
  "data" : {
    "name" : "12"
  },
  "traverse" : "http://localhost:7474/db/data/node/46/traverse/{returnType}",
  "all_typed_relationships" : "http://localhost:7474/db/data/node/46/relationships/all/{-list|&|types}",
  "property" : "http://localhost:7474/db/data/node/46/properties/{key}",
  "self" : "http://localhost:7474/db/data/node/46",
  "outgoing_typed_relationships" : "http://localhost:7474/db/data/node/46/relationships/out/{-list|&|types}",
  "properties" : "http://localhost:7474/db/data/node/46/properties",
  "incoming_relationships" : "http://localhost:7474/db/data/node/46/relationships/in",
  "extensions" : {
  },
  "create_relationship" : "http://localhost:7474/db/data/node/46/relationships",
  "paged_traverse" : "http://localhost:7474/db/data/node/46/paged/traverse/{returnType}{?pageSize,leaseTime}",
  "all_relationships" : "http://localhost:7474/db/data/node/46/relationships/all",
  "incoming_typed_relationships" : "http://localhost:7474/db/data/node/46/relationships/in/{-list|&|types}"
}, {
  "outgoing_relationships" : "http://localhost:7474/db/data/node/47/relationships/out",
  "data" : {
    "name" : "13"
  },
  "traverse" : "http://localhost:7474/db/data/node/47/traverse/{returnType}",
  "all_typed_relationships" : "http://localhost:7474/db/data/node/47/relationships/all/{-list|&|types}",
  "property" : "http://localhost:7474/db/data/node/47/properties/{key}",
  "self" : "http://localhost:7474/db/data/node/47",
  "outgoing_typed_relationships" : "http://localhost:7474/db/data/node/47/relationships/out/{-list|&|types}",
  "properties" : "http://localhost:7474/db/data/node/47/properties",
  "incoming_relationships" : "http://localhost:7474/db/data/node/47/relationships/in",
  "extensions" : {
  },
  "create_relationship" : "http://localhost:7474/db/data/node/47/relationships",
  "paged_traverse" : "http://localhost:7474/db/data/node/47/paged/traverse/{returnType}{?pageSize,leaseTime}",
  "all_relationships" : "http://localhost:7474/db/data/node/47/relationships/all",
  "incoming_typed_relationships" : "http://localhost:7474/db/data/node/47/relationships/in/{-list|&|types}"
}, {
  "outgoing_relationships" : "http://localhost:7474/db/data/node/48/relationships/out",
  "data" : {
    "name" : "14"
  },
  "traverse" : "http://localhost:7474/db/data/node/48/traverse/{returnType}",
  "all_typed_relationships" : "http://localhost:7474/db/data/node/48/relationships/all/{-list|&|types}",
  "property" : "http://localhost:7474/db/data/node/48/properties/{key}",
  "self" : "http://localhost:7474/db/data/node/48",
  "outgoing_typed_relationships" : "http://localhost:7474/db/data/node/48/relationships/out/{-list|&|types}",
  "properties" : "http://localhost:7474/db/data/node/48/properties",
  "incoming_relationships" : "http://localhost:7474/db/data/node/48/relationships/in",
  "extensions" : {
  },
  "create_relationship" : "http://localhost:7474/db/data/node/48/relationships",
  "paged_traverse" : "http://localhost:7474/db/data/node/48/paged/traverse/{returnType}{?pageSize,leaseTime}",
  "all_relationships" : "http://localhost:7474/db/data/node/48/relationships/all",
  "incoming_typed_relationships" : "http://localhost:7474/db/data/node/48/relationships/in/{-list|&|types}"
}, {
  "outgoing_relationships" : "http://localhost:7474/db/data/node/49/relationships/out",
  "data" : {
    "name" : "15"
  },
  "traverse" : "http://localhost:7474/db/data/node/49/traverse/{returnType}",
  "all_typed_relationships" : "http://localhost:7474/db/data/node/49/relationships/all/{-list|&|types}",
  "property" : "http://localhost:7474/db/data/node/49/properties/{key}",
  "self" : "http://localhost:7474/db/data/node/49",
  "outgoing_typed_relationships" : "http://localhost:7474/db/data/node/49/relationships/out/{-list|&|types}",
  "properties" : "http://localhost:7474/db/data/node/49/properties",
  "incoming_relationships" : "http://localhost:7474/db/data/node/49/relationships/in",
  "extensions" : {
  },
  "create_relationship" : "http://localhost:7474/db/data/node/49/relationships",
  "paged_traverse" : "http://localhost:7474/db/data/node/49/paged/traverse/{returnType}{?pageSize,leaseTime}",
  "all_relationships" : "http://localhost:7474/db/data/node/49/relationships/all",
  "incoming_typed_relationships" : "http://localhost:7474/db/data/node/49/relationships/in/{-list|&|types}"
}, {
  "outgoing_relationships" : "http://localhost:7474/db/data/node/50/relationships/out",
  "data" : {
    "name" : "16"
  },
  "traverse" : "http://localhost:7474/db/data/node/50/traverse/{returnType}",
  "all_typed_relationships" : "http://localhost:7474/db/data/node/50/relationships/all/{-list|&|types}",
  "property" : "http://localhost:7474/db/data/node/50/properties/{key}",
  "self" : "http://localhost:7474/db/data/node/50",
  "outgoing_typed_relationships" : "http://localhost:7474/db/data/node/50/relationships/out/{-list|&|types}",
  "properties" : "http://localhost:7474/db/data/node/50/properties",
  "incoming_relationships" : "http://localhost:7474/db/data/node/50/relationships/in",
  "extensions" : {
  },
  "create_relationship" : "http://localhost:7474/db/data/node/50/relationships",
  "paged_traverse" : "http://localhost:7474/db/data/node/50/paged/traverse/{returnType}{?pageSize,leaseTime}",
  "all_relationships" : "http://localhost:7474/db/data/node/50/relationships/all",
  "incoming_typed_relationships" : "http://localhost:7474/db/data/node/50/relationships/in/{-list|&|types}"
}, {
  "outgoing_relationships" : "http://localhost:7474/db/data/node/51/relationships/out",
  "data" : {
    "name" : "17"
  },
  "traverse" : "http://localhost:7474/db/data/node/51/traverse/{returnType}",
  "all_typed_relationships" : "http://localhost:7474/db/data/node/51/relationships/all/{-list|&|types}",
  "property" : "http://localhost:7474/db/data/node/51/properties/{key}",
  "self" : "http://localhost:7474/db/data/node/51",
  "outgoing_typed_relationships" : "http://localhost:7474/db/data/node/51/relationships/out/{-list|&|types}",
  "properties" : "http://localhost:7474/db/data/node/51/properties",
  "incoming_relationships" : "http://localhost:7474/db/data/node/51/relationships/in",
  "extensions" : {
  },
  "create_relationship" : "http://localhost:7474/db/data/node/51/relationships",
  "paged_traverse" : "http://localhost:7474/db/data/node/51/paged/traverse/{returnType}{?pageSize,leaseTime}",
  "all_relationships" : "http://localhost:7474/db/data/node/51/relationships/all",
  "incoming_typed_relationships" : "http://localhost:7474/db/data/node/51/relationships/in/{-list|&|types}"
}, {
  "outgoing_relationships" : "http://localhost:7474/db/data/node/52/relationships/out",
  "data" : {
    "name" : "18"
  },
  "traverse" : "http://localhost:7474/db/data/node/52/traverse/{returnType}",
  "all_typed_relationships" : "http://localhost:7474/db/data/node/52/relationships/all/{-list|&|types}",
  "property" : "http://localhost:7474/db/data/node/52/properties/{key}",
  "self" : "http://localhost:7474/db/data/node/52",
  "outgoing_typed_relationships" : "http://localhost:7474/db/data/node/52/relationships/out/{-list|&|types}",
  "properties" : "http://localhost:7474/db/data/node/52/properties",
  "incoming_relationships" : "http://localhost:7474/db/data/node/52/relationships/in",
  "extensions" : {
  },
  "create_relationship" : "http://localhost:7474/db/data/node/52/relationships",
  "paged_traverse" : "http://localhost:7474/db/data/node/52/paged/traverse/{returnType}{?pageSize,leaseTime}",
  "all_relationships" : "http://localhost:7474/db/data/node/52/relationships/all",
  "incoming_typed_relationships" : "http://localhost:7474/db/data/node/52/relationships/in/{-list|&|types}"
}, {
  "outgoing_relationships" : "http://localhost:7474/db/data/node/53/relationships/out",
  "data" : {
    "name" : "19"
  },
  "traverse" : "http://localhost:7474/db/data/node/53/traverse/{returnType}",
  "all_typed_relationships" : "http://localhost:7474/db/data/node/53/relationships/all/{-list|&|types}",
  "property" : "http://localhost:7474/db/data/node/53/properties/{key}",
  "self" : "http://localhost:7474/db/data/node/53",
  "outgoing_typed_relationships" : "http://localhost:7474/db/data/node/53/relationships/out/{-list|&|types}",
  "properties" : "http://localhost:7474/db/data/node/53/properties",
  "incoming_relationships" : "http://localhost:7474/db/data/node/53/relationships/in",
  "extensions" : {
  },
  "create_relationship" : "http://localhost:7474/db/data/node/53/relationships",
  "paged_traverse" : "http://localhost:7474/db/data/node/53/paged/traverse/{returnType}{?pageSize,leaseTime}",
  "all_relationships" : "http://localhost:7474/db/data/node/53/relationships/all",
  "incoming_typed_relationships" : "http://localhost:7474/db/data/node/53/relationships/in/{-list|&|types}"
}, {
  "outgoing_relationships" : "http://localhost:7474/db/data/node/55/relationships/out",
  "data" : {
    "name" : "21"
  },
  "traverse" : "http://localhost:7474/db/data/node/55/traverse/{returnType}",
  "all_typed_relationships" : "http://localhost:7474/db/data/node/55/relationships/all/{-list|&|types}",
  "property" : "http://localhost:7474/db/data/node/55/properties/{key}",
  "self" : "http://localhost:7474/db/data/node/55",
  "outgoing_typed_relationships" : "http://localhost:7474/db/data/node/55/relationships/out/{-list|&|types}",
  "properties" : "http://localhost:7474/db/data/node/55/properties",
  "incoming_relationships" : "http://localhost:7474/db/data/node/55/relationships/in",
  "extensions" : {
  },
  "create_relationship" : "http://localhost:7474/db/data/node/55/relationships",
  "paged_traverse" : "http://localhost:7474/db/data/node/55/paged/traverse/{returnType}{?pageSize,leaseTime}",
  "all_relationships" : "http://localhost:7474/db/data/node/55/relationships/all",
  "incoming_typed_relationships" : "http://localhost:7474/db/data/node/55/relationships/in/{-list|&|types}"
}, {
  "outgoing_relationships" : "http://localhost:7474/db/data/node/65/relationships/out",
  "data" : {
    "name" : "31"
  },
  "traverse" : "http://localhost:7474/db/data/node/65/traverse/{returnType}",
  "all_typed_relationships" : "http://localhost:7474/db/data/node/65/relationships/all/{-list|&|types}",
  "property" : "http://localhost:7474/db/data/node/65/properties/{key}",
  "self" : "http://localhost:7474/db/data/node/65",
  "outgoing_typed_relationships" : "http://localhost:7474/db/data/node/65/relationships/out/{-list|&|types}",
  "properties" : "http://localhost:7474/db/data/node/65/properties",
  "incoming_relationships" : "http://localhost:7474/db/data/node/65/relationships/in",
  "extensions" : {
  },
  "create_relationship" : "http://localhost:7474/db/data/node/65/relationships",
  "paged_traverse" : "http://localhost:7474/db/data/node/65/paged/traverse/{returnType}{?pageSize,leaseTime}",
  "all_relationships" : "http://localhost:7474/db/data/node/65/relationships/all",
  "incoming_typed_relationships" : "http://localhost:7474/db/data/node/65/relationships/in/{-list|&|types}"
} ]
18.13.6. 翻阅结果分页的遍历
分页遍历在服务器上保持状态，并允许客户端对结果进行分页遍历。处理下一页的遍历结果，客户端发出一个在分页遍历能够导致遍历填补下一也的HTTP GET请求（或者如果结果可用并不够一页也可部分填入)
注意如果一个遍历到期不活动，它会在下一个GET请求响应404.遍历租赁在每一个成功重新访问同也的最初的规定时间。
当分页遍历达到最后结果，客户端可以期待一个404响应的遍历由服务器处理。
请求示例
GET http://localhost:7474/db/data/node/67/paged/traverse/node/fcce249364714bada272d082749cea53
Accept: application/json

响应示例
200: OK
Content-Type: application/json
[ {
  "outgoing_relationships" : "http://localhost:7474/db/data/node/398/relationships/out",
  "data" : {
    "name" : "331"
  },
  "traverse" : "http://localhost:7474/db/data/node/398/traverse/{returnType}",
  "all_typed_relationships" : "http://localhost:7474/db/data/node/398/relationships/all/{-list|&|types}",
  "property" : "http://localhost:7474/db/data/node/398/properties/{key}",
  "self" : "http://localhost:7474/db/data/node/398",
  "outgoing_typed_relationships" : "http://localhost:7474/db/data/node/398/relationships/out/{-list|&|types}",
  "properties" : "http://localhost:7474/db/data/node/398/properties",
  "incoming_relationships" : "http://localhost:7474/db/data/node/398/relationships/in",
  "extensions" : {
  },
  "create_relationship" : "http://localhost:7474/db/data/node/398/relationships",
  "paged_traverse" : "http://localhost:7474/db/data/node/398/paged/traverse/{returnType}{?pageSize,leaseTime}",
  "all_relationships" : "http://localhost:7474/db/data/node/398/relationships/all",
  "incoming_typed_relationships" : "http://localhost:7474/db/data/node/398/relationships/in/{-list|&|types}"
}, {
  "outgoing_relationships" : "http://localhost:7474/db/data/node/408/relationships/out",
  "data" : {
    "name" : "341"
  },
  "traverse" : "http://localhost:7474/db/data/node/408/traverse/{returnType}",
  "all_typed_relationships" : "http://localhost:7474/db/data/node/408/relationships/all/{-list|&|types}",
  "property" : "http://localhost:7474/db/data/node/408/properties/{key}",
  "self" : "http://localhost:7474/db/data/node/408",
  "outgoing_typed_relationships" : "http://localhost:7474/db/data/node/408/relationships/out/{-list|&|types}",
  "properties" : "http://localhost:7474/db/data/node/408/properties",
  "incoming_relationships" : "http://localhost:7474/db/data/node/408/relationships/in",
  "extensions" : {
  },
  "create_relationship" : "http://localhost:7474/db/data/node/408/relationships",
  "paged_traverse" : "http://localhost:7474/db/data/node/408/paged/traverse/{returnType}{?pageSize,leaseTime}",
  "all_relationships" : "http://localhost:7474/db/data/node/408/relationships/all",
  "incoming_typed_relationships" : "http://localhost:7474/db/data/node/408/relationships/in/{-list|&|types}"
}, {
  "outgoing_relationships" : "http://localhost:7474/db/data/node/418/relationships/out",
  "data" : {
    "name" : "351"
  },
  "traverse" : "http://localhost:7474/db/data/node/418/traverse/{returnType}",
  "all_typed_relationships" : "http://localhost:7474/db/data/node/418/relationships/all/{-list|&|types}",
  "property" : "http://localhost:7474/db/data/node/418/properties/{key}",
  "self" : "http://localhost:7474/db/data/node/418",
  "outgoing_typed_relationships" : "http://localhost:7474/db/data/node/418/relationships/out/{-list|&|types}",
  "properties" : "http://localhost:7474/db/data/node/418/properties",
  "incoming_relationships" : "http://localhost:7474/db/data/node/418/relationships/in",
  "extensions" : {
  },
  "create_relationship" : "http://localhost:7474/db/data/node/418/relationships",
  "paged_traverse" : "http://localhost:7474/db/data/node/418/paged/traverse/{returnType}{?pageSize,leaseTime}",
  "all_relationships" : "http://localhost:7474/db/data/node/418/relationships/all",
  "incoming_typed_relationships" : "http://localhost:7474/db/data/node/418/relationships/in/{-list|&|types}"
}, {
  "outgoing_relationships" : "http://localhost:7474/db/data/node/428/relationships/out",
  "data" : {
    "name" : "361"
  },
  "traverse" : "http://localhost:7474/db/data/node/428/traverse/{returnType}",
  "all_typed_relationships" : "http://localhost:7474/db/data/node/428/relationships/all/{-list|&|types}",
  "property" : "http://localhost:7474/db/data/node/428/properties/{key}",
  "self" : "http://localhost:7474/db/data/node/428",
  "outgoing_typed_relationships" : "http://localhost:7474/db/data/node/428/relationships/out/{-list|&|types}",
  "properties" : "http://localhost:7474/db/data/node/428/properties",
  "incoming_relationships" : "http://localhost:7474/db/data/node/428/relationships/in",
  "extensions" : {
  },
  "create_relationship" : "http://localhost:7474/db/data/node/428/relationships",
  "paged_traverse" : "http://localhost:7474/db/data/node/428/paged/traverse/{returnType}{?pageSize,leaseTime}",
  "all_relationships" : "http://localhost:7474/db/data/node/428/relationships/all",
  "incoming_typed_relationships" : "http://localhost:7474/db/data/node/428/relationships/in/{-list|&|types}"
}, {
  "outgoing_relationships" : "http://localhost:7474/db/data/node/438/relationships/out",
  "data" : {
    "name" : "371"
  },
  "traverse" : "http://localhost:7474/db/data/node/438/traverse/{returnType}",
  "all_typed_relationships" : "http://localhost:7474/db/data/node/438/relationships/all/{-list|&|types}",
  "property" : "http://localhost:7474/db/data/node/438/properties/{key}",
  "self" : "http://localhost:7474/db/data/node/438",
  "outgoing_typed_relationships" : "http://localhost:7474/db/data/node/438/relationships/out/{-list|&|types}",
  "properties" : "http://localhost:7474/db/data/node/438/properties",
  "incoming_relationships" : "http://localhost:7474/db/data/node/438/relationships/in",
  "extensions" : {
  },
  "create_relationship" : "http://localhost:7474/db/data/node/438/relationships",
  "paged_traverse" : "http://localhost:7474/db/data/node/438/paged/traverse/{returnType}{?pageSize,leaseTime}",
  "all_relationships" : "http://localhost:7474/db/data/node/438/relationships/all",
  "incoming_typed_relationships" : "http://localhost:7474/db/data/node/438/relationships/in/{-list|&|types}"
}, {
  "outgoing_relationships" : "http://localhost:7474/db/data/node/448/relationships/out",
  "data" : {
    "name" : "381"
  },
  "traverse" : "http://localhost:7474/db/data/node/448/traverse/{returnType}",
  "all_typed_relationships" : "http://localhost:7474/db/data/node/448/relationships/all/{-list|&|types}",
  "property" : "http://localhost:7474/db/data/node/448/properties/{key}",
  "self" : "http://localhost:7474/db/data/node/448",
  "outgoing_typed_relationships" : "http://localhost:7474/db/data/node/448/relationships/out/{-list|&|types}",
  "properties" : "http://localhost:7474/db/data/node/448/properties",
  "incoming_relationships" : "http://localhost:7474/db/data/node/448/relationships/in",
  "extensions" : {
  },
  "create_relationship" : "http://localhost:7474/db/data/node/448/relationships",
  "paged_traverse" : "http://localhost:7474/db/data/node/448/paged/traverse/{returnType}{?pageSize,leaseTime}",
  "all_relationships" : "http://localhost:7474/db/data/node/448/relationships/all",
  "incoming_typed_relationships" : "http://localhost:7474/db/data/node/448/relationships/in/{-list|&|types}"
}, {
  "outgoing_relationships" : "http://localhost:7474/db/data/node/458/relationships/out",
  "data" : {
    "name" : "391"
  },
  "traverse" : "http://localhost:7474/db/data/node/458/traverse/{returnType}",
  "all_typed_relationships" : "http://localhost:7474/db/data/node/458/relationships/all/{-list|&|types}",
  "property" : "http://localhost:7474/db/data/node/458/properties/{key}",
  "self" : "http://localhost:7474/db/data/node/458",
  "outgoing_typed_relationships" : "http://localhost:7474/db/data/node/458/relationships/out/{-list|&|types}",
  "properties" : "http://localhost:7474/db/data/node/458/properties",
  "incoming_relationships" : "http://localhost:7474/db/data/node/458/relationships/in",
  "extensions" : {
  },
  "create_relationship" : "http://localhost:7474/db/data/node/458/relationships",
  "paged_traverse" : "http://localhost:7474/db/data/node/458/paged/traverse/{returnType}{?pageSize,leaseTime}",
  "all_relationships" : "http://localhost:7474/db/data/node/458/relationships/all",
  "incoming_typed_relationships" : "http://localhost:7474/db/data/node/458/relationships/in/{-list|&|types}"
}, {
  "outgoing_relationships" : "http://localhost:7474/db/data/node/468/relationships/out",
  "data" : {
    "name" : "401"
  },
  "traverse" : "http://localhost:7474/db/data/node/468/traverse/{returnType}",
  "all_typed_relationships" : "http://localhost:7474/db/data/node/468/relationships/all/{-list|&|types}",
  "property" : "http://localhost:7474/db/data/node/468/properties/{key}",
  "self" : "http://localhost:7474/db/data/node/468",
  "outgoing_typed_relationships" : "http://localhost:7474/db/data/node/468/relationships/out/{-list|&|types}",
  "properties" : "http://localhost:7474/db/data/node/468/properties",
  "incoming_relationships" : "http://localhost:7474/db/data/node/468/relationships/in",
  "extensions" : {
  },
  "create_relationship" : "http://localhost:7474/db/data/node/468/relationships",
  "paged_traverse" : "http://localhost:7474/db/data/node/468/paged/traverse/{returnType}{?pageSize,leaseTime}",
  "all_relationships" : "http://localhost:7474/db/data/node/468/relationships/all",
  "incoming_typed_relationships" : "http://localhost:7474/db/data/node/468/relationships/in/{-list|&|types}"
}, {
  "outgoing_relationships" : "http://localhost:7474/db/data/node/477/relationships/out",
  "data" : {
    "name" : "410"
  },
  "traverse" : "http://localhost:7474/db/data/node/477/traverse/{returnType}",
  "all_typed_relationships" : "http://localhost:7474/db/data/node/477/relationships/all/{-list|&|types}",
  "property" : "http://localhost:7474/db/data/node/477/properties/{key}",
  "self" : "http://localhost:7474/db/data/node/477",
  "outgoing_typed_relationships" : "http://localhost:7474/db/data/node/477/relationships/out/{-list|&|types}",
  "properties" : "http://localhost:7474/db/data/node/477/properties",
  "incoming_relationships" : "http://localhost:7474/db/data/node/477/relationships/in",
  "extensions" : {
  },
  "create_relationship" : "http://localhost:7474/db/data/node/477/relationships",
  "paged_traverse" : "http://localhost:7474/db/data/node/477/paged/traverse/{returnType}{?pageSize,leaseTime}",
  "all_relationships" : "http://localhost:7474/db/data/node/477/relationships/all",
  "incoming_typed_relationships" : "http://localhost:7474/db/data/node/477/relationships/in/{-list|&|types}"
}, {
  "outgoing_relationships" : "http://localhost:7474/db/data/node/478/relationships/out",
  "data" : {
    "name" : "411"
  },
  "traverse" : "http://localhost:7474/db/data/node/478/traverse/{returnType}",
  "all_typed_relationships" : "http://localhost:7474/db/data/node/478/relationships/all/{-list|&|types}",
  "property" : "http://localhost:7474/db/data/node/478/properties/{key}",
  "self" : "http://localhost:7474/db/data/node/478",
  "outgoing_typed_relationships" : "http://localhost:7474/db/data/node/478/relationships/out/{-list|&|types}",
  "properties" : "http://localhost:7474/db/data/node/478/properties",
  "incoming_relationships" : "http://localhost:7474/db/data/node/478/relationships/in",
  "extensions" : {
  },
  "create_relationship" : "http://localhost:7474/db/data/node/478/relationships",
  "paged_traverse" : "http://localhost:7474/db/data/node/478/paged/traverse/{returnType}{?pageSize,leaseTime}",
  "all_relationships" : "http://localhost:7474/db/data/node/478/relationships/all",
  "incoming_typed_relationships" : "http://localhost:7474/db/data/node/478/relationships/in/{-list|&|types}"
}, {
  "outgoing_relationships" : "http://localhost:7474/db/data/node/479/relationships/out",
  "data" : {
    "name" : "412"
  },
  "traverse" : "http://localhost:7474/db/data/node/479/traverse/{returnType}",
  "all_typed_relationships" : "http://localhost:7474/db/data/node/479/relationships/all/{-list|&|types}",
  "property" : "http://localhost:7474/db/data/node/479/properties/{key}",
  "self" : "http://localhost:7474/db/data/node/479",
  "outgoing_typed_relationships" : "http://localhost:7474/db/data/node/479/relationships/out/{-list|&|types}",
  "properties" : "http://localhost:7474/db/data/node/479/properties",
  "incoming_relationships" : "http://localhost:7474/db/data/node/479/relationships/in",
  "extensions" : {
  },
  "create_relationship" : "http://localhost:7474/db/data/node/479/relationships",
  "paged_traverse" : "http://localhost:7474/db/data/node/479/paged/traverse/{returnType}{?pageSize,leaseTime}",
  "all_relationships" : "http://localhost:7474/db/data/node/479/relationships/all",
  "incoming_typed_relationships" : "http://localhost:7474/db/data/node/479/relationships/in/{-list|&|types}"
}, {
  "outgoing_relationships" : "http://localhost:7474/db/data/node/480/relationships/out",
  "data" : {
    "name" : "413"
  },
  "traverse" : "http://localhost:7474/db/data/node/480/traverse/{returnType}",
  "all_typed_relationships" : "http://localhost:7474/db/data/node/480/relationships/all/{-list|&|types}",
  "property" : "http://localhost:7474/db/data/node/480/properties/{key}",
  "self" : "http://localhost:7474/db/data/node/480",
  "outgoing_typed_relationships" : "http://localhost:7474/db/data/node/480/relationships/out/{-list|&|types}",
  "properties" : "http://localhost:7474/db/data/node/480/properties",
  "incoming_relationships" : "http://localhost:7474/db/data/node/480/relationships/in",
  "extensions" : {
  },
  "create_relationship" : "http://localhost:7474/db/data/node/480/relationships",
  "paged_traverse" : "http://localhost:7474/db/data/node/480/paged/traverse/{returnType}{?pageSize,leaseTime}",
  "all_relationships" : "http://localhost:7474/db/data/node/480/relationships/all",
  "incoming_typed_relationships" : "http://localhost:7474/db/data/node/480/relationships/in/{-list|&|types}"
}, {
  "outgoing_relationships" : "http://localhost:7474/db/data/node/481/relationships/out",
  "data" : {
    "name" : "414"
  },
  "traverse" : "http://localhost:7474/db/data/node/481/traverse/{returnType}",
  "all_typed_relationships" : "http://localhost:7474/db/data/node/481/relationships/all/{-list|&|types}",
  "property" : "http://localhost:7474/db/data/node/481/properties/{key}",
  "self" : "http://localhost:7474/db/data/node/481",
  "outgoing_typed_relationships" : "http://localhost:7474/db/data/node/481/relationships/out/{-list|&|types}",
  "properties" : "http://localhost:7474/db/data/node/481/properties",
  "incoming_relationships" : "http://localhost:7474/db/data/node/481/relationships/in",
  "extensions" : {
  },
  "create_relationship" : "http://localhost:7474/db/data/node/481/relationships",
  "paged_traverse" : "http://localhost:7474/db/data/node/481/paged/traverse/{returnType}{?pageSize,leaseTime}",
  "all_relationships" : "http://localhost:7474/db/data/node/481/relationships/all",
  "incoming_typed_relationships" : "http://localhost:7474/db/data/node/481/relationships/in/{-list|&|types}"
}, {
  "outgoing_relationships" : "http://localhost:7474/db/data/node/482/relationships/out",
  "data" : {
    "name" : "415"
  },
  "traverse" : "http://localhost:7474/db/data/node/482/traverse/{returnType}",
  "all_typed_relationships" : "http://localhost:7474/db/data/node/482/relationships/all/{-list|&|types}",
  "property" : "http://localhost:7474/db/data/node/482/properties/{key}",
  "self" : "http://localhost:7474/db/data/node/482",
  "outgoing_typed_relationships" : "http://localhost:7474/db/data/node/482/relationships/out/{-list|&|types}",
  "properties" : "http://localhost:7474/db/data/node/482/properties",
  "incoming_relationships" : "http://localhost:7474/db/data/node/482/relationships/in",
  "extensions" : {
  },
  "create_relationship" : "http://localhost:7474/db/data/node/482/relationships",
  "paged_traverse" : "http://localhost:7474/db/data/node/482/paged/traverse/{returnType}{?pageSize,leaseTime}",
  "all_relationships" : "http://localhost:7474/db/data/node/482/relationships/all",
  "incoming_typed_relationships" : "http://localhost:7474/db/data/node/482/relationships/in/{-list|&|types}"
}, {
  "outgoing_relationships" : "http://localhost:7474/db/data/node/483/relationships/out",
  "data" : {
    "name" : "416"
  },
  "traverse" : "http://localhost:7474/db/data/node/483/traverse/{returnType}",
  "all_typed_relationships" : "http://localhost:7474/db/data/node/483/relationships/all/{-list|&|types}",
  "property" : "http://localhost:7474/db/data/node/483/properties/{key}",
  "self" : "http://localhost:7474/db/data/node/483",
  "outgoing_typed_relationships" : "http://localhost:7474/db/data/node/483/relationships/out/{-list|&|types}",
  "properties" : "http://localhost:7474/db/data/node/483/properties",
  "incoming_relationships" : "http://localhost:7474/db/data/node/483/relationships/in",
  "extensions" : {
  },
  "create_relationship" : "http://localhost:7474/db/data/node/483/relationships",
  "paged_traverse" : "http://localhost:7474/db/data/node/483/paged/traverse/{returnType}{?pageSize,leaseTime}",
  "all_relationships" : "http://localhost:7474/db/data/node/483/relationships/all",
  "incoming_typed_relationships" : "http://localhost:7474/db/data/node/483/relationships/in/{-list|&|types}"
}, {
  "outgoing_relationships" : "http://localhost:7474/db/data/node/484/relationships/out",
  "data" : {
    "name" : "417"
  },
  "traverse" : "http://localhost:7474/db/data/node/484/traverse/{returnType}",
  "all_typed_relationships" : "http://localhost:7474/db/data/node/484/relationships/all/{-list|&|types}",
  "property" : "http://localhost:7474/db/data/node/484/properties/{key}",
  "self" : "http://localhost:7474/db/data/node/484",
  "outgoing_typed_relationships" : "http://localhost:7474/db/data/node/484/relationships/out/{-list|&|types}",
  "properties" : "http://localhost:7474/db/data/node/484/properties",
  "incoming_relationships" : "http://localhost:7474/db/data/node/484/relationships/in",
  "extensions" : {
  },
  "create_relationship" : "http://localhost:7474/db/data/node/484/relationships",
  "paged_traverse" : "http://localhost:7474/db/data/node/484/paged/traverse/{returnType}{?pageSize,leaseTime}",
  "all_relationships" : "http://localhost:7474/db/data/node/484/relationships/all",
  "incoming_typed_relationships" : "http://localhost:7474/db/data/node/484/relationships/in/{-list|&|types}"
}, {
  "outgoing_relationships" : "http://localhost:7474/db/data/node/485/relationships/out",
  "data" : {
    "name" : "418"
  },
  "traverse" : "http://localhost:7474/db/data/node/485/traverse/{returnType}",
  "all_typed_relationships" : "http://localhost:7474/db/data/node/485/relationships/all/{-list|&|types}",
  "property" : "http://localhost:7474/db/data/node/485/properties/{key}",
  "self" : "http://localhost:7474/db/data/node/485",
  "outgoing_typed_relationships" : "http://localhost:7474/db/data/node/485/relationships/out/{-list|&|types}",
  "properties" : "http://localhost:7474/db/data/node/485/properties",
  "incoming_relationships" : "http://localhost:7474/db/data/node/485/relationships/in",
  "extensions" : {
  },
  "create_relationship" : "http://localhost:7474/db/data/node/485/relationships",
  "paged_traverse" : "http://localhost:7474/db/data/node/485/paged/traverse/{returnType}{?pageSize,leaseTime}",
  "all_relationships" : "http://localhost:7474/db/data/node/485/relationships/all",
  "incoming_typed_relationships" : "http://localhost:7474/db/data/node/485/relationships/in/{-list|&|types}"
}, {
  "outgoing_relationships" : "http://localhost:7474/db/data/node/486/relationships/out",
  "data" : {
    "name" : "419"
  },
  "traverse" : "http://localhost:7474/db/data/node/486/traverse/{returnType}",
  "all_typed_relationships" : "http://localhost:7474/db/data/node/486/relationships/all/{-list|&|types}",
  "property" : "http://localhost:7474/db/data/node/486/properties/{key}",
  "self" : "http://localhost:7474/db/data/node/486",
  "outgoing_typed_relationships" : "http://localhost:7474/db/data/node/486/relationships/out/{-list|&|types}",
  "properties" : "http://localhost:7474/db/data/node/486/properties",
  "incoming_relationships" : "http://localhost:7474/db/data/node/486/relationships/in",
  "extensions" : {
  },
  "create_relationship" : "http://localhost:7474/db/data/node/486/relationships",
  "paged_traverse" : "http://localhost:7474/db/data/node/486/paged/traverse/{returnType}{?pageSize,leaseTime}",
  "all_relationships" : "http://localhost:7474/db/data/node/486/relationships/all",
  "incoming_typed_relationships" : "http://localhost:7474/db/data/node/486/relationships/in/{-list|&|types}"
}, {
  "outgoing_relationships" : "http://localhost:7474/db/data/node/488/relationships/out",
  "data" : {
    "name" : "421"
  },
  "traverse" : "http://localhost:7474/db/data/node/488/traverse/{returnType}",
  "all_typed_relationships" : "http://localhost:7474/db/data/node/488/relationships/all/{-list|&|types}",
  "property" : "http://localhost:7474/db/data/node/488/properties/{key}",
  "self" : "http://localhost:7474/db/data/node/488",
  "outgoing_typed_relationships" : "http://localhost:7474/db/data/node/488/relationships/out/{-list|&|types}",
  "properties" : "http://localhost:7474/db/data/node/488/properties",
  "incoming_relationships" : "http://localhost:7474/db/data/node/488/relationships/in",
  "extensions" : {
  },
  "create_relationship" : "http://localhost:7474/db/data/node/488/relationships",
  "paged_traverse" : "http://localhost:7474/db/data/node/488/paged/traverse/{returnType}{?pageSize,leaseTime}",
  "all_relationships" : "http://localhost:7474/db/data/node/488/relationships/all",
  "incoming_typed_relationships" : "http://localhost:7474/db/data/node/488/relationships/in/{-list|&|types}"
}, {
  "outgoing_relationships" : "http://localhost:7474/db/data/node/498/relationships/out",
  "data" : {
    "name" : "431"
  },
  "traverse" : "http://localhost:7474/db/data/node/498/traverse/{returnType}",
  "all_typed_relationships" : "http://localhost:7474/db/data/node/498/relationships/all/{-list|&|types}",
  "property" : "http://localhost:7474/db/data/node/498/properties/{key}",
  "self" : "http://localhost:7474/db/data/node/498",
  "outgoing_typed_relationships" : "http://localhost:7474/db/data/node/498/relationships/out/{-list|&|types}",
  "properties" : "http://localhost:7474/db/data/node/498/properties",
  "incoming_relationships" : "http://localhost:7474/db/data/node/498/relationships/in",
  "extensions" : {
  },
  "create_relationship" : "http://localhost:7474/db/data/node/498/relationships",
  "paged_traverse" : "http://localhost:7474/db/data/node/498/paged/traverse/{returnType}{?pageSize,leaseTime}",
  "all_relationships" : "http://localhost:7474/db/data/node/498/relationships/all",
  "incoming_typed_relationships" : "http://localhost:7474/db/data/node/498/relationships/in/{-list|&|types}"
}, {
  "outgoing_relationships" : "http://localhost:7474/db/data/node/508/relationships/out",
  "data" : {
    "name" : "441"
  },
  "traverse" : "http://localhost:7474/db/data/node/508/traverse/{returnType}",
  "all_typed_relationships" : "http://localhost:7474/db/data/node/508/relationships/all/{-list|&|types}",
  "property" : "http://localhost:7474/db/data/node/508/properties/{key}",
  "self" : "http://localhost:7474/db/data/node/508",
  "outgoing_typed_relationships" : "http://localhost:7474/db/data/node/508/relationships/out/{-list|&|types}",
  "properties" : "http://localhost:7474/db/data/node/508/properties",
  "incoming_relationships" : "http://localhost:7474/db/data/node/508/relationships/in",
  "extensions" : {
  },
  "create_relationship" : "http://localhost:7474/db/data/node/508/relationships",
  "paged_traverse" : "http://localhost:7474/db/data/node/508/paged/traverse/{returnType}{?pageSize,leaseTime}",
  "all_relationships" : "http://localhost:7474/db/data/node/508/relationships/all",
  "incoming_typed_relationships" : "http://localhost:7474/db/data/node/508/relationships/in/{-list|&|types}"
} ]

18.13.7. 分页遍历页面大小
默认页面大小是50条目，但根据应用程序大小的页面大小可能比较合适。这可以通过添加一个pageSize的查询参数设置。
请求示例
POST http://localhost:7474/db/data/node/544/paged/traverse/node?pageSize=1
Accept: application/json
Content-Type: application/json
{
  "prune_evaluator" : {
    "language" : "builtin",
    "name" : "none"
  },
  "return_filter" : {
    "language" : "javascript",
    "body" : "position.endNode().getProperty('name').contains('1');"
  },
  "order" : "depth_first",
  "relationships" : {
    "type" : "NEXT",
    "direction" : "out"
  }
}
响应示例
201: Created
Content-Type: application/json
Location: http://localhost:7474/db/data/node/544/paged/traverse/node/c4e710da115c461eba929a97ca002db7
[ {
  "outgoing_relationships" : "http://localhost:7474/db/data/node/545/relationships/out",
  "data" : {
    "name" : "1"
  },
  "traverse" : "http://localhost:7474/db/data/node/545/traverse/{returnType}",
  "all_typed_relationships" : "http://localhost:7474/db/data/node/545/relationships/all/{-list|&|types}",
  "property" : "http://localhost:7474/db/data/node/545/properties/{key}",
  "self" : "http://localhost:7474/db/data/node/545",
  "outgoing_typed_relationships" : "http://localhost:7474/db/data/node/545/relationships/out/{-list|&|types}",
  "properties" : "http://localhost:7474/db/data/node/545/properties",
  "incoming_relationships" : "http://localhost:7474/db/data/node/545/relationships/in",
  "extensions" : {
  },
  "create_relationship" : "http://localhost:7474/db/data/node/545/relationships",
  "paged_traverse" : "http://localhost:7474/db/data/node/545/paged/traverse/{returnType}{?pageSize,leaseTime}",
  "all_relationships" : "http://localhost:7474/db/data/node/545/relationships/all",
  "incoming_typed_relationships" : "http://localhost:7474/db/data/node/545/relationships/in/{-list|&|types}"
} ]
18.13.8. 分页查询超时
分页查询默认的超时为60秒，但是这取决于你的应用程序大小来设置更加合理。这里可以添加带有数字秒的分页查询最后的时间leaseTime查询参数来进行设置

请求示例
POST http://localhost:7474/db/data/node/577/paged/traverse/node?leaseTime=10
Accept: application/json
Content-Type: application/json
{
  "prune_evaluator" : {
    "language" : "builtin",
    "name" : "none"
  },
  "return_filter" : {
    "language" : "javascript",
    "body" : "position.endNode().getProperty('name').contains('1');"
  },
  "order" : "depth_first",
  "relationships" : {
    "type" : "NEXT",
    "direction" : "out"
  }
}
响应示例
201: Created
Content-Type: application/json
Location: http://localhost:7474/db/data/node/577/paged/traverse/node/d5cf0819450c439fa3feb8afcd2ebe23
[ {
  "outgoing_relationships" : "http://localhost:7474/db/data/node/578/relationships/out",
  "data" : {
    "name" : "1"
  },
  "traverse" : "http://localhost:7474/db/data/node/578/traverse/{returnType}",
  "all_typed_relationships" : "http://localhost:7474/db/data/node/578/relationships/all/{-list|&|types}",
  "property" : "http://localhost:7474/db/data/node/578/properties/{key}",
  "self" : "http://localhost:7474/db/data/node/578",
  "outgoing_typed_relationships" : "http://localhost:7474/db/data/node/578/relationships/out/{-list|&|types}",
  "properties" : "http://localhost:7474/db/data/node/578/properties",
  "incoming_relationships" : "http://localhost:7474/db/data/node/578/relationships/in",
  "extensions" : {
  },
  "create_relationship" : "http://localhost:7474/db/data/node/578/relationships",
  "paged_traverse" : "http://localhost:7474/db/data/node/578/paged/traverse/{returnType}{?pageSize,leaseTime}",
  "all_relationships" : "http://localhost:7474/db/data/node/578/relationships/all",
  "incoming_typed_relationships" : "http://localhost:7474/db/data/node/578/relationships/in/{-list|&|types}"
}, {
  "outgoing_relationships" : "http://localhost:7474/db/data/node/587/relationships/out",
  "data" : {
    "name" : "10"
  },
  "traverse" : "http://localhost:7474/db/data/node/587/traverse/{returnType}",
  "all_typed_relationships" : "http://localhost:7474/db/data/node/587/relationships/all/{-list|&|types}",
  "property" : "http://localhost:7474/db/data/node/587/properties/{key}",
  "self" : "http://localhost:7474/db/data/node/587",
  "outgoing_typed_relationships" : "http://localhost:7474/db/data/node/587/relationships/out/{-list|&|types}",
  "properties" : "http://localhost:7474/db/data/node/587/properties",
  "incoming_relationships" : "http://localhost:7474/db/data/node/587/relationships/in",
  "extensions" : {
  },
  "create_relationship" : "http://localhost:7474/db/data/node/587/relationships",
  "paged_traverse" : "http://localhost:7474/db/data/node/587/paged/traverse/{returnType}{?pageSize,leaseTime}",
  "all_relationships" : "http://localhost:7474/db/data/node/587/relationships/all",
  "incoming_typed_relationships" : "http://localhost:7474/db/data/node/587/relationships/in/{-list|&|types}"
}, {
  "outgoing_relationships" : "http://localhost:7474/db/data/node/588/relationships/out",
  "data" : {
    "name" : "11"
  },
  "traverse" : "http://localhost:7474/db/data/node/588/traverse/{returnType}",
  "all_typed_relationships" : "http://localhost:7474/db/data/node/588/relationships/all/{-list|&|types}",
  "property" : "http://localhost:7474/db/data/node/588/properties/{key}",
  "self" : "http://localhost:7474/db/data/node/588",
  "outgoing_typed_relationships" : "http://localhost:7474/db/data/node/588/relationships/out/{-list|&|types}",
  "properties" : "http://localhost:7474/db/data/node/588/properties",
  "incoming_relationships" : "http://localhost:7474/db/data/node/588/relationships/in",
  "extensions" : {
  },
  "create_relationship" : "http://localhost:7474/db/data/node/588/relationships",
  "paged_traverse" : "http://localhost:7474/db/data/node/588/paged/traverse/{returnType}{?pageSize,leaseTime}",
  "all_relationships" : "http://localhost:7474/db/data/node/588/relationships/all",
  "incoming_typed_relationships" : "http://localhost:7474/db/data/node/588/relationships/in/{-list|&|types}"
}, {
  "outgoing_relationships" : "http://localhost:7474/db/data/node/589/relationships/out",
  "data" : {
    "name" : "12"
  },
  "traverse" : "http://localhost:7474/db/data/node/589/traverse/{returnType}",
  "all_typed_relationships" : "http://localhost:7474/db/data/node/589/relationships/all/{-list|&|types}",
  "property" : "http://localhost:7474/db/data/node/589/properties/{key}",
  "self" : "http://localhost:7474/db/data/node/589",
  "outgoing_typed_relationships" : "http://localhost:7474/db/data/node/589/relationships/out/{-list|&|types}",
  "properties" : "http://localhost:7474/db/data/node/589/properties",
  "incoming_relationships" : "http://localhost:7474/db/data/node/589/relationships/in",
  "extensions" : {
  },
  "create_relationship" : "http://localhost:7474/db/data/node/589/relationships",
  "paged_traverse" : "http://localhost:7474/db/data/node/589/paged/traverse/{returnType}{?pageSize,leaseTime}",
  "all_relationships" : "http://localhost:7474/db/data/node/589/relationships/all",
  "incoming_typed_relationships" : "http://localhost:7474/db/data/node/589/relationships/in/{-list|&|types}"
}, {
  "outgoing_relationships" : "http://localhost:7474/db/data/node/590/relationships/out",
  "data" : {
    "name" : "13"
  },
  "traverse" : "http://localhost:7474/db/data/node/590/traverse/{returnType}",
  "all_typed_relationships" : "http://localhost:7474/db/data/node/590/relationships/all/{-list|&|types}",
  "property" : "http://localhost:7474/db/data/node/590/properties/{key}",
  "self" : "http://localhost:7474/db/data/node/590",
  "outgoing_typed_relationships" : "http://localhost:7474/db/data/node/590/relationships/out/{-list|&|types}",
  "properties" : "http://localhost:7474/db/data/node/590/properties",
  "incoming_relationships" : "http://localhost:7474/db/data/node/590/relationships/in",
  "extensions" : {
  },
  "create_relationship" : "http://localhost:7474/db/data/node/590/relationships",
  "paged_traverse" : "http://localhost:7474/db/data/node/590/paged/traverse/{returnType}{?pageSize,leaseTime}",
  "all_relationships" : "http://localhost:7474/db/data/node/590/relationships/all",
  "incoming_typed_relationships" : "http://localhost:7474/db/data/node/590/relationships/in/{-list|&|types}"
}, {
  "outgoing_relationships" : "http://localhost:7474/db/data/node/591/relationships/out",
  "data" : {
    "name" : "14"
  },
  "traverse" : "http://localhost:7474/db/data/node/591/traverse/{returnType}",
  "all_typed_relationships" : "http://localhost:7474/db/data/node/591/relationships/all/{-list|&|types}",
  "property" : "http://localhost:7474/db/data/node/591/properties/{key}",
  "self" : "http://localhost:7474/db/data/node/591",
  "outgoing_typed_relationships" : "http://localhost:7474/db/data/node/591/relationships/out/{-list|&|types}",
  "properties" : "http://localhost:7474/db/data/node/591/properties",
  "incoming_relationships" : "http://localhost:7474/db/data/node/591/relationships/in",
  "extensions" : {
  },
  "create_relationship" : "http://localhost:7474/db/data/node/591/relationships",
  "paged_traverse" : "http://localhost:7474/db/data/node/591/paged/traverse/{returnType}{?pageSize,leaseTime}",
  "all_relationships" : "http://localhost:7474/db/data/node/591/relationships/all",
  "incoming_typed_relationships" : "http://localhost:7474/db/data/node/591/relationships/in/{-list|&|types}"
}, {
  "outgoing_relationships" : "http://localhost:7474/db/data/node/592/relationships/out",
  "data" : {
    "name" : "15"
  },
  "traverse" : "http://localhost:7474/db/data/node/592/traverse/{returnType}",
  "all_typed_relationships" : "http://localhost:7474/db/data/node/592/relationships/all/{-list|&|types}",
  "property" : "http://localhost:7474/db/data/node/592/properties/{key}",
  "self" : "http://localhost:7474/db/data/node/592",
  "outgoing_typed_relationships" : "http://localhost:7474/db/data/node/592/relationships/out/{-list|&|types}",
  "properties" : "http://localhost:7474/db/data/node/592/properties",
  "incoming_relationships" : "http://localhost:7474/db/data/node/592/relationships/in",
  "extensions" : {
  },
  "create_relationship" : "http://localhost:7474/db/data/node/592/relationships",
  "paged_traverse" : "http://localhost:7474/db/data/node/592/paged/traverse/{returnType}{?pageSize,leaseTime}",
  "all_relationships" : "http://localhost:7474/db/data/node/592/relationships/all",
  "incoming_typed_relationships" : "http://localhost:7474/db/data/node/592/relationships/in/{-list|&|types}"
}, {
  "outgoing_relationships" : "http://localhost:7474/db/data/node/593/relationships/out",
  "data" : {
    "name" : "16"
  },
  "traverse" : "http://localhost:7474/db/data/node/593/traverse/{returnType}",
  "all_typed_relationships" : "http://localhost:7474/db/data/node/593/relationships/all/{-list|&|types}",
  "property" : "http://localhost:7474/db/data/node/593/properties/{key}",
  "self" : "http://localhost:7474/db/data/node/593",
  "outgoing_typed_relationships" : "http://localhost:7474/db/data/node/593/relationships/out/{-list|&|types}",
  "properties" : "http://localhost:7474/db/data/node/593/properties",
  "incoming_relationships" : "http://localhost:7474/db/data/node/593/relationships/in",
  "extensions" : {
  },
  "create_relationship" : "http://localhost:7474/db/data/node/593/relationships",
  "paged_traverse" : "http://localhost:7474/db/data/node/593/paged/traverse/{returnType}{?pageSize,leaseTime}",
  "all_relationships" : "http://localhost:7474/db/data/node/593/relationships/all",
  "incoming_typed_relationships" : "http://localhost:7474/db/data/node/593/relationships/in/{-list|&|types}"
}, {
  "outgoing_relationships" : "http://localhost:7474/db/data/node/594/relationships/out",
  "data" : {
    "name" : "17"
  },
  "traverse" : "http://localhost:7474/db/data/node/594/traverse/{returnType}",
  "all_typed_relationships" : "http://localhost:7474/db/data/node/594/relationships/all/{-list|&|types}",
  "property" : "http://localhost:7474/db/data/node/594/properties/{key}",
  "self" : "http://localhost:7474/db/data/node/594",
  "outgoing_typed_relationships" : "http://localhost:7474/db/data/node/594/relationships/out/{-list|&|types}",
  "properties" : "http://localhost:7474/db/data/node/594/properties",
  "incoming_relationships" : "http://localhost:7474/db/data/node/594/relationships/in",
  "extensions" : {
  },
  "create_relationship" : "http://localhost:7474/db/data/node/594/relationships",
  "paged_traverse" : "http://localhost:7474/db/data/node/594/paged/traverse/{returnType}{?pageSize,leaseTime}",
  "all_relationships" : "http://localhost:7474/db/data/node/594/relationships/all",
  "incoming_typed_relationships" : "http://localhost:7474/db/data/node/594/relationships/in/{-list|&|types}"
}, {
  "outgoing_relationships" : "http://localhost:7474/db/data/node/595/relationships/out",
  "data" : {
    "name" : "18"
  },
  "traverse" : "http://localhost:7474/db/data/node/595/traverse/{returnType}",
  "all_typed_relationships" : "http://localhost:7474/db/data/node/595/relationships/all/{-list|&|types}",
  "property" : "http://localhost:7474/db/data/node/595/properties/{key}",
  "self" : "http://localhost:7474/db/data/node/595",
  "outgoing_typed_relationships" : "http://localhost:7474/db/data/node/595/relationships/out/{-list|&|types}",
  "properties" : "http://localhost:7474/db/data/node/595/properties",
  "incoming_relationships" : "http://localhost:7474/db/data/node/595/relationships/in",
  "extensions" : {
  },
  "create_relationship" : "http://localhost:7474/db/data/node/595/relationships",
  "paged_traverse" : "http://localhost:7474/db/data/node/595/paged/traverse/{returnType}{?pageSize,leaseTime}",
  "all_relationships" : "http://localhost:7474/db/data/node/595/relationships/all",
  "incoming_typed_relationships" : "http://localhost:7474/db/data/node/595/relationships/in/{-list|&|types}"
}, {
  "outgoing_relationships" : "http://localhost:7474/db/data/node/596/relationships/out",
  "data" : {
    "name" : "19"
  },
  "traverse" : "http://localhost:7474/db/data/node/596/traverse/{returnType}",
  "all_typed_relationships" : "http://localhost:7474/db/data/node/596/relationships/all/{-list|&|types}",
  "property" : "http://localhost:7474/db/data/node/596/properties/{key}",
  "self" : "http://localhost:7474/db/data/node/596",
  "outgoing_typed_relationships" : "http://localhost:7474/db/data/node/596/relationships/out/{-list|&|types}",
  "properties" : "http://localhost:7474/db/data/node/596/properties",
  "incoming_relationships" : "http://localhost:7474/db/data/node/596/relationships/in",
  "extensions" : {
  },
  "create_relationship" : "http://localhost:7474/db/data/node/596/relationships",
  "paged_traverse" : "http://localhost:7474/db/data/node/596/paged/traverse/{returnType}{?pageSize,leaseTime}",
  "all_relationships" : "http://localhost:7474/db/data/node/596/relationships/all",
  "incoming_typed_relationships" : "http://localhost:7474/db/data/node/596/relationships/in/{-list|&|types}"
}, {
  "outgoing_relationships" : "http://localhost:7474/db/data/node/598/relationships/out",
  "data" : {
    "name" : "21"
  },
  "traverse" : "http://localhost:7474/db/data/node/598/traverse/{returnType}",
  "all_typed_relationships" : "http://localhost:7474/db/data/node/598/relationships/all/{-list|&|types}",
  "property" : "http://localhost:7474/db/data/node/598/properties/{key}",
  "self" : "http://localhost:7474/db/data/node/598",
  "outgoing_typed_relationships" : "http://localhost:7474/db/data/node/598/relationships/out/{-list|&|types}",
  "properties" : "http://localhost:7474/db/data/node/598/properties",
  "incoming_relationships" : "http://localhost:7474/db/data/node/598/relationships/in",
  "extensions" : {
  },
  "create_relationship" : "http://localhost:7474/db/data/node/598/relationships",
  "paged_traverse" : "http://localhost:7474/db/data/node/598/paged/traverse/{returnType}{?pageSize,leaseTime}",
  "all_relationships" : "http://localhost:7474/db/data/node/598/relationships/all",
  "incoming_typed_relationships" : "http://localhost:7474/db/data/node/598/relationships/in/{-list|&|types}"
}, {
  "outgoing_relationships" : "http://localhost:7474/db/data/node/608/relationships/out",
  "data" : {
    "name" : "31"
  },
  "traverse" : "http://localhost:7474/db/data/node/608/traverse/{returnType}",
  "all_typed_relationships" : "http://localhost:7474/db/data/node/608/relationships/all/{-list|&|types}",
  "property" : "http://localhost:7474/db/data/node/608/properties/{key}",
  "self" : "http://localhost:7474/db/data/node/608",
  "outgoing_typed_relationships" : "http://localhost:7474/db/data/node/608/relationships/out/{-list|&|types}",
  "properties" : "http://localhost:7474/db/data/node/608/properties",
  "incoming_relationships" : "http://localhost:7474/db/data/node/608/relationships/in",
  "extensions" : {
  },
  "create_relationship" : "http://localhost:7474/db/data/node/608/relationships",
  "paged_traverse" : "http://localhost:7474/db/data/node/608/paged/traverse/{returnType}{?pageSize,leaseTime}",
  "all_relationships" : "http://localhost:7474/db/data/node/608/relationships/all",
  "incoming_typed_relationships" : "http://localhost:7474/db/data/node/608/relationships/in/{-list|&|types}"
} ]










