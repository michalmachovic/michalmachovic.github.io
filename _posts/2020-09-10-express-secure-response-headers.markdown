---
layout: post
date:   2020-09-10 13:13:26 +0200
title:  "Express.js: Secure response headers"
category: express
tags: [express]
---

Helmet helps you secure your Express apps by setting various HTTP headers. It's not a silver bullet, but it can help. Its adding some special headers to your responses

{% highlight javascript %}
npm install --save helmet
{% endhighlight %}


<h2>app.js</h2>
{% highlight javascript %}
...
const helmet = require("helmet");
const app = express();
app.use(helmet());
...
{% endhighlight %}

