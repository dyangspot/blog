title: "Ember-in-action"
date: 2015-04-03 00:28:26
tags: emberjs
---
App.js

``` 
App = Ember.Application.create();
App.Router.map(function(){
	// put your routes here
});

App.IndexRoute = Ember.Route.extend({
	model:function(){
		return ['red','yellow','blue'];
	}
});

```

When you load *http://localhost:8000* in browser:

1. The HTML page loads, which loads and executes your app.js file.
2. app.js instantiates an Ember Application
3. When the Application starts up, it immediately looks to see if you've written your own **ApplicationRoute**,loading it if there is one, and loading its own either way.
4. The Application does the same load-yours-too-if-you-wrote-one routine with **ApplicationController**.
5. If you've defined your own ApplicationRoute- and defined any event hooks within that should be fired(such as an activate definition) - they will now be fired.
6. Ember then looks for an *application template* in your HTML and sets it up with **ApplicationController** as its controller, which we'll see later becomes the conduit through which data flows into your tempaltes, replacing placeholders with live data.
7. Because we are at the *root URL* of your application(given that no route in particular is specified), Ember will now locate and instantiate the **IndexRoute** you specified in app.js.
8. Ember will now identify that you didn't specify an ""IndexController"" and will instantiate one for you,but Ember has some choices to make about what kind of default controller will be most useful. Because you defined a model in **IndexRoute** that is an array,Ember will instantiate an *ArrayController* as your **IndexController**.
9. Ember will find the template named *index* in the HTML document and render it to the *outlet* helper in our application.

When you load *http://localhost:8000/#/examples/1 in browser, because we haven't defined the route, let's see what Ember will look for:

1. Because of the */examples*, Ember will look for a route named **ExamplesRoute**.
2. Ember will look for or create an **ExamplesController**.
3. Ember will cal the *model* function of *ExamplesRoute*, passing it the last portion of your URL, looking a bit like this, were you to define an *ExamplesRoute*:
```
model: function(params){
	return App.Example.find(params.example_id);
}
```
4. Ember can now retrieve your model data and populate a template named *examples* in your HTML, rendering it to the **outlet** helper in your application template.
