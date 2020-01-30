---
layout: post
date:   2020-01-30 09:13:26 +0200
title:  "Shopware - API"
category: shopware
tags: [shopware, api]
---

We will use Insomnia REST Client. (`https://insomnia.rest`)

{% highlight javascript %}
sudo snap install insomnia
{% endhighlight %}

<br /><br />
In Shopware admin (`http://localhost:8000/admin`) go to `Settings -> System -> Integrations` and `Add integration`.
<br /><br />

<h2>Authentication</h2>
In Insomnia create new POST request:

{% highlight javascript %}
POST: http://localhost:8000/api/oauth/token

JSON:
{
	"client_id": "SHOPWARE_ACCESS_KEY_ID",
	"client_secret": "SHOPWARE_SECRET_KEY",
	"grant_type": "client_credentials"
}
{% endhighlight %}

After running this request copy generated `access_token`.

<br /><br />

<h2>Get Orders</h2>
{% highlight javascript %}
GET: http://localhost:8000/api/v1/order
{% endhighlight %}

<br /><br />


<h2>Get Order</h2>
{% highlight javascript %}
GET: http://localhost:8000/api/v1/order?filter[order.id]=197DD5C23E584BADB19EBF15082A6286
{% endhighlight %}

<br /><br />

<h2>Get Order Line Items</h2>
{% highlight javascript %}
GET: http://localhost:8000/api/v1/order/197dd5c23e584badb19ebf15082a6286/line-items
{% endhighlight %}

<br /><br />
https://docs.shopware.com/en/shopware-platform-dev-en/api/filter-search-limit?category=shopware-platform-dev-en/api