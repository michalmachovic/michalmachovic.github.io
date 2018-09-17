---
layout: post
title:  "Magento: Get data from db using model"
date:   2018-09-17 06:13:26 +0200
category: magento
tags: [magento,db,model]
---

<h2>How to get data from Magento db table using model</h2>
We want to get all records from `cron_schedule` table where `job_code` is `personaliseit`, `status` is `pending`, job wasnt finished so `finished_at` is `NULL` and we want to ignore all entries which are older than 1 hour, so `executed_at` is less than 1 hour.

<br />
{% highlight php %}
error_reporting(E_ALL);
ini_set('display_errors', 1);

require_once './app/Mage.php';
Mage::init();
Mage::app();

$fromDate = date('Y-m-d H:i:s', strtotime('-1 hour'));
$toDate = date('Y-m-d H:i:s', strtotime(now()));    		
	
$data = Mage::getModel('cron/schedule');
$collection = $data->getCollection()
	->addFieldToFilter('job_code', 'personaliseit')
	->addFieldToFilter('status', 'pending')
	->addFieldToFilter('finished_at', array('null' => true))
	->addFieldToFilter('executed_at', array(
		'from' => $fromDate,
	    'to' => $toDate,
	    'date' => true
));

if (count($collection)>0) 
{
	echo 'there are records';
} 
else 
{
	echo 'there are not records'
}
{% endhighlight %}
<br /><br />

