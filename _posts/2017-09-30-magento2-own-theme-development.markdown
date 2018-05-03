---
layout: post
title:  "Magento2: Own theme development"
date:   2017-09-30 06:13:26 +0200
category: Magento2
tags: [magento2]
---

Dont inherit from luma, it isnt working properly. Inherit from blank !

<h3>Theme structure</h3>
Customgateway = vendor name<br />
entertainer = theme name

`app/design/frontend/Customgateway/entertainer`

<br />

<h3>Overriding Magento template files</h3>
example - you want to change htdocs/vendor/magento/module-theme/view/frontend/templates/html/header.phtml (this can you see in template hints) <br />

copy file<br />
`htdocs/vendor/magento/module-theme/view/frontend/templates/html/header.phtml` <br />
to <br />
 `htdocs/app/design/frontend/Customgateway/entertainer/Magento_Theme/templates/html/header.phtml` <br />
<br />

<h3>Css change - manually</h3>
1. update css file under app/design... <br />
2. delete css file under pub/static... <br />
 `rm -rf pub/static/frontend/Customgateway/entertainer/en_GB/css/local-l.css` <br />
3. deploy static content only for that theme, exclude unnecessary folders <br />
 `php bin/magento setup:static-content:deploy en_GB --theme Customgateway/entertainer --no-html --no-fonts --no-images --no-less --no-javascript --no-html-minify`

<br />
<h3>Css change - automatically</h3>
use grunt for copying files





