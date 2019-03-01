---
layout: post
date:   2019-03-01 06:13:26 +0200
title:  "Magento2: How to override block, helper, model, template"
category: php, magento2
tags: [magento2,module,frontend,override,block,helper,model,template]
---

This article refer to https://michalmachovic.github.io/php,%20magento2/2019/02/08/magento2-basic-module.html, follow that article to create frontend module first.

<h2>Override Block function</h2>
We want to override `create()` function from `vendor/magento/module-catalog/Block/Product/ImageBuilder.php`.

<h3>MichalMachovic/Helloworld/etc/di.xml</h3>
{% highlight xml %}
<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:framework:ObjectManager/etc/config.xsd">
        <preference for="Magento\Catalog\Block\Product\ImageBuilder" type="MichalMachovic\Helloworld\Block\Product\ImageBuilder" />
</config>
{% endhighlight %}

<h3>MichalMachovic/Helloworld/Block/Product/ImageBuilder.php</h3>
{% highlight php %}
<?php
namespace MichalMachovic\Helloworld\Block\Product;

class ImageBuilder extends \Magento\Catalog\Block\Product\ImageBuilder
{
  public function create()
  {
    \\do your stuff here
  }
{% endhighlight %}

