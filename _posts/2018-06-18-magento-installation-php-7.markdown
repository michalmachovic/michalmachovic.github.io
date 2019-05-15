---
layout: post
title:  "Magento: Installation on PHP7"
date:   2018-06-18 06:13:26 +0200
category: magento
tags: [magento,php,php7]
---

If you are installing Magento on PHP7, you can get error message `This page isn’t working localhost is currently unable to handle this request. HTTP ERROR 500`.

<br /><br />

Go to `index.php` and turn on error display `ini_set('display_errors', 1);` <br /><br />

You will get error message `Fatal error: Uncaught Error: Function name must be a string in .../app/code/core/Mage/Core/Model/Layout.php:555` <br /><br />

In file `app/code/core/Mage/Core/Model/Layout.php` change: <br /><br />
`$out .= $this->getBlock($callback[0])->$callback[1]();` <br />
to <br />
`$out .= $this->getBlock($callback[0])->{$callback[1]}();` <br />


<br /><br /><br />
In db configuration you can get error `Database server does not support the InnoDB storage engine.`;<br /><br />

In file `app/code/core/Mage/Install/Model/Installer/Db/Mysql4.php` change: <br /><br />

{% highlight ts %}
public function supportEngine()
{
    $variables  = $this->_getConnection()
        ->fetchPairs('SHOW VARIABLES');
    return (!isset($variables['have_innodb']) || $variables['have_innodb'] != 'YES') ? false : true;
}
{% endhighlight %}

<br />to<br />
{% highlight ts %}
public function supportEngine()
{
    $variables  = $this->_getConnection()
        ->fetchPairs('SHOW ENGINES');
    return (isset($variables['InnoDB']) && $variables['InnoDB'] != 'NO');
}
{% endhighlight %}


<br /><br />
<h2>Image upload not working in admin on product page</h2>

In file `lib/Varien/File/Uploader.php` change: <br /><br />

{% highlight ts %}
$params['object']->$params['method']($this->_file['tmp_name']);
{% endhighlight %}

<br />to<br />
{% highlight ts %}
$params['object']->{$params['method']}($this->_file['tmp_name']);
{% endhighlight %}
