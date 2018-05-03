---
layout: post
title:  "Magento2: Print to log file"
date:   2017-09-30 08:13:26 +0200
category: Magento2
tags: [magento2]
---



{% highlight php %}
<?php
logger = \Magento\Framework\App\ObjectManager::getInstance()->get('\Psr\Log\LoggerInterface');
$logger->debug('*********************************************TEST****');
?>
{% endhighlight %}



