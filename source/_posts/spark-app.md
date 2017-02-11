---
title: Spark应用的调用方法
tags: spark
categories: 大数据计算
toc: true
---
## spark应用编译注意事项
sbt编译时运行sbt compile时build.sbt文件中spark的lib依赖不加provided

运行sbt assembly
时build.sbt文件中spark的lib依赖需要加provided，否则运行sbt assembly时有文件冲突。
![](build-sbt-spark.jpg)

另外，在assembly.sbt中为避免冲突，添加如下代码：
    mergeStrategy in assembly <<= (mergeStrategy in assembly) { (old) =>
      {
	    case "META-INF/ECLIPSEF.RSA" => MergeStrategy.last
	    case "META-INF/mailcap" => MergeStrategy.last
	    case "plugin.properties" => MergeStrategy.last
	    case "log4j.properties" => MergeStrategy.last
	    case x => old(x)
      }
    }
    

## Spark集群的启动
### mesos集群启动
```bash
$MESOS_HOME/sbin/mesos-start-all.sh
```

+ 通过webUI http://master:5050可查看mesos集群状态

### Hadoop2集群启动
```bash
$HADOOP_HOME/sbin/start-all.sh
```

+ 通过webUI http://master:50070查看hdfs文件系统
+ 通过webUI http：//master:8088查看yarn集群状态

```bash
$SPARK_HOME/sbin/start-all.sh
$SPARK_HOME/sbin/start-history-server.sh
$SPARK_HOME/sbin/start-mesos-dispatcher.sh -m mesos://master:5050
```

-  通过webUI http：//master:8080查看spark集群状态
-  通过webUI http：//master:18080 查看spark集群历史状态
-  通过webUI http：//master:4040查看运行中的spark计算状态

**上述所有集群启动命令完成后，提示启动过程的日志文件，启动中出现的状态、问题均记录在对应log文件中，有些情况下不同计算服务的端口是有变化，可以在日志中看到！！**



## spark应用提交到spark集群
### 将应用jar提交到hadoop

注意，如果提交到spark on mesos来计算的话，需要将mesos中的配置文件mesos\_master\_env.sh及mesos\_slave\_env.sh中加入HADOOP_HOME的设置,这样mesos集群才能运行hadoop的命令。

### 一般情况 
进入$SPARK_HOME，运行

```bash
$SPARK_HOME/bin/spark-submit --class spark.tutorial.geo.GeoTutorialExec --master spark://master:6066 --deploy-mode cluster file:///root/test-jar/spark-cluster-test.jar
```

**注意：file:///.../**.jar，jar包应该在集群所有节点上的相同目录下均有该包，file后面是三个反斜杠！！！**

或者将jar上传至hadoop集群文件系统，用hdfs：//路径也可以

### spark应用包的生成 

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


### spark on mesos的情况
必须先在spark home的sbin目录下启动
```bash
./start-mesos-dispatcher.sh -m mesos://192.168.1.103:5050
```
查看其log文件，如图
![](mesos-dispatcher-port.jpg)
由此可见7077端口已被standalone spark集群占据了，故spark on mesos采用7078端口！！，后面用spark-submit提交spark app需要采用7078。

然后再提交spark应用
``` bash
$SPARK_HOME/bin/spark-submit --class spark.tutorial.geo.GeoTutorialExec --master mesos://master:7078 --deploy-mode cluster file:///root/test-jar/spark-cluster-test.jar
```

### 远程网络服务调用spark应用，将其提交到spark集群

+ javascript调用spark restful 接口无法完成，因为hadoop1的jetty无法进行跨域访问

+ 采用java服务的方式来访问

+ 远程提交spark应用中，需要在集群的节点中添加/tmp/spark-events/目录

