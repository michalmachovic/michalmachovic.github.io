---
layout: post
title:  ".htaccess: Rewrite rules tips"
date:   2019-01-09 06:13:26 +0200
category: htacess
tags: [htaccess,rewrite,rules,redirect]
---


<br /><br />
<h2>Return 403 for URI that match specific string and parameter match specific string</h2>
<br /> <br />
Imagine you want to return 403 for search on your site if in parameter is some value. For example `http://yoursite.com/catalogsearch/?q=a%22%2F%2A%2A%2FAND%2F%2A%2A%2F%28SELECT%2F%2A%2A%2F%2A%2F%2A%2A%2FFROM%2F%2A%2A%2F%28SELECT%28SLEEP%28100-%28IF%28ORD%28MID%28%28SELECT%2F%2A%2A%2Fcolumn_name%2F%2A%2A%2FFROM%2F%2A%2A%2FINFORMATION_SCHEMA.COLUMNS%2F%2A%2A%2FWHERE%2F%2A%2A%2Ftable_name%3D0x61646d696e5f75736572%2F%2A%2A%2FAND%2F%2A%2A%2Ftable_schema%3D0x7469676572%2F%2A%2A%2FLIMIT%2F%2A%2A%2F6%2C1%29%2C5%2C1%29%29%2F%2A%2A%2FNOT%2F%2A%2A%2FBETWEEN%2F%2A%2A%2F0%2F%2A%2A%2FAND%2F%2A%2A%2F115%2C0%2C1%29%29%29%29%29EJOa%29%2F%2A%2A%2FAND%2F%2A%2A%2F%22XiFP%22%3D%22XiFP`
<br /><br />
This can be used for SQL Sleep injection. Using following rewrite rule we can return 403 if in URI is `catalogsearch` and if `q` parameter match one from following strings `sleep|SLEEP|select|SELECT|drop|DROP|table|TABLE|column|COLUMN`.
<br /><br />

{% highlight javascript %}
RewriteCond %{QUERY_STRING} q=(.*)(sleep|SLEEP|select|SELECT|drop|DROP|table|TABLE|column|COLUMN)(.*)
RewriteRule ^(.*)catalogsearch(.*)$ http://yoursite.com/ [R=403,L]
{% endhighlight %}
<br /><br />




