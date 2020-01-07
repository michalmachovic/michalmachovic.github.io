---
layout: post
date:   2020-01-03 09:13:26 +0200
title:  "Express.js"
category: nodejs
tags: [javascript, nodejs, express]
---
`Express.js` is fast, unopinionated, minimalist web framework for Node.js. We can for example process requests, do routes, etc...

<h2>Init npm, install Express.js and Nodemon</h2>
{% highlight javascript %}
npm init
npm install --save express
npm install --save-dev nodemon


//then in package.json in "scripts" section
"start": "nodemon app.js"
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
