---
layout: post
title:  "Magento2: Troubleshooting"
date:   2018-08-31 06:13:26 +0200
category: magento2
tags: [magento2]
---

<h2>Cant login to my account on localhost</h2>
Change your url localhost to 127.0.0.1 of magento.

{% highlight ts %}
update core_config_data set value = "http://127.0.0.1/magento2/sites/magento2_test10/" where path = "web/unsecure/base_url";
{% endhighlight %}

<br /><br />
