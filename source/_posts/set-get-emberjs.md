title: “Emberjs中的set与get”
date: 2015-04-05 19:51:03
tags: emberjs
---

以Java为例，在面向对象的编程思想中，通过实现setter跟getter方法，来进行对属性的赋值和取值操作。 Emerbjs中如何实现属性的setter跟getter方法呢？
显而易见,**.set(“propertyName”,”propertyValue”)** 与**.get(”propertyName”)**方法来对object中的properties进行赋值与取值的操作.另外,通过Emerbjs的API可以了解到在Ember.observable mixing下，对Ember.object对象定义了观察功能，也就是说，对于使用set方法来赋值，或者使用get方法来取值，这些属性操作的一举一动都可以马上被Emberjs来观察（observe）到.