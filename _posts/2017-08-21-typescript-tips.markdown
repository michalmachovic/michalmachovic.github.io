---
layout: post
title:  "Typescript: Tips"
date:   2017-08-15 06:13:26 +0200
category: typescript
tags: [typescript]
---



{% highlight ts %}
export class Recipe {
	public name: string;
	public description: string;
	public imagePath: string;
}

contructor(name: string, desc: string, imagePath: string) {
	this.name = name;
	this.desc = desc;
	this.imagePath = imagePath;
}
{% endhighlight %}

is the same as

{% highlight ts %}
export class Recipe {
	constructor(public name: string, public desc:string, public imagePath:string) {}
}
{% endhighlight %}

<br /><br />
