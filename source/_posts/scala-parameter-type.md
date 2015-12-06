title: "Scala中的call by name与call by value区别"
date: 2015-04-26 00:32:05
tags: scala
---

在Scala中对于参数类型T 或=>T，前者被称为call by value,后者叫call by name.
以上两种传递参数有什么区别呢？以下进行详细的说明，参照[stackoverflow](http://stackoverflow.com/questions/11992134/scala-parameter-of-type-t-or-t)上的解答.

首先给与两个函数分别为：

```
def stringGen:String = util.Random.nextInt().toString

def callByValue(s: String) = {...}

def callByName(s: =>String) = {...}
```
其中，callByValue 函数中传递的是String类型的参数s， CallByName函数传递的是**=> String**的函数s， 函数s的返回结果为String类型.
