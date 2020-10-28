---
layout: post
date:   2020-10-28 13:13:26 +0200
title:  "Error Network Change Ipv6"
category: error
tags: [error]
---

Sometimes you can see error `ERROR NETWORK CHANGE` usually in Chrome. You can fix this by disabling ipv6.
<br /><br />

Open `/etc/sysctl.conf` and add following at the end of the file:
<br /><br />

{% highlight javascript %}
net.ipv6.conf.all.disable_ipv6 = 1
net.ipv6.conf.default.disable_ipv6 = 1
net.ipv6.conf.lo.disable_ipv6 = 1
{% endhighlight %}
<br /><br />

Then run
{% highlight javascript %}
cat /proc/sys/net/ipv6/conf/all/disable_ipv6
{% endhighlight %}

and you should get 1.

