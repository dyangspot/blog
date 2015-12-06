title: "reactive-prog-recap"
date: 2015-04-13 22:43:26
tags: [scala,reactive programming]
---

### Representation of JSON in Scala

```
abstract class JSON
case class JSeq(elems:List[JSON]) extends JSON
case class JObj(bindings:Map[String,JSON]) extends JSON
case class JNum(num:Double) extends JSON
case class JStr(str:String) extends JSON
case class JBool(b: Boolean) extends JSON
case object JNull extends JSON
```

Example 

```
{ ” firstName ” : ” John ” ,
  ” lastName ” : ” Smith ” ,
   ” address ”: {
  ” streetAddress ”: ”21 2 nd Street ” ,
	” state ”: ” NY ” ,
	” postalCode ”: 10021
 },
” phoneNumbers ”: [
  { ” type ”: ” home ” , ” number ”: ”212 555 -1234” } ,
  { ” type ”: ” fax ” , ” number ”: ”646 555 -4567” }
 ]
}
```
Scala Rep

```
val data = JObj(Map(
”firstName” -> JStr(”John”),
”lastName” -> JStr(”Smith”),
”address” -> JObj(Map(
”streetAddress” -> JStr(”21 2nd Street”),
”state” -> JStr(”NY”),
”postalCode” -> JNum(10021)
)),
”phoneNumbers” -> JSeq(List(
JObj(Map(
”type” -> JStr(”home”), ”number” -> JStr(”212 555-1234”)
)),
JObj(Map(
”type” -> JStr(”fax”), ”number” -> JStr(”646 555-4567”)
)) )) ))
```

### Functions Are Objects

In Scala, every concrete type is the type of some class or trait.
The function type is no exception. A type like

JBinding => String

is just a shorthand for: scala.Function1[JBinding,String]
where scala.Function1 is a trait and JBinding and String are its type arguments.

#### The Function1 Trait
```
trait Function1[-A, +R]{
	def apply(x:A): R
}
```

The pattern matching block

```{case (key,value) => key + ": " + value}

```
expands to the Function1 instance:

```
new Function1[JBinding,String]{
	def apply(x:JBinding) = x match{
		case(key,value) => key + ": " + show(value)
	}
}
```

### Subclassing Functions
One nice aspect of functions being traits is that we can subclass the function type.
e.g. mpas care functions from keys to values:

trait Map[Key,Value] extends (Key=>Value)...

Sequences are functions from Int indices to values:

trait Seq[Elem] extends (Int=>Elem)
for sequence(and array) indexing

###Partial Matches

{case "ping" =>"pong"} 
can be given type String=>String

```
val f:String=> String ={case "ping" => "pong"}
                           //> f  : String => String = <function1>
f("ping")                        //> res1: String = pong
```

### Partial Functions

```
val f:PartialFunction[String,String] = {case "ping" => "pong"}
f.isDefinedAt("ping")  // true
f.isDefinedAt("pong") // false
```
The partial function trait is defined as follows:
trait PartialFunction[-A,+R] extends Function1[-A,+R]{
	def apply(x:A):R
	def isDefinedAt(x:A):Boolean
}

### Partial Function Objects

if the expected type is a PartialFunction, the Scala compiler will expand {case "ping" => "pong"} 
as follows:

```
new PartialFunction[String,String]{
	def apply(x:String) = x match{
		case "ping" => "poing"
	}
	def isDefinedAt(x:String) = x match{
		case "ping" => true
		case _ => false
	}
}
```
