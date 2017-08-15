---
layout: post
title:  "Angular: Models"
date:   2017-08-15 06:13:26 +0200
category: angular
tags: [angular]
---

If we are going to use some structure, some object a lot in our app, we will define model, so everywhere in our app it will be the same. Model is just blueprint for how our model looks. We can use typescript class for this, because it can be instantianed 

`app/recipes/recipe.model.ts`
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

<br /><br />
