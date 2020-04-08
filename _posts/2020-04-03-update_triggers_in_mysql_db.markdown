---
layout: post
date:   2020-04-03 09:13:26 +0200
title:  "Update triggers in Mysql Database"
category: mysql
tags: [mysql, trigges]
---

Magento2 is using triggers in database. When you are downloading live database to local environment and you are using different database user, you can then have issue that you cant delete product in admin. This is because in trigger is still defined old user from live server
<br /><br />


<h3>Export triggers to file</h3>
{% highlight javascript %}
mysqldump -u DB_USER -pDB_PASSWORD --triggers --add-drop-trigger --no-create-info --no-data --no-create-db --skip-opt DB_NAME > triggers.sql
{% endhighlight %}
<br /><br >

<h3>Change old user to new user</h3>
In your text editor replace old user with new user.
<br /><br >

<h3>Import updated triggers</h3>
{% highlight javascript %}
mysqldump -u DB_USER -pDB_PASSWORD DB_NAME < triggers.sql
{% endhighlight %}
