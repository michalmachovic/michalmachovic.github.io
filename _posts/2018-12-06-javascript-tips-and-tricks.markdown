---
layout: post
title:  "Javascript: Tips and tricks"
date:   2018-12-06 06:13:26 +0200
category: javascript
tags: [javascript,tips,tricks]
---

<h2>Query selector</h2>
When you want to select some element.

<br />
{% highlight php %}
document.querySelector('.test').style.display = 'none';
{% endhighlight %}
<br /><br />



<h2>Event listener</h2>
https://developer.mozilla.org/en-US/docs/Web/Events
<br />
{% highlight php %}
function callback() {
	//do something	
}

document.querySelector('.test').addEventListener('click', callback);
{% endhighlight %}

<br /><br />
or we can do the same with anonymous function (because this function will be used only in this one case)
{% highlight php %}
document.querySelector('.another-test').addEventListener('click', function() { 
   //do something here
});

{% endhighlight %}
<br /><br />