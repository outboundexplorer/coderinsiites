GET _search
{
  "query": {
    "match_all": {}
  }
}

GET /company/employee/_mget
{
  "ids": [
    "1",
    "2"
  ]
}

POST /_bulk
{ "create": { "_index":"company","_type":"employee","_id":3}}
{ "first_name" : "Mathew", "last_name":"Lamb"}
{ "index": { "_index":"company","_type":"employee"}}
{ "first_name" : "Paul", "last_name":"Liu" }
{ "update" : { "_index": "company", "_type": "employee", "_id": 3, "_retry_on_conflict" : 3 }}
{ "doc" : { "first_name": "Terry", "last_name" : "Daniels" }}
  
GET /company/employee/_search

GET /company/_search?from=10&size=5

GET /company/_mapping/employee

GET _analyze?analyzer=standard
{
  "":"Text to analyze"
}

PUT /gb2
{
  "mappings" : {
    "tweet" : {
      "properties": {
        "tweet" : {
          "type" : "string"
        },
        "date" : {
          "type" : "date" 
        },
        "name" : {
          "type" : "string"
        },
        "user_id" : {
          "type" : "long"
        }
      }
    }
  }
}

PUT /gb2
{
  "mappings" : {
    "tweet" : {
      "properties": {
        "tweet" : {
          "type" : "string"
        },
        "date" : {
          "type" : "date" 
        },
        "name" : {
          "type" : "string"
        },
        "user_id" : {
          "type" : "long"
        }
      }
    }
  }
}

POST /gb2/tweet
{
  "tweeet" : "This one is about rock",
  "date" : "2015-05-01",
  "name" : "Fred Kacks",
  "user_id" : 1
}


GET /gb2/_mapping

PUT /gb/_mapping/tweet
{
  "properties": {
    "tag" : {
      "type" : "string",
      "index" : "not_analyzed"
    }
  }
}

PUT /company/employee/AU493GvrOyoD_e-ojrNO 
{
  "first_name" : "Paul",
  "last_name" : "Liu",
  "age" : 32,
  "colors" : ["blue","red"],
  "hobbies" : ["football","cricket"]
}

GET /gb/_analyze?field=tweet
{
  "" : "Black-cats"
}

GET /gb/_analyze?field=tweet
{
  "" : "Black-cats"
}


GET /company/_search
{
  "from" : 5,
  "size" : 10
}

GET /company/_search
{
  "query" : {
    "match_all" : {}
  }
}

GET /company/employee/_search 
{
  "query" : {
    "bool" : {
      "must" : {
        "match" : {
          "first_name" : "Paul"
        }
      },
      "must_not" : {
        "match" : {
          "last_name" : "Demon"
        }
      },
      "should" : {
        "match" : {
          "hobbies" : "cricket"
        }
      }
    } 
  }
}


GET /company/_search
{
  "query" : {
    "filtered": {
      "query": {
        "match" : {
          "hobbies" : "cricket"
        }
      },
      "filter": {
        "term" : {
          "age" : 32
        }
      }
    }
  }
}


GET /gb2/tweet/_validate/query?explain
{
  "query" : {
    "match" : {
      "tweet" : "rock music"
    }
  }
}

GET /gb2/_search
{
  "query" : {
    "filtered" : {
      "filter" : {
        "term" : {
          "user_id" : 1
        }
      }
    }
  },
  "sort" : {
    "date" : {
      "order" : "desc"
    }
  }
}


GET /gb2/tweet/_search
{
  "query" : {
    "filtered" : {
      "query" : {
        "match" : {
          "tweeet" : "rock"    
        }
      },
      "filter" : {
        "term" : {
          "user_id" : 1
        }
      }
    }
  },
  "sort" : [
    {
      "date" : {
      "order" : "desc"
      }
    },
    {
      "_score" : {
        "order" : "desc"
      }
    }
  ]
}

GET /gb/tweet/_mapping




GET /gb2/tweet/_search
{
  "query" : {
    "filtered" : {
      "query" : {
        "match" : {
          "tweeet" : "rock"
        }
      },
      "filter" : {
        "range" : {
          "date" : {
            "gte" : "2015-01-01"
          }
        }
      }
    }
  }
}


GET /gb2/tweet/_search?explain
{
  "query" : {
    "match" : {
        "tweeet" : "rock"
    }
  }
}


GET /gb2/tweet/_search


GET /gb2/tweet/_search
{
  "query" : {
    "filtered" : {
      "query" : {
        "match" : {
          "tweeet" : "rock"
        }
      },
      "filter" : {
        "bool" : {
          "must" : [
            {
              "range" : {
                "date" : {
                  "gte" : "2015-01-01",
                  "lte" : "2015-06-01"
                }
              }
            },
            {
              "term" : {
                "user_id" : 1
              }
            }
          ]
        }
      }
    }
  }
}


PUT /my_index
{
  "settings" : {
    "analysis": {
      "char_filter": {
        "&_to_and" : {
          "type" : "mapping",
          "mappings" : [ "&=> and "]
        }
      },
      "filter": {
        "my_stopwords" : {
          "type" : "stop",
          "stopwords" : [ "the", "a"]
        }
      },
      "analyzer": {
        "my_analyzer" : {
          "type" : "custom",
          "char_filter" : ["html_strip", "&_to_and"],
          "tokenizer" : "standard",
          "filter" : ["lowercase","my_stopwords"]
        }
      }
    }
  }
}

GET /my_index/_analyze?analyzer=my_analyzer
{
  "" : "<html>The quick & brown fox</html>"
}

POST /my_index/content
{
  "body" : "<html>The quick & brown fox</html>"
}


DELETE /my_index

GET /real_index/_search


POST /my_index/_mapping/content
{
  "_id" : {
    "path" : "doc_id"
  },
  "properties" : {
    "body" : {
      "type": "string",
      "analyzer": "my_analyzer"
    }
  }
}

GET /my_index/_mapping

POST /my_index/content
{
  "doc_id" : 123,
  "body" : "<h1>The last man standing</h1>"
}


PUT /my_index_v1

PUT /my_index/_alias/real_index

GET /*/_alias/real_index

POST /my_store/products/_bulk
{ "index" : { "_id" : 1 }}
{ "price" : 10, "productID" : "XHDK-A-1293-#fJ3" }
{ "index" : { "_id" : 2 }}
{ "price" : 20, "productID" : "KDKE-B-9947-#kL5" }
{ "index" : { "_id" : 3 }}
{ "price" : 30, "productID" : "JODL-X-1937-#pV7" }
{ "index" : { "_id" : 4 }}
{ "price" : 40, "productID" : "QQPX-R-3956-#aD8" }


GET /my_store/products/_search
{
  "query" : {
    "filtered" : {
      "query" : {
        "match_all": {}
      },
      "filter" : {
        "term" : {
          "price" : 20
        }
      }
    }
  }
}


GET /my_store/products/_search
{
  "query" : {
    "filtered" : {
      "filter" : {
        "term": {
          "productID" : "XHDK-A-1293-#fJ3"
        }
      }
    }
  }
}

GET /my_store/_analyze?field=productID
{
  "" : "XHDK-A-1293-#fJ3"
}



DELETE /my_store

PUT /my_store
{
  "mappings" : {
    "products" : {
      "properties" : {
        "productID" : {
          "type" : "string",
          "index" : "not_analyzed"
        }
      }
    }
  }
}




GET /my_store/products/_search
{
  "query" : {
    "filtered" : {
      "filter" : {
        "term": {
          "productID" : "XHDK-A-1293-#fJ3"
        }
      }
    }
  }
}


GET /my_store/products/_search
{
  "query" : {
    "filtered" : {
      "filter" : {
        "bool" : {
          "should" :[
            { "term" : { "price" : 20 }},
            { "term" : { "productID" : "XHDK-A-1293-#fJ3" }}
          ],
          "must_not": [
            { "term" : { "price" : 30 }}
          ]
        }
      }
    }
  }
}


# SELECT document FROM products WHERE productID = ? OR (productID = ? AND price = ?)
GET /my_store/products/_search
{
  "query" : {
    "filtered" : {
      "filter" : {
        "bool" : {
          "should": [
            { "term" : { "productID" : "KDKE-B-9947-#kL5" }},
            { "bool" : {
              "must" : [
                { "term" : { "productID" : "JODL-X-1937-#pV7" }},
                { "term" : { "price" : 30 }}
              ]
            }}
          ]
        }
      }
    }
  }
}

GET /my_store/products/_search
{
  "query" : {
    "filtered": {
      "filter": {
        "terms" : {
          "price" : [20,30]
        }
      }
    }
  }
}

POST /index_01/posts/_bulk
{ "index": {"_id":1}}
{ "tags" : ["search"]}
{ "index": {"_id":2}}
{ "tags" : ["search", "open_source"]}
{ "index": {"_id":3}}
{ "other_field" : "some data" }
{ "index": {"_id":4}}
{ "tags" : null }
{ "index": {"_id":5}}
{ "tags" : ["search", null]}


GET /index_01/posts/_search
{
  "query" :
  {
    "filtered": {
      "filter": {
        "exists" : { "field" : "tags" }
      }
    }
  }
}



GET /index_01/posts/_search
{
  "query" :
  {
    "filtered": {
      "filter": {
        "missing" : { "field" : "tags" }
      }
    }
  }
}

DELETE /index_01

PUT /index_01
{
  "settings" : { 
    "number_of_shards" : 1 
    
  }
}

POST /index_01/my_type/_bulk
{ "index" : { "_id" : 1 }}
{ "title" : "The quick brown fox" }
{ "index" : { "_id" : 2 }}
{ "title" : "The quick brown fox jumps over the lazy dog" }
{ "index" : { "_id" : 3 }}
{ "title" : "The quick brown fox jumps over the quick dog" }
{ "index" : { "_id" : 4 }}
{ "title" : "Brown fox brown dog" }


GET /index_01/my_type/_search
{
  "query" : {
    "match" : {
      "title" : "BROWN DOG!"
    }
  }
}

GET /index_01/my_type/_search
{
  "query" : {
    "match" : {
      "title" : {
        "query" : "BROWN DOG!"
      }
    }
  }
}

GET /index_01/my_type/_search
{
  "query" : {
    "match" : {
      "title" : {
        "query" : "BROWN DOG!",
        "operator" : "and"
      }
    }
  }
}


GET /index_01/my_type/_search
{
  "query" : {
    "match" : {
      "title" : {
        "query" : "quick brown dog lazy",
        "minimum_should_match" : "75%"
      }
    }
  }
}


POST /index_02/articles/
{
    "title" : "My first Insiite article",
    "body" : "There are lots of things in this article that talk about the growth of insiite and how it has come to be the default tool for knowledge management",
    "note" : "See how important initial user growth was at the beginning"
}

POST /index_02/articles/
{
    "title" : "My first article",
    "body" : "There are many ways that we can encourage the growth of indoor plants.  Some of them like being indoors all year.",
    "note" : "We must make sure that we water the plants lots in the beginning"
}

POST /index_02/articles/AU5NTMnZedrrjyELa79l
{
    "title" : "Insiite and me",
    "body" : "This is a short story about how insiite got started and all the important things that we learnt along the way.",
    "note" : "There were lots of difficult things happening at the beginning."
}

POST /index_02/articles/
{
    "title" : "Trying so hard",
    "body" : "It can be a difficult time for youngsters, their bodies are growing so quickly and there are so many things that they are learning for the first time.",
    "note" : "Must remember to help the little ones when they need it at the beginning"
  
}

POST /index_02/articles/
{
    "title" : "School in the beginning",
    "body" : "It can be a difficult time for youngsters, their bodies are growing so quickly and there are so many things that they are learning for the first time.",
    "note" : "The children need a lot of help at the beginning"
  
}

POST /index_02/articles/
{
    "title" : "Love at the beginning",
    "body" : "There are many things that confuse us when we are in love.  Especially in the beginning we are unsure as to how we should respond to different kinds of situation.",
    "note" : "We all need a bit more faith at the start."
  
}

POST /index_02/articles/
{
    "title" : "Going for gold",
    "body" : "There are many challenges that make things difficult for us at the beginning.",
    "note" : "We all need a bit more faith at the start."
  
}


DELETE /index_02


# Search based on title and content
GET /index_02/articles/_search
{
  "query" : {
    "bool" : {
      "must" : {
        "match" : {
          "body" : {
            "query" : "insiite in the beginning",
            "minimum_should_match" : "50%"
          }
        }
      },
      "should" : {
        "match" : {
          "title" : {
            "query" : "insiite in the beginning",
            "boost" : 3
          }
        }
      }
    }
  }
}

GET /index_02/articles/_search
{
  "query" : {
    "bool" : {
      "must" : {
        "bool" : {
          "should" : [
            {
              "match" : {
                "body" : {
                  "query" : "insiite beginning",
                  "minimum_should_match" : "70%"
                }
              }
            },
            {
              "match" : {
                "note" : {
                  "query" : "insiite beginning",
                  "minimum_should_match" : "70%"
                }
              }
            }
          ]
        }
      },
      "should" : {
        "match" : {
          "title" : {
            "query" : "insiite beginning"
          }
        }
      }
    }
  }
}

GET /index_02/articles/_search


GET /index_02/articles/_search?explain
{
  "query" : {
    "dis_max" : {
      "queries" : [
        {
          "match" : {
            "body" : {
              "query" : "insiite at the start",
              "minimum_should_match" : "70%",
              "boost": 5
            }
          }
        },
        {
          "match" : {
            "note" : {
              "query" : "insiite at the start",
              "minimum_should_match" : "70%",
              "boost": 1
            }
          }
        },
        {
          "match" : {
            "title" : {
              "query" : "insiite at the start",
              "boost" :1
            }
          }
        }
      ],
      "tie_breaker": 0.3
    }
  }
}

DELETE index_01

PUT index_01
{
  "settings" : {
    "number_of_shards": 1
  },
  "mappings" : {
    "my_type": {
      "properties": {
        "title" : {
          "type" : "string",
          "analyzer": "english",
          "fields" : {
            "std" : {
              "type" : "string",
              "analyzer": "standard"
            }
          }
        }
      }
    }
  }
}

PUT /index_01/my_type/1
{
  "title" : "My rabbit jumps"
}

PUT /index_01/my_type/2
{
  "title" : "Jumping jack rabbits"
}

GET /index_01/_search

GET index_01/_search
{
  "query" : {
    "match" : {
      "title" : "jumping rabbits"
    }
  }
}

GET index_01/_search
{
  "query" : {
    "match" : {
      "title" : "jumping rabbits"
    }
  }
}

GET /index_01/my_type/_search
{
  "query" : {
    "bool" : {
      "should" : [
        {
          "match" : {
            "title" : {
              "query" : "jumping rabbits",
              "minimum_should_match" : "60%",
              "boost" : 10
            }
          }
        },
        {
          "match" : {
            "title.std" : {
              "query" : "jumping rabbits",
              "minimum_should_match" : "60%"
            }
          }
        }
      ]
    }
  }
}

PUT /index_05/articles/1
{
  "title" : "My rabbit jumps"
}

PUT /index_05/articles/2
{
  "title" : "Jumping jack rabbits"
}

GET /index_02/_search


GET /index_05/articles/_search
{
  "query" : {
    "bool" : {
      "should" : [
        {
          "match" : {
            "title" : {
              "query" : "jumping rabbits",
              "minimum_should_match" : "60%",
              "boost" : 10
            }
          }
        },
        {
          "match" : {
            "title.standard" : {
              "query" : "jumping rabbits",
              "minimum_should_match" : "60%"
            }
          }
        }
      ]
    }
  }
}

PUT /index_06
{
  "settings" : { 
    "number_of_shards" : 1 
    
  }
}

POST /index_06/posts/_bulk
{ "index" : { "_id" : 1 }}
{ "title" : "The quick brown fox" }
{ "index" : { "_id" : 2 }}
{ "title" : "The quick brown fox jumps over the lazy dog" }
{ "index" : { "_id" : 3 }}
{ "title" : "The quick brown fox jumps over the quick dog" }
{ "index" : { "_id" : 4 }}
{ "title" : "Brown fox brown dog" }


GET /index_06/posts/_search
{
  "query" : {
    "match_phrase": {
      "title": {
        "query": "quick dog",
        "slop" : 10
      }
    }
  }
}


GET /index_06/posts/_search
{
  "query" : {
    "bool" : {
      "must" : {
        "match": {
          "title" : {
            "query" : "quick dog",
            "minimum_should_match" : "70%"
          }
        }
      },
      "should" : {
        "match_phrase": {
          "title": {
            "query": "quick dog",
            "slop" : 100
          }  
        }
      }
    }
  }
}




GET /index_06/posts/_search
{
  "query" : {
    "match": {
      "title" : {
        "query" : "quick brown fox",
        "minimum_should_match" : "50%"
      }
    }
  },
  "rescore" : {
    "window_size" : 50,
    "query" : {
      "rescore_query" : {
        "match_phrase" : {
          "title": {
            "query": "quick brown fox",
            "slop" : 100
          }  
        }
      }
    }
  }
}


PUT /index_07 
{
  "settings" : {
    "number_of_shards": 1,
    "analysis": {
      "filter" : {
        "my_shingle_filter" : {
          "type" : "shingle",
          "min_shingle_size" : 2,
          "max_shingle_size" : 2,
          "output_unigrams" : false
        }
      },
      "analyzer": {
        "my_shingle_analyzer" : {
          "type" : "custom",
          "tokenizer" : "standard",
          "filter" : [
            "lowercase",
            "my_shingle_filter"
          ]
        }
      }
    }
  }
}


PUT /index_07/_mapping/articles
{
  "articles" : {
    "properties": {
      "title" : {
        "type" : "string",
        "fields" : {
          "shingles" : {
            "type" : "string",
            "analyzer" : "my_shingle_analyzer"
          }
        }
      }
    }
  }
}


POST /index_07/articles/_bulk
{ "index": { "_id" : 1 }}
{ "title": "Sue ate the alligator" }
{ "index": { "_id" : 2 }}
{ "title": "The alligator ate Sue" }
{ "index": { "_id" : 3 }}
{ "title": "Sue never goes anywhere without her alligator skin purse" }

GET /index_07/articles/_search
{
  "query" : {
    "bool" : {
      "must" : {
        "match" : {
          "title" : "the hungry alligator ate sue",
          "minimum_match"
        }
      },
      "should"
    }
    "match" : {
      "title" : "the hungry alligator ate sue"
    }
  }
}

DELETE index_08

PUT /index_08
{
  "settings" : {
    "number_of_shards": 1
  },
  "mappings" : {
    "posts": {
      "properties": {
        "title" : {
          "type" : "multi_field",
          "fields" : {
            "title" : {
              "type" : "string",
              "analyzer": "english"
            },
            "standard" : {
              "type" : "string",
              "analyzer": "standard"
            }
          }
        }
      }
    }
  }
}







PUT /index_08/posts/1
{
  "title" : "My rabbit jumps"
}

PUT /index_08/posts/2
{
  "title" : "Jumping jack rabbits"
}




GET /index_08/posts/_search
{
  "query" : {
    "match" : {
      "title" : {
        "query" : "jumping rabbits"
      }
    }
  }
}

GET /index_08/posts/_search
{
  "query" : {
    "match" : {
      "title.standard" : {
        "query" : "jumping rabbits"
      }
    }
  }
}



GET /index_08/posts/_search
{
  "query" : {
    "bool" : {
      "should" : [
        {
          "match" : {
            "title" : {
              "query" : "jumping rabbits",
              "minimum_should_match" : "60%",
              "boost" : 10
            }
          }
        },
        {
          "match" : {
            "title.standard" : {
              "query" : "jumping rabbits",
              "minimum_should_match" : "60%"
            }
          }
        }
      ]
    }
  }
}


GET /index_08/posts/_search
{
  "query" : {
    "bool" : {
      "must" : [
        {
          "match" : {
            "title" : {
              "query" : "jumping rabbits",
              "minimum_should_match" : "60%",
              "boost" : 10
            }
          }
        }
      ],
      "should" : [
        {
          "match" : {
            "title.standard" : {
              "query" : "jumping rabbits",
              "minimum_should_match" : "60%"
            }
          }
        }
      ]
    }
  }
}

DELETE index_09

PUT /index_09 
{
  "settings" : {
    "number_of_shards": 1,
    "analysis": {
      "filter" : {
        "my_shingle_filter" : {
          "type" : "shingle",
          "min_shingle_size" : 2,
          "max_shingle_size" : 2,
          "output_unigrams" : false
        }
      },
      "analyzer": {
        "my_shingle_analyzer" : {
          "type" : "custom",
          "tokenizer" : "standard",
          "filter" : [
            "lowercase",
            "my_shingle_filter"
          ]
        }
      }
    }
  }
}

PUT /index_09/_mapping/articles
{
  "articles" : {
    "properties": {
      "title" : {
        "type" : "multi_field",
        "fields" : {
          "title" : {
            "type" : "string",
            "analyzer" : "standard"
          },
          "shingles" : {
            "type" : "string",
            "analyzer" : "my_shingle_analyzer"
          }
        }
      }
    }
  }
}


POST /index_09/articles/_bulk
{ "index": { "_id" : 1 }}
{ "title": "Sue ate the alligator" }
{ "index": { "_id" : 2 }}
{ "title": "The alligator ate Sue" }
{ "index": { "_id" : 3 }}
{ "title": "Sue never goes anywhere without her alligator skin purse" }

GET /index_09/articles/_search
{
  "filter" : {
    "ids": "2"
  }
}

GET /index_09/articles/_search
{
  "query" : {
    "bool" : {
      "must" : [
        {
          "match" : {
            "title" : {
              "query" : "the hungry alligator ate sue",
              "minimum_should_match" : "50%"
            }
          }
        }
      ],
      "should" : [
        {
          "match" : {
            "title.shingles" : {
              "query" : "the hungry alligator ate sue"
            }
          }
        }
      ]
    }
  }
}



PUT /index_10
{
  "mappings" : {
    "address" : {
      "properties" : {
        "postcode" : {
          "type" : "string",
          "index" : "not_analyzed"
        }
      }
    }
  }
}

PUT /index_10/address/1
{
  "postcode" : "W1V 3DG"
}


PUT /index_10/address/2
{
  "postcode" : "W2F 8HW"
}

PUT /index_10/address/3
{
  "postcode" : "W1F 7HW"
}

PUT /index_10/address/4
{
  "postcode" : "WC1N 1LZ"
}

PUT /index_10/address/5
{
  "postcode" : "SW5 0BE"
}


GET /index_10/_search
{
  "query" : {
    "prefix": {
      "postcode": {
        "value": "W1"
      }
    }
  }
}




PUT /index_11/posts/1
{
  "userID" : 123,
  "title" : "this is the first one",
  "status" : 1
}

PUT /index_11/posts/2
{
  "userID" : 123,
  "title" : "this is the second one",
  "status" : 1
}

PUT /index_11/posts/4
{
  "userID" : 123,
  "title" : "this is the second one",
  "status" : 0
}

PUT /index_11/posts/3
{
  "userID" : 100,
  "title" : "this is the someone else",
  "status" : 1
}


GET /index_11/_search


GET /index_11/posts/_search
{
  "filter" : {
    "and" : [
      {
        "term" : {
          "userID" : 100
        }
      },
      {
        "term" : {
          "status" : 1
        }
      }
    ]
  }
}




PUT /index_12
{
  "settings" : {
    "number_of_shards": 1,
    "analysis": {
      "filter" : {
        "autocomplete_filter" : {
          "type" : "edge_ngram",
          "min_gram" : 1,
          "max_gram" : 20
        }
      },
      "analyzer": {
        "autocomplete" : {
          "type" : "custom",
          "tokenizer" : "standard",
          "filter" : [
            "lowercase",
            "autocomplete_filter"
          ]
        }
      }
    }
  }
}


GET /index_12/_analyze?analyzer=autocomplete
{
  "" : "quick brown"
}


PUT /index_12/_mapping/post
{
  "post" : {
    "properties" : {
      "name" : {
        "type" : "string",
        "index_analyzer" : "autocomplete",
        "search_analyzer" : "standard"
      }
    }
  }
}

POST /index_12/post/_bulk
{ "index" : { "_id" : 1 }}
{ "name" : "Brown foxes" }
{ "index" : { "_id" : 2 }}
{ "name" : "Yellow furballs" }


GET /index_12/post/_search
{
  "query" : {
    "match": {
      "name": "brown fo"
    }
  }
}




POST /index_13/articles/1
{
    "title" : "My first Insiite article",
    "body" : "There are lots of things in this article that talk about the growth of insiite and how it has come to be the default tool for knowledge management",
    "note" : "See how important initial user growth was at the beginning",
    "votes" : 3
}

PUT /index_13/articles/2
{
    "title" : "My first article",
    "body" : "There are many ways that we can encourage the growth of indoor plants.  Some of them like being indoors all year.",
    "note" : "We must make sure that we water the plants lots in the beginning",
    "votes" : 2
}

POST /index_13/articles/3
{
    "title" : "Insiite and me",
    "body" : "This is a short story about how insiite got started and all the important things that we learnt along the way.",
    "note" : "There were lots of difficult things happening at the beginning.",
    "votes" : 324
}

POST /index_13/articles/4
{
    "title" : "Trying so hard",
    "body" : "It can be a difficult time for youngsters, their bodies are growing so quickly and there are so many things that they are learning for the first time.",
    "note" : "Must remember to help the little ones when they need it at the beginning",
    "votes" : 23
  
}

POST /index_13/articles/5
{
    "title" : "School in the beginning",
    "body" : "It can be a difficult time for youngsters, their bodies are growing so quickly and there are so many things that they are learning for the first time.",
    "note" : "The children need a lot of help at the beginning",
    "votes" : 2
  
}

POST /index_13/articles/6
{
    "title" : "Love at the beginning",
    "body" : "There are many things that confuse us when we are in love.  Especially in the beginning we are unsure as to how we should respond to different kinds of situation.",
    "note" : "We all need a bit more faith at the start.",
    "votes" : 4
  
}

PUT /index_13/articles/7
{
    "title" : "Going for gold",
    "body" : "There are many challenges that make things difficult for us at the beginning.",
    "note" : "We all need a bit more faith at the start.",
    "votes" : 6
  
}


GET /index_13/articles/_search
{
  "query" : {
    "multi_match": {
      "query": "insiite beginning",
      "fields": ["title","body","note"]
    }
    
  }
}




GET /index_13/articles/_search
{
  "query" : {
    "function_score": {
      "query": {
        "multi_match": {
          "query": "insiite beginning",
          "fields": ["title","body","note"]
        }
      },
      "field_value_factor": {
          "field" : "votes"
      }
    }
  }
}



GET /index_13/articles/_search
{
  "query" : {
    "function_score": {
      "query": {
        "bool" : {
          "must" : {
            "bool" : {
              "should" : [
                {
                  "match" : {
                    "body" : {
                      "query" : "insiite beginning",
                      "minimum_should_match" : "70%",
                      "cutoff_frequency" : "0.01"
                    }
                  }
                },
                {
                  "match" : {
                    "note" : {
                      "query" : "insiite beginning",
                      "minimum_should_match" : "70%",
                      "cutoff_frequency" : "0.01"
                    }
                  }
                }
              ]
            }
          },
          "should" : {
            "match" : {
              "title" : {
                "query" : "insiite beginning"
              }
            }
          }
        }
      },
      "field_value_factor": {
          "field" : "votes",
          "modifier": "log1p",
          "factor" : 2
      }
    } 
  }
}

GET /index_13/articles/_search
{
  "query" : {
    "function_score": {
      "query": {
        "bool" : {
          "must" : {
            "bool" : {
              "should" : [
                {
                  "match" : {
                    "body" : {
                      "query" : "insiite beggining",
                      "fuzziness" : "AUTO",
                      "prefix_length" : 3,
                      "max_expansions" : 10,
                      "minimum_should_match" : "70%",
                      "cutoff_frequency" : "0.01"
                    }
                  }
                },
                {
                  "match" : {
                    "note" : {
                      "query" : "insiite beggining",
                      "fuzziness" : "AUTO",
                      "prefix_length" : 3,
                      "max_expansions" : 10,
                      "minimum_should_match" : "70%",
                      "cutoff_frequency" : "0.01"
                    }
                  }
                }
              ]
            }
          },
          "should" : {
            "match" : {
              "title" : {
                "query" : "insiite beggining"
              }
            }
          }
        }
      },
      "field_value_factor": {
          "field" : "votes",
          "modifier": "log1p",
          "factor" : 2
      }
    } 
  }
}


DELETE /index_14

GET /_analyze?tokenizer=standard
{
  "" : "<p>Some more information at the following link <a href='http://somewebsite.com'>Website</a></p>"
}

PUT /index_14
{
  "settings" : {
    "analysis": {
      "analyzer" : {
        "my_html_analyzer" : {
          "tokenizer" : "standard",
          "char_filter" : ["html_strip"]
        }
      }
    }
  }
}

GET /index_14/_analyze?analyzer=my_html_analyzer
{
  "" : "<p>Some more information at the following link <a href='http://somewebsite.com'>Website</a></p>"
}


PUT /index_15
{
  "settings" : {
    "analysis": {
      "filter": {
        "my_synonym_filter" : {
          "type" : "synonym",
          "synonyms" : [
            "british, english",
            "queen, monarch"
          ]
        }
      },
      "analyzer": {
        "my_synonyms" : {
          "tokenizer" : "standard",
          "filter" : [
            "lowercase",
            "my_synonym_filter"
          ]
        }
      }
    }
  }
}

GET /index_15/_analyze?analyzer=my_synonyms
{
  "": "Elizabeth is the English queen"
}


POST /index_16/articles/_bulk
{"index": {"_id" : 1 }}
{"text" : "Surprise me!"}
{"index": {"_id" : 2 }}
{"text" : "That was surprising"}
{"index": {"_id" : 3 }}
{"text" : "I wasn't surprised"}

GET /index_16/articles/_search
{
  "query" : {
    "match" : {
      "text" : {
        "query" : "surprize me!",
        "fuzziness" : "AUTO",
        "prefix_length": 3,
        "max_expansions": 10
      }
    }
  }
}


DELETE /cars

POST /cars/transactions/_bulk
{"index" : {}}
{"price" : 10000, "color" : "red", "make": "honda", "sold": "2014-10-28"}
{"index" : {}}
{"price" : 20000, "color" : "red", "make": "honda", "sold": "2014-11-05"}
{"index" : {}}
{"price" : 30000, "color" : "green", "make": "ford", "sold": "2014-05-18"}
{"index" : {}}
{"price" : 15000, "color" : "blue", "make": "toyota", "sold": "2014-07-02"}
{"index" : {}}
{"price" : 12000, "color" : "green", "make": "toyota", "sold": "2014-08-19"}
{"index" : {}}
{"price" : 20000, "color" : "red", "make": "honda", "sold": "2014-11-05"}
{"index" : {}}
{"price" : 80000, "color" : "red", "make": "bmw", "sold": "2014-01-01"}
{"index" : {}}
{"price" : 25000, "color" : "blue", "make": "ford", "sold": "2014-02-12"}

DELETE /cars/transactions/AU5njxQpcgnoD-VKCGUk

GET /cars/_search

GET /cars/transactions/_search?search_type=count
{
  "aggs" : {
    
    "colors" : {
      "terms" : {
        "field" : "color"
      }
    }
  }
}


GET /cars/transactions/_search?search_type=count
{
  "aggs" : {
    "colors" : {
      "terms" : {
        "field" : "color"
      },
      "aggs" : {
        "average_price" : {
          "avg" : {
            "field" : "price"
          }
        }
      }
    }
  }
}

GET /cars/transactions/_search?search_type=count
{
  "aggs" : {
    "colors" : {
      "terms" : {
        "field" : "color"
      },
      "aggs" : {
        "average_price" : {
          "avg" : {
            "field" : "price"
          }
        },
        "make" : {
          "terms" : {
            "field" : "make"
          }
        }
      }
    }
  }
}

GET /cars/transactions/_search?search_type=count
{
  "aggs" : {
    "colors" : {
      "terms" : {
        "field" : "color"
      },
      "aggs" : {
        "average_price" : {
          "avg" : {
            "field" : "price"
          }
        },
        "make" : {
          "terms" : {
            "field" : "make"
          },
          "aggs" : {
            "min_price" : {
              "min" : {
                "field" : "price"
              }
            },
            "max_price" : {
              "max" : {
                "field" : "price"
              }
            }
          }
        }
      }
    }
  }
}


GET /cars/transactions/_search?search_type=count
{
  "aggs" : {
    "price" : {
      "histogram" : {
        "field" : "price",
        "interval": 20000
      },
      "aggs" : {
        "revenue" : {
          "sum" : {
            "field" : "price"
          }
        }
      }
    }
  }
}

GET /cars/transactions/_search?search_type=count
{
  "aggs" : {
    "sales" : {
      "date_histogram" : {
        "field" : "sold",
        "interval": "quarter",
        "format" : "yyyy-MM-dd",
        "min_doc_count": 0,
        "extended_bounds" : {
          "min" : "2014-01-01",
          "max" : "2014-12-31"
        }
      },
      "aggs" : {
        "by_make" : {
          "terms" : {
            "field" : "make"
          }
        }
      }
    }
  }
}

GET /cars/transactions/_search
{
  "query" : {
    "match" : {
      "make" : "ford"
    }
  },
  "aggs" : {
    "colors" : {
      "terms" : {
        "field" : "color"
      }
    }
  }
}

PUT _snapshot/sigterms
{
  "type" : "url",
  "settings" : {
    "url" : "http://download.elasticsearch.org/definitiveguide/sigterms_demo/"}
}


GET _snapshot/sigterms/_all
POST _snapshot/sigterms/snapshot/_restore

GET /mlmovies/_search

GET /mlratings/_search

GET mlratings/_search?search_type=count
{
  "query" : {
    "filtered" : {
      "filter" : {
        "term" : {
          "movie" : 46970
        }
      }
    }
  },
  "aggs" : {
    "most_popular" : {
      "terms" : {
        "field" : "movie",
        "size" : 6
      }
    }
  }
}

GET mlmovies/_search
{
  "query" : {
    "match" : {
      "title": "Dodgeball"
    }
  }
}

GET mlmovies/_search
{
  "query" : {
    "filtered": {
      "filter": {
        "ids" : {
          "values" : [2571,318,296,2959,260]
        }
      }
    }
  }
}

GET mlratings/_search?search_type=count
{
  "query" : {
    "filtered" : {
      "filter" : {
        "term" : {
          "movie" :46970
        }
      }
    }
  },
  "aggs" : {
    "most_significant" : {
      "significant_terms" : {
        "field" : "movie",
        "size" : 6
      }
    }
  }
}

GET mlmovies/_search
{
  "query" : {
    "filtered": {
      "filter": {
        "ids" : {
          "values" : [52245,44840,54004,56805,60756]
        }
      }
    }
  }
}


GET mlratings/_search?search_type=count
{
  "query" : {
    "filtered" : {
      "filter" : {
        "term" : {
          "movie" : [46970]
        }
      }
    }
  },
  "aggs" : {
    "most_significant" : {
      "significant_terms" : {
        "field" : "movie",
        "size" : 6
      }
    }
  }
}

DELETE index_17

PUT /index_17
{
  "mappings" : {
    "blogpost" : {
      "properties": {
        "comments" : {
          "type" : "nested",
          "properties" : {
            "name" : { "type":"string" },
            "comment" : { "type" : "string" },
            "commentID" : { "type" : "short" },
            "age" : { "type":"short" },
            "stars" : { "type" : "short" },
            "date" : { "type":"date" }
          }
        }
      }
    }
  }
}

PUT /index_17/blogpost/1
{
  "title" : "Nest eggs",
  "body" : "Make your money work for you",
  "tags" : ["cash","savings"],
  "comments" : [
    {
      "name" : "John Smith",
      "comment" : "Great article",
      "commentID" : 9,
      "age" : 28,
      "stars" : 4,
      "date" : "2014-09-01"
    },
    {
      "name" : "Alice White",
      "comment" : "More like this please!",
      "commentID" : 8,
      "age" : 31,
      "stars" : 5,
      "date" : "2014-10-22"
    } 
  ]
}

PUT /index_17/blogpost/2
{
  "title" : "My money plan",
  "body" : "Make more with your money",
  "tags" : ["cash","savings"],
  "comments" : [
    {
      "name" : "John waite",
      "comment" : "I like it",
      "commentID" : 10,
      "age" : 33,
      "stars" : 4,
      "date" : "2014-10-01"
    },
    {
      "name" : "Alice Moody",
      "comment" : "More!!",
      "commentID" : 11,
      "age" : 32,
      "stars" : 5,
      "date" : "2014-1-22"
    } 
  ]
}


GET /index_17/blogpost/_search
{
  "query" : {
    "bool" : {
      "must" : [
        { 
          "match" : {
            "title" : {
              "query" : "eggs john"
            }
          }
        },
        {
          "nested" : {
            "path" : "comments",
            "query": {
              "bool" : {
                "must" : [
                  {
                    "match" : {
                      "comments.name" : {
                        "query" : "john"
                      }
                    }
                  },
                  {
                    "match" : {
                      "comments.age" : {
                        "query" : 28
                      }
                    }
                  }
                ]
              }
            }
          }
        }
      ]
    }
  },
  "highlight": {
    "fields" : {
      "title" : {},
      "comments.name": {}
    }
  }
}

GET /index_17/blogpost/_search
{
  "query" : {
    "bool" : {
      "should" : [
        { 
          "match" : {
            "title" : {
              "query" : "eggs john"
            }
          }
        },
        {
          "nested" : {
            "path" : "comments",
            "query": {
              "bool" : {
                "must" : [
                  {
                    "match" : {
                      "comments.name" : {
                        "query" : "eggs john"
                      }
                    }
                  }
                ]
              }
            }
          }
        }
      ]
    }
  },
  "highlight": {
    "fields" : {
      "title" : {},
      "comments.name": {}
    }
  }
}

DELETE index_18

PUT /index_18
{
  "settings" : {
    "analysis" : {
      "analyzer": {
        "my_stopwords_analyzer" : {
          "type" : "standard",
          "stopwords" : [
            "and", "a", "are", "an", "as", "at", "be", "but", "by",
            "for", "if", "in", "into", "is", "it", "of", "on", "or", "such",
            "that", "the", "their", "then", "there", "these", "they", "this",
            "to", "was", "will", "with"
            
          ]
        }
      }
    }
  },
  "mappings" : {
    "articles" : {
      "properties": {
        "title" : {
          "type" : "string",
          "analyzer": "my_stopwords_analyzer"
        },
        "nestMap" : {
          "type" : "string",
          "analyzer": "standard"
        },
        "body" : {
          "type" : "string",
          "analyzer": "my_stopwords_analyzer"
        },
        "notesCount" : {
          "type" : "short"
        },
        "notes" : {
          "type" : "nested",
          "properties" : {
            "note" : { 
              "type" : "string", 
              "analyzer" : "my_stopwords_analyzer" 
            },
            "noteID" : { 
              "type" : "short", 
              "analyzer" : "my_stopwords_analyzer" 
            },
            "userID" : {
              "type" : "short"
            },
            "date" : { 
              "type" : "date", 
              "analyzer" : "my_stopwords_analyzer" 
            }
          }
        }
      }
    }
  }
}

PUT /index_18/articles/1
{
  "title" : "This is the first article about insiite",
  "nestMap" : [ "notes" ],
  "body" : "When we first started building insiite, we created lots of different tools",
  "notesCount" : 2,
  "notes" : [
    {
      "note" : "notes009# This is the ninth note that we made",
      "noteID" : 9,
      "userID" : 1,
      "date" : "2014-09-01"
    },
    {
      "note" : "notes008# This is the eigth note that we made",
      "noteID" : 8,
      "userID" : 2,
      "date" : "2014-09-10"
    } 
  ]
}

PUT /index_18/articles/2
{
  "title" : "This is the second article about insiite",
  "nestMap" : ["notes"],
  "body" : "When we first started building, we saw lots of differen ways that we could improve the way that everything worked",
  "notesCount" : 2,
  "notes" : [
    {
      "note" : "notes010# This is the tenth note that we made, it talks about the start",
      "noteID" : 10,
      "userID" : 1,
      "date" : "2014-09-01"
    },
    {
      "note" : "notes011# This is the eleventh note that we made about insiite",
      "noteID" : 11,
      "userID" : 1,
      "date" : "2014-10-22"
    } 
  ]
}


GET /index_18/articles/_search
{
  "query" : {
    "bool" : {
      "should" : [
        { 
          "match" : {
            "title" : {
              "query" : "insiite at the start"
            }
          }
        },
        { 
          "match" : {
            "body" : {
              "query" : "insiite at the start"
            }
          }
        },
        {
          "nested" : {
            "path" : "notes",
            "query": {
              "match" : {
                "notes.note" : {
                  "query" : "insiite at the start"
                }
              }
            }
          }
        }
      ]
    }
  },
  "highlight": {
    "fields" : {
      "title" : {},
      "body" : {},
      "notes.note" : {}
    }
  }
}


GET /index_18/articles/_search
{
  "query" : {
    "bool" : {
      "must" : [
        {
          "bool" : {
            "should" : [
              { 
                "match" : {
                  "title" : {
                    "query" : "insiite at the start"
                  }
                }
              },
              { 
                "match" : {
                  "body" : {
                    "query" : "insiite at the start"
                  }
                }
              },
              {
                "nested" : {
                  "path" : "notes",
                  "query": {
                    "match" : {
                      "notes.note" : {
                        "query" : "insiite at the start"
                      }
                    }
                  }
                }
              }
            ]
          }
        },
        {
          "nested" : {
            "path" : "notes",
            "filter" : {
              "range" : {
                "notes.date" : {
                  "gte" : "2014-10-01",
                  "lte" : "2014-12-01"
                }
              }
            }
          }  
        }
      ]
    }
  },
  "highlight": {
    "fields" : {
      "title" : {},
      "body" : {},
      "notes.note" : {}
    }
  }
}

GET /index_18/articles/_search
{
  "query" : {
    "nested" : {
      "path" : "notes",
      "filter" : {
        "bool" : {
          "should" : [
            {
              "term" : {
                "notes.noteID" : 8
              }
            },
            {
              "term" : {
                "notes.noteID" : 10
              }
            }
          ]
        }
      }
    }  
  }
}


# return articles that have notes.noteID of either 6 or 10 and are between
# the dates of '2014-10-01' and '2014-12-01'

GET /index_18/articles/_search
{
  "query" : {
    "nested" : {
      "path" : "notes",
      "filter" : {
        "bool" : {
          "must" : 
          [
            {
              "bool" : {
                "should" : 
                [
                  {
                    "term" : {
                    "notes.noteID" : 6
                    }
                  },
                  {
                    "term" : {
                      "notes.noteID" : 10
                    }
                  }
                ]
              }
            },
            {
              "range" : {
                "notes.date" : {
                  "gte" : "2014-10-01",
                  "lte" : "2014-12-01"
                }
              }
            }
          ]
        }
      }
    }  
  }
}


# return articles that have notes.userID of either 1 or 2 and are between
# the dates of '2014-10-01' and '2014-12-01'

GET /index_18/articles/_search
{
  "query" : {
    "nested" : {
      "path" : "notes",
      "filter" : {
        "bool" : {
          "must" : 
          [
            {
              "bool" : {
                "should" : 
                [
                  {
                    "term" : {
                    "notes.userID" : 1
                    }
                  },
                  {
                    "term" : {
                      "notes.userID" : 2
                    }
                  }
                ]
              }
            },
            {
              "range" : {
                "notes.date" : {
                  "gte" : "2014-09-01",
                  "lte" : "2014-12-01"
                }
              }
            }
          ]
        }
      }
    }  
  }
}

GET /index_18/articles/_search
{
  "query" : {
    "bool" : {
      "must" : 
      [
        {
          "bool" : {
            "should" : 
            [
              {
                "match" : {
                  "title" : {
                    "query" : "insiite at the start"
                  }
                }
              },
              {
                "match" : {
                 "body" : {
                    "query" : "insiite at the start",
                    "minimum_should_match" : "50"
                  }
                }
              }
            ]
          }
        },
        {
          "nested" : {
            "path" : "notes",
            "filter" : {
              "bool" : {
                "must" : 
                [
                  {
                    "bool" : {
                      "should" : 
                      [
                        {
                          "term" : {
                            "notes.userID" : 1
                          }
                        },
                        {
                          "term" : {
                            "notes.userID" : 2
                          }
                        }
                      ]
                    }
                  },
                  {
                    "range" : {
                      "notes.date" : {
                        "gte" : "2014-09-01",
                        "lte" : "2014-12-01"
                      }
                    }
                  }
                ]
              }
            }
          }
        }
      ]
    }
  },
  "highlight": {
    "fields" : {
      "title" : {},
      "body" : {},
      "notes.note": {}
    }
  }
}

# Search for "insiite at the start" in "title", "body" or "notes.note" where a note has been made
# by "userID = 1" or "userID = 2" in the time period "2014-09-01" to "2014-12-01".  Note that for 
# the first part of the query, we can directly use "notes.note" as we are only checking for the 
# presence of the terms.  However for the "notes.userID" and "notes.date" part, we must use the 
# nested path approach as we want to confirm that these hold true within the same nested object
GET /index_18/articles/_search
{
  "query" : {
    "bool" : {
      "must" : 
      [
        {
          "bool" : {
            "should" : 
            [
              {
                "match" : {
                  "title" : {
                    "query" : "insiite at the start"
                  }
                }
              },
              {
                "match" : {
                 "body" : {
                    "query" : "insiite at the start",
                    "minimum_should_match" : "50"
                  }
                }
              },
              {
                "match" : {
                 "notes.note" : {
                    "query" : "insiite at the start",
                    "minimum_should_match" : "50"
                  }
                }
              }
            ]
          }
        },
        {
          "nested" : {
            "path" : "notes",
            "filter" : {
              "bool" : {
                "must" : 
                [
                  {
                    "bool" : {
                      "should" : 
                      [
                        {
                          "term" : {
                            "notes.userID" : 1
                          }
                        },
                        {
                          "term" : {
                            "notes.userID" : 2
                          }
                        }
                      ]
                    }
                  },
                  {
                    "range" : {
                      "notes.date" : {
                        "gte" : "2014-09-01",
                        "lte" : "2014-12-01"
                      }
                    }
                  }
                ]
              }
            }
          }
        }
      ]
    }
  },
  "highlight": {
    "fields" : {
      "title" : {},
      "body" : {},
      "notes.note": {}
    }
  }
}


# Simple groovy script to increment 'notesCount' field
# This could be used everytime we add a nested objects in the notes field as this means that we 
# could keep a simple record of the activity 
#
# We create the groovy script by /etc/elasticsearch$ sudo touch scripts/increment-notesCount.groovy
# we then edit the script by /etc/elasticsearch$ sudo vim scripts/increment-notesCount.groovy
# file://increment-notesCount.groovy
# ctx._source.notesCount += 1

POST /index_18/articles/2/_update
{
   "script" : {
     "lang" : "groovy",
     "script_file" : "increment-notesCount"
   }
}

# ctx._source.notes += note
POST /index_18/articles/2/_update
{
   "script" : {
     "lang" : "groovy",
     "script_file" : "add-note",
     "params" : {
       "note" : {
          "note" : "notes020# This is the twentieth note that we made about insiite",
          "noteID" : 20,
          "userID" : 3,
          "date" : "2014-10-22"  
       }
     }     
   }
}


# // file: /etc/elasticsearch/scripts/add-note-and-increment.groovy
#
# ctx._source.notesCount += 1
# ctx._source.notes += note
POST /index_18/articles/1/_update
{
  "script" : {
    "lang" : "groovy",
    "script_file" : "add-note-and-increment",
    "params" : {
      "note" : {
          "note" : "notes020# This is the twentieth note that we made about insiite",
          "noteID" : 20,
          "userID" : 3,
          "date" : "2014-10-22"  
       }
    }
  }
}

GET /index_18/articles/_search

# remove a 'note' from 'notes' and update 'notesCount'
#
# // file: /etc/elasticsearch/scripts/remove-note.groovy
#
#for (int i = 0; i < ctx._source.notes.size(); i++){
#  if(ctx._source.notes[i].noteID == id){
#    ctx._source.notes.remove(i)
#    i--
#  }
#}
#ctx._source.notesCount -= 1
POST /index_18/articles/1/_update
{
   "script" : {
     "lang" : "groovy",
     "script_file" : "remove-note",
     "params" : {
       "id" : 9  
     }     
   }
}

# update a 'note'
#
# // file: /etc/elasticsearch/scripts/update-note.groovy
#
#for (int i = 0; i < ctx._source.notes.size(); i++){
#  if(ctx._source.notes[i].noteID == id){
#    ctx._source.notes[i].note = updatedNote
#  }
#}
#ctx._source.notesCount -= 1
POST /index_18/articles/1/_update
{
  "script" : {
    "lang" : "groovy",
    "script_file" : "update-note",
    "params" : {
       "id" : 8,
       "updatedNote" : "notes008# This is the eigth note that we made. Yeah baby"
    }     
  }
}

DELETE index_19

PUT /index_19
{
  "settings" : {
    "analysis" : {
      "analyzer": {
        "my_stopwords_analyzer" : {
          "type" : "standard",
          "stopwords" : [
            "and", "a", "are", "an", "as", "at", "be", "but", "by",
            "for", "if", "in", "into", "is", "it", "of", "on", "or", "such",
            "that", "the", "their", "then", "there", "these", "they", "this",
            "to", "was", "will", "with"
            
          ]
        }
      }
    }
  },
  "mappings" : {
    "articles" : {
      "properties": {
        "title" : {
          "type" : "string",
          "analyzer": "my_stopwords_analyzer"
        },
        "nestMap" : {
          "type" : "string",
          "analyzer": "standard"
        },
        "body" : {
          "type" : "string",
          "analyzer": "my_stopwords_analyzer"
        },
        "notesCount" : {
          "type" : "short"
        }
      }
    },
    "note" : {
      "_parent" : {
        "type" : "articles"
      },
      "properties" : {
        "note" : { 
          "type" : "string", 
          "analyzer" : "my_stopwords_analyzer" 
        },
        "noteID" : { 
          "type" : "short", 
          "analyzer" : "my_stopwords_analyzer" 
        },
        "userID" : {
          "type" : "short"
        },
        "date" : { 
          "type" : "date", 
          "analyzer" : "my_stopwords_analyzer" 
        }
      }
    }
  }
}

PUT /index_19/articles/1
{
  "title" : "This is the first article about insiite",
  "nestMap" : [ "notes" ],
  "body" : "When we first started building insiite, we created lots of different tools",
  "notesCount" : 2
}

GET /index_19/_search

PUT /index_19/note/9?parent=1
{
  "note" : "notes009# This is the ninth note that we made",
  "noteID" : 9,
  "userID" : 1,
  "date" : "2014-09-01"
}

PUT /index_19/note/8?parent=1
{
  "note" : "notes008# This is the eigth note that we made",
  "noteID" : 8,
  "userID" : 2,
  "date" : "2014-09-10"
} 

PUT /index_19/articles/2
{
  "title" : "This is the second article about insiite",
  "nestMap" : ["notes"],
  "body" : "When we first started building, we saw lots of differen ways that we could improve the way that everything worked",
  "notesCount" : 2
}

PUT /index_19/note/10?parent=2
{
  "note" : "notes010# This is the tenth note that we made, it talks about the start",
  "noteID" : 10,
  "userID" : 1,
  "date" : "2014-09-01"
}

PUT /index_19/note/11?parent=2
```json
{
  "note" : "notes011# This is the eleventh note that we made about insiite",
  "noteID" : 11,
  "userID" : 1,
  "date" : "2014-10-22"
} 
```
 
 
GET /index_19/articles/_search
```json
{
  "query" : {
    "bool" : {
      "should" : [
        { 
          "match" : {
            "title" : {
              "query" : "eleventh"
            }
          }
        },
        { 
          "match" : {
            "body" : {
              "query" : "eleventh"
            }
          }
        },
        {
          "has_child": {
            "type": "note",
            "query": {
              "match" : {
                "note" : "eleventh"
              }
            }
          }
        }
      ]
    }
  },
  "highlight": {
    "fields" : {
      "title" : {},
      "body" : {},
    }
  }
}
```
