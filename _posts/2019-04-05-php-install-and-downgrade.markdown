---
layout: post
date:   2019-04-05 06:13:26 +0200
title:  "PHP: Install and downgrade"
category: php
tags: [php, install]
---

{% highlight javascript %}
sudo apt-get install mysql-server
sudo apt-get install php
sudo apt-get install libapache2-mod-php7.2
sudo apt-get install php-mysql
sudo apt-get install php7.2-xml

//install 7.0
sudo add-apt-repository ppa:ondrej/php
sudo apt-get update
sudo apt-get install php7.0 php5.6 php5.6-mysql php-gettext php5.6-mbstring php-mbstring php7.0-mbstring php-xdebug libapache2-mod-php5.6 libapache2-mod-php7.0


//which PHP CLI version should be used
sudo update-alternatives --set php /usr/bin/php7.0

//disable one version and enable antoher
sudo a2dismod php7.2
sudo a2enmod php7.0
sudo service apache2 restart

//remove completely PHP from your system
sudo apt-get purge `dpkg -l | grep php| awk '{print $2}' |tr "\n" " "`
{% endhighlight %}
