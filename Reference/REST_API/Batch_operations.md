批处理操作
========  

18.15.1. [批处理中执行多个操作](#multiexec)  
18.15.2. [在相同的批处理任务中引用先前创建的项目](#prefixprojs)  
18.15.3. [在批处理流中执行多个操作](#execmultiop)  
-------------------
<h2 id="multiexec">18.15.1. 批处理中执行多个操作</h2>  

它允许你通过一个单一的HTTP调用执行多个API的调用，这显著提高了大范围的插入和更新操作的性能。
批处理期望获得一批工作描述作为输入，每个工作描述通过正常服务器的API要执行的一个动作。
这个服务是事务性的。如果任何操作失败（就会返回一个non-2xx HTTP状态代码），事物将回滚并且所有改变将被取消。
每个工作描述应当包含一个to的属性，其值相对于数据API的根（所以http://localhost:7474/db/data/node成为/node),一个包含HTTP的method属性作为动词来使用。

你可能会提供一个body属性，和一个id属性来帮助你跟踪响应，尽管响应被保证被接收到的返回相同的工作描述
下面概括了不同部分的工作描述

请求示例  

```
POST http://localhost:7474/db/data/batch
Accept: application/json
Content-Type: application/json
[ {
  "method" : "PUT",
  "to" : "/node/211/properties",
  "body" : {
    "age" : 1
  },
  "id" : 0
}, {
  "method" : "GET",
  "to" : "/node/211",
  "id" : 1
}, {
  "method" : "POST",
  "to" : "/node",
  "body" : {
    "age" : 1
  },
  "id" : 2
}, {
  "method" : "POST",
  "to" : "/node",
  "body" : {
    "age" : 1
  },
  "id" : 3
} ]  
```  

响应示例  

```
200: OK
Content-Type: application/json
[ {
  "id" : 0,
  "from" : "/node/211/properties"
}, {
  "id" : 1,
  "body" : {
    "extensions" : {
    },
    "paged_traverse" : "http://localhost:7474/db/data/node/211/paged/traverse/{returnType}{?pageSize,leaseTime}",
    "outgoing_relationships" : "http://localhost:7474/db/data/node/211/relationships/out",
    "traverse" : "http://localhost:7474/db/data/node/211/traverse/{returnType}",
    "all_typed_relationships" : "http://localhost:7474/db/data/node/211/relationships/all/{-list|&|types}",
    "property" : "http://localhost:7474/db/data/node/211/properties/{key}",
    "all_relationships" : "http://localhost:7474/db/data/node/211/relationships/all",
    "self" : "http://localhost:7474/db/data/node/211",
    "outgoing_typed_relationships" : "http://localhost:7474/db/data/node/211/relationships/out/{-list|&|types}",
    "properties" : "http://localhost:7474/db/data/node/211/properties",
    "incoming_relationships" : "http://localhost:7474/db/data/node/211/relationships/in",
    "incoming_typed_relationships" : "http://localhost:7474/db/data/node/211/relationships/in/{-list|&|types}",
    "create_relationship" : "http://localhost:7474/db/data/node/211/relationships",
    "data" : {
      "age" : 1
    }
  },
  "from" : "/node/211"
}, {
  "id" : 2,
  "location" : "http://localhost:7474/db/data/node/212",
  "body" : {
    "extensions" : {
    },
    "paged_traverse" : "http://localhost:7474/db/data/node/212/paged/traverse/{returnType}{?pageSize,leaseTime}",
    "outgoing_relationships" : "http://localhost:7474/db/data/node/212/relationships/out",
    "traverse" : "http://localhost:7474/db/data/node/212/traverse/{returnType}",
    "all_typed_relationships" : "http://localhost:7474/db/data/node/212/relationships/all/{-list|&|types}",
    "property" : "http://localhost:7474/db/data/node/212/properties/{key}",
    "all_relationships" : "http://localhost:7474/db/data/node/212/relationships/all",
    "self" : "http://localhost:7474/db/data/node/212",
    "outgoing_typed_relationships" : "http://localhost:7474/db/data/node/212/relationships/out/{-list|&|types}",
    "properties" : "http://localhost:7474/db/data/node/212/properties",
    "incoming_relationships" : "http://localhost:7474/db/data/node/212/relationships/in",
    "incoming_typed_relationships" : "http://localhost:7474/db/data/node/212/relationships/in/{-list|&|types}",
    "create_relationship" : "http://localhost:7474/db/data/node/212/relationships",
    "data" : {
      "age" : 1
    }
  },
  "from" : "/node"
}, {
  "id" : 3,
  "location" : "http://localhost:7474/db/data/node/213",
  "body" : {
    "extensions" : {
    },
    "paged_traverse" : "http://localhost:7474/db/data/node/213/paged/traverse/{returnType}{?pageSize,leaseTime}",
    "outgoing_relationships" : "http://localhost:7474/db/data/node/213/relationships/out",
    "traverse" : "http://localhost:7474/db/data/node/213/traverse/{returnType}",
    "all_typed_relationships" : "http://localhost:7474/db/data/node/213/relationships/all/{-list|&|types}",
    "property" : "http://localhost:7474/db/data/node/213/properties/{key}",
    "all_relationships" : "http://localhost:7474/db/data/node/213/relationships/all",
    "self" : "http://localhost:7474/db/data/node/213",
    "outgoing_typed_relationships" : "http://localhost:7474/db/data/node/213/relationships/out/{-list|&|types}",
    "properties" : "http://localhost:7474/db/data/node/213/properties",
    "incoming_relationships" : "http://localhost:7474/db/data/node/213/relationships/in",
    "incoming_typed_relationships" : "http://localhost:7474/db/data/node/213/relationships/in/{-list|&|types}",
    "create_relationship" : "http://localhost:7474/db/data/node/213/relationships",
    "data" : {
      "age" : 1
    }
  },
  "from" : "/node"
} ]  
```  

<h2 id="prefixprojs>18.15.2. 在相同的批处理任务中引用先前创建的项目</h2>  
  

批量操作API允许你引用从随后的工作描述中创建资源返回的URI，在相同的批处理调用。  
  
在接下来的工作中，使用{[JOB ID]}特殊语法注入到URIs从被创建到JSON字符串中的资源  

请求示例  

```
POST http://localhost:7474/db/data/batch
Accept: application/json
Content-Type: application/json
[ {
  "method" : "POST",
  "to" : "/node",
  "id" : 0,
  "body" : {
    "name" : "bob"
  }
}, {
  "method" : "POST",
  "to" : "/node",
  "id" : 1,
  "body" : {
    "age" : 12
  }
}, {
  "method" : "POST",
  "to" : "{0}/relationships",
  "id" : 3,
  "body" : {
    "to" : "{1}",
    "data" : {
      "since" : "2010"
    },
    "type" : "KNOWS"
  }
}, {
  "method" : "POST",
  "to" : "/index/relationship/my_rels",
  "id" : 4,
  "body" : {
    "key" : "since",
    "value" : "2010",
    "uri" : "{3}"
  }
} ]  
```

响应示例  

```
200: OK
Content-Type: application/json
[ {
  "id" : 0,
  "location" : "http://localhost:7474/db/data/node/214",
  "body" : {
    "extensions" : {
    },
    "paged_traverse" : "http://localhost:7474/db/data/node/214/paged/traverse/{returnType}{?pageSize,leaseTime}",
    "outgoing_relationships" : "http://localhost:7474/db/data/node/214/relationships/out",
    "traverse" : "http://localhost:7474/db/data/node/214/traverse/{returnType}",
    "all_typed_relationships" : "http://localhost:7474/db/data/node/214/relationships/all/{-list|&|types}",
    "property" : "http://localhost:7474/db/data/node/214/properties/{key}",
    "all_relationships" : "http://localhost:7474/db/data/node/214/relationships/all",
    "self" : "http://localhost:7474/db/data/node/214",
    "outgoing_typed_relationships" : "http://localhost:7474/db/data/node/214/relationships/out/{-list|&|types}",
    "properties" : "http://localhost:7474/db/data/node/214/properties",
    "incoming_relationships" : "http://localhost:7474/db/data/node/214/relationships/in",
    "incoming_typed_relationships" : "http://localhost:7474/db/data/node/214/relationships/in/{-list|&|types}",
    "create_relationship" : "http://localhost:7474/db/data/node/214/relationships",
    "data" : {
      "name" : "bob"
    }
  },
  "from" : "/node"
}, {
  "id" : 1,
  "location" : "http://localhost:7474/db/data/node/215",
  "body" : {
    "extensions" : {
    },
    "paged_traverse" : "http://localhost:7474/db/data/node/215/paged/traverse/{returnType}{?pageSize,leaseTime}",
    "outgoing_relationships" : "http://localhost:7474/db/data/node/215/relationships/out",
    "traverse" : "http://localhost:7474/db/data/node/215/traverse/{returnType}",
    "all_typed_relationships" : "http://localhost:7474/db/data/node/215/relationships/all/{-list|&|types}",
    "property" : "http://localhost:7474/db/data/node/215/properties/{key}",
    "all_relationships" : "http://localhost:7474/db/data/node/215/relationships/all",
    "self" : "http://localhost:7474/db/data/node/215",
    "outgoing_typed_relationships" : "http://localhost:7474/db/data/node/215/relationships/out/{-list|&|types}",
    "properties" : "http://localhost:7474/db/data/node/215/properties",
    "incoming_relationships" : "http://localhost:7474/db/data/node/215/relationships/in",
    "incoming_typed_relationships" : "http://localhost:7474/db/data/node/215/relationships/in/{-list|&|types}",
    "create_relationship" : "http://localhost:7474/db/data/node/215/relationships",
    "data" : {
      "age" : 12
    }
  },
  "from" : "/node"
}, {
  "id" : 3,
  "location" : "http://localhost:7474/db/data/relationship/119",
  "body" : {
    "extensions" : {
    },
    "start" : "http://localhost:7474/db/data/node/214",
    "property" : "http://localhost:7474/db/data/relationship/119/properties/{key}",
    "self" : "http://localhost:7474/db/data/relationship/119",
    "properties" : "http://localhost:7474/db/data/relationship/119/properties",
    "type" : "KNOWS",
    "end" : "http://localhost:7474/db/data/node/215",
    "data" : {
      "since" : "2010"
    }
  },
  "from" : "http://localhost:7474/db/data/node/214/relationships"
}, {
  "id" : 4,
  "location" : "http://localhost:7474/db/data/index/relationship/my_rels/since/2010/119",
  "body" : {
    "extensions" : {
    },
    "start" : "http://localhost:7474/db/data/node/214",
    "property" : "http://localhost:7474/db/data/relationship/119/properties/{key}",
    "self" : "http://localhost:7474/db/data/relationship/119",
    "properties" : "http://localhost:7474/db/data/relationship/119/properties",
    "type" : "KNOWS",
    "end" : "http://localhost:7474/db/data/node/215",
    "data" : {
      "since" : "2010"
    },
    "indexed" : "http://localhost:7474/db/data/index/relationship/my_rels/since/2010/119"
  },
  "from" : "/index/relationship/my_rels"
} ]  
```  


<h2 id="#execmultiop">18.15.3. 在批处理流中执行多个操作</h2>  

请求示例  

```
POST http://localhost:7474/db/data/batch
Accept: application/json
Content-Type: application/json
X-Stream: true
[ {
  "method" : "PUT",
  "to" : "/node/69/properties",
  "body" : {
    "age" : 1
  },
  "id" : 0
}, {
  "method" : "GET",
  "to" : "/node/69",
  "id" : 1
}, {
  "method" : "POST",
  "to" : "/node",
  "body" : {
    "age" : 1
  },
  "id" : 2
}, {
  "method" : "POST",
  "to" : "/node",
  "body" : {
    "age" : 1
  },
  "id" : 3
} ]  
```

响应示例  

```  
200: OK
Content-Type: application/json
[ {
  "id" : 0,
  "from" : "/node/69/properties",
  "body" : null,
  "status" : 204
}, {
  "id" : 1,
  "from" : "/node/69",
  "body" : {
    "extensions" : {
    },
    "paged_traverse" : "http://localhost:7474/db/data/node/69/paged/traverse/{returnType}{?pageSize,leaseTime}",
    "outgoing_relationships" : "http://localhost:7474/db/data/node/69/relationships/out",
    "traverse" : "http://localhost:7474/db/data/node/69/traverse/{returnType}",
    "all_typed_relationships" : "http://localhost:7474/db/data/node/69/relationships/all/{-list|&|types}",
    "property" : "http://localhost:7474/db/data/node/69/properties/{key}",
    "all_relationships" : "http://localhost:7474/db/data/node/69/relationships/all",
    "self" : "http://localhost:7474/db/data/node/69",
    "outgoing_typed_relationships" : "http://localhost:7474/db/data/node/69/relationships/out/{-list|&|types}",
    "properties" : "http://localhost:7474/db/data/node/69/properties",
    "incoming_relationships" : "http://localhost:7474/db/data/node/69/relationships/in",
    "incoming_typed_relationships" : "http://localhost:7474/db/data/node/69/relationships/in/{-list|&|types}",
    "create_relationship" : "http://localhost:7474/db/data/node/69/relationships",
    "data" : {
      "age" : 1
    }
  },
  "status" : 200
}, {
  "id" : 2,
  "from" : "/node",
  "body" : {
    "extensions" : {
    },
    "paged_traverse" : "http://localhost:7474/db/data/node/70/paged/traverse/{returnType}{?pageSize,leaseTime}",
    "outgoing_relationships" : "http://localhost:7474/db/data/node/70/relationships/out",
    "traverse" : "http://localhost:7474/db/data/node/70/traverse/{returnType}",
    "all_typed_relationships" : "http://localhost:7474/db/data/node/70/relationships/all/{-list|&|types}",
    "property" : "http://localhost:7474/db/data/node/70/properties/{key}",
    "all_relationships" : "http://localhost:7474/db/data/node/70/relationships/all",
    "self" : "http://localhost:7474/db/data/node/70",
    "outgoing_typed_relationships" : "http://localhost:7474/db/data/node/70/relationships/out/{-list|&|types}",
    "properties" : "http://localhost:7474/db/data/node/70/properties",
    "incoming_relationships" : "http://localhost:7474/db/data/node/70/relationships/in",
    "incoming_typed_relationships" : "http://localhost:7474/db/data/node/70/relationships/in/{-list|&|types}",
    "create_relationship" : "http://localhost:7474/db/data/node/70/relationships",
    "data" : {
      "age" : 1
    }
  },
  "location" : "http://localhost:7474/db/data/node/70",
  "status" : 201
}, {
  "id" : 3,
  "from" : "/node",
  "body" : {
    "extensions" : {
    },
    "paged_traverse" : "http://localhost:7474/db/data/node/71/paged/traverse/{returnType}{?pageSize,leaseTime}",
    "outgoing_relationships" : "http://localhost:7474/db/data/node/71/relationships/out",
    "traverse" : "http://localhost:7474/db/data/node/71/traverse/{returnType}",
    "all_typed_relationships" : "http://localhost:7474/db/data/node/71/relationships/all/{-list|&|types}",
    "property" : "http://localhost:7474/db/data/node/71/properties/{key}",
    "all_relationships" : "http://localhost:7474/db/data/node/71/relationships/all",
    "self" : "http://localhost:7474/db/data/node/71",
    "outgoing_typed_relationships" : "http://localhost:7474/db/data/node/71/relationships/out/{-list|&|types}",
    "properties" : "http://localhost:7474/db/data/node/71/properties",
    "incoming_relationships" : "http://localhost:7474/db/data/node/71/relationships/in",
    "incoming_typed_relationships" : "http://localhost:7474/db/data/node/71/relationships/in/{-list|&|types}",
    "create_relationship" : "http://localhost:7474/db/data/node/71/relationships",
    "data" : {
      "age" : 1
    }
  },
  "location" : "http://localhost:7474/db/data/node/71",
  "status" : 201
} ]  
```







