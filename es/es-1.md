## 概念
基本概念没啥好说的，找个mysql 的概念对比方便对照理解

mysql 的 database   ----->   ES 的 index  即 索引

mysql 的 table      ----->  ES 的 type  即 类型

mysql 的 row     ---- >   ES 的 document 即 文档 

mysql 的 Column  ----->  ES 的 FIELD

mysql 的 schema(数据库的组织和结构) ------>  ES 的mapping

mysql 的 sql   ------>   ES的query  DSL (domain specific language)

> 从以上概念可以看出, es的索引就是mysql 的库的概念， type 类型对应的是 mysql 的表的概念
>
> 一个mysql库可以有多张表， 一个es索引可以有多个类型
>
> mysql 依赖于索引来优化查询， es 所有的文档都是被索引的 


## 概念上的区别
### type 和 mysql table 的区别 
 - 不同type 的同名字段需要保持一致
    > 一个index 下不同type的两个同名字段的类型和配置必须相同
- 只在某个type存在的字段， 在没有该字段的type也会消耗资源
- 得分是由index内的统计数据决定的， 一个type的文档的得分会影响另一个type文档的得分

> 这边看来， 一个index 也有一些和 mysql table 的共同之处 

## index 和 type 的理解
### index
index 就是一个db， 是有分片的，分片 (shards) , 是es 存储数据的最小单元块， 对于有多个分片的一个index，查询的结果就是在多个分片上查询结果的聚合。
如果index 分片太多， 会导致合并分片的结果的开销过大， 如果查询涉及到m个index 每个index 都有n个分片，每次查询都要合并m*n 个分片的结果，如果滥用index，并且充斥着大量多index 的查询的业务，会有很大的内存和cpu消耗于结果的聚合

 ### type
 type 允许在一个index存储多个类型的数据， 搜索一个index下的多个type 和 只搜索一个type的开销是一样的，因为需要合并的分片数量是一样的
 但 type 的使用也有缺点， 如上:
 
    - 不同type 的同名字段需要保持一致
        > 一个index 下不同type的两个同名字段的类型和配置必须相同
    - 只在某个type存在的字段， 在没有该字段的type也会消耗资源
    - 得分是由index内的统计数据决定的， 一个type的文档的得分会影响另一个type文档的得分
    
 建议只有同一个index 的type都具有相同映射，才应该使用type， 否则应该少用type
 
 ### 选择index 还是type
 - 是否需要父子文档？ 如果需要，需要在一个index 建立多个type
 - 文档的映射是否相似？ 不相似，则使用多个index
 
 
 ### es7 的更新
 es7规定了一个index只能有一个type, 且名字只能叫_doc, 虚浮了