---
layout: post
date:   2019-10-08 07:13:26 +0200
title:  "How to create vhost"
category: php
tags: [php]
---

<h3>/etc/apache2/sites-available/shopware1.lan.conf</h3>
{% highlight javascript %}
<VirtualHost *:80>
ServerAdmin webmaster@ostechnix1.lan
ServerName shopware1.lan
ServerAlias www.shopware1.lan
DocumentRoot /var/www/html/shopware/sites/shopware1.lan/public

ErrorLog ${APACHE_LOG_DIR}/error.log
CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
{% endhighlight %}

<br /><br />

<h3>/etc/hosts</h3>
{% highlight javascript %}
127.0.0.1       shopware1.lan
{% endhighlight %}

<br /><br />
After this you need to run
{% highlight javascript %}
sudo a2ensite shopware1.lan.conf
systemctl reload apache2
{% endhighlight %}
