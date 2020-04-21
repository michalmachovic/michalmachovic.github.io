---
layout: post
title:  "Magento2: Print to log file"
date:   2017-09-30 08:13:26 +0200
category: Magento2
tags: [magento2]
---

For `$logger->debug` you need to set `"Stores" -> "Configuration" -> "Advanced" -> "Developer" -> "Debug" -> "Log to File"` to '`Yes`;


{% highlight php %}
<?php
$logger = \Magento\Framework\App\ObjectManager::getInstance()->get('\Psr\Log\LoggerInterface');
$logger->info('*******TEST****');
$logger->debug('*********TEST****');
?>
{% endhighlight %}



Or you can use this:

{% highlight php %}
class Test {
 
    protected $_logger;
 
    public function __construct(
        \Psr\Log\LoggerInterface $logger
    ) {
        $this->_logger = $logger;
    }

    public function execute() {
        $this->_logger->info('*****TEST NEW CRON****');
        $this->_logger->debug('********TEST NEW CRON****');
        return $this;
    }
}
{% endhighlight %}

<br /><br />
Logging SQL queries:

{% highlight php %}
$objectManager = \Magento\Framework\App\ObjectManager::getInstance();
$conn = $objectManager->create('\Magento\Framework\App\ResourceConnection');
$connection = $conn->getConnection();

$tablename = $connection->getTableName('url_rewrite');        

$sql = $connection->select()->from(
		['url_rewrite' => $connection->getTableName('url_rewrite')], ['request_path', 'store_id']
		)->joinLeft(
		['cpe' => $connection->getTableName('catalog_product_entity')], "cpe.entity_id = url_rewrite.entity_id"
		)->where('request_path IN (?)', $urlKey)
		->where('store_id IN (?)', 1)
		->where('cpe.sku not in (?)', $sku);

$this->_logger->debug($sql->__toString());    
{% endhighlight %}

<br /><br />


{% highlight php %}
echo $this->getSelect()->joinInner(
    array('sbao' => $this->getTable('sales/billing_agreement_order')),
    'main_table.entity_id = sbao.order_id',
    array()
)->__toString()
{% endhighlight %}

