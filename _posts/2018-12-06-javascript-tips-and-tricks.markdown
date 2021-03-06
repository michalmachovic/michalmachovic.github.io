---
layout: post
title:  "Javascript: Tips and tricks"
date:   2018-12-06 06:13:26 +0200
category: javascript
tags: [javascript,tips,tricks]
---

<h2>Query selector</h2>
This return first element which has class="test" and changes its css display to none.

<br />
{% highlight php %}
document.querySelector('.test').style.display = 'none';
{% endhighlight %}
<br /><br />



<h2>getElementById</h2>
This return element which has id="test". It is faster than querySelector.

<br />
{% highlight php %}
document.getElementById('test').style.display = 'none';
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


<h2>Objects</h2>
<b>Instantiation and inheritance</b> 
All methods which we want to be inherited, we put in prototype property. Putting it directly into Person object isnt good idea, because then each object will have this function, which isnt effective.
<br /><br />
{% highlight php %}
var Person = function(name, yearOfBirth, job) {
	this.name = name;
	this.yearOfBirth = yearOfBirth;
	this.job = job;
}

Person.prototype.calculateAge = function() {
	console.log(2018 - this.yearOfBirth);
}

var john = new Person('john', 1990, 'teacher');
john.calculateAge();
{% endhighlight %}