---
layout: post
title:  "Javascript: Module Pattern"
date:   2018-12-19 06:13:26 +0200
category: javascript
tags: [javascript,module,pattern]
---

The Module pattern is used to mimic the concept of classes (since JavaScript doesn’t natively support classes) so that we can store both public and private methods and variables inside a single object — similar to how classes are used in other programming languages like Java or Python. That allows us to create a public facing API for the methods that we want to expose to the world, while still encapsulating private variables and methods in a closure scope.
<br /><br />


First we start using a anonymous closure. Anonymous closures are just functions that wrap our code and create an enclosed scope around it. Closures help keep any state or privacy within that function. Closures are one of the best and most powerful features of JavaScript. <br />
This pattern is well known as a Immediately Invoked Function Expression or IIFE. The function is evaluated then immediately invoked. Its also a good practice to run your modules in ES5 strict mode. Strict mode will protect you from some of the more dangerous parts in JavaScript.
<br /> <br />

{% highlight javascript %}
(function() {
    'use strict';
    // Your code here
    // All function and variables are scoped to this function
}());
{% endhighlight %}

<br /><br />

<h2>Example 1: Anonymous closure</h2>
With this construct, our anonymous function has its own evaluation environment or “closure”, and then we immediately evaluate it. This lets us hide variables from the parent (global) namespace.
What’s nice about this approach is that is that you can use local variables inside this function without accidentally overwriting existing global variables, yet still access the global variables, like so:
<br />
{% highlight javascript %}
var global = 'Hello, I am a global variable :)';
(function () {
  // We keep these variables private inside this closure scope
  
  var myGrades = [93, 95, 88, 0, 55, 91];
  
  var average = function() {
      return 'Your average grade is ' + total / myGrades.length;
  }
  console.log(average());
  console.log(global);

}());
{% endhighlight %}
<br /><br />



<h2>Example 2: Global Import</h2>
Another popular approach used by libraries like jQuery is global import. It’s similar to the anonymous closure we just saw, except now we pass in globals as parameters. In this example, globalVariable is the only variable that’s global. The benefit of this approach over anonymous closures is that you declare the global variables upfront, making it crystal clear to people reading your code.
<br />
{% highlight javascript %}
var global = 'Hello, I am a global variable :)';
(function (global) {

  // Keep this variables private inside this closure scope
  var privateFunction = function() {
    console.log('Shhhh, this is private!');
  }

  console.log(global);

}(global));
{% endhighlight %}


<br /><br />

<h2>Example 3: Object interface</h2>
Yet another approach is to create modules using a self-contained object interface. JavaScript does not have a private keyword by default but using closures we can create private methods and private state. Because our private properties are not returned they are not available outside of out module. Only our public method has given us access to our private methods. This gives us ability to create private state and encapsulation within our code.
<br /><br />
You may have noticed the _ before our private methods and properties. Because JavaScript does not have a private keyword its common to prefix private properties with an underscore.
<br />
{% highlight javascript %}
var myModule = (function() {
    'use strict';
 
    var _privateProperty = 'Hello World';
     
    function _privateMethod() {
        console.log(_privateProperty);
    }
     
    return {
        publicMethod: function() {
            _privateMethod();
        }
    };
}());
{% endhighlight %}

<br /><br />

<h2>Example 4: Revealing Module Pattern</h2>
The Revealing Module Pattern is one of the most popular ways of creating modules. Using the return statement we can return a object literal that ‘reveals’ only the methods or properties we want to be publicly available.

<br />
{% highlight javascript %}
var myModule = (function() {
    'use strict';
 
    var _privateProperty = 'Hello World';
    var publicProperty = 'I am a public property';
  
    function _privateMethod() {
        console.log(_privateProperty);
    }
  
  	function publicMethod() {
    	_privateMethod();
  	}
     
    return {
        publicMethod: publicMethod,
        publicProperty: publicProperty
    };
}());
  
myModule.publicMethod();    		        // outputs 'Hello World'   
console.log(myModule.publicProperty);       // outputs 'I am a public property'
console.log(myModule._privateProperty);     // is undefined protected by the module closure
myModule._privateMethod();                  // is TypeError protected by the module closure

{% endhighlight %}