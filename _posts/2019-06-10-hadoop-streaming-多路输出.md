---
layout: post
title: "hadoop streaming 多路输出"
date: 2019-06-10 21:12 +0800
categories: learn
---

1. 多路输出指定outputformat

   `-outputformat org.apache.hadoop.mapred.lib.SuffixMultipleTextOutputFormat`

   `-outputformat org.apache.hadoop.mapred.lib.SuffixMultipleSequenceFileOutputFormat`

2. 在reduce的输出<key, value>变为输出<key, value#X>, #X后缀中X是A~Z的26个英文字母

3. 坑：对于TextInputFormat，reduce输出数据经常是一行一行的文本，如果一行的中间没有"\t"的话，就代表value为空，如果直接将"#X"后缀放在行尾，"#X"会作为key的一部分而不是value的一部分（因为加上后缀后一行中间仍然没有"\t"）这样会导致出错，解决的方式是在#X后缀前面添加"\t"使它变成value的一部分而不是key的一部分

