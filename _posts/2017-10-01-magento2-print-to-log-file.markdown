---
layout: post
title:  "Magento2: Print to log file"
date:   2017-09-30 08:13:26 +0200
category: Magento2
tags: [magento2]
---

For `$logger->debug` you need to set `"Stores" -> "Configuration" -> "Advanced" -> "Developer" -> "Debug" -> "Log to File"` to '`Yes`;


{% highlight php %}
<?php
$logger = \Magento\Framework\App\ObjectManager::getInstance()->get('\Psr\Log\LoggerInterface');
$logger->info('*********************************************TEST****');
$logger->debug('*********************************************TEST****');
?>
{% endhighlight %}



Or you can use this:

{% highlight php %}
class Test {
 
    protected $_logger;
 
    public function __construct(
        \Psr\Log\LoggerInterface $logger
    ) {
        $this->_logger = $logger;
    }

    public function execute() {
        $this->_logger->info('*********************************************TEST NEW CRON****');
        $this->_logger->debug('*********************************************TEST NEW CRON****');
        return $this;
    }
}



