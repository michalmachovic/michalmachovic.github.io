---
layout: post
date:   2019-12-17 08:13:26 +0200
title:  "Javascript: Destructuring"
category: javascript
tags: [javascript, destructuring]
---
It allows us to write more understandable code, so in function definition we see which properties are actually using.

<h2>Object destructuring</h2>
{% highlight javascript %}
const person = {
    name: 'Max',
    age: 29,
    greet() {
        console.log('Hi. I am ' + this.name);
    }
};

const printName = ({ name, age }) => {
    console.log(name + age);
}

printName(person);

/////////////////////

const { name, age } = person;  //this will create two new constants

{% endhighlight %}
<br /><br />

<h2>Array destructuring</h2>

{% highlight javascript %}
const hobbies = ['Sports', 'Cooking'];
const [hobby1, hobby2] = hobbies;
console.log(hobby1, hobby2);
{% endhighlight %}
