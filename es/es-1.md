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