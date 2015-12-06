title: "set-up-emberjs-dev-env"
date: 2015-04-03 01:01:01
tags: [emberjs,nodejs]
---
# Setting up dev environment for Emberjs

## Installing Yeoman

```
$ npm install -g yo grunt-cli bower
```

There you have it. Now check our install by simply typing the following:
```
$ yo
```
## Using Yo’s Ember Application Generator

### Installing Dependencies

- Ruby

```
$ which ruby/usr/bin/ruby$ ruby -vruby 2.0.0p247 (2013-06-27 revision 41674) [universal.x86_64-darwin13]
```
- Compass
Compass is an open source authoring framework for the Sass CSS preprocessor. You will need Compass to run the Ember generators and compile Sass to CSS. To install:

```
$ gem install compass
```
- Install the Generator 

```
$ npm search yeoman-generator
```
When finish, open a new terminal and summon Yo:
```
$ yo
```
Select the *ember-plus* you want to install. 
I just installed your generator by running:

```
npm install -g generator-ember-plus
```
- Running the Generator

For more info,goes here [Ember-Plus](https://www.npmjs.com/package/generator-ember-plus)
- Using Bower
Bower is used to delcare all the dependencies within a particular project. You can do this by *bower.json* .

- Grunt

Grunt.js is a task runner that allows you to automate all of the tasks that you normally have to worry about on your own or with separate toolsets. Grunt can do the following:

+ Watch processes such as LiveReload
+ Auto-compile CoffeeScript, Compass, and Sass
+ Auto-lint
+ Benchmark image optimization
+ Generate an app cache manifest
+ Minify and concatenate static resources
+ Run a built-in preview server

How Gruntfile works? Take a look at Gruntfile.js:
The initConfig method provides the configuration for each of the tasks we register later in grunt.registerTask(), for example ,the task jshint:

```
grunt.initConfig({	yeoman: yeomanConfig,	jshint: {		options: {			jshintrc: '.jshintrc',			reporter: require('jshint-stylish')		},		all: [			'Gruntfile.js',			'<%= yeoman.app %>/scripts/{,*/}*.js',			'!<%= yeoman.app %>/scripts/vendor/*',			'test/spec/{,*/}*.js'
		]
	},
}):		

```

grunt.loadNpmTasks
Before register tasks, we then need to load them as Node.js modules. This *require* declaration runs the load-grunt-tasks task that loads all the other tasks in the directory:

```require('load-grunt-tasks')(grunt)
```

since the dependencies are available, the Yeoman-generated Gruntfile includes the register calls for each task:

```
grunt.registerTask('default',[
	'jshint'
]);
```

To build the application, just run:

```
$ grunt
```

To view the app:

```
$ grunt serve
```

This should fire up the application on http://localhost:9000
To run test:

```
$ grunt test
```

