---
title: ES初识
date: 2020-04-11 15:41:13
urlname: es-use
tags: [中间件]
categories: 中间件
---

# ES初识
 
 
## 1、什么是ES，能解决什么问题
  分布式搜索分析引擎，可以提供近实时的搜索，分析，存储功能
## 2、ES主要的使用场景是什么？
 搜索、分析、日志、Kibana整合进行可视化图标统计等
##3 主要的关键字解释
- 文档：document -我们存储的数据，数据存储到索引中，会被分词以倒排索引的形式存储。
- 索引-index ：文档的数据集合
- type
- 映射-mapping：即数据存储模板，决定了数据存到索引中以后的行为，如是否需要分词，使用什么分词等。
- 定义映射的时候要确定：字段是否需要全文索引，还是准确的查找，是否需要单独的分词，一段文字可以根据不同使用情况，存储多个key中，是否需要自定义数据类型
- 分词：各种分词插件，搜索和存储的时候会用到
##4下面是数据查询文档的整理：

## 下面是数据查询文档的整理：
 
### 1 搜索
> - wildcard-通配符 ： https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-wildcard-query.html
> - exists-值是否存在：https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-exists-query.html
> - fuzzy-相似模糊：https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-fuzzy-query.html
> - prefix-前缀：https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-prefix-query.html
> - range-范围：https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-range-query.html
> - term-字段匹配：https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-term-query.html
> - terms-字段in匹配：https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-terms-query.html
> - terms-set - 集合子集匹配：https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-terms-set-query.html
> - Match-all-全集合查询：https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-match-all-query.html
> - Mathch-全文检索：https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-match-query.html
> - Mathch-Phrase - 短语全文检索：https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-match-query-phrase.html
> - Bool - 组合查询：https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-bool-query.html
> - boosting-得分分配：https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-boosting-query.html
> - Query与Filter区别：https://www.elastic.co/guide/en/elasticsearch/reference/current/query-filter-context.html

### 2 组合统计
> - AVG-平均数：https://www.elastic.co/guide/en/elasticsearch/reference/current/search-aggregations-metrics-avg-aggregation.html
> - MAX-最大值：https://www.elastic.co/guide/en/elasticsearch/reference/current/search-aggregations-metrics-max-aggregation.html
> - MIN-最小值：https://www.elastic.co/guide/en/elasticsearch/reference/current/search-aggregations-metrics-min-aggregation.html
> - Stat-最大最小平均数：https://www.elastic.co/guide/en/elasticsearch/reference/current/search-aggregations-metrics-stats-aggregation.html、
> - Sum-求和：https://www.elastic.co/guide/en/elasticsearch/reference/current/search-aggregations-metrics-sum-aggregation.html
> - 分组统计：https://www.elastic.co/guide/en/elasticsearch/reference/current/search-aggregations-bucket-terms-aggregation.html
> - 先过滤在统计：https://www.elastic.co/guide/en/elasticsearch/reference/current/search-aggregations-bucket-filter-aggregation.html

> - 数据类型：https://www.elastic.co/guide/en/elasticsearch/reference/current/mapping-types.html
> - 自带元数据：https://www.elastic.co/guide/en/elasticsearch/reference/current/mapping-fields.html
> - 创建索引：https://www.elastic.co/guide/en/elasticsearch/reference/current/indices-create-index.html
> - 创建mapping: https://www.elastic.co/guide/en/elasticsearch/reference/current/properties.html
> - 创建mapping：https://www.elastic.co/guide/en/elasticsearch/reference/current/indices-put-mapping.html
> - 更新查看mapping：https://www.elastic.co/guide/en/elasticsearch/reference/current/mapping.html#update-mapping
### 3 java api的入口
> - api：https://www.elastic.co/guide/en/elasticsearch/client/index.html
> - 创建索引：https://www.elastic.co/guide/en/elasticsearch/client/java-rest/current/java-rest-high-document-index.html
> - Get请求：https://www.elastic.co/guide/en/elasticsearch/client/java-rest/current/java-rest-high-document-get.html
> - 是否存在搜索内容：https://www.elastic.co/guide/en/elasticsearch/client/java-rest/current/java-rest-high-document-exists.html
> - 批量执行：https://www.elastic.co/guide/en/elasticsearch/client/java-rest/current/java-rest-high-document-bulk.html
> - 搜索：https://www.elastic.co/guide/en/elasticsearch/client/java-rest/current/java-rest-high-search.html




