---
layout: post
date:   2020-09-04 13:13:26 +0200
title:  "Express.js: Environment variables"
category: express
tags: [express]
---


Environment variables will be injected when Node server starts, that allows us to use different values for development and production. Typicall we use this for login, passwords, etc...

<h2>app.js</h2>
{% highlight javascript %}
const MONGODB_URI = `mongodb+srv://${process.env.MONGO_USER}:${process.env.MONGO_PASSWORD}@cluster0-ntrpwp.mongodb.net/${process.env.MONGO_DEFAULT_DATABASE}`;
...
app.listen(process.env.PORT || 3000);
{% endhighlight %}

<br /><br />
With `nodemon` we can provide configuration file `nodemon.json`.
<h2>nodemon.json</h2>
{% highlight javascript %}
{
    "env": {
        "MONGO_USER": "michal",
        "MONGO_PASSWORD": "password",
        "MONGO_DEFAULT_DATABASE": "shop"
    }
}
{% endhighlight %}

<br /><br />
But in production we will not use `nodemon` because we will be not restarting server after every change and also we will not be doing any changes in production. So we can update `package.json` file. 

<h2>package.json</h2>
{% highlight javascript %}
...
"scripts": {
    "start": "NODE_ENV=production MONGO_USER=michal MONGO_PASSWORD=password MONGO_DEFAULT_DATABASE=shop node app.js",
    "start-server": "node app.js",
    "start:dev": "nodemon app.js"
}
{% endhighlight %}