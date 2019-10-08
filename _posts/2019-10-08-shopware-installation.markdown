---
layout: post
date:   2019-10-08 07:13:26 +0200
title:  "Shopware6: Installation"
category: php
tags: [php]
---

<h3>Shopware6 Installation</h3>
{% highlight javascript %}
sudo apt install -y php7.3 php7.3-cli php7.3-fpm php7.3-common php7.3-mysql php7.3-curl php7.3-json php7.3-zip php7.3-gd php7.3-xml php7.3-mbstring php7.3-opcache php7.3-intl


sudo a2dismod php5.6 //disable your current version
sudo a2enmod php7.3 //enabled php7.3
sudo service apache2 restart
{% endhighlight %}
