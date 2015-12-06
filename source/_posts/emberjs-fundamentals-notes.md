title: "Emberj.js Fundamentals Notes"
date: 2015-03-30 22:10:42
tags: emberjs
---

This is [Ember.js Fundamental](http://www.pluralsight.com/training/Player?author=rob-conery&name=ember-fundamentals-m2-github&clip=4&course=emberjs-fundamentals) course provided by Pluralsight, I'm taking notes and pushing them here, so i can later use this page as a Emberj.js quick reference. 
 
### First App,First Template

Debug: set Log_TRANSITION:True
 
``` html 
 
 <div id="github-app"></div>
 <script type="text/x-handlebars">
 	<h1>Hello from Emberjs</h1>
 	{{outlet}}
 </script> 
 
 <script>
 	UserAdmin = Emberjs.Application.create({
 		rootElement:'#github-app'
 	});
 </script>	
```
 
### The incantation 
-  A URL wants a Route
-  a Route sets the Model
-  A Route prepares the Controller
-  The Controller backs the template

### Conventional Wisdom

/ -> IndexRoute -> IndexController

```
Github.IndexController = Ember.ArrayController.extend(){
	renderedOn:function(){
		return new Date();
	}.property()
};	


```

###Mapping a Route
```
this.resource("thing")
this.route("action")
```

### Nesting Routes

```
Github.UserIndexRoute = Ember.Route.extend({
	model:function(){
		return this.modelFor('user'); // grab the model for user and return it. 		
	}
});	
	
Github.UserUserRoute = Ember.Route.extend({
	model:function(){
		return this.modelFor('user');
	}
});
```
As we can these, this.modelFor('') can be used in different routes sharing data.

### Sharing Data & Deep Nesting

Use *link-to* bind model as a parameter. 
```
{{#link-to 'respository' this}} {{name}} {{/link-to}}
```
Pass along *respository* model

Share the data in Route and Controller.

```
Github.Router.map(function(){
	this.resource("user",{path:"users/:login"},function(){
		this.resource("repositories");
	});
});

Github.RepositoriesRoute = Ember.Route.extend({
	model:function(){/:
		var user = this.modelFor("user");
		return Ember.$.getJSON(user.repos_url);
	}
});	

Github.RepositoriesController = Ember.ArrayController.extend({
	needs:["user"], // */user* route is the parent route for */respositories* route
	user:Ember.computed.alias("controllers.user"); // goes to controllers.user and alias it, when user changes, this also change
});
```

### Summary

- $.getJSON, Emberjs does Promise

```
Github.RepositoriesRoute = Ember.Route.extend({
	model:function(){
		var user = this.modelFor("user");
		return Ember.$.getJSON(user.repos_url);
	}
});	
```

- Nested: Resource vs. Route
A resource is for a thing, a route is for something you do to that thing.

```
Github.Router.map(function(){
	this.resource("user",{path:"users/:login"},function(){
		this.resource("repositories");
		this.resource("repository",{path:"repositories/:reponame"},function(){
			this.resource("issues");
			this.resource("forks");
		});
	});
});

```

- If your UI is nested, your route should be nested.
- Caching and Naming
```
{{#link-to 'user' this.login}}	
  			vs 
{{#link-to 'user' this}}  
  
```

- Computed Properties
```
Github.RepositoryController = Ember.ObjectController.extend({
	needs:["user"],
	user:Ember.conputed.alias("controllers.user"),
	forked:Ember.computed.alias("fork")
});
```

Here tell ember to alias fork as 'forked', so we can use it in our template directly.

### Use Third party JS API, Helper!

Register helper for moment.js 

```
Ember.Handlebars.registerBoundHelper('fromDate',function(theDate){
	var today = moment();
	var target = moment(theDate);
	return target.from(today);	
});
```

In the template should goes

```
<p class="text-muted">{{fromDate created_at}}</p>
```

### Saving Data

```
Github.Router.map(function(){
	this.resource("user",{path:"users/:login"},function(){
		this.resource("repositories");
		this.resource("repository",{path:"repositories/:reponame"},function(){
			this.resource("issues");
			this.resource("forks");
			this.route("newissue");  // <- 
		});
	});
});

<script type="text/x-handlebars" id="repository/newissue"> // not a resource, so put nested route directly 
...
</script>

{{#link-to 'repository.newissue'}}New Issue{{/link-to}}


{{input value=issueTitle class="form-control" placeholder="Title of issue"}}

{{textarea value=issueBody class="form-control" placeholder="What's your issue" rows=5}}

//create an issue object

Github.Issue = Ember.Object.extend({
	issueTitle:"",
	issueBody:"",
	isValid:function(){
		//do some validations...
		return true;
	}
});


Github.REpositoryNewissueController = Ember.Controller.extend({
	issueTitle: "Ember confused me"  // set up issueTitle like this
	needs:['repository'],
	repo:Ember.computed.alias("controllers.repository"),
	issue:function(){
		return Github.Issue.create();
	}.property("repo.model"), // create an issue,that is going to return, every time accessing issue, since add the binding into issue. whenever the repo changes, gonna change the binding in issue. which means when repo changes, gonna force a new issue to create
	actions:{
		submitIssue:function(){
			//var vals = this.getProperties("issueTitle","issueBody") // create an object for you 
			
			var issue = this.get('issue'); // get the properity issue
			var url = this.get("repo").get("issues_url"); // get the properity 'repo'
			
			Ember.$.post(url,{title:issueTitle,body:issueBody},function(result){
			//success...
			//reset it 
			this.set("issue",Github.Issue.create());
			this.transitionToRoute("issues");
			})
		}
	}
});


```

### Review 

- Helpres 

```
{{fromDate created_at}}

Ember.Handlebars.registerBoundHelper('fromDate',function(theDate){
	var today = moment();
	var target = moment(theDate);
	return target.from(today);	
});
```
- $.post
```
Ember.$.post(url,{title:issueTitle,body:issueBody},function(result){
			//success..
			this.transitionToRoute("issues");
			})
```

- needs vs. modelFor

```
Github.RepositoryController = Ember.ObjectController.extend({
	needs:['user'],
	user:Ember.computed.alias("controllers.user"),
	forked: Ember.computed.alias("fork")
});

Github.RepositoryRoute = Ember.Route.extend({
	model:function(params){
		var user = this.modelFor("user");
		// ...
	}
});
```

Bascially, controllers can need other controllers, routes use other models in other routes

- Resource vs.Route
A resource has it's own route name, a route must include the resource name.

|Template  | Route   | Controller  | Route Name  |   
|---|---|---|---|---|
|repositories RepositoriesRoute  |RepositoriesController   |repositories   |   |  
|issues |IssuesRoute   |IssuesController   |issues   |   
|commits   |CommitsRoute   |CommitsController   |commits  |   
|**repository/newissue**   |**RepositoryNewissueRoute**   |**RepositoryNewissueController**   |**repository.newissue**   |   

- Computed Properties


```
Github.REpositoryNewissueController = Ember.Controller.extend({
	issueTitle: "Ember confused me"  // set up issueTitle like this
	needs:['repository'],
	repo:Ember.computed.alias("controllers.repository"),
	issue:function(){
		return Github.Issue.create();
	}.property("repo.model")
});
	
```




