---
layout: post
title: "hadoop partitioner类"
date: 2019-06-10 21:04 +0800
categories: learn
---

1. 指定partitioner类（二次排序）

   `-partitioner org.apache.hadoop.mapred.lib.KeyFieldBasedPartitioner`

2. 指定自定义方法切分行来形成key/value对

   `-jobconf stream.reduce.output.field.separator=SEP # 指定分隔符，默认是tab符` 

   `-jobconf stream.num.reduce.output.fields=NUM # 指定在第n（n>=1）个分隔符分隔，而不是默认的第1个`

3. 指定map输出数据分桶的列数（基于key值的前缀）

   `map.output.key.field.separator=. # 指定切分map输出的分隔符为. ` 

   `num.key.fields.for.partition=2 # 指定使用key的前2个块部分来切分map输出`

