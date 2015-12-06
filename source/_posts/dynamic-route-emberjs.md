title: “Emberjs中的dynamic route”
date: 2015-04-04 23:37:51
tags:  emberjs
---

Emerbjs中对于单个资源的的动态路径获取再次做个实例总结，

```
RocknrollcallYeoman.dummySearchResultsArtists = [
	{
		id: 1,		name: 'Tom Waits',		type: 'artist',		enid: 'ARERLPG1187FB3BB39',		hotttnesss: '1'
	},
	{		id: 2,		name: 'Thomas Alan Waits',		type: 'artist',		enid: 'ARERLPG1187FB3BB39',		hotttnesss: '.89'	},
]
```
根据以上数据模型，我们定义routes：

RocknrollcallYeoman.Router.map(function(){
	this.route(‘artist’,{
		path: ‘artist/:enid’
	});
});

对于artist，如果用户想指定访问单一artist，例如：Tom Waits，可以通过访问 *http://localhost:9000/#/artist/ARERLPG1187FB3BB39*来获取。 