---
layout: post
date:   2021-04-16 13:13:26 +0200
title:  "Mobile - Android debugging (adb)"
category: mobile
tags: [mobile]
---



<h2>How to debug mobile issues</h2>
You can connect your mobile to your computer to see your mobile screen on your computer and also see console output from Chrome. So you can put logs into your code and see what is happening.

<br /><br />
<h3>Install adb (android-tools-adb)</h3>

{% highlight php %}
apt-get install android-tools-adb android-tools-fastboot
{% endhighlight %}

<br /><br />


<h3>Restart adb server</h3>
{% highlight php %}
sudo adb kill-server
sudo adb start-server
{% endhighlight %}

