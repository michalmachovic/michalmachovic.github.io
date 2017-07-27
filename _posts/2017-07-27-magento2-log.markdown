---
layout: post
title:  "How to log in Magento2"
date:   2017-07-27 10:11:26 +0200
categories: magento2
---

Here is how you can log to Magento2 debug.log file, located under `var/system/log`.

{% highlight php %}
 logger = \Magento\Framework\App\ObjectManager::getInstance()->get('\Psr\Log\LoggerInterface');
 $logger->debug('TEST');
{% endhighlight %}


