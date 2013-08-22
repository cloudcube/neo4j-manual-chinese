目录
18.1. Service root
18.2. Streaming
18.3. Cypher queries
18.4. Nodes
18.5. Relationships
18.6. Relationship types
18.7. Node properties
18.8. Relationship properties
18.9. Indexes
18.10. Unique Indexes
18.11. Automatic Indexes
18.12. Configurable Automatic Indexing
18.13. Traversals
18.14. Built-in Graph Algorithms
18.15. Batch operations
18.16. WADL Support
18.17. Cypher Plugin
18.18. Gremlin Plugin
Neo4j REST API 被设计的很合理，因此，你可以使用请求开始在“18.1. Service root“并且通过这个发现的URL去执行其他请求。
在下面的例子中使用了URIs；
他们在未来可能发生变化，因此在未来验证发现的可用的URIs,而不是依赖当前的设计。默认的表示是json,同时也可使用POST/PUT对响应和数据的传输。
下面是使用REST API互动的清单列表，对编程语言绑定到REST API，查看"Chapter 5, Neo4j Remote Client Libraries."
有效的使用JSON接口，你必须为这些带数据的响应的请求明确的设置请求头Accept:application/json，如果你请求传输数据,你应该设置头Content-Type:application/json,例如当你创建一个关系，又例如包括相关请求和响应头。
neo4j 服务器支持结果文件流，这种方式可以很大程度上获得更好的性能和更低的内存消耗。查看
Section 18.2, “Streaming” 获得更多的信息。



