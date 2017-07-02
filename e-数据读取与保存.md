##### 文件格式

> 文本文件

```scala
//读取
val input = sc.textFile("README.md")  //每行都是一个RDD元素
val input = sc.wholeTextFile("README.md")  //返回一个pair RDD，键是文件名，值是文件内容
//写入
result.saveAsTextFile(outputFile)
```

> JSON

```scala
//写入
result.filter(p => p.lovesPandas).map(mapper.writerValueAsString(_)).saveAsTextFile(outputFile)
```

> 逗号分割值与制表符分割值

```scala
//读取
//通过textFile读取
import Java.io.StringReader
import au.com.bytecode.opencsv.CSVReader
val input = sc.textFile("READE.md")
val result = input.map{line => 
	val reader = new CSVReader(new StringReader(line))
  	reader.readNext()
}
//读取完成CSV
case class Person(name:String, favoriteAnimal:String)
val input = sc.wholeTextFiles(input)
val result = input.flatMap{case(_,txt) => 
  val reader = new CSVReader(new StringReader(txt))
  reader.readAll().map(x => Person(x(0),x(1)))
}
//保存
pandaLovers.map(person => List(person.name, person.favoriteAnimal).toArray)
	.mapPartitions{people =>
	val stringWriter = new StringWriter();
	val csvWriter = new CSVWriter(stringWriter);
	csvWriter.writeAll(people.toList)
	Iterator(stringWriter.toString)
}.saveAsTextFile(outFile)
```

> SequenceFile

```scala
//读取
val data = sc.sequenceFile(inFile, classOf[Text], classOf[IntWritable]).
map{case (x, y) => (x.toString, y.get())}
//保存
val data = sc.parallelize(List(("Panda", 3), ("Kay", 6), ("Snail", 2)))
data.saveAsSequenceFile(outputFile)
```

> 对象文件

```scala
objectFile()
saveAsObjectFile
```

> Hadoop输入输出格式

```scala
val input = sc.hadoopFile[Text, Text, KeyValueTextInputFormat](inputFile).map{
case (x, y) => (x.toString, y.toString)
}
```

> Apache Hive

```scala
//读取
org.apache.spark.sql.hive.HiveContext
val hiveCtx = new org.apache.spark.sql.hive.HiveContext(sc)
val rows = hiveCtx.sql("SELECT name, age FROM users")
val firstRow = rows.first()
println(firstRow.getString(0)) // 字段0是name字段
```

##### 数据库

> Cassandra

```scala
//配置Cassandra属性
val conf = new SparkConf(true)
		.set("spark.cassandra.connection.host", "hostname")
val sc = new SparkContext(conf)
```



