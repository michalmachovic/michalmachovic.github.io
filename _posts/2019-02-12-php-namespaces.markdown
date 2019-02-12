---
layout: post
date:   2019-02-12 06:13:26 +0200
title:  "PHP: Namespaces"
category: php
tags: [php,namespace]
---

Namespacing does for functions and classes what scope does for variables. It allows you to use the same function or class name in different parts of the same program without causing a name collision.
<br /><br />
In simple terms, think of a namespace as a person's surname. If there are two people named "John" you can use their surnames to tell them apart.




{% highlight php %}
<?php
namespace My\SpecialNamespace;
class MyClass
{
}

$object = new MyClass;
{% endhighlight %}

<br /><br />
Then in another file we call it like this:
{% highlight php %}
$object = new \My\SpecialNamespace\MyClass;
{% endhighlight %}


<br /><br />
Another thing to watch out for is using classes in a different namespace. For example, most built-in PHP classes are located in the global top level namespace. For example, if you tried using the following code.
{% highlight php %}
<?php
namespace My\SpecialNamespace;
class MyClass
{
    public function __construct()
    {
        $test = new DomDocument;
    }
}    
$object = new MyClass;
{% endhighlight %}
<br />
PHP would yell at you with the following error message
{% highlight php %}
PHP Fatal error:  Class 'My\SpecialNamespace\DomDocument' not 
found in /path/to/some/file.php on line 7   
{% endhighlight %}

<br /><br />
PHP assumed we wanted a class named My\SpecialNamespace\DomDocument. The same thing that lets us say new MyClass also means we need to be explicit with our other classes.
<br />
We have two options here:
<br /><br />
<b>1.</b> Use leading slash, this tells PHP to use `global` class `DomDocument`;
{% highlight php %}
$test = new \DomDocument;
{% endhighlight %}

<br />
<b>2.</b>Import that class with `use`. Then you can use `DomDocument` without leading slash.
{% highlight php %}
use DomDocument;
{% endhighlight %}

<br /><br /><br />
This will work with any class, not just built-in PHP classes. Consider the Some\Other\Class\Thing class below.
{% highlight php %}
<?php
namespace My\SpecialNamespace;
use Some\Other\Class\Thing;

$object = new \Some\Other\Class\Thing;
$object = new Thing;
{% endhighlight %}
