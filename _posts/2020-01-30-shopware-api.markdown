---
layout: post
date:   2020-01-30 09:13:26 +0200
title:  "Shopware - API and Guzzle"
category: shopware
tags: [shopware, api]
---


<h2>With Insomnia REST Client</h2>
We will use Insomnia REST Client. (`https://insomnia.rest`)
{% highlight javascript %}
sudo snap install insomnia
{% endhighlight %}

<br /><br />
In Shopware admin (`http://localhost:8000/admin`) go to `Settings -> System -> Integrations` and `Add integration`.
<br /><br />

<h3>Authentication</h3>
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

<h3>Get Orders</h3>
{% highlight javascript %}
GET: http://localhost:8000/api/v1/order
{% endhighlight %}

<br /><br />


<h3>Get Order</h3>
{% highlight javascript %}
GET: http://localhost:8000/api/v1/order?filter[order.id]=197DD5C23E584BADB19EBF15082A6286
{% endhighlight %}

<br /><br />

<h3>Get Order Line Items</h3>
{% highlight javascript %}
GET: http://localhost:8000/api/v1/order/197dd5c23e584badb19ebf15082a6286/line-items
{% endhighlight %}

<br /><br />
https://docs.shopware.com/en/shopware-platform-dev-en/api/filter-search-limit?category=shopware-platform-dev-en/api

<br /><br /><br />
<h2>With Guzzle</h2>
Guzzle is a PHP HTTP client that makes it easy to send HTTP requests and trivial to integrate with web services.
<br /><br />

{% highlight php %}
<?php
error_reporting(E_ALL);
ini_set('display_errors', 1);

require('vendor/autoload.php');

$client = new GuzzleHttp\Client();
$res = $client->request('POST', 'http://localhost:8000/api/oauth/token', [
    'json' => ["client_id" => "SWIAWM9RNHE1QNVMMHE5AEE4SG",
                "client_secret" => "T21NQlpGM2dCcGk2cGFNVlFEdGgwcld4TEd4WmM4d1A4SEZpQTk",
                "grant_type" => "client_credentials"]
    ]);

    $body = json_decode($res->getBody());

    $guzzle = new GuzzleHttp\Client(['base_uri' => 'http://localhost:8000/']);
    $headers = [
        'Authorization' => 'Bearer ' . $body->access_token,        
        'Accept'        => 'application/json',
    ];
   
    $response = $client->request('GET', 'http://localhost:8000/api/v1/order/197dd5c23e584badb19ebf15082a6286/line-items',  [
        'headers' => $headers
    ]);

   echo $response->getBody();
?>
{% endhighlight %}

