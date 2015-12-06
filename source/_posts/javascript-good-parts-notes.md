title: "javascript-the-good-parts-notes"
date: 2015-04-09 22:36:16
tags: javascript
---

##语法

### 2.4 字符串
Javascript再被创建的时候，Unicode是一个16位的字符集，所以Javascript中的所有字符都是16位的。
字符串是不可变的，一旦创建，就永远无法改变。
字符串有一个length属性。
If做条件判断时，为false的情况：

+ false
+ null
+ undefined
+ 空字符串“ ”
+ 数字0
+ 数字NaN
其他所有值都为true，包括true 跟字符串"false"，包括所有对象

### 3.1 对象自变量
对象属性的访问
object["propertyName"] or object.property

### 3.6 反射
确定对象属性类型: typeof

```
typeof flight.number   // 'number'
typeof flight.number	// 'string'
typeof flight.arrival  // 'object'
typeof flight.manifest	// 'undefined'
typeof flight.toString	// 'function'
typeof flight.constructor // 'function'
```

hasOwnProperty方法，可以用来判断是否是对象独有属性，如果是，将返回true。注意：hasOwnProperty不会检查原型链. - 用来判断某个属性是否定义
flight.hasOwnProperty('number')		// true
flight.hasOwnPeoperty('constructor')  // false

### 3.7 枚举
利用for in来遍历一个对象中所有属性名- 枚举过程列出所有的属性，其中包括函数和原型中的属性. 
可以用hasOwnProperty或 typeof 来进行筛选.

```
var name;
for(name in another_stooge){
	if(typeof another_shooge[name] !== 'function'){
		document.writeln(name + ';' + another_stooge[name]);
	}
}
``` 
尽量避免使用for in来遍历属性， 推荐用for. 可以避免发掘出原型链中的属性.
```
var i;
var properties = {
	'first-name',
	'last-name',
	'gender',
	'profession'
};
for(i = 0; i < properties.length;i++){
	...
}

```

### 4.1 函数对象
函数即对象Object，也就是“name/value”的集合。 
由Object生成的objects对象继承于Object.prototype
Function objects对象继承于Function.prototype

### 4.3 调用
JavaScript的四种调用模式： 方法调用模式，函数调用模式，构造器调用模式，跟apply调用模式.

- 方法调用模式
当函数作为对象属性时，被称之为方法. 
当对象的一个方法被调用时候，this被绑定到该对象.
如果一个调用表达式包含一个属性存取表达式（object.property 或object[property]）,这时表达式也会被当作一个方法来调用.

另外， 方法可以使用this来访问对象.
- 函数调用模式
指的是函数不作为对象属性时，仅仅会被当作一个函数来调用:
```var sum = add(3,4);  
```
在此调用模式下，this被绑定到全局对象.  js语言设计上的bug.
为何错误： 假设设计正确，当内部函数被调用时，this应该仍然绑定到外部函数的this变量。这个设计错误的后果是方法不能利用内部函数来帮助它工作，因为内部函数的this被绑定了错误的值，所以不能共享该方法对对象的访问权.

解决方案：将该方法定义一个变量并赋值为this，那么内部函数就可以通过这个变量访问到this.

```
myObject.double = function(){
	var that = this;  // 访问权绑定到that
	var helper = function(){
		that.value = add(that.value,that.value)
	};
	helper();  //以函数形式调用helper
}
//以方法形式调用double
myObject.double();

```

- 构造器调用
用new关键字来创建一个新对象，其对象具有一个隐藏连接到该函数prototype的成员,这个成员也被绑定到新创建的对象上.

```
var Quo = function(string){
	this.status = string;
};

//给Quo所有实例提供一个get_status的公共方法
Guo.prototype.get_status = function(){
	return this.status;
};

//创建Quo实例
var myQuo = new Quo("confused");
document.writeln(myQuo.get_status());
```
- Apply调用


### 4.5 返回
一个函数总是会有一个返回值，没有指定return的，则返回undefined.
如果函数前面加上new 来调用，返回值不是一个对象，则返回this

### 4.6 异常
throw 与 try...catch

throw 包含一个可识别异常类的name属性和解释原因的message熟悉。
```
var add = function(a,b){
	if(typeof a !== 'number' || typeof b !== 'number'){
		throw{
			name:'TypeError',
			message:'add needs numbers'
		};
	}
	return a + b;
}
```
try...catch， 一个try语句只包含一个catch代码块

```
var try_it = function(){
	try{
		add("seven");
	}catch(e){
		document.writeln(e.name + ": " + e.message );
	}
}
```
