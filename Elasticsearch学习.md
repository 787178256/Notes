## Elasticsearch

------

### **基本概念**：

- **集群**：可以有多个节点，同一集群中所有节点的cluster_name都是一样的，都是集群的名字，每个节点自己的名字可以不一样
- **节点**：每个节点都是独立的服务
- **索引**：含有相同属性的文档集合(类比为一张数据库)，索引需要英文且为小写
- **类型**：索引可以定义一个或多个类型，文档必须属于一个类型(类比为一张表)
- **文档**：文档是可以被索引的基本数据单位(类比为一条记录)
- **分片**：每个索引都有多个分片，每个分片是一个Lucene索引，默认5个分片
- **备份**：拷贝一份分片就完成了分片的备份，默认1个备份

### **基本用法**：

- RESTFul API:http://<ip>:<port>/<索引>/<类型>/<文档id>

- 常用HTTP动词：GET/PUT/POST/DELETE

- 创建索引

- ```
  PUT /people?pretty
  {
  	"settings": {
  		"number_of_shards": 3,
  		"number_of_replicas": 1
  	},
  	"mappings": {
  		"man": {
  			"properties": {
  				"name": {
  					"type": "text"
  				},
  				"country": {
  					"type": "keyword"
  				},
  				"age": {
  					"type": "integer"
  				},
  				"date": {
  					"type": "date",
  					"format": "yyyy-MM-dd HH:mm:ss||epoch_millis||yyyy-MM-dd"
  				}
  			}
  		}
  	}
  }
  ```

- 插入数据

  ```
  PUT /people/man/1?pretty
  {
    "name": "kimvra",
    "country": "China",
    "age": 24,
    "date": "1995-12-20 13:59:00"
  }
  response：
  {
    "_index" : "people",
    "_type" : "man",
    "_id" : "1",
    "_version" : 1,
    "result" : "created",
    "_shards" : {
      "total" : 2,
      "successful" : 2,
      "failed" : 0
    },
    "_seq_no" : 0,
    "_primary_term" : 4
  }
  ```

- 查询数据

  ```
  GET /people/man/1?pretty
  返回：
  {
    "_index" : "people",
    "_type" : "man",
    "_id" : "1",
    "_version" : 1,
    "found" : true,
    "_source" : {
      "name" : "kimvra",
      "country" : "China",
      "age" : 24,
      "date" : "1995-12-20 13:59:00"
    }
  }
  
  GET /book/_search
  {
    "query": {
      "match_all": {}
    }
  }
  
  GET /book/_search
  {
    "query": {
      "match": {
        "title": "Elasticsearch"
      }
    },
    "sort": [
      {
      	"publish_date":
      		{
      		"order": "desc"
      		}
      }
    ]
  }
  
  GET /book/_search
  {
    "aggs": {
      "group_by_word_count": {
        "terms": {
          "field": "word_count",
          "size": 10
        }
      },
      "group_by_publish_date": {
        "terms": {
          "field": "publish_date",
          "size": 10
        }
      }
    }
  }
  
  GET /book/_search
  {
    "aggs": {
      "grades_word_count": {
        "stats": {
          "field": "word_count"
        }
      }
    }
  }
  ```

- 修改数据

- ```
  POST /people/man/1/_update
  {
    "doc": {
    		"name": "simba"
  	}
  }
  ```

- 删除文档

  ```
  DELETE /people/man/1?pretty
  ```

- 删除索引

- ```
  DELETE /people
  ```

- Query

  - 子条件查询
    - Query Context：在查询过程中，除了判断文档是否满足查询条件之外，ES还会计算一个**_score**来标识匹配的程度，旨在判断目标文档和查询条件匹配的有多好
      - 全文本查询：针对文本类型数据
      - 字段级别查询：针对结构化数据，如数字、日期等
    - 与Filter Context：在查询过程中，只判断该文档是否满足条件，只有yes或者no
  - 复合条件查询