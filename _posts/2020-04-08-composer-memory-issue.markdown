---
layout: post
date:   2020-04-08 09:13:26 +0200
title:  "Composer memory issue"
category: composer
tags: [composer]
---

When using `composer update` you can get following memory error:
<br /><br />

{% highlight javascript %}
PHP Fatal error:  Allowed memory size of 1610612736 bytes exhausted (tried to allocate 67108864 bytes) in phar:///usr/bin/composer/src/Composer/DependencyResolver/Solver.php on line 220

Fatal error: Allowed memory size of 1610612736 bytes exhausted (tried to allocate 67108864 bytes) in phar:///usr/bin/composer/src/Composer/DependencyResolver/Solver.php on line 220

Check https://getcomposer.org/doc/articles/troubleshooting.md#memory-limit-errors for more info on how to handle out of memory errors.[
{% endhighlight %}

<br /><br /><br /><br />

{% highlight javascript %}
//find path to composer, for example /usr/bin/composer
which composer 
php -d memory_limit=2000M /usr/bin/composer update
{% endhighlight %}