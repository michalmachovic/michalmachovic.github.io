---
layout: post
date:   2020-04-21 10:13:26 +0200
title:  "Magento2: Importing 2 products with same name"
category: magento2
tags: [magento2]
---
If you are creating products in Magento2 programatically and you have 2 products with same name, you can have error `URL key for specified store already exists`. This is because of bug described here `https://github.com/magento/magento2/issues/7298`.

<br /><br />

{% highlight php %}
public function __construct(
    --
    \Magento\Store\Model\StoreManagerInterface $storeManager, 
    \Magento\Framework\App\ResourceConnection $resource, 
    --
) {
    --
    $this->_storeManager = $storeManager;
    $this->_resource = $resource;
    --  
}



public function createUrlKey($title, $sku) 
{
    $url = preg_replace('#[^0-9a-z]+#i', '-', $title);
    $urlKey = strtolower($url);
    $storeId = (int) $this->_storeManager->getStore()->getStoreId();

    $isUnique = $this->checkUrlKeyDuplicates($sku, $urlKey, $storeId);
    if ($isUnique) {
        return $urlKey;
    } else {
        return $urlKey . '-' . time();
    }
}

/*
 * Function to check URL Key Duplicates in Database
 */

private function checkUrlKeyDuplicates($sku, $urlKey, $storeId) 
{
    $urlKey .= '.html';

    $connection = $this->_resource->getConnection(\Magento\Framework\App\ResourceConnection::DEFAULT_CONNECTION);

    $tablename = $connection->getTableName('url_rewrite');
    $sql = $connection->select()->from(
                    ['url_rewrite' => $connection->getTableName('url_rewrite')], ['request_path', 'store_id']
            )->joinLeft(
                    ['cpe' => $connection->getTableName('catalog_product_entity')], "cpe.entity_id = url_rewrite.entity_id"
            )->where('request_path IN (?)', $urlKey)
            ->where('store_id IN (?)', $storeId)
            ->where('cpe.sku not in (?)', $sku);

    $urlKeyDuplicates = $connection->fetchAssoc($sql);

    if (!empty($urlKeyDuplicates)) {
        return false;
    } else {
        return true;
    }
}
{% endhighlight %}