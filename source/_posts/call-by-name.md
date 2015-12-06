title: "Scala中的=>, ()=> 与 Unit=>区别"
date: 2015-04-26 00:54:46
tags: scala
---

在此总结下Call-by-Name的传递函数的类型问题.


### expr:=>Type
=>Type 表示call by name. [wiki解释](http://en.wikipedia.org/wiki/Evaluation_strategy#Call_by_name)。 call by name本质上是利用给定的函数参数替换函数中的参数. 例如:

```
def f(x:=>Int)= x*x

```

调用函数 

```
var y=0
f(y+=1;y)
```

代码执行如下

```
{y+=1;y} *{y+=1;y}
```

### expr:()=>Type

()=>Type 表示Function0, 即0-arity Functions.表示一个函数不给定初始参数，但返回结果是Type类型.  例如，调用函数size()，表示的是不给定输入参数，size（）执行后返回一个数值.
**注意**： 与匿名函数表示进行区分，例如

```() => println("I'm an anonymous function")
```

以上匿名函数类型是 ()=>Unit,所以我们也可以写作:

```val expr: () => Unit = () => println("I'm an anonymous function")
```
[A Tour of Scala:匿名函数](http://www.scala-lang.org/old/node/133)
### Unit => Type

这个实际上是Function1，其中第一个参数类型为Unit。此外，其他书写方式包括(Unit) => Type或者 Function1[Unit,Type]。 
其中使用*Unit*的目的是表示 给定的唯一的参数值，我们不关心这个具体的值，固用Unit来表示。例如：
···
def f(x:Unit) = ...
···

参数x只有一个值，我们也不需要去接收这个值，我们可以应用到chaining 函数上面

```
val f = (x: Unit) => println("I'm f")             //> f  : Unit => Unit = <function1>
val g = (x: Unit) => println("I'm g")             //> g  : Unit => Unit = <function1>
val h = f andThen g                               //> h  : Unit => Unit = <function1>

```

andThen 只能定义在Function1，我们用来chaining的函数返回Unit. 也就是说我们需要定义Function1[Unit,Unit]的函数才能做chaining.