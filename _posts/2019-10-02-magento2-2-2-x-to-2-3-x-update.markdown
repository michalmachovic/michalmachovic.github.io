---
layout: post
date:   2019-10-02 09:13:26 +0200
title:  "Magento2: 2.2.x to 2.3.x update"
category: magento2
tags: [magento2]
---

<h3>Step 1</h3>
Backup code and database
<br /><br />

<h3>Step 2</h3>
`composer require magento/product-community-edition=2.3.0 --no-update`
<br /><br />

<h3>Step 3</h3>
`composer require --dev phpunit/phpunit:~6.2.0 friendsofphp/php-cs-fixer:~2.10.1 lusitanian/oauth:~0.8.10 pdepend/pdepend:2.5.2 sebastian/phpcpd:~3.0.0 squizlabs/php_codesniffer:3.2.2 --no-update`
<br /><br />

<h3>Step 4</h3>
`composer remove --dev sjparkinson/static-review fabpot/php-cs-fixer --no-update`
<br /><br />

<h3>Step 5</h3>
in `composer.json` file add <br />
`"Zend\\Mvc\\Controller\\": "setup/src/Zend/Mvc/Controller/"`
<br />to `autoload` `psr-4` section
<br /><br />

<h3>Step 6</h3>
`composer update`
<br /><br />

<h3>Step 7</h3>
`sudo php bin/magento setup:upgrade` <br />
`sudo php bin/magento setup:di:compile` <br />
`sudo n98-magerun2.phar cache:clean && sudo n98-magerun2.phar cache:flush && sudo chmod 777 var -R && sudo chmod 777 pub -R`
