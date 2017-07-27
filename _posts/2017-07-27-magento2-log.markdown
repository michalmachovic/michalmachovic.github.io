---
layout: post
title:  "Magento2: Write message to log"
date:   2017-07-27 10:11:26 +0200
category: magento2
tags: [magento2]
---

Here is how you can log to Magento2 debug.log file, located under `var/system/log`.

{% highlight php %}
 logger = \Magento\Framework\App\ObjectManager::getInstance()->get('\Psr\Log\LoggerInterface');
 $logger->debug('TEST');
{% endhighlight %}


