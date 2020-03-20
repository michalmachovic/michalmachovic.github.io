---
layout: post
date:   2019-10-08 07:13:26 +0200
title:  "PHP: Switch version"
category: php
tags: [php]
---

{% highlight javascript %}
sudo a2dismod php5.6
sudo a2enmod php7.2
sudo service apache2 restart

//for cli
sudo update-alternatives --set php /usr/bin/php7.2
{% endhighlight %}
