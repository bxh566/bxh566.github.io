---
title: zeppelin
date: 2018-01-24 07:01:00
tags: [bigdata]
---
### docker

```
docker run -p --rm 8080:8080 --name zeppelin apache/zeppelin:0.7.3
```

### add KUDU dependency to spark interpreter
1. Interpreters
1. add repo(optional)
1. goto Spark interpreter
1. edit button
1. add artifact in Dependencies
1. restart interpreter

### use KUDU by spark
```scala
import org.apache.kudu.spark.kudu._
import org.apache.kudu.client._
import collection.JavaConverters._
// Read a table from Kudu
val df = sqlContext.read.options(Map("kudu.master" ->"192.168.58.100:7051","kudu.table" -> "impala::default.test1")).kudu
// register a temporary table and use SQL
df.registerTempTable("test1")
val df2 = sqlContext.read.options(Map("kudu.master" ->"192.168.58.100:7051","kudu.table" -> "impala::default.test2")).kudu
df2.registerTempTable("test2")
sqlContext.sql("select sum(t1.value)/sum(t2.value) from test1 t1 inner join test2 t2 on t2.key = t1.key").show()
```
