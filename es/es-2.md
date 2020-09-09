GET _search
{
  "query": {
    "match_all": {}
  }
}


PUT matchtest
{
  
}

PUT matchtest/_mapping
{
  "properties":{
    "age":{
      "type":"integer"
    },
    "hobbies":{
      "type":"text"
    },
    "name":{
      "type":"keyword"
    }
  }
}

GET matchtest/_mapping

GET matchtest/_search

PUT matchtest/_doc/2
{
  "name":"Tom",
  "age":12,
  "hobbies":"swimming, football"
}

# 简单的查询

GET matchtest/_search
{
  "query": {
    "match": {
      "hobbies": "football basketball"
    }
  }
}

# 简单的查询默认的operator 是or, 改成and试一下, 表示即需要football 也需要 basketball 词
GET matchtest/_search
{
  "query": {
    "match": {
      "hobbies": {
        "query": "football basketball",
        "operator": "and"
      }
    }
  }
}

# lenient 默认为false， 设置为true 会忽略类型不匹配的报错



# 该条查询会报错
GET matchtest/_search
{
  "query": {
    "match": {
      "age": {
        "query": "test"
      }
    }
  }
}

# 该条查询不会报错
GET matchtest/_search
{
  "query": {
    "match": {
      "age": {
        "query": "test",
        "lenient": "true"
      }
    }
  }
}

# fuziness 模糊匹配 ,AUTO:3,6,  [0,2]长度单词精确匹配, [3,5]接受最大编辑距离为1, [6,+无穷]接受最大编辑距离为2

# 会匹配到 最大编辑距离为2
GET matchtest/_search
{
  "query": {
    "match": {
      "hobbies": {
        "query": "footba22",
        "fuzziness": "AUTO"
      }
    }
  }
}

GET matchtest/_search
{
  "query": {
    "match": {
      "hobbies": {
        "query": "foatba2l",
        "fuzziness": "AUTO"
      }
    }
  }
}

# 会匹配不到 c编辑距离为3
GET matchtest/_search
{
  "query": {
    "match": {
      "hobbies": {
        "query": "footb222",
        "fuzziness": "AUTO"
      }
    }
  }
}

# 会匹配不到 c编辑距离为3
GET matchtest/_search
{
  "query": {
    "match": {
      "hobbies": {
        "query": "footb222",
        "fuzziness": 2
      }
    }
  }
}

# prefix_length 代表不能被模糊化的前多少个字符

# 此例查询不到结果 因为前三位字符限制无法模糊匹配
GET matchtest/_search
{
  "query": {
    "match": {
      "hobbies": {
        "query": "foatball",
        "fuzziness": "AUTO",
        "prefix_length": 3
      }
    }
  }
}
