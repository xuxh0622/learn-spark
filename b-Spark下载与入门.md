## 下载和安装Spark

参看[spark安装说明文档](https://github.com/xuxh0622/learn-linuxsoftware/blob/master/d-spark.md)

## Spark中Python和Scala的Shell

参看[官网入门教程](http://spark.apache.org/docs/latest/quick-start.html)，运行`paspark:python`或`spark-shell:scala`学习编程。

```scala
[java@** /]$ ./spark-shell
scala> val lines = sc.textFile("README.md")
scala> lines.count()
scala> lines.first()
```

## Spark核心概念介绍

每个Spark应用都是右一个驱动器程序来发起集群上的各种并行操作，驱动器程序通过一个SparkContex对象来访问Spark，shell启动的时候自动创建一个SparkContext对象，是一个叫做sc的变量。

## 独立应用

##### 添加依赖包

```maven
groupId = org.apache.spark
artifactId = spark-core_2.10
version = 1.2.0
```

##### 初始化SparkContext

> scala中初始化

```scala
import org.apache.spark.SparkConf
import org.apache.spark.SparkContext
import org.apache.spark.SparkContext._
//setMaster配置集群URL，local无需连接集群；setAppName设置应用名
val conf = new SparkConf().setMaster("local").setAppName("My App")
val sc = new SparkContext(conf)
```
