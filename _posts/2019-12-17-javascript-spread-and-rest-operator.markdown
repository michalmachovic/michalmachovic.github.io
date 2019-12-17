---
layout: post
date:   2019-12-17 07:13:26 +0200
title:  "Javascript: Spread and Rest Operators"
category: javascript
tags: [javascript, spread, rest]
---

MongoDB is a document database with the scalability and flexibility that you want with the querying and indexing that you need.


<h2>Spread Operator</h2>
`...` It takes array or object after operator and pull out all elements of array or properties of object and put it in whatever is around that operator.

{% highlight javascript %}
const hobbies = ['Sports', 'Cooking'];
const copiedArray = [...hobbies];


const person = {
    name: 'Max',
    age: 29
}
const copiedPerson = {...person};

{% endhighlight %}
<br /><br />


<h2>Rest Operator</h2>
It looks like Spread operator `...`. It merge multiple arguments into array. So if you use `...` in the argument list of function, then its the Rest operator

{% highlight javascript %}
const toArray = (...args) => {
    return args;
}

console.log(toArray(1,2,3,4));
{% endhighlight %}
