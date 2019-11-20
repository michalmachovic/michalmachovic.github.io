---
layout: post
date:   2019-11-20 07:13:26 +0200
title:  "Using Zend Http Client locally on self signed domain"
category: php, zend
tags: [php, zend]
---

If you want to use Zend Http Client locally on your computer on self signed domain without valid certificate, you can get error `Unable to enable crypto on TCP connection make sure the "sslcafile" or "sslcapath" option are properly set for the environment`. This can be solved by creating client with following options.


{% highlight php %}
$options = array(
    'sslallowselfsigned' => true,
    'sslverifypeer' => false,
    'sslverifypeername' => false
);

$this->_httpClient = new \Zend\Http\Client(null, $options);
}
{% endhighlight %}
