---
layout: post
title:  "Magento2: Overriding core files - for example topmenu"
date:   2017-09-30 08:13:26 +0200
category: Magento2
tags: [magento2]
---

Topmenu in Magento2 is generated from Block php file. That means when you want to override core Topmenu and make some changes to it, you need to create your own module.
<br /><br />

<h3>Module structure</h3>

{% highlight js %}
app
|
--code
     |
     --Customgateway
                   |
                   --Topmenu
                           |
                           --Block
                           |     |
                           |     --Rewrite
                           |             |
                           |             --Html
                           |                  |
                           |                  --Topmenu.php    
                           |
                           --etc
                           |   |
                           |   --di.xml
                           |   --module.xml  
                           |
                           --registration.php
{% endhighlight %}


<br /><br />
`app/code/Customgateway/Topmenu/Block/Rewrite/Html/Topmenu.php`<br />
 (original file is under `/vendor/magento/module-theme/Block/Html/Topmenu.php`)<br />
 <br />

{% highlight php %}
<?php
    namespace Customgateway\Topmenu\Block\Rewrite\Html;
 
    class Topmenu extends \Magento\Theme\Block\Html\Topmenu
    {
        protected function _getHtml(
            \Magento\Framework\Data\Tree\Node $menuTree,
            $childrenWrapClass,
            $limit,
            $colBrakes = []
        ) {

               //do your stuff here
        }
    }
{% endhighlight %}



<br /><br />
`app/code/Customgateway/Topmenu/etc/di.xml`<br />
{% highlight xml %}
<?xml version="1.0"?>
<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:framework:ObjectManager/etc/config.xsd">
    <preference for="Magento\Theme\Block\Html\Topmenu" type="Customgateway\Topmenu\Block\Rewrite\Html\Topmenu" />
</config>
{% endhighlight %}


<br /><br />
`app/code/Customgateway/Topmenu/etc/module.xml`<br />
{% highlight xml %}
<?xml version="1.0"?>
<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:framework:Module/etc/module.xsd">
    <module name="Customgateway_Topmenu" setup_version="1.0.0">
    </module>
</config>
{% endhighlight %}


<br /><br />
`app/code/Customgateway/Topmenu/registration.php`<br />
{% highlight php %}
<?php
  use \Magento\Framework\Component\ComponentRegistrar;
  ComponentRegistrar::register(ComponentRegistrar::MODULE, 'Customgateway_Topmenu', __DIR__);
{% endhighlight %}

<br /><br />
<b>!!!THERE IS OPENED BUG IN MAGENTO!!!</b> described here: [https://github.com/magento/magento2/issues/4564]  <br /><br />
To fix this, under your own theme in file <br />
`/htdocs/app/design/frontend/Customgateway/entertainer/Magento_Theme/layout/default.xml`
<br /><br />
change
{% highlight xml %}
  <block class="Magento\Theme\Block\Html\Topmenu" name="catalog.topnav" template="html/topmenu.phtml" ttl="3600" before="-"/>
{% endhighlight %}
to
{% highlight xml %}
  <block class="Magento\Theme\Block\Html\Topmenu" name="catalog.topnav" template="Magento_Theme::html/topmenu.phtml" ttl="3600" before="-"/>
{% endhighlight %}

<br /><br />
<br />
after you upload files to server, do this <br />
 `php bin/magento setup:upgrade`<br />
 `bin/magento setup:di:compile`<br />
 `sudo chmod 777 var -R`<br />
 `sudo chmod 777 pub -R`<br />
<br /><br />
probably you would need to deploy static content again<br />
 `php bin/magento setup:static-content:deploy en_GB --theme Magento/backend`<br />
 `php bin/magento setup:static-content:deploy en_GB --theme Customgateway/entertainer`   (your own theme)




[https://github.com/magento/magento2/issues/4564]: https://github.com/magento/magento2/issues/4564


