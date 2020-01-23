---
layout: post
date:   2019-10-08 07:13:26 +0200
title:  "Shopware6: Installation"
category: php
tags: [php]
---

<h3>With docker</h3>

`git clone https://github.com/shopware/development.git` <br />
`cd development` <br />
`git clone https://github.com/shopware/platform.git` <br />

1. Build and start the containers <br />
`./psh.phar docker:start`
<br /><br />

2. Access the application container <br />
`./psh.phar docker:ssh`
<br /><br />

3. Execute the installer inside the docker container <br />
`./psh.phar install`

<br /><br />

After this you should see frontend on `http://localhost:8000/` and backend on `http://localhost:8000/admin`.<br />
For admin use credentials `admin/shopware`.

<br /><br />

<h3>Switching between different Shopware installations</h3>
Go to your installation, `development`.
<br />
1. Build and start the containers <br />
`./psh.phar docker:start`
<br /><br />

2. Access the application container <br />
`./psh.phar docker:ssh`
<br /><br />

<br /><br />

<h3>Without docker</h3>
{% highlight javascript %}
sudo apt install -y php7.3 php7.3-cli php7.3-fpm php7.3-common php7.3-mysql php7.3-curl php7.3-json php7.3-zip php7.3-gd php7.3-xml php7.3-mbstring php7.3-opcache php7.3-intl


sudo a2dismod php5.6 //disable your current version
sudo a2enmod php7.3 //enabled php7.3
sudo service apache2 restart
{% endhighlight %}

<br /><br />
Dont forget to enable `mod_rewrite`.
<br /><br />

When installation was complete, i was redirected to admin where First Run Wizard begin. I wasnt able to go through last step where domain should be verified.

{% highlight javascript %}
Verification failed
The domain could not be verified

errors: [{code: "FRAMEWORK__STORE_ERROR", status: "500", title: "Verification failed",…}]
0: {code: "FRAMEWORK__STORE_ERROR", status: "500", title: "Verification failed",…}
code: "FRAMEWORK__STORE_ERROR"
detail: "The domain could not be verified"
meta: {documentationLink: "shop-domain-verification-failed"}
status: "500"
title: "Verification failed"

In documentation is written

It should be noted that the domain entered here must already be externally accessible and that the web server must refer to the public-Directory within the Shopware 6 installation.
{% endhighlight %}

<br /><br />
Here is my testing config

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

Dont forget to run `sudo a2ensite shopware1.lan.conf`.

<br /><br />

I remove First Run Wizard directly in code
<h3>/var/www/html/shopware/sites/shopware1.lan/vendor/shopware/administration/Controller/AdministrationController.php</h3>
{% highlight php %}
public function index(): Response
{
    $template = $this->finder->find('administration/index.html.twig');

    return $this->render($template, [
        'features' => FeatureConfig::getAll(),
        'systemLanguageId' => Defaults::LANGUAGE_SYSTEM,
        'defaultLanguageIds' => [Defaults::LANGUAGE_SYSTEM],
        'liveVersionId' => Defaults::LIVE_VERSION,
        //'firstRunWizard' => $this->firstRunWizardClient->frwShouldRun(),
        'firstRunWizard' => false
    ]);
}
{% endhighlight %}

<br /><br />

<h3>Possible issues</h3>
If you see messages like this>
{% highlight javascript %}
SQLSTATE[HY000] [2054] The server requested authentication method unknown to the client
{% endhighlight %}

{% highlight javascript %}
ERROR 2059 (HY000): Authentication plugin 'caching_sha2_password' cannot be loaded: /usr/lib/x86_64-linux-gnu/mariadb18/plugin/caching_sha2_password.so: cannot open shared object file: No such file or directory
{% endhighlight %}

go to mysql docker container
{% highlight javascript %}
docker exec -ti development_app_mysql_1 bash -l
mysql -u app -papp
ALTER USER 'app'@'%' IDENTIFIED WITH mysql_native_password BY 'app';
{% endhighlight %}
