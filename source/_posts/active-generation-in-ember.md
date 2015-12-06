title: “Emerbjs中的Acive Generation”
date: 2015-04-04 23:13:26
tags: emberjs
---

所谓的Active Generation是指的在Emberjs中，如果*route* 找不到相对应的*handler*或*controller*下，Emerbjs往往会自动生成一个相对应的handler或controller。 如何判断Emberjs是否给你生成的呢？可以利用如下方法：

```
var app = window.app = Ember.Application.create({
	LOG_TRANSITIONS: true,
	LOG_ACTIVE_GENERATION:true
});
```

此外，在Emerbjs中，我们往往不需要定义*application* 或 *index*的route，因为Emerbjs会默认生成相对应的route。 
