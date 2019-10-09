---
layout: post
date:   2019-10-09 07:13:26 +0200
title:  "How to reset mysql root password"
category: mysql
tags: [mysql]
---


{% highlight javascript %}
sudo /etc/init.d/mysql stop
sudo mysqld_safe --skip-grant-tables &
mysql -uroot

- MySQL version < 5.7
update user set password=PASSWORD("mynewpassword") where User='root';

-- MySQL 5.7, mysql.user table "password" field -> "authentication_string"
update user set authentication_string=password('mynewpassword') where user='root';

update user set plugin="mysql_native_password" where user='root';

sudo /etc/init.d/mysql stop
sudo /etc/init.d/mysql start
{% endhighlight %}

<br /><br />
When you see a socket error

{% highlight javascript %}
mkdir -p /var/run/mysqld && chown mysql:mysql /var/run/mysqld  
{% endhighlight %}
