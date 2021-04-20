---
layout: post
date:   2021-04-20 13:13:26 +0200
title:  "Magento2 - Translations"
category: magento2
tags: [magento2]
---



<h3>app/i18n/slovak/sk_SK/sk_SK.csv</h3>
{% highlight php %}
"Search Terms","Vyhľadávanie"
{% endhighlight %}
<br /><br />



<h3>app/i18n/slovak/sk_SK/language.xml</h3>
{% highlight php %}
<?xml version="1.0"?>
<language xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:framework:App/Language/package.xsd">
    <code>sk_SK</code>
    <vendor>slovak</vendor>
    <package>sk_sk</package>
</language>
{% endhighlight %}
<br /><br />


<h3>app/i18n/slovak/sk_SK/registration.php</h3>
{% highlight php %}
<?php
    \Magento\Framework\Component\ComponentRegistrar::register(
    \Magento\Framework\Component\ComponentRegistrar::LANGUAGE,
    'slovak_sk_sk',
    __DIR__
    );
{% endhighlight %}
<br /><br />


<h2>Clean cache</h2>
{% highlight php %}
php bin/magento cache:clean && php bin/magento cache:flush && sudo chmod 777 var -R && sudo chmod 777 pub -R
{% endhighlight %}
 
 <br /><br />


<h2>Deploy static content</h2>
{% highlight php %}
 php bin/magento setup:static-content:deploy -f
 {% endhighlight %}
