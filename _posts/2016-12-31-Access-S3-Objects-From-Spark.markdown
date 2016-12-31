---
title: Access S3 Objects from Spark
layout: post
---

<script type="text/javascript" src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML"></script>
<script type="text/x-mathjax-config">MathJax.Hub.Config({tex2jax: {inlineMath: [['$','$'], ['\\(','\\)']]}});</script>

The main references are

[http://stackoverflow.com/questions/30851244/spark-read-file-from-s3-using-sc-textfile-s3n](http://stackoverflow.com/questions/30851244/spark-read-file-from-s3-using-sc-textfile-s3n)
[https://www.cloudera.com/documentation/enterprise/5-5-x/topics/spark_s3.html](https://www.cloudera.com/documentation/enterprise/5-5-x/topics/spark_s3.html)

As of 2016/12, use the `s3a` protocol for accessing S3 objects. Also append `--packages org.apache.hadoop:hadoop-aws:2.7.3` to `spark-shell` or `spark-submit`.

