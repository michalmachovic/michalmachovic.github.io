---
layout: post
title:  "Magento2: Recompile and content deploy"
date:   2017-09-09 10:11:26 +0200
category: magento2
tags: [magento2]
---

If you want to make change to content of your theme, css or images, you are doing these changes under `app/design` folder.

Magento is looking for files under `pub/static` folder, that means you must deploy this static content there.

`MyCompany` is name of your company
`mytheme` is name of your theme

You need to remove files from `pub/static` folder first, Magento will not remove them. So if you upload new image under `app/design` then remove first images from `pub/static` and then run command for deploying static content.

{% highlight php %}
php bin/magento setup:static-content:deploy en_GB --theme Magento/backend
php bin/magento setup:static-content:deploy en_GB --theme MyCompany/mytheme 
{% endhighlight %}


If you install some new module, you need then recompile files. 

{% highlight php %}
php bin/magento setup:upgrade
bin/magento setup:di:compile
sudo chmod 777 var -R
sudo chmod 777 pub -R
{% endhighlight %}
