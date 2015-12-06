title: “Emberjs中的Computed Properties”
date: 2015-04-05 19:28:34
tags: emberjs
---

Emberjs中的Computed Properties指的是其中属性的值依赖于给定的另外一个属性的值。 而被依赖的属性可以来自于其他的controller，model等。 在已经指定属性间依赖关系的前提下，如果被依赖属性发生改变，依赖属性也会根据被依赖属性的值进行改变。 例如：

```
applicationName:function(){
	var st=this.get(‘searchTerms’);
	if(st){
		return st+”???”
	else{
		return “Rock’n Roll Call”
	}
}.property(‘searchTerms’)
```

其中，*applicationName* 作为computed property， 而searchTerms作为dependency.当然，单个computed propriety还可以依赖多个指定属性的值，例如：

```
Person = Ember.Object.extend({
  // these will be supplied by `create`
  firstName: null,
  lastName: null,

  fullName: function() {
    return this.get('firstName') + ' ' + this.get('lastName');
  }.property('firstName', 'lastName')
});

var ironMan = Person.create({
  firstName: "Tony",
  lastName:  "Stark"
});

ironMan.get('fullName'); // "Tony Stark"
```
