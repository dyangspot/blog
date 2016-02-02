title: "Stream extractor in Scala"
date: 2015-12-10 10:04:06
tags: scala
---

The way to destructure lists and streams is to use cons operator:
 *::* and *#::*, respectively:
 
```
val xs = 58 #:: 43 #:: 93 #:: Stream.empty
xs match {
  case first #:: second #:: _ => first - second
  case _ => -1
}
```

Why *#::* operator works in Scala?
Scala allows extractors to be used in a infix notation.  So, the

 ``` e(p1, p2) ``` 

, e is the extractor, p1 and p2 are parameters to be extracted from a given data structure. It can also be written as

 ``` p1 e p2 ```
 
Therefore, the infix operation pattern

 ```head #:: tail ```
 
  can also be written as 
  
  ```#::(head, tail) ``` 
  
And also our *PremiumUser* extractor:


```
object PremiumUser {
  def unapply(user: PremiumUser): Option[(String, Int)] = Some((user.name, user.score))
}
```

Can also be written as 

```
name premiumUser score
```

Let's go back to the stream extractor. The extractor is called for the initial stream xs that passed to the match block. Then, the extractor will return *Some((xs.head, xs.tail))*, the *xs.head* wis bound to 58 and the xs will be passed to the extractor again. Again, it will return the head and the tail as a *Tuple2* was wrapped in a *Some*, so that the second xs.head will be bound to t43. the tail is bound to the wildcard _. 
 
 
