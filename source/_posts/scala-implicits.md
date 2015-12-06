title: "Scala中的Implicits"
date: 2015-12-03 21:36:27
tags: Scala
---

Implicits引入在scala中有两种形式， 一种是Parameters, 另一种是Conversions. 其中Conversions又包括Type conversions跟Extension Methods.

### Type conversions
```
implicit def b2a(a: B): A = new A
class A
class B
val a: A = new B
```

Compiler将会搜索implicits的B => A类型
其中Compiler的搜索范围包括
#### Resolve scope
- Declared implicits
- Imported implicits
- Implicitly imported implicits

#### Extended scope 

```
class Type

class T[Y]

class D extends Container
class Container
object Container {
	implicit def conv(c: Child): Type = new Type
}

class Child extends T[D]


val x: Type = new Child
``` 

给定以上代码, 创建的new Child实例对象赋给Type类型的x。这其中Child => Type类型的转换通过extended scope 来不停的向上寻找完成的。首先，Child类型继承与T[D], 而D 又继承与Container类，Container的半生对象object Container中implicit了 Child => Type类型的转换，所以new Child的实例化对象也是Type类型的.


