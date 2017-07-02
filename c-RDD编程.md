核心抽象-弹性分布式数据集（Resilient Distributed Dataset，简称RDD）

##### RDD操作

> 转换操作：（filter、map）定义新的RDD，惰性求值。
>
> 行动操作：（count）返回结果或把结果写入外部系统的操作，来触发一次并行计算。

##### 创建RDD

> 读取外部数据集：`sc.textFile("README.md")`
>
> 对集合进行并行化：`val lines = sc.parallelize(List("pandas","i like"))`

##### 常见的转换操作和行动操作

###### 转换操作

```scala
val lines = sc.parallelize(List("coffee panda","happy panda"))
//filter接收一个函数，返回满足该函数的元素放入新RDD中返回
val result = lines.filter(line =>line.contains("panda"))  
//map接收一个函数用于RDD中每个元素，返回结果：{["coffee","panda"],["happy","panda"]}
val result1 = result.map(line => line.split(" "))  
//flatMap每个输入元素生成多个输出元素：{"coffee","panda","happy","panda"}
val result2 = result.flatMap(line => line.split(" "))  
//伪集合操作
val rdd = sc.parallelize(List(1, 2, 3))
val other = sc.parallelize(List(3, 4, 5))
rdd.union(other)  //返回包含两个RDD的所有元素的RDD：{1, 2, 3, 3, 4, 5}
rdd.intersection(other)  //只返回两个RDD钟都有的元素：{3}
rdd.subtract(other)  //返回只存在于第一个中，不存在第二个RDD中元素：{1, 2}
rdd.cartesian(other)  //笛卡尔积，混合所有可能：{(1,3),(1,4)....3,5()}
```

###### 行动操作

```scala
val rdd = sc.parallelize(List(1, 2, 3, 3))
rdd.collect()  //返回所有元素：{1, 2, 3, 3}
rdd.count()  //计算元素个数：4
rdd.countByValue()  //各元素出现次数：{(1,1),(2,1),(3,2)}
rdd.take(2)  //返回2个元素：{1, 2}
top(2)  //返回最前面2个元素：{3, 3}
rdd.reduce((x,y) => x+y)  //并行整合所有元素：9
rdd.fold(0)((x,y) => x+y)  //和reduce一样，需要初始值：9
rdd.aggregate((0,0))  //h额reduce相似，返回不同类型函数：(9,4)
rdd.foreach(func)  //对每个元素使用给定函数
```

##### 持久化（缓存）

```scala
val result = input.map(x => x*x)
result.persist(StorageLevel.DISK_ONLY)  //persist缓存，里面是缓存级别
```



