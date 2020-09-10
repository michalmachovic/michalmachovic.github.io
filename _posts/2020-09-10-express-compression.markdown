---
layout: post
date:   2020-09-10 14:13:26 +0200
title:  "Express.js: Compression"
category: express
tags: [express]
---

This package allows you to compress assets, like css and js files.

{% highlight javascript %}
npm install --save compression
{% endhighlight %}


<h2>app.js</h2>
{% highlight javascript %}
...
const helmet = require("compression");
const app = express();
app.use(compression());
...
{% endhighlight %}

