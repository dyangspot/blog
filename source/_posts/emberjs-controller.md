title: “Emberjs中的Controller”
date: 2015-04-05 01:52:48
tags: emberjs
---

在Emberjs中，controllers大概做以下几件事情

> 
- Manipulate the data within the application’s models
- Store transient data, whether standalone or made up of data retrieved from models
- Listen to events dispatched by and dispatch events intended for other controllers,views, and templates
- Instigate the transition from one route to another

也就是说，controller的作用能够处理models中的数据，暂时存储来自models中的数据，监听由其他controllers，views，templates发起的事件，对route进行跳转操作。

页面关键字搜索实例：

```
RocknrollcallYeoman.ApplicationController =Em.ObjectController.extend({	searchTerms: '',	applicationName: "Rock'n'Roll Call",	actions: {	submit: function() {		this.transitionToRoute('search-results',this.get('searchTerms'));		}	}});

```

以上controller代码中，创建本地临时变量searchTerms，用来获取view中用户输入的关键字。

```
<ul class="nav navbar-nav search-lockup">	<li class="search-group">		{{input type="text" class="search-input" placeholder="Search for artists or song names" valueBinding="controller.searchTerms"}}	<button {{action "submit"}} class="btn btn-primary"><i class="glyphicon glyphicon-play"></i></button>	</li></ul>
```

通过valueBinding属性来将之前定义的controller.searchTerms 绑定到input helper中。 当用户在input field中输入关键字的时候，关键字内容会绑定到controller中的searchTerms属性上。当用户点击submit后，会触发之前在template中定义的*submit* action。 在*submit* 中调用了transitionToRoute，之后emberjs会跳转到’search-results’ route并将searchTerms的值已参数的形式传递.



