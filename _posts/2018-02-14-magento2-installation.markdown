---
layout: post
title:  "Magento2: Installation"
date:   2018-02-14 06:13:26 +0200
category: magento2
tags: [magento2]
---

<h2>Install Composer</h2>
You can skip this step if you’ve already installed the Composer.
{% highlight ts %}
curl -sS https://getcomposer.org/installer | php
{% endhighlight %}
<br /><br />


<h2>Make Composer Globally Available</h2>
If you wish, you can additionally install Composer globally so you don’t have to type php/path/to/composer.phar every time. The Windows installer will automatically set up the PATH system variable.

{% highlight ts %}
mv composer.phar /usr/local/bin/composer
{% endhighlight %}
<br /><br />



<h2>Download Magento 2</h2>
Run the following command in the root directory.
{% highlight ts %}
composer create-project --repository-url=https://repo.magento.com/ magento/project-community-edition .

//or for specific version
composer create-project --repository-url=https://repo.magento.com/ magento/project-community-edition=2.2.6 .
{% endhighlight %}
<br /><br />
You can get message about missing libraries, following you will require:
<br />
ext-mbstring<br />
ext-gd<br />
ext-mcrypt<br />
ext-bcmath<br />
ext-curl<br />
ext-intl<br />
ext-zip<br />
ext-soap<br />
<br />
You can install them with following command:
{% highlight ts %}
sudo apt-get install php7.0-mbstring php7.0-gd php7.0-mcrypt php7.0-bcmath php7.0-curl php7.0-intl php7.0-zip php7.0-soap
{% endhighlight %}
<br /><br />

<h2>Set Up Permissions</h2>
After all the dependencies are retrieved, you should set the correct permissions on the entire Magento 2 installation directory. The official documentation recommends chmod’ing all directories to 700 and all files to a level of 600, however that didnt work for me.
{% highlight ts %}
find . -type f -exec chmod 644 {} \; // 644 permission for files
find . -type d -exec chmod 755 {} \; // 755 permission for directory
find ./var -type d -exec chmod 777 {} \; // 777 permission for var folder find ./pub/media -type d -exec chmod 777 {} \;
find ./pub/static -type d -exec chmod 777 {} \;
chmod 777 ./app/etc && chmod 644 ./app/etc/*.xml
{% endhighlight %}
<br /><br />


<h2>Create The Database</h2>
<br /><br />


<h2>After Installation</h2>
If you dont see styles, check paths in sourcode. If you have there something like /version..../ run following querty
{% highlight ts %}
INSERT INTO `core_config_data` (`scope`, `scope_id`, `path`, `value`) VALUES ('default', 0, 'dev/static/sign', '0');
{% endhighlight %}
<br /><br />


<h2>Admin not working</h2>
in file `/etc/apache2/apache2.conf` make sure you have `AllowOverride All`
{% highlight ts %}
<Directory /var/www/>
Options Indexes FollowSymLinks
AllowOverride All
Require all granted
{% endhighlight %}

<br /><br />

{% highlight ts %}
sudo a2enmod rewrite
sudo service apache2 restart
{% endhighlight %}

<br /><br />


<h2>Admin not working - redirect issue</h2>
To correct this, please try to set Use Secure URLs in Admin to true, in System -> Config -> General -> Web.

{% highlight ts %}
UPDATE core_config_data set value = "1" where path like '%web/secure/use_in_adminhtml%';
{% endhighlight %}

<br /><br />



<h2>Clear cache with magerun</h2>
{% highlight ts %}

n98-magerun2.phar cache:clean && n98-magerun2.phar cache:flush && sudo chmod 777 var -R && sudo chmod 777 pub -R
{% endhighlight %}
<br /><br />

<h2>Deploy content</h2>
{% highlight ts %}
php bin/magento setup:static-content:deploy en_GB en_US -f
{% endhighlight %}
<br /><br />

<h2>Upgrade and compile (if you are adding some new modules with composer)</h2>
 {% highlight ts %}
 sudo php bin/magento setup:upgrade
 sudo php bin/magento setup:di:compile
 sudo n98-magerun2.phar cache:clean && sudo n98-magerun2.phar cache:flush && sudo chmod 777 var -R && sudo chmod 777 pub -R
 {% endhighlight %}
