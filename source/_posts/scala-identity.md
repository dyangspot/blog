title: "Scala中的identity"
date: 2015-11-30 22:31:20
tags: [scala]
---

针对与Future[Future[Seq[MyObject]]]类型，想要映射为Future[Seq[MyObject]]类型，这里有个小trick， 可以直接调用flatMap(identity),等同于flatMap(x => x)

```
scala> val result = Future{Future{Seq(1,2,34,4)}}
result: scala.concurrent.Future[scala.concurrent.Future[Seq[Int]]] = scala.concurrent.impl.Promise$DefaultPromise@55f57b35

scala> result.flatMap(identity)
res7: scala.concurrent.Future[Seq[Int]] = scala.concurrent.impl.Promise$DefaultPromise@505acb8b

```


