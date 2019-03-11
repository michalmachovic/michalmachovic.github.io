---
layout: post
date:   2019-03-01 06:13:26 +0200
title:  "Magento2: How to set different prices for different websites"
category: php, magento2
tags: [magento2]
---


{% highlight php %}
<?php
protected $productResourceModel;
protected $productFactory;

public function __construct(
    \Magento\Catalog\Model\ResourceModel\Product $productResourceModel,
    \Magento\Catalog\Model\ProductFactory $productFactory
) {
    $this->productResourceModel = $productResourceModel;
    $this->productFactory = $productFactory;
}

public function setPricesPerStore($productId, $storeId, $price, $specialPrice)
{
    $productFactory = $this->productFactory->create();
    $this->productResourceModel->load($productFactory, $productId);
    $productFactory->setStoreId($storeId);
    $productFactory->setPrice($price);
    $this->productResourceModel->saveAttribute($productFactory, 'price');
}{% endhighlight php %}