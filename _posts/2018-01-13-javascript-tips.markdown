---
layout: post
title:  "Javascript: Tips"
date:   2018-01-13 07:13:26 +0200
category: javascript
tags: [javascript]
---


<h2>Define object with functions</h2>
{% highlight ts %}
var extra = {
    myvar : "12",
    
    update : function(test) {
    	var a = 'hello'; 
    	return a; 
    },

    ui: (function() {
    	var ua = navigator.userAgent;
	    
		return {
	      IE:             !!window.attachEvent && !isOpera,
	      Opera:          isOpera,
	      WebKit:         ua.indexOf('AppleWebKit/') > -1,
	      Gecko:          ua.indexOf('Gecko') > -1 && ua.indexOf('KHTML') === -1,
	      MobileSafari:   /Apple.*Mobile.*Safari/.test(ua)
	    }
    })(),


}
{% endhighlight %}
<br /><br/>

() at the end of function == It's just an anonymous function that is executed right after it's created.
<br /><br />
It's just as if you assigned it to a variable, and used it right after, only without the variable:

{% highlight ts %}
var f = function () {
};
f();
{% endhighlight %}