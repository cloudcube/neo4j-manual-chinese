18.5.1. Get Relationship by ID
18.5.2. Create relationship
18.5.3. Create a relationship with properties
18.5.4. Delete relationship
18.5.5. Get all properties on a relationship
18.5.6. Set all properties on a relationship
18.5.7. Get single property on a relationship
18.5.8. Set single property on a relationship
18.5.9. Get all relationships
18.5.10. Get incoming relationships
18.5.11. Get outgoing relationships
18.5.12. Get typed relationships
18.5.13. Get relationships on a node without relationships
关系在Neo4j REST API中是头等公民。他们能够即可单独访问或者通过他们所连接的节点进行访问。
一般的通过节点获取关系的模式：
GET http://localhost:7474/db/data/node/123/relationships/{dir}/{-list|&|types}
在all,in,out和type中的一个dir是一个被符号分割的类型列表，具体详情请看下面示例

18.5.1. 通过ID获取关系
请求示例
GET http://localhost:7474/db/data/relationship/1
Accept: application/json
响应示例
200: OK
Content-Type: application/json
{
  "extensions" : {
  },
  "start" : "http://localhost:7474/db/data/node/13",
  "property" : "http://localhost:7474/db/data/relationship/1/properties/{key}",
  "self" : "http://localhost:7474/db/data/relationship/1",
  "properties" : "http://localhost:7474/db/data/relationship/1/properties",
  "type" : "know",
  "end" : "http://localhost:7474/db/data/node/12",
  "data" : {
  }
}
18.5.2. 创建关系
在成功创建关系后，新的创建的关系被返回
请求示例
POST http://localhost:7474/db/data/node/89/relationships
Accept: application/json
Content-Type: application/json
{
  "to" : "http://localhost:7474/db/data/node/88",
  "type" : "LOVES"
}
响应示例
201: Created
Content-Type: application/json
Location: http://localhost:7474/db/data/relationship/70
{
  "extensions" : {
  },
  "start" : "http://localhost:7474/db/data/node/89",
  "property" : "http://localhost:7474/db/data/relationship/70/properties/{key}",
  "self" : "http://localhost:7474/db/data/relationship/70",
  "properties" : "http://localhost:7474/db/data/relationship/70/properties",
  "type" : "LOVES",
  "end" : "http://localhost:7474/db/data/node/88",
  "data" : {
  }
}
18.5.3. 创建带有属性的关系
在成功创建关系后，新的创建的关系被返回。
请求示例
POST http://localhost:7474/db/data/node/91/relationships
Accept: application/json
Content-Type: application/json
{
  "to" : "http://localhost:7474/db/data/node/90",
  "type" : "LOVES",
  "data" : {
    "foo" : "bar"
  }
}
响应示例
201: Created
Content-Type: application/json
Location: http://localhost:7474/db/data/relationship/72
{
  "extensions" : {
  },
  "start" : "http://localhost:7474/db/data/node/91",
  "property" : "http://localhost:7474/db/data/relationship/72/properties/{key}",
  "self" : "http://localhost:7474/db/data/relationship/72",
  "properties" : "http://localhost:7474/db/data/relationship/72/properties",
  "type" : "LOVES",
  "end" : "http://localhost:7474/db/data/node/90",
  "data" : {
    "foo" : "bar"
  }
}
18.5.4. 删除关系
请求示例
DELETE http://localhost:7474/db/data/relationship/7
Accept: application/json
响应示例
204: No Content
18.5.5. 获取一个关系的所有属性
请求示例
GET http://localhost:7474/db/data/relationship/11/properties
Accept: application/json
响应示例
200: OK
Content-Type: application/json
{
  "since" : "1day",
  "cost" : "high"
}
18.5.6. 设置一个关系的所有属性
请求示例
PUT http://localhost:7474/db/data/relationship/10/properties
Accept: application/json
Content-Type: application/json
{
  "happy" : false
}
响应示例
204: No Content
18.5.7. 获取一个关系的单独属性
请求示例
GET http://localhost:7474/db/data/relationship/8/properties/cost
Accept: application/json
响应示例
200: OK
Content-Type: application/json
"high"
18.5.8. 设置一个关系的单独属性
请求示例
PUT http://localhost:7474/db/data/relationship/9/properties/cost
Accept: application/json
Content-Type: application/json
"deadly"
响应示例
204: No Content
18.5.9. 获取所有关系
请求示例
GET http://localhost:7474/db/data/node/80/relationships/all
Accept: application/json
响应示例
200: OK
Content-Type: application/json
[ {
  "start" : "http://localhost:7474/db/data/node/80",
  "data" : {
  },
  "self" : "http://localhost:7474/db/data/relationship/38",
  "property" : "http://localhost:7474/db/data/relationship/38/properties/{key}",
  "properties" : "http://localhost:7474/db/data/relationship/38/properties",
  "type" : "LIKES",
  "extensions" : {
  },
  "end" : "http://localhost:7474/db/data/node/81"
}, {
  "start" : "http://localhost:7474/db/data/node/82",
  "data" : {
  },
  "self" : "http://localhost:7474/db/data/relationship/39",
  "property" : "http://localhost:7474/db/data/relationship/39/properties/{key}",
  "properties" : "http://localhost:7474/db/data/relationship/39/properties",
  "type" : "LIKES",
  "extensions" : {
  },
  "end" : "http://localhost:7474/db/data/node/80"
}, {
  "start" : "http://localhost:7474/db/data/node/80",
  "data" : {
  },
  "self" : "http://localhost:7474/db/data/relationship/40",
  "property" : "http://localhost:7474/db/data/relationship/40/properties/{key}",
  "properties" : "http://localhost:7474/db/data/relationship/40/properties",
  "type" : "HATES",
  "extensions" : {
  },
  "end" : "http://localhost:7474/db/data/node/83"
} ]
18.5.10. 获取传入关系

请求示例
GET http://localhost:7474/db/data/node/90/relationships/in
Accept: application/json
响应示例
200: OK
Content-Type: application/json
[ {
  "start" : "http://localhost:7474/db/data/node/92",
  "data" : {
  },
  "self" : "http://localhost:7474/db/data/relationship/45",
  "property" : "http://localhost:7474/db/data/relationship/45/properties/{key}",
  "properties" : "http://localhost:7474/db/data/relationship/45/properties",
  "type" : "LIKES",
  "extensions" : {
  },
  "end" : "http://localhost:7474/db/data/node/90"
} ]
18.5.11. 获取传出关系
请求示例
GET http://localhost:7474/db/data/node/95/relationships/out
Accept: application/json
响应示例
200: OK
Content-Type: application/json
[ {
  "start" : "http://localhost:7474/db/data/node/95",
  "data" : {
  },
  "self" : "http://localhost:7474/db/data/relationship/47",
  "property" : "http://localhost:7474/db/data/relationship/47/properties/{key}",
  "properties" : "http://localhost:7474/db/data/relationship/47/properties",
  "type" : "LIKES",
  "extensions" : {
  },
  "end" : "http://localhost:7474/db/data/node/96"
}, {
  "start" : "http://localhost:7474/db/data/node/95",
  "data" : {
  },
  "self" : "http://localhost:7474/db/data/relationship/49",
  "property" : "http://localhost:7474/db/data/relationship/49/properties/{key}",
  "properties" : "http://localhost:7474/db/data/relationship/49/properties",
  "type" : "HATES",
  "extensions" : {
  },
  "end" : "http://localhost:7474/db/data/node/98"
} ]
18.5.12. 获取类型化的关系
注意，在从终端使用cURL的时候，“&”需要向“%26”的编码
请求示例
GET http://localhost:7474/db/data/node/100/relationships/all/LIKES&HATES
Accept: application/json
响应示例
200: OK
Content-Type: application/json
[ {
  "start" : "http://localhost:7474/db/data/node/100",
  "data" : {
  },
  "self" : "http://localhost:7474/db/data/relationship/50",
  "property" : "http://localhost:7474/db/data/relationship/50/properties/{key}",
  "properties" : "http://localhost:7474/db/data/relationship/50/properties",
  "type" : "LIKES",
  "extensions" : {
  },
  "end" : "http://localhost:7474/db/data/node/101"
}, {
  "start" : "http://localhost:7474/db/data/node/102",
  "data" : {
  },
  "self" : "http://localhost:7474/db/data/relationship/51",
  "property" : "http://localhost:7474/db/data/relationship/51/properties/{key}",
  "properties" : "http://localhost:7474/db/data/relationship/51/properties",
  "type" : "LIKES",
  "extensions" : {
  },
  "end" : "http://localhost:7474/db/data/node/100"
}, {
  "start" : "http://localhost:7474/db/data/node/100",
  "data" : {
  },
  "self" : "http://localhost:7474/db/data/relationship/52",
  "property" : "http://localhost:7474/db/data/relationship/52/properties/{key}",
  "properties" : "http://localhost:7474/db/data/relationship/52/properties",
  "type" : "HATES",
  "extensions" : {
  },
  "end" : "http://localhost:7474/db/data/node/103"
} ]
18.5.13. 获取一个没有关系节点的关系
请求示例
GET http://localhost:7474/db/data/node/119/relationships/all
Accept: application/json
响应示例
200: OK
Content-Type: application/json
[ ]


