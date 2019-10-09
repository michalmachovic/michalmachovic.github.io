---
layout: post
date:   2019-10-09 07:13:26 +0200
title:  "Shopware6: Plugin development"
category: php, shopware
tags: [php, shopware]
---
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
