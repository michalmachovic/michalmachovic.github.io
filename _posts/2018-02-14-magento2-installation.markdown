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


<h2>Deploy static content and clear cache with magerun</h2>
{% highlight ts %}
n98-magerun2.phar cache:clean && n98-magerun2.phar cache:flush && sudo chmod 777 var -R && sudo chmod 777 pub -R
{% endhighlight %}
<br /><br />