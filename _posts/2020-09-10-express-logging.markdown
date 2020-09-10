---
layout: post
date:   2020-09-10 15:13:26 +0200
title:  "Express.js: Logging"
category: express
tags: [express]
---

This package allows you to do logs

{% highlight javascript %}
npm install --save morgan
{% endhighlight %}


<h2>app.js</h2>
{% highlight javascript %}
...
const fs = require('fs');
...

const accessLogStream = fs.createWriteStream(path.join(__dirname, 'access.log'), { flags: 'a' });

app.use(morgan('combined', { stream: accessLogStream }));
...
{% endhighlight %}

<br /><br />
Advanced logging: `https://blog.risingstack.com/node-js-logging-tutorial/`
