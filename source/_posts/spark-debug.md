---
title: spark调试
date: 2017-02-06 14:30:36
tags: spark
categories: 大数据计算
toc: true
---
# Spark应用程序调试方法

# Spark调试问题

## 远程调试方法

[这里](http://www.voidcn.com/blog/ASIA_kobe/article/p-5783303.html)
[here](http://www.voidcn.com/blog/ASIA_kobe/article/p-5783303.html)

### Spark源代码的跟踪调试

1 spark assembly库包加入IDEA的工程中，并关联上源码

2 设置环境变量
export JAVA_OPTS =-Xdebug -Xrunjdwp:transport=dt_socket,server=y,suspend=y,address=5005

+ JVM调试参数说明：

-Xdebug 启用调试特性

-Xrunjdwp 启用JDWP实现，包含若干子选项：

transport=dt_socket JPDA front-end和back-end之间的传输方法。dt_socket表示使用套接字传输。

address=5005 JVM在5005端口上监听请求，这个设定为一个不冲突的端口即可。

server=y y表示启动的JVM是被调试者。如果为n，则表示启动的JVM是调试器。

suspend=y y表示启动的JVM会暂停等待，直到调试器连接上才继续执行。suspend=n，则JVM不会暂停等待。


3 修改spark-class脚本
本机器上的spark-class脚本位于/hadoopLearning/spark-1.5.0-bin-hadoop2.4/bin目录
将脚本中的内容

```bash
done < <("$RUNNER" -cp "$LAUNCH_CLASSPATH" org.apache.spark.launcher.Main "$@")
```

修改为

```bash
done < <("$RUNNER" -cp "$LAUNCH_CLASSPATH" org.apache.spark.launcher.Main $JAVA_OPTS "$@")
```

4 IDEA添加远程调试

端口号与JVM设置的参数一致，host为spark-submit提交的机器master，即spark运行的机器
远程debug方式选择attach

5 在spark源码设置断点
比如 sparksubmit.scala中main方法设置断点

6 standalone方式运行spark app

```bash
$SPARK_HOME/bin/spark-submit --class spark.tutorial.geo.GeoTutorialExec --master spark://master:6066 --deploy-mode cluster file:///root/test-jar/spark-cluster-test.jar
```

返回
    
    Listening for transport dt_socket at address: 5005


7 启动idea中的远程调试，即可跟踪到spark源码sparksubmit.scala代码中


这种方式调试跟踪无法在spark app的代码上跟踪进去！！


### Spark应用的Driver调试

调试跟踪spark app的源代码在driver上运行的情况，无须单独设置jvm参数

1 提交spark-submit时添加如下参数

```bash
--driver-java-options -agentlib:jdwp=transport=dt_socket,server=y,suspend=y,address=5005
```
address端口可以设定为其他值，如果5005端口被占用了的话。

2 启动idea的调试
在webui地址master:8080查看driver的机器，设置remote debug的机器及端口，debug选择attach方式。即可跟踪spark app的代码在driver上运行的那一部分。

### Spark应用的Excutor调试

调试跟踪spark app的源代码在excutor上运行的情况（此种方式，还未成功跟踪进去！！！）

1 提交spark-submit时添加如下参数

```bash
--num-executors 1 --executor-cores 1 --conf "spark.executor.extraJavaOptions=-agentlib:jdwp=transport=dt_socket,server=n,address=master:5006,suspend=n"
```

2 启动idea的调试

由于submit中excutor的debug方式是非suspend的，所以idea的调试可以先于spark-submit启动，由于不一定excutor在那个机器，可以每个机器都设置一个，remote debug方式选择listen。

Replace the address with your local computer's address. (It's a good idea to test that you can access it from your spark cluster).

In this case, start your debugger in listening mode, then start your spark program and wait for the executor to attach to your debugger. It's important to set the number of executors to 1 or multiple executors will all try to connect to your debugger, likely causing problems.

These examples are for running with sparkMaster set as yarn-client although they may also work when running under mesos. If you're running using yarn-cluster mode you may have to set the driver to attach to your debugger rather than attaching your debugger to the driver, since you won't necessarily know in advance what node the driver will be executing on.

参见[网站](http://stackoverflow.com/questions/29090745/debugging-spark-applications)
