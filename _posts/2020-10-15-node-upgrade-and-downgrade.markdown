---
layout: post
date:   2020-10-15 13:13:26 +0200
title:  "Node.js Upgrade and Downgrade"
category: nodejs
tags: [nodejs]
---


<br /><br />
<h2>Upgrade</h2>
{% highlight javascript %}
npm install -g n  // install Node version manager
n stable  // install latest stable version
{% endhighlight %}
<br /><br />


<h2>Downgrade</h2>
{% highlight javascript %}
npm install node@8.10.0  // install version 8.10.0
n 8.10.0  // use version 8.10.0
{% endhighlight %}
<br /><br />