title: "处理Nested Future结构总结"
date: 2015-11-30 18:10:23
tags: [scala, play]
---

Play中经常会遇到在结构体中循环嵌套Async调用， 导致最后出现Future[Future[myObject]]类似代码，我们期望最终返回结果为Future[myObject]类型，而非嵌套Future的结构。在异步调用中，为了实现None-blocking I/O，应当尽量避免用Await方法.
### flatMap

```
def flatMap[A](f: T => Future[A]): Future[A]
 
def map[A](f: T => A): Future[A]		
```

根据上述定义，我们可以用flatMap方法将一个Future[A]类型映射成我们想要的类型

```
TemplateStoreDao.findAll(page, perPage) flatMap  { templates => // Seq[Tempalte]
      val appList = new scala.collection.mutable.ArrayBuffer[JsObject]()
      val response = ObjectStoreDao.query(query) map { tryObj => // Try[Object]
        tryObj map {obj => 
          val favoriteApps = (obj(0) \ "data" \ "vars" \"favoriteApps").as[JsArray]
          for {
            template <- templates
            appId <- favoriteApps.value
          } yield {
            val jsonObj = Json.toJson(obj).as[JsObject]
            val prefObj = if (appId.as[String] == template.id) {
              val favField = ("fav" -> JsBoolean(true))
              jsonObj + favField
            } else {
              jsonObj
            }
            appList += prefObj	// add to list
          }
          // convert favorite list to JsArray
          val jsArray = appList.foldLeft(JsArray())((acc, x) => acc ++ Json.arr(x))
          jsArray.as[JsArray]
        } getOrElse {
          Json.toJson(templates).as[JsArray]
        }
      }
		// response is Future[JsArray]
      response flatMap { res => // res is JsArray
        Future.successful(res.value map(_.as[TTemplate])) // convert JsObject to Scala Object , will return Future[Seq[Object]] 
      }
    }

```

如上所示， 我们对于处理Future的时候先进行flatMap操作，将Future[A] 包裹的T类型暴漏出来，进行各种操作，最后再返回一个 Future[A] 类型

### for loop
能用flatMap情况下，我们当然也可以用for 循环来解决，例如

```
val future3 = for {
	x <- future1;
	y <- future2
} yield (x + y)
```

上述代码可以重写为
```
val future3 = future1.flatMap(x => future2.map(y => x+y))
```

###  Future.sequence => Future[Seq[Future[MyObject]]]
对于Future[Seq[Future[MyObject]]]情况， 这是一群Future[MyObject]组成的Sequence，最外层包裹着一个Future。 这里可以用Future.sequence 方法把Future[Seq[Future[MyObject]]] 映射为Future[Future[Seq[MyObject]]]，再用flatMap(identity)得到我们想要的类型Future[Seq[MyObject]]


```
import scala.concurrent.Future
import scala.concurrent.ExecutionContext.Implicits.global

scala> val result = Future{Seq(Future(1),Future(2),Future(3))}
result: scala.concurrent.Future[Seq[scala.concurrent.Future[Int]]] = scala.concurrent.impl.Promise$DefaultPromise@7e349560

scala> val resultSeq = result.map(Future.sequence(_)) 
res10: scala.concurrent.Future[scala.concurrent.Future[Seq[Int]]] = scala.concurrent.impl.Promise$DefaultPromise@6d1818bd

scala> val resultSeq = result.map(x => Future.sequence(x))
resultSeq: scala.concurrent.Future[scala.concurrent.Future[Seq[Int]]] = scala.concurrent.impl.Promise$DefaultPromise@114e9c16

scala> resultSeq.flatMap(identity)
res12: scala.concurrent.Future[Seq[Int]] = scala.concurrent.impl.Promise$DefaultPromise@b50676f

```

### Future.traverse



### Flatten 

```
implicit def flatten[A](fofoa: Future[Option[Future[Option[A]]]]): Future[Option[A]] = {
	fofoa.flatMap(_.getOrElse(Future.successful(None)))
}
```

借鉴上述flatten方法


### Reference

[Composing depending futures](https://tersesystems.com/2014/07/10/composing-dependent-futures/)
[Get rid of Scala Future nesting](http://stackoverflow.com/questions/20276872/get-rid-of-scala-future-nesting)
[The Neophyte's Guide to Scala Part 8: Welcome to the Future](http://danielwestheide.com/blog/2013/01/09/the-neophytes-guide-to-scala-part-8-welcome-to-the-future.html)
[Futures and promises](http://docs.scala-lang.org/overviews/core/futures.html)
