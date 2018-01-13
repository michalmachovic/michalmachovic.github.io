---
layout: post
title:  "Typescript: Quick Tutorial"
date:   2018-01-13 06:13:26 +0200
category: typescript
tags: [typescript]
---



{% highlight ts %}
npm install -g typescript
tsc main.ts
{% endhighlight %}
<br />
<h2>Static Typing</h2>
A very distinctive feature of TypeScript is the support of static typing. This means that you can declare the types of variables, and the compiler will make sure that they aren't assigned the wrong types of values. If type declarations are omitted, they will be inferred automatically from your code.
<br /><br />
Here is an example. Any variable, function argument or return value can have its type defined on initialization:
<br />
{% highlight ts %}
var burger: string = 'hamburger',     // String 
    calories: number = 300,           // Numeric
    tasty: boolean = true;            // Boolean

// Alternatively, you can omit the type declaration:
// var burger = 'hamburger';

// The function expects a string and an integer.
// It doesn't return anything so the type of the function itself is void.

function speak(food: string, energy: number): void {
  console.log("Our " + food + " has " + energy + " calories.");
}

speak(burger, calories);
{% endhighlight %}

<br /><br />
