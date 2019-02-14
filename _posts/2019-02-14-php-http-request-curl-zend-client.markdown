---
layout: post
date:   2019-02-14 06:13:26 +0200
title:  "PHP: HTTP Request with curl and Zend Client"
category: php
tags: [php,http,request,curl,zend,client]
---

<h2>curl</h2>
You can use file_get_contents function to get content from some server. In some cases this function can be blocked because of security reasons or you need to download big file where usually file_get_contents crash. In this case you can use curl to download content.
{% highlight php %}
<?php
 $url = 'YOUR_URL/main.jpg';
 $cookie = tempnam ("/tmp", "CURLCOOKIE");
 $ch = curl_init ($url);
 curl_setopt($ch, CURLOPT_FOLLOWLOCATION, true);
 curl_setopt($ch, CURLOPT_USERAGENT, "Mozilla/5.0 (Debian) Firefox/0.10.1");
 curl_setopt($ch, CURLOPT_COOKIEJAR, $cookie);
 curl_setopt($ch, CURLOPT_HEADER, false);
 curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
 curl_setopt($ch, CURLOPT_BINARYTRANSFER,true);
 curl_setopt($ch, CURLOPT_CONNECTTIMEOUT, 10);
 curl_setopt($ch, CURLOPT_TIMEOUT, 10);
 curl_setopt($ch, CURLOPT_MAXREDIRS, 5);
 curl_setopt($ch, CURLOPT_ENCODING, "");
 curl_setopt($ch, CURLINFO_EFFECTIVE_URL, true);
 curl_setopt($ch, CURLOPT_REFERER, "YOUR_URL");
 curl_setopt($ch, CURLOPT_POST, false);
 $rawdata=curl_exec($ch);
 curl_close($ch);
 file_put_contents("main3.jpg", $rawdata);
?>
{% endhighlight %}


<br /><br />

<h2>Zend HTTP Client</h2>
Here we create new request with Zend Http Client to send username and password with POST to autenthicate user.
{% highlight php %}
$this->_httpClient = new \Zend\Http\Client();
$data = array(
	'username' => $this->_userName, 
	'password' => $this->_userPass
);


$this->_httpClient->setUri($this->_httpAuthUrl);
$this->_httpClient->setMethod('POST');
$this->_httpClient->setRawBody(json_encode($data));
$response = $this->_httpClient->send();	
{% endhighlight %}

