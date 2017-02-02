---
title: 将Spark运行在mesos上
---

运行启动各个集群服务前，必须保证/etc/hosts中的机器名和对应ip是在每个机器上实际正确对应的！！！


## 启动mesos

进入/usr/local/mesos/sbin执行
```bash
$ ./mesos-start-cluster.sh
```
## 启动hadoop 1

进入/usr/local/hadoop/hadoop-1.2.1/sbin执行
``` bash
$ ./start-all.sh
```

将spark二进制包上传至hdfs,进入/usr/local/spark执行
``` bash
$ hadoop fs -put spark-1.4.1-bin-hadoop1.tgz /spark-1.4.1-bin-hadoop1.tgz
```
## spark on mesos的配置与启动

+ 修改spark的配置文件

```bash
$ cd /usr/local/spark/spark-1.4.1-bin-hadoop1/conf
$ sudo vi spark-env.sh
```

添加以下几行内容

	#mesos
	export MESOS_NATIVE_JAVA_LIBRARY=/usr/local/lib/libmesos.so
	export SPARK_EXECUTOR_URI=hdfs://spark-1.4.0-bin-hadoop2.6.tgz

打开配置文件
   
```bash
$ sudo vi spark-defaults.conf
```

添加以下几行内容

	#mesos
	spark.executor.uri hdfs://spark-1.4.1-bin-hadoop1.tgz
	spark.master mesos://Master:5050

+ 启动spark

  进入/usr/local/spark/spark-1.4.1-bin-hadoop1/sbin

```bash
$ ./start-history-server.sh
$ ./start-all.sh
$ ./start-mesos-dispatch.sh
$ cd ..
$ cd bin
$ ./spark-shell
```
### 问题1
master机器jps命令下，spark的Master未启动
+  原因
master的logs文件中发现命令行的ip为老局域网ip，在spark工作目录的conf下的spark-env.sh文件中，修改SPARK_MASTER_IP为新局域网中的master机器ip。

### 问题2
在mesos之上运行spark的应用程序,出现下面问题
	Failed to load native Mesos library from /home/dummy/.java/jdk1.6.0_45/jre/lib/amd64/server:/home/dummy/.java/jdk1.6.0_45/jre/lib/amd64:/home/dummy/.java/jdk1.6.0_45/jre/../lib/amd64:/usr/java/packages/lib/amd64:/usr/lib64:/lib64:/lib:/usr/lib
	java.lang.UnsatisfiedLinkError: no mesos in java.library.path

+ 原因

    spark_env.sh不会自动配置，需要运行spark_env.sh
 
    ```bash
	$ source spark_env.sh
    ```

可以考虑将这个执行语句放到.bashrc文件中。

### 问题3
在intelliJ Idea下运行spark应用，出现

	New master detected at master@192.168.1.103:5050
	No credentials provided. Attempting to register without authentication

+ 原因

     spark未运行start-mesos-dispatch.sh ?