---
layout: post
date:   2019-01-30 06:13:26 +0200
title:  "HTML: localStorage"
category: javascript
tags: [html,javascript, localstorage]
---


<br /><br />
The localStorage object stores the data with no expiration date. The data will not be deleted when the browser is closed, and will be available the next day, week, or year. Value must be always `string` so if we want to store `array` we will use `JSON.stringify`. To check what is actually in localStore, simply type `localStorage` in `console`.
<br /><br />


{% highlight javascript %}
localStorage.setItem('id','test');
let item = localStorage.getItem('id');

//store this.likes array
localStorage.setItem('likes', JSON.stringify(this.likes));
let item = JSON.parse(localStorage.getItem('likes'))
{% endhighlight %}
