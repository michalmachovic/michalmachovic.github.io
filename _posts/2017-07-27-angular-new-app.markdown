---
layout: post
title:  "Angular: New app"
date:   2017-07-27 10:13:26 +0200
category: angular
tags: [angular]
---

{% highlight php %}
ng new my-dream-app
cd my-dream-app
npm install --save bootstrap  (for bootstrap)
ng serve
{% endhighlight %}

To include bootstrap open file .angular-cli.json and into styles array 	insert
{% highlight php %}
 "../node_modules/bootstrap/dist/css/bootstrap.min.css"
{% endhighlight %}

Now go to [http://localhost:4200] and you will see your app.
<br /><br /><br /><br />
It may happen that you are changing files, but ng serve isnt detecting it, so your app isnt reloaded with changes. In this case do this:
{% highlight php %}
sudo sysctl fs.inotify.max_user_watches=524288
sudo sysctl -p --system	
{% endhighlight %}
The problem was related with Inotify Watches Limit on Linux.
To solve to issue I increased the watches limit to 512K










[http://localhost:4200]: http://localhost:4200




