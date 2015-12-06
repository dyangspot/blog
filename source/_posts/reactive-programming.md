title: "reactive-programming-intro"
date: 2015-04-13 22:02:21
tags: [scala, reactive programming]
---

### Changing Requirements...
	             	     
Server nodes          10's      1000's 
Response times      seconds   milliseconds
Maintenance downtimes  hours      none
Data volume           GBs        TBs -> PBs	

|   | 10 years ago  |   Now |   |   |
|---|---|---|---|---|
| Server nodes   | 10's  | 1000's   |   |   |
| Response times  | seconds  | milliseconds  |   |   |
| Maintenance downtimes  | hours  | none  |   |   |
| Data volume   | GBs   | TBs -> PBs	  |   |   |

### Need New Architectures

Previously: Managed servers and containers

Now: Reactive application

+ event-driven -> react to events
+ scalable -> react to load
+ resilient -> react to failures
+ responsive -> react to users

### Event-Driven
Traditionally: Systems are composed of multiple threads, which communicated with shared, synchronized state.

+ String coupling, hard to compose

Now: Systems are composed from loosely coupled event handlers.
+ Events can be handled asynchronously, without blocking.
none-blocking: resource can be utilized more efficiently.

### Scalable 
An application is **scalable** if it is able to be expanded according to its usage.

+ scale up: make use of parallelism in multi-core systems.
+ scale out: make use of multiple server nodes in a data center or in the cloud.

Important for scalability: Minimize shared mutable state.
Important for scale out: Location transparency,resilience.

Location transparency: it doesn't matter where the location is ,could be at the same computer or across the internet.

### Resilient

An application is **resilient** if it can recover quickly from failures.
Failures can be:
+ software failures
+ hardware failures, or
+ connection failures.

Typically, resilience cannot be added as an afterthought; it needs to be part of the design from the beginning.
Needed:
+ loose coupling.
+ strong encapsulation of state.
+ pervasive supervisor hierachies.

### Responsive

An application is **responsive** if it provides rich, real-time interaction with its users even under load and in the presence of failures.

Responsive applications can be built on an event-driven,scalable, and resilient architecture.

### Call-backs

Handling events is often done using call-backs. E.g. using Java observers:
```
class Counter extends ActionListener{
	private var count = 0;
	button.addActionListener(this)
	
	def actionPerformed(e:ActionEvent):Unit = {
		count +=1
	}
}
```

Problems:
+ needs shared mutable state.
+ cannot be composed.
+ leads quickly to "call-back hell"

Soveld in this course:
to get **composable** event abstractions.
+ Events are first class.
+ Events are often represented as messages.
+ Handlers of events are also first-class.
+ Complex handlers can be composed from primitive ones.


	