
作用：分布式搜索引擎，

# es中的概念：
Document: 文档，一条数据叫做一个document，类似于MySQL中的row概念。
Field：字段，对应于MySQL中的column 概念，一条数据中有多个Field。
index：索引，对应于MySQL中table的概念，相同数据结构的数据属于同一个index。

# es运行原理
es解决的是MySQL中模糊搜索时会全表搜索的问题，比如select * from table_a where title like %hello%，mysql会遍历每条数据。
es会创建一个倒排索引，会把一个语句分词器分成多个词条（Term），比如“我是中国人”分解成“我”，“中国”，“中国人”等几个词条。检索时先按照词条检索对应的文档id，再根据文档id检索出文档数据。

# 相关性算法
BM25算法：