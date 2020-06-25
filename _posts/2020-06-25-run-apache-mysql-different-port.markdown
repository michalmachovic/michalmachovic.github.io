---
layout: post
date:   2020-06-25 13:13:26 +0200
title:  "How to run Apache and Mysql on different port"
category: apache, mysql
tags: [apache, mysql]
---
Default ports are 80 for apache and 3306 for mysql.
<br /><br />

<h2>How to run Apache on different port 8079</h2>
<h3>/etc/apache2/ports.conf</h3>
{% highlight javascript %}
Listen 8079
{% endhighlight %}
<br /><br />

<h3>/etc/apache2/sites-enabled/000-default.conf</h3>
{% highlight javascript %}
<VirtualHost *: 8079>
{% endhighlight %}
<br /><br />
{% highlight javascript %}
sudo service apache2 restart
{% endhighlight %}


<br /><br />
<h2>How to run Mysql on different port 3308</h2>
<h3>/etc/mysql/mysql.conf.d/mysqld.cnf</h3>
{% highlight javascript %}
port = 3308
{% endhighlight %}
<br /><br />

{% highlight javascript %}
sudo service mysql restart
{% endhighlight %}
