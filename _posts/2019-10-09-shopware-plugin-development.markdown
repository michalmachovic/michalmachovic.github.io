---
layout: post
date:   2019-10-09 07:13:26 +0200
title:  "Shopware6: Plugin development"
category: php, shopware
tags: [php, shopware]
---


<h2>Auto generating plugin</h2>
You can skip first three steps and auto generate plugin with
`bin/console plugin:create SwagBundleExample`
<br /><br />


<h2>Creating plugin manually</h2>

1. Create plugin directory under `<shopware root>/custom/plugins`, for example `<shopware root>/custom/plugins/SwagBundleExample`.
`Swag` is company/vendor, `BundleExample` is name of plugin.

<br /><br />
2. Create `composer.json` under your plugin directory
<h3>/custom/plugins/SwagBundleExample/composer.json</h3>
{% highlight php %}
{
    "name": "swag/bundle-example",
    "description": "Bundle example",
    "version": "v1.0.0",
    "license": "MIT",
    "authors": [
        {
            "name": "shopware AG"
        }
    ],
    "type": "shopware-platform-plugin",
    "autoload": {
        "psr-4": {
            "Swag\\BundleExample\\": "src/"
        }
    },
    "extra": {
        "shopware-plugin-class": "Swag\\BundleExample\\BundleExample",
        "copyright": "(c) by shopware AG",
        "label": {
            "de-DE": "Beispiel für Shopware",
            "en-GB": "Example for Shopware"
        }
    }
}
{% endhighlight %}

<br /><br />

3. Create plugin base class
<h3>/custom/plugins/SwagBundleExample/src/BundleExample.php</h3>
{% highlight php %}
<?php declare(strict_types=1);
{% highlight php %}
namespace Swag\BundleExample;

use Shopware\Core\Framework\Plugin;

class BundleExample extends Plugin
{
}
{% endhighlight %}

<br /><br />

4. Install the plugin

`./bin/console plugin:refresh` <br />
`./bin/console plugin:install --activate --clearCache BundleExample`


<br /><br />

<h2>Clear cache</h2>
This should work in 99% cases<br />
`bin/console cache:clear`

<br /><br />
Hard clean cache <br />
`./psh.phar cache`

<br /><br />
<h2>Rebuild storefront - after css changes</h2>
`./psh.phar storefront:dev && ./psh.phar storefront:build && ./psh.phar cache`
