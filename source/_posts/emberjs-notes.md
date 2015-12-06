title: "emberjs-notes"
date: 2015-03-29 23:01:42
tags: emberjs
---

今儿被一个奇怪的问题困扰了一整天，大概是用emberjs要实现一个List-Detail的结构，屏幕左侧是个选项栏，选择选项栏中的项目，会在屏幕右侧展现你所选中的具体内容。
错误做法：如果实现的List放在在IndexRouter中，Detail放在 DetailRouter中。Detail page会跳转， 无法实现List-Deail在同一页面的效果。具体原因目前不详

正确做法： 把List放在ApplicationRouter中，Detail放在对应的DetailRouter中，List放在Application template下，Detail页面布局的 {{outlet}} 放在Application template下。 

[例子](http://jsbin.com/ijejap/1/edit?html,js)