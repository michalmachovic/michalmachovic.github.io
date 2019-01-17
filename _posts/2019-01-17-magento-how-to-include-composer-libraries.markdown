---
layout: post
date:   2019-01-17 06:13:26 +0200
title:  "Magento: How to include composer libraries"
category: javascript
tags: [magento,composer]
---


<br /><br />
This article is taken from [http://davemacaulay.com/how-to-include-composer-libraries-within-magento/].
<br /><br />
Recently I’ve been playing around with Elasticsearch and I had the need to use their PHP library with Magento. As this library wasn’t conforming to the Zend standard I had some issues getting everything up and running.
<br /><br />
Firstly create a new folder in your lib directory. In my case I created one called `lib/Elasticsearch/` then within this directory create your `composer.json` file, in my instance I used this.

<br />
{% highlight php %}
{
    "require": {
        "elasticsearch/elasticsearch": "~1.0"
    }
}
{% endhighlight %}
 <br /><br />

Then navigate to the folder via command line and run the standard
<br /><br />

{% highlight php %}
   composer install
{% endhighlight %}

This will create a vendor folder in your directory, so now you’ll have `lib/Elasticsearch/vendor` with the required packages and dependencies.
<br /><br />

Now you’ll need to create a new module to get the autoloading working. Considering I’m building a Magento Elasticsearch module for Gene I called mine `Gene/Elasticsearch`. Once you’ve created this module create a new observer event within your config.xml for the `controller_front_init_before` event.

<br /><br />

{% highlight php %}
<events>
    <!-- Support the lib/Elasticsearch/ folder -->
    <controller_front_init_before>
        <observers>
            <elasticsearch_lib_load>
                <type>singleton</type>
                <class>gene_elasticsearch/observer</class>
                <method>controllerFrontInitBefore</method>
            </elasticsearch_lib_load>
        </observers>
    </controller_front_init_before>
</events>
{% endhighlight %}

<br /><br />
Within this we’re declaring that we want the `controllerFrontInitBefore` function to run when Magento does it’s own controller_front_init_before. This is an early event allowing us to get our autoloader in on every page that requires a controller. If you need it elsewhere then keep reading!
<br /><br />
Now you need to create your `Observer.php` in your Model folder. Below is an example of the file I’m using.
<br /><br />

{% highlight php %}
<?php
 
/**
 * Class Gene_Elasticsearch_Model_Observer
 * @author Dave Macaulay <dave@gene.co.uk>
 */
class Gene_Elasticsearch_Model_Observer
{
    /**
     * Include our composer auto loader for the ElasticSearch modules
     *
     * @param Varien_Event_Observer $event
     */
    public function controllerFrontInitBefore(Varien_Event_Observer $event)
    {
        self::init();
    }
 
    /**
     * Add in auto loader for Elasticsearch components
     */
    static function init()
    {
        // Add our vendor folder to our include path
        set_include_path(get_include_path() . PATH_SEPARATOR . Mage::getBaseDir('lib') . DS . 'Elasticsearch' . DS . 'vendor');
 
        // Include the autoloader for composer
        require_once(Mage::getBaseDir('lib') . DS . 'Elasticsearch' . DS . 'vendor' . DS . 'autoload.php');
    }
 
}
{% endhighlight %}

You will need to alter the ‘Elasticseach’ path to the name you gave your folder within lib. I declared init as a static function so if the event wasn’t ran for any reason (say a standalone php script which just runs Mage::app()) that I’d be able to just run
<br /><br />

<h2>My updates</h2>
In the controller file, in some action when i want to use it, then i have to run `Gene_Elasticsearch_Model_Observer::init();`. Also if PHP library is using namespaces, in th beginning of controller file i have to add `use Auth0\SDK\Auth0;` (i was using php `Auth0` library - [https://github.com/auth0/auth0-PHP]).
