---
layout: post
date:   2019-02-09 06:13:26 +0200
title:  "Magento2: Basic Module with Cron"
category: php, magento2
tags: [magento2,module,cron]
---

Here is simple module which will run cron method. For `$logger->debug` you need to set `"Stores" -> "Configuration" -> "Advanced" -> "Developer" -> "Debug" -> "Log to File"` to '`Yes`;
Also dont forget to set cron on your localhost with `crontab -e`.

```
*/1 * * * * php /var/www/html/magento2/sites/m2test/bin/magento cron:run | grep -v "Ran jobs by schedule" >> /var/www/html/magento2/sites/m2test/var/log/magento.cron.log
*/1 * * * * php /var/www/html/magento2/sites/m2test/update/cron.php >> /var/www/html/magento2/sites/m2test/var/log/update.cron.log
*/1 * * * * php /var/www/html/magento2/sites/m2test/bin/magento setup:cron:run >> /var/www/html/magento2/sites/m2test/var/log/setup.cron.log

```



{% highlight php %}
Cron
 |
 --Test.php
etc
 |
 module.xml
 crontab.xml
registration.php
{% endhighlight %}


{% highlight php %}
//registration.php
<?php

\Magento\Framework\Component\ComponentRegistrar::register(
    \Magento\Framework\Component\ComponentRegistrar::MODULE,
    'MichalMachovic_CronTest',
    __DIR__
);
{% endhighlight %}


{% highlight xml %}
//etc/module.xml
<?xml version="1.0"?>

<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:framework:Module/etc/module.xsd">
  <module name="MichalMachovic_CronTest" setup_version="0.0.1"></module>
</config>
{% endhighlight %}

{% highlight xml %}
//etc/crontab.xml

<?xml version="1.0"?>
  <config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:module:Magento_Cron:etc/crontab.xsd">
    <group id="default">
      <job name="your_cron_name" instance="MichalMachovic\CronTest\Cron\Test" method="execute">
        <schedule>*/2 * * * *</schedule>
      </job>
    </group>
  </config>

{% endhighlight %}

{% highlight php %}
//Cron/Test.php
<?php
namespace Gateway3D\AutoImport\Cron;
 
class Test {
 
    protected $_logger;
 
    public function __construct(
        \Psr\Log\LoggerInterface $logger
    ) {
        $this->_logger = $logger;
    }
    
    /**
     * Method executed when cron runs in server
     */
    public function execute() {
        $this->_logger->info('******TEST NEW CRON****');
        $this->_logger->debug('*****TEST NEW CRON****');
        return $this;
    }
}
{% endhighlight %}
