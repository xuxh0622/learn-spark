##### 创建Pair RDD

> 创建键值对RDD

```scala
val pairs = lines.map(x => (x.split(" ")(0),x))
```

##### Pair RDD的转化操作

> 针对单个pair RDD

```scala
val rdd = sc.parallelize(Map{(1, 2), (3, 4), (3, 6)})
rdd.reduceByKey((x,y) => x+y)  //合并具有相同键的值：{(1,2), (3,10)}
rdd.groupByKey()  //对相同键的值分组[k,Iterable[V]]：{(1,[2]),(3,[4,6])}
rdd.combineByKey()
rdd.mapValues(x => x+1)  //对每个值执行函数：{(1,3),(3,5),(3,7)}
rdd.flatMapValues(x => (x to 5))  //对每个值迭代：{(1,2),(1,3),(1,4),(1,5),(3,4),(3,5)}
rdd.keys()  //返回只包含键的RDD：{1,3,3}
rdd.values()  //返回只包含值得RDD：{2,4,6}
rdd.sortByKey()  //返回根据键排序的RDD：{(1,2),(3,4),(3,6)}
```

> 针对两个pair RDD

```scala
val rdd = sc.parallelize(Map{(1, 2), (3, 4), (3, 6)})
val other = sc.parallelize(Map{(3, 9)})
rdd.subtractByKey(other)  //删除RDD中见与other中相同元素：{(1,2)}
rdd.join(other)  //行内连接：{(3,(4,9)),(3,(6,9))}
rdd.rightOuterJoin(other)  //右外连接：{(3,(Some(4),9)),(3,(Some(6),9))}
rdd.leftOuterJoin(other)  //左外连接：{(1,(2,None)),(3,(4,Some(9))),(3,(6,Some(9)))：
rdd.cogroup(other)  //相同键数据分组[k,(Iterable[v],Iterable[w])]：{(1,([2],[])),(3,([4,6],[9]))}
```

> 针对普通RDD函数

```scala
val rdd = sc.parallelize(Map{(1, 2), (3, 4), (3, 6)})
rdd.filter{case (key, value) => value < 5}  //过滤：{(1, 2), (3, 4)}
```

> 聚合操作

```scala
//计算每个键对应的平均值
rdd.mapValues(x => (x,1)).reduceByKey((x,y) => (x._1 + y._1,x._2 + y._2))
//对单词进行计数
input.flatMap(x => x.split(" "))  //{"a","a","b"}
	.map(x => (x,1))  //{("a",1),("a",1),("b",1)}
	.reduceByKey((x,y) => x+y)  //{("a",2),("b",1)}
input.flatMap(x => x.split(" ")).countByValue()  //{("a",2),("b",1)}
```

##### Pair RDD的行动操作

```scala
val rdd = sc.parallelize(Map{(1, 2), (3, 4), (3, 6)})
rdd.countByKey()  //对每个键对应的元素分别结束：{(1,1),(3,2)}
rdd.collectAsMap()  //将结果以映射表形式返回，以便查询：Map{(1,2),(3,4),(3,6)}
rdd.lookup(3)  //返回给定键对应所有值：[4,6]
```

##### 数据分区







