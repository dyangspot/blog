title: "Ember Tricks"
date: 2015-04-08 23:16:05
tags:
---

### router.js

```
this.resource('product',{path,'/user'});
```

/user -> ProductRoute -> Product -> ProductController -> product.handlebars

### Web Components

demo.hardlebars

```{{doge-text soText="evail" suchText="trout"}}
```

components/doge-text

```<img src="images/doge.jpg"> wow so <b>{{soText}}</b> {{suchText}}</b>
```

### Tick #1 Computed Property Macros

object.js - object model

```
App.User = Ember.Object.extend({});
var user = App.User.create({name:'Cool User'});
console.log(user.get('name')); // Cool User
```
staff.js

```
App.User = Ember.Object.extend({
	staff:function(){
		return this.get('moderator') || this.get('admin');
	}.property('moderator','admin')
}); 
var user = App.User.create({moderator:true});
console.log(user.get('staff')); // true   -> cached
```

macros_extras.js

```
App.User = Ember.Object.extend({
	validUsername: Ember.computed.match('username',/^[a-z0-9]+$/),
	adult:Ember.computed.gte('age',18),
	candaian:Ember.computed.equal('country','CA'),
});
```
Composable: 
Filter:

macros_array.js


```
App.Project = Ember.Object.extend({
	openTickets: Ember.computed.filterBy('tickets','open',true);
});
project = App.Project.create({
	tickets:[{
		id:1,open:true},
		{id:2,open:false}]
});
console.log(project.get('openTickets'));
```

### Trick #2 The Run Loop
#### Async Everything

not_updated.js

```
this.set('productId',1234);
this.get('productId'); // new value
$('#product-id').html() // still old value!
```

set_timeout.js 
```
setTimeout(function(){
	//do something later!
},1);
```

schedule.js
Scheduling:
	- Is there an Run Loop SCHEDULED?
	  -If YES, add event to it/
	 - If No , "setTimeout(fn,1)"

height.js

```
App.PostView = Ember.View.extend({
	didInsertElement:function(){
		var h = this.$().height();  // get current view height
		this.$().height(h*2)   // resize view height
	}
});
```
Ember View
Performance: More Vies = Slower Rendering
Infinte Scrolling 
![EmberView](http://7xi7ot.com1.z0.glb.clouddn.com/emberview.png)
