---
layout: post
title:  "Magento2: Adding CSS to a CMS page using Layout Update XML"
date:   2017-10-01 08:13:26 +0200
category: Magento2
tags: [magento2]
---


Create Module Folder Structure:<br />
`app / code / [Vendor] / [ModuleName] `<br />
`app / code / [Vendor] / [ModuleName] / etc`<br />
`app / code / [Vendor] / [ModuleName] / view / frontend / layou`<br />
<br />

Create Module files:<br />
`app / code / [Vendor] / [ModuleName] / registration.php`<br />
`app / code / [Vendor] / [ModuleName] / etc / module.xml`<br />
`app / code / [Vendor] / [ModuleName] / view / frontend / layout / cms_index_index.xml`<br />
<br />




`registration.php`<br />
{% highlight php %}
<?php
\Magento\Framework\Component\ComponentRegistrar::register(
\Magento\Framework\Component\ComponentRegistrar::MODULE,
'[Vendor]_[ModuleName]',
__DIR__
);
{% endhighlight %}
<br /><br />


`module.xml`<br />
{% highlight xml %}
<?xml version="1.0"?>
<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="../../../../../vendor/magento/framework/Module/etc/module.xsd">
<module name="[Vendor]_[ModuleName]" setup_version="0.0.1" />
</config>
{% endhighlight %}
<br /><br />

`cms_index_index.xml`<br />
{% highlight xml %}
<?xml version="1.0"?>
<page xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="../../../../../vendor/magento/framework/Module/etc/module.xsd">
<head>
    <css src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.6/css/bootstrap.min.css" src_type="url" />
    <script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.6/js/bootstrap.min.js" src_type="url" />
</head>
{% endhighlight %}
<br /><br />	

Enable Module (SSH to magento root): <br />
`php -f bin/magento module:enable --clear-static-content [Vendor]_[ModuleName]`<br />
`php -f bin/magento setup:upgrad` <br />

<br />

Deploy static resources (SSH to magento root):<br />
`php bin/magento setup:static-content:deploy`

<br />
Page source code result:
{% highlight html %}
<link  rel="stylesheet" type="text/css"  media="all" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.6/css/bootstrap.min.css" />

<script  type="text/javascript"  src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.6/js/bootstrap.min.js"></script>
{% endhighlight %}


<h2>Adding CSS to a specific CMS page using Layout Update XML</h2>

If you want to insert css to only one specific cms page, there is opened Magento2 bug (https://github.com/magento/magento2/issues/4454), which dont allow you to insert <head> block using Layou XML update.<br />

You need to update<br />

`vendor/magento/framework/View/Layout/etc/page_layout.xsd`  
You need to add <br />
{% highlight xml %}
 <xs:include schemaLocation="urn:magento:framework:View/Layout/etc/head.xsd"/>
    <xs:include schemaLocation="urn:magento:framework:View/Layout/etc/body.xsd"/>

            <xs:element name="head" type="headType" minOccurs="0" maxOccurs="unbounded"/>
            <xs:element name="body" type="bodyType" minOccurs="0" maxOccurs="unbounded"/>
{% endhighlight %}

<br /><br />

Complete file:<br />
{% highlight xml %}
<?xml version="1.0" encoding="UTF-8"?>
<!--
/**
 * Copyright © 2013-2017 Magento, Inc. All rights reserved.
 * See COPYING.txt for license details.
 */
-->
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema">
    <xs:include schemaLocation="urn:magento:framework:View/Layout/etc/elements.xsd"/>
    <xs:include schemaLocation="urn:magento:framework:View/Layout/etc/head.xsd"/>
    <xs:include schemaLocation="urn:magento:framework:View/Layout/etc/body.xsd"/>

    <xs:complexType name="pageLayoutType">
        <xs:sequence minOccurs="0" maxOccurs="unbounded">
            <xs:element ref="referenceContainer" minOccurs="0" maxOccurs="unbounded"/>
            <xs:element ref="container" minOccurs="0" maxOccurs="unbounded"/>
            <xs:element ref="update" minOccurs="0" maxOccurs="unbounded"/>
            <xs:element ref="move" minOccurs="0" maxOccurs="unbounded"/>
            <xs:element name="head" type="headType" minOccurs="0" maxOccurs="unbounded"/>
            <xs:element name="body" type="bodyType" minOccurs="0" maxOccurs="unbounded"/>
        </xs:sequence>
    </xs:complexType>

    <xs:element name="layout" type="pageLayoutType">
        <xs:unique name="containerKey">
            <xs:selector xpath=".//container"/>
            <xs:field xpath="@name"/>
        </xs:unique>
    </xs:element>
</xs:schema>
{% endhighlight %}

<br />

Now you can go to your CMS page and Layout update XML section add<br />

{% highlight html %}
<head>
    <css src="css/brands.css"/>
</head>            
{% endhighlight %}