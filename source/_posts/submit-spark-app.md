---
title: Spark应用的调用方法
tags: spark
categories: 大数据计算
toc: true
---
## spark应用提交到spark集群

+ 进入$SPARK_HOME，运行

```bash
bin/spark-submit --class spark.tutorial.geo.GeoTutorialExec --master spark://master:6066 --deploy-mode cluster file:///root/Downloads/spark-cluster-test/target/scala-2.10/spark-cluster-test-assembly-1.0.jar
```

+ spark应用包的生成 

在应用程序目录中执行包

```bash
sbt package
```

这样生成的spark包，关联包无法加载

需要在应用程序目录中执行

```bash
sbt assembly
```

在project中添加plugin.sbt，在项目根目录下添加assembly.sbt，在项目根目录下的build.sbt中依赖库spark的后面加上"provided"。这样，才能生成assembly的jar包。



## 远程网络服务调用spark应用，将其提交到spark集群

+ javascript调用spark restful 接口无法完成，因为hadoop1的jetty无法进行跨域访问

+ 采用java服务的方式来访问

+ 远程提交spark应用中，需要在集群的节点中添加/tmp/spark-events/目录

