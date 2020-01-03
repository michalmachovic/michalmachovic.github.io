---
layout: post
date:   2020-01-03 09:13:26 +0200
title:  "Express.js"
category: nodejs
tags: [javascript, nodejs, express]
---
`Express.js` is fast, unopinionated, minimalist web framework for Node.js. We can for example process requests, do routes, etc...

{% highlight javascript %}
npm install --save express
{% endhighlight %}
<br /><br />


<h3>app.js</h3>
{% highlight javascript %}
const express = require('express');
const app = express();

//use new middleware function which will be executed for every new request
app.use((req, res, next) => {
    console.log('in the middleware');
    next(); //this will allow to go to another middleware
});


app.use((req, res, next) => {
    console.log('in the another middleware');
    res.send('<h1>Hello from Express !</h1>');
});

app.listen(3000);
{% endhighlight %}
<br /><br />


<h2>Routes</h2>
<h3>app.js</h3>
{% highlight javascript %}
const express = require('express');
const app = express();

app.use('/add-product', (req, res, next) => {
    console.log('in the another middleware');
    res.send('<h1>The Add product page</h1>');
});

app.use('/', (req, res, next) => {
    console.log('in the another middleware');
    res.send('<h1>Hello from Express !</h1>');
});

app.listen(3000);
{% endhighlight %}
