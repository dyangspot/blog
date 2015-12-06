title: “Emberjs中的Model”
date: 2015-04-05 01:22:38
tags: emberjs
---

总结下在Emberjs中利用Ember.Object来实现面向对象。
首先定义对象:

```
RocknrollcallYeoman.Artist = Em.Object.extend({
	id:null,
	name:null,
	enid:null,
	biography:null,
	hotttnesss:null,
	image:null,
	videos:null
});
```

定义的Artist对象的初始化属性值都为null。
然后，通过系统给对象生成的create方法，创建Artist对象实例：

```
var artist = RocknrollcallYeoman.Artist.create({
	enid: 'ARERLPG1187FB3BB39',	name: 'Tom Waits',	hotttnesss: 100,	biography: 'Rain dog.',	image: 'http://is.gd/GeaUhC'
});
```

利用Ajax的Promise实现创建对象

Promise 大概写法如下：

```
Promise.all([functionOnWhichWeDepend(),anotherFunctionOnWhichWeDepend(),yetAnotherFunctionOnWhichWeDepend()]).then(function() {	console.log("They're all finished, success is ours!")}, function() {	console.error("One or more FAILED!")});
```

通常情况下通过调用jquery.get方法来获取JSON并以Promise类型返回，示例如下：

```
RocknrollcallYeoman.ArtistRoute = Ember.Route.extend({	model: function(params) {
		var url = "http://developer.echonest.com/api/v4/artist/profile?api_key=<YOUR-API-KEY>&format=json&bucket=biographies&bucket=blogs&bucket=familiarity&bucket=hotttnesss&bucket=images&bucket=news&bucket=reviews&bucket=terms&bucket=urls&bucket=video&bucket=id:musicbrainz”;		obj = {"id": params.enid};
		
		return Ember.$.getJSON(url,obj).then(function(data){
			var entry = data.response.artist;			var bio = null;			var img = null;
			for (i = 0;i<entry.biographies.length;i++) {				if (entry.biographies[i].site.toLowerCase() == "wikipedia") {					bio = entry.biographies[i];				}			}			if (!bio && entry.biographies.length > 0) {				bio = entry.biographies[0];			}
			if (entry.images.length) {				img = entry.images[0];
			}			var lastVideo = 4;			if (entry.video.length < 4) {				lastVideo = entry.video.length;			}			var videos = [];			for (i=0;i<lastVideo;i++) {				videos.push(entry.video[i]);			}
			return RocknrollcallYeoman.Artist.create({				enid: entry.id,				name: entry.name,				hotttnesss: entry.hotttnesss,				biography: bio,				image: img,				videos: videos			});	
				
		});
	}
});
```
