两种共享变量，`累加器`与`广播变量`。

##### 累加器

> 提供了将工作节点中的值聚合到驱动器程序中的简单语法。

```scala
val sc = new SparkContext(...)
val file = sc.textFile("file.txt")
val blankLines = sc.accumulator(0) // 创建Accumulator[Int]并初始化为0
val callSigns = file.flatMap(line => {  //flatMap惰性操作
  if (line == "") {
	blankLines += 1 // 累加器加1
	}
	line.split(" ")
})
callSigns.saveAsTextFile("output.txt")  //saveAsTextFile行动操作
println("Blank lines: " + blankLines.value)
```

