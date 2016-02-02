title: "@在Scala中的用法总结"
date: 2015-12-13 23:06:28
tags: scala
---

最近在看[The Neophyte's Guide to Scala ](http://danielwestheide.com/scala/neophytes.html)的时候，在part1-extractors跟part3-patterns中都提到了*@*，对此进行总结下，上述文章中出现的代码例子如下：

extractors：

```
val user: User = new FreeUser("Daniel", 2500, 0.8d)
user match {
  case freeUser @ premiumCandidate() => initiateSpamProgram(freeUser)
  case _ => sendRegularNewsletter(user)
}
```

patterns everywhere：

```
val lists = List(1, 2, 3) :: List.empty :: List(5, 3) :: Nil

for {
  list @ head :: _ <- lists
} yield list.size
```

*@*在scala中被称之为Pattern Binder， 给出的[定义](http://www.scala-lang.org/files/archive/spec/2.11/08-pattern-matching.html#pattern-binders)如下，

```
  Pattern2        ::=  varid `@' Pattern3
```

A pattern binder x@p consists of a pattern variable x and a pattern p. The type of the variable x is the static type T of the pattern p. This pattern matches any value v matched by the pattern p, provided the run-time type of v is also an instance of T, and it binds the variable name to that value.

定义如上，根据定义可以我们每次的实例x都要被绑定到模式p上面， 可以理解为x是模式p的类型T的实例。 


