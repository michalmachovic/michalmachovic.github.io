---
layout: post
date:   2019-02-15 06:13:26 +0200
title:  "Magento2: Basic Frontend Module with Database Table and get Collection"
category: php, magento2
tags: [magento2,module,frontend,db,collection]
---

This article refer to https://michalmachovic.github.io/php,%20magento2/2019/02/08/magento2-basic-module.html, follow that article to create frontend module first.

<h2>Updated Structure</h2>
For working with database we need `Model` and `Setup` folders.
{% highlight php %}
Block
 |
 --Helloworld.php
Controller
 |
 Index
   |
   --Index.php
Model
 |
 --Category.php
 |
 ResourceModel
   |
   --Category.php  
   |
   --Category
       |
       --Collection.php
Setup       
 |
 --InstallSchema.php
etc
 |
 module.xml
 frontend
    |
    --routes.xml
view
 |
 layout
  |
  --helloworld_index_index.xml
  |
  templates
  |
  --helloworld.phtml
registration.php
{% endhighlight %}


<br /><br />
<h2>Schema/InstallSchema.php</h2>
This file will create db table.
{% highlight php %}

<?php

namespace MichalMachovic\Helloworld\Setup;

use Magento\Framework\Setup\InstallSchemaInterface;
use Magento\Framework\Setup\ModuleContextInterface;
use Magento\Framework\Setup\SchemaSetupInterface;
use Magento\Framework\DB\Ddl\Table;

class InstallSchema implements InstallSchemaInterface
{
    public function install(SchemaSetupInterface $setup, ModuleContextInterface $context)
    {
        $installer = $setup;
        $installer->startSetup();

        // Get tutorial_simplenews table
        $tableName = $installer->getTable('my_category');
        // Check if the table already exists
        if ($installer->getConnection()->isTableExists($tableName) != true) {
            // Create tutorial_simplenews table
            $table = $installer->getConnection()
                ->newTable($tableName)
                ->addColumn(
                    'id',
                    Table::TYPE_INTEGER,
                    10,
                    [
                        'identity' => true,
                        'unsigned' => true,
                        'nullable' => false,
                        'primary' => true
                    ],
                    'ID'
                )
                ->addColumn(
                    'datasource_code',
                    Table::TYPE_TEXT,
                    255,
                    ['nullable' => false, 'default' => ''],
                    'Datasource code'
                )
                ->addColumn(
                    'datasource_name',
                    Table::TYPE_TEXT,
                    255,
                    ['nullable' => false, 'default' => ''],
                    'Datasource name'
                )
                ->addColumn(
                    'updated_at',
                    Table::TYPE_DATETIME,
                    null,
                    ['nullable' => false],
                    'Updated At'
                )
                ->addColumn(
                    'parent_code',
                    Table::TYPE_TEXT,
                    255,
                    ['nullable' => false, 'default' => ''],
                    'Parent code'
                )
                ->addColumn(
                    'category_id',
                    Table::TYPE_INTEGER,
                    10,
                    [],
                    'Category Id'
                )
                ->addColumn(
                    'processed',
                    Table::TYPE_INTEGER,
                    4,
                    ['nullable' => false, 'default' => 0],
                    'Processed'
                )    
                ->addColumn(
                    'status',
                    Table::TYPE_TEXT,
                    255,
                    ['nullable' => false, 'default' => ''],
                    'Status'
                )
                ->addColumn(
                    'serialised',
                    Table::TYPE_BLOB,
                    null,
                    ['nullable' => false, 'default' => ''],
                    'Serialised'
                )
                ->addColumn(
                    'deleted',
                    Table::TYPE_INTEGER,
                    4,
                    ['nullable' => false, 'default' => 0],
                    'Deleted'
                )
                ->setComment('My Abstract Category')
                ->setOption('type', 'InnoDB')
                ->setOption('charset', 'utf8');
            $installer->getConnection()->createTable($table);
        }

        $installer->endSetup();
    }
}



<br /><br />
<h2>Model/Category.php</h2>

{% highlight php %}
<?php

namespace MichalMachovic\Helloworld\Model;

use Magento\Framework\Model\AbstractModel;

class Category extends \Magento\Framework\Model\AbstractModel implements \Magento\Framework\DataObject\IdentityInterface
{ 

  const CACHE_TAG = 'my_category';

  protected $_cacheTag = 'my_category';

  protected $_eventPrefix = 'my_category';


  
    /**
     * Define resource model
     */
    protected function _construct()
    {
        $this->_init('MichalMachovic\Helloworld\Model\ResourceModel\Category');
    }


    public function getIdentities()
  {
    return [self::CACHE_TAG . '_' . $this->getId()];
  }

  public function getDefaultValues()
  {
    $values = [];

    return $values;
  }


}

{% endhighlight %}





<br /><br />
<h2>Model/ResourceModel/Category.php</h2>

{% highlight php %}
<?php

namespace MichalMachovic\Helloworld\Model\ResourceModel;

class Category  extends \Magento\Framework\Model\ResourceModel\Db\AbstractDb
{ 
  public function __construct(
    \Magento\Framework\Model\ResourceModel\Db\Context $context
  )
  {
    parent::__construct($context);
  }

    /**
     * Define main table
     */
    protected function _construct()
    {
        $this->_init('my_category', 'id'); //db table
    }
}
{% endhighlight %}



<br /><br />
<h2>Model/ResourceModel/Category/Collection.php</h2>

{% highlight php %}
<?php

namespace MichalMachovic\Helloworld\Model\ResourceModel\Category;

use Magento\Framework\Model\ResourceModel\Db\Collection\AbstractCollection;

class Collection extends \Magento\Framework\Model\ResourceModel\Db\Collection\AbstractCollection
{
    /**
     * Define model & resource model
     */
    protected function _construct()
    {
        $this->_init(
            'MichalMachovic\Helloworld\Model\Category',
            'MichalMachovic\Helloworld\Model\ResourceModel\Category'
        );
    }
}

{% endhighlight %}







<br /><br />
<h2>Controller/Index/Index.php</h2>

{% highlight php %}
<?php
 
namespace MichalMachovic\Helloworld\Controller\Index;
use Magento\Framework\App\Action\Context;
use MichalMachovic\HelloWorld\Model\HelloFactory;
 
class Index extends \Magento\Framework\App\Action\Action
{
    protected $_resultPageFactory;
    protected $_modelHelloFactory;
 
    public function __construct(
      Context $context, 
      \Magento\Framework\View\Result\PageFactory $resultPageFactory,
      HelloFactory $modelHelloFactory)
    {
        $this->_resultPageFactory = $resultPageFactory;
        parent::__construct($context);
    }
 
    public function execute()
    { 

        $resultModelPage = $this->_modelHelloFactory->create();
        $collection = $resultModelPage->getCollection(); //Get Collection of module data
        var_dump($collection->getData())

        $resultPage = $this->_resultPageFactory->create();
        return $resultPage;
    }
}
{% endhighlight %}

<br /><br />

<h2>Sources</h2>
https://magento.stackexchange.com/questions/73908/how-can-i-get-the-collection-of-a-custom-module-in-magento-2<br />
https://www.mage-world.com/blog/create-a-module-with-custom-database-table-in-magento-2.html<br />
https://magehit.com/blog/way-to-use-model-and-collection-in-magento-2/