---
layout: post
title:  "Javascript: Modules"
date:   2019-01-14 06:13:26 +0200
category: javascript
tags: [javascript,es6,es2015,es8,es2017,modules]
---


<br /><br />
We will use MVC pattern with following structure.
<br />
{% highlight javascript %}
src
 --js
   --models
     Search.js
   --views
     searchView.js
   index.js    
{% endhighlight %}
 <br /><br />

<h2>Default export</h2>
We want to export just one thing, we dont specify any variable.
<br />
{% highlight javascript %}
//Search.js
export default 'I am an exported string.'

//index.js 
//str is name which we will use in index.js
import str from './models/Search';
{% endhighlight %}

<br /><br />


<h2>Named export</h2>
We want to export multiple things, we need to specify any variables.
<br />
{% highlight javascript %}
//Search.js
export const add = (a, b) => a + b;
export const multiply = (a, b) => a * b;
export const ID = 23;


//index.js 
import {add, multiply} from './models/Search';
console.log(`Using imported functions. ${add(ID,2)} and ${multiply(3, 5)}`);
{% endhighlight %}

<br /><br />
We can use different names for our functions.
<br />
{% highlight javascript %}
//index.js 
import {add as a, multiply as m} from './models/Search';
console.log(`Using imported functions. ${a(ID,2)} and ${m(3, 5)}`);
{% endhighlight %}


<br /><br />
We can import also everything.
<br />
{% highlight javascript %}
//index.js 
import * as searchModel from './models/Search';
console.log(`Using imported functions. ${searchModel.add(searchModel.ID,2)} and ${searchModel.m(3, 5)}`);
{% endhighlight %}
