---
layout: post
date:   2019-10-09 07:13:26 +0200
title:  "Shopware6: Plugin development"
category: php, shopware
tags: [php, shopware]
---

`http://localhost:8000` - shopware <br />
`http://localhost:8000/admin` - admin (admin/shopware) <br />
`http://localhost:8001` - adminer (mysql/app/app) <br />

<br /><br />

Go to folder when you installed Shopware, enter `development` folder. Build and start the containers and access the application containers.
{% highlight php %}
./psh.phar docker:start
./psh.phar docker:ssh
{% endhighlight %}

<br /><br />
<h2>Auto generating plugin</h2>
You can skip first three steps and auto generate plugin with
{% highlight php %}
bin/console plugin:create MichalMachovicTesting
bin/console plugin:refresh
bin/console plugin:install --activate MichalMachovicTesting
bin/console plugin:refresh
{% endhighlight %}

<h3>Creating migration</h3>
{% highlight php %}
bin/console database:create-migration -p MichalMachovicTesting
{% endhighlight %}

<h3>Database</h3>
{% highlight php %}
 ./psh.phar mysql
 {% endhighlight %}
<br /><br /><br />


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

<br /><br />
<h2>Rebuild admin</h2>
 ./psh.phar administration:build


<br /><br />
<h2>Plugin folder structure</h2>
Plugins live under `custom\plugins` folder. Migration folder is generated with `bin/console database:create-migration -p MichalMachovicTesting`.
<Br /><Br />
`Core\Content\Testing` - we want to use our own DB table, so we need to specify `Collection, Definition and Entity`. <br />
`Migration` - here is SQL query, which will create our own table<br />
`Resources` - config files and views
<br /><br />
{% highlight php %}
MichalMachovicTesting
|
|--src
|    |
|    |-Core
|    |    |
|    |     Content
|    |           |
|    |           Testing
|    |                 |
|    |                 |--TestingCollection.php
|    |                 |--TestingDefinition.php
|    |                 |--TestingEntity.php
|    |
|    |--Migration
|    |          |
|    |          --Migration<TIMESTAP>Testing.php
|    |
|    |--Resources
|    |         |
|    |         |--config
|    |         |       |
|    |         |       |--config.xml
|    |         |       |--routes.xml
|    |         |       |--services.xml
|    |         |
|    |         |--views
|    |                |
|    |                |--page
|    |                      |
|    |                      product-detail
|    |                                   |
|    |                                   |--index.html.twig
|    |
|    |--Storefront
|    |           |
|    |           |--Controller
|    |                       |
|    |                       |--A2cController.php
|    |
|    |--Testing.php
|
|--composer.json

{% endhighlight %}

<br /><br />

<h3>MichalMachovicTesting/composer.json</h3>
{% highlight php %}
{
    "name": "michalmachovic/testing",
    "description": "Testing",
    "version": "v1.0.0",
    "license": "MIT",
    "authors": [
        {
            "name": "Michal Machovic"
        }
    ],
    "type": "shopware-platform-plugin",
    "autoload": {
        "psr-4": {
            "MichalMachovic\\Testing\\": "src/"
        }
    },
    "extra": {
        "shopware-plugin-class": "MichalMachovic\\Testing\\Testing",
        "copyright": "(c) by Michal Machovic",
        "label": {
            "de-DE": "Testing",
            "en-GB": "Testing"
        }
    }
}

{% endhighlight %}
<br /><br />



<h3>MichalMachovicTesting/src/Testing.php</h3>
{% highlight php %}
<?php declare(strict_types=1);

namespace MichalMachovic\Testing;

use Shopware\Core\Framework\Plugin;
use Shopware\Core\Framework\Plugin\Context\InstallContext;
use Shopware\Core\Framework\Plugin\Context\UninstallContext;
use Shopware\Core\Framework\CustomField\CustomFieldTypes;
use Shopware\Core\Framework\Uuid\Uuid;

class Testing extends Plugin
{
    //what will happen if plugin is installed    
    public function install(InstallContext $context): void
    {
        //this example will set two new custom fields on product
        $customFieldSetRepository = $this->container->get('custom_field_set.repository');
        $id = Uuid::randomHex();
        $attributeSet = [
            'id' => $id,
            'name' => 'MichalMachovic Testing',
            'config' => ['description' => 'MichalMachovic Testing'],
            'customFields' => [
                [
                    'id' => Uuid::randomHex(),
                    'name' => 'testing_defaultAppUrl',
                    'type' => CustomFieldTypes::TEXT,
                    'config' => [
                        'label' => [
                            'de-DE' => 'Default App Url',
                            'en-GB' => 'Default App Url'
                        ],
                        'componentName' => "sw-field",
                        'customFieldType' => "text",
                        'customFieldPosition' => 1
                    ]
                ],
                [
                    'id' => Uuid::randomHex(),
                    'name' => 'testing_mobileAppUrl',
                    'type' => CustomFieldTypes::TEXT,
                    'config' => [
                        'label' => [
                            'de-DE' => 'Mobile App Url',
                            'en-GB' => 'Mobile App Url'
                        ],
                        'componentName' => "sw-field",
                        'customFieldType' => "text",
                        'customFieldPosition' => 2
                    ]
                ],
            ],
            'relations' => [
                [
                    'entityName' => 'product',
                ]
            ],
        ];
        $result = $customFieldSetRepository->create([$attributeSet], $context->getContext());
    }

    public function uninstall(UninstallContext $context): void
    {
    }
{% endhighlight %}
<br /><br />



<h3>MichalMachovicTesting/src/Migration/Migration<TIMESTAMP>.php</h3>
{% highlight php %}
<?php declare(strict_types=1);

namespace MichalMachovic\Testing\Migration;

use Doctrine\DBAL\Connection;
use Shopware\Core\Framework\Migration\InheritanceUpdaterTrait;
use Shopware\Core\Framework\Migration\MigrationStep;

class Migration1554708925Testing extends MigrationStep
{
    use InheritanceUpdaterTrait;

    public function getCreationTimestamp(): int
    {
        return 1554708925;
    }

    public function update(Connection $connection): void
    {
      $connection->executeQuery('
      CREATE TABLE IF NOT EXISTS `michalmachovic_testing` (
        `id` BINARY(16) NOT NULL,
        `buy_request` LONGTEXT NOT NULL,
        `created_at` DATETIME(3) NOT NULL,
        `updated_at` DATETIME(3) NULL,
        PRIMARY KEY (`id`)
      ) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;
  ');
  //$this->updateInheritance($connection, 'product', 'personaliseit');
    }

    public function updateDestructive(Connection $connection): void
    {
    }
}

{% endhighlight %}



<br /><br />
<h3>MichalMachovicTesting/src/Core/Content/Testing/TestingEntity.php</h3>
{% highlight php %}
<?php declare(strict_types=1);

namespace MichalMachovic\Testing\Core\Content\Testing;

use Shopware\Core\Framework\DataAbstractionLayer\Entity;
use Shopware\Core\Framework\DataAbstractionLayer\EntityIdTrait;

class TestingEntity extends Entity
{
    use EntityIdTrait;

    /**
     * @var string
     */
    protected $buyRequest;

  
    public function getBuyRequest(): string
    {
        return $this->defaultAppUrl;
    }

    public function setBuyRequest(string $buyRequest): void
    {
        $this->buyRequest = $buyRequest;
    }
}

{% endhighlight %}

<br /><br />
<h3>MichalMachovicTesting/src/Core/Content/Testing/TestingDefinition.php</h3>
{% highlight php %}
<?php declare(strict_types=1);

namespace MichalMachovic\Testing\Core\Content\Testing;

use Shopware\Core\Framework\DataAbstractionLayer\EntityDefinition;
use Shopware\Core\Framework\DataAbstractionLayer\Field\Flag\PrimaryKey;
use Shopware\Core\Framework\DataAbstractionLayer\Field\Flag\Required;
use Shopware\Core\Framework\DataAbstractionLayer\Field\FloatField;
use Shopware\Core\Framework\DataAbstractionLayer\Field\IdField;
use Shopware\Core\Framework\DataAbstractionLayer\Field\LongTextField;
use Shopware\Core\Framework\DataAbstractionLayer\FieldCollection;

class TestingDefinition extends EntityDefinition
{
    public const ENTITY_NAME = 'michalmachovic_testing';

    public function getEntityName(): string
    {
        return self::ENTITY_NAME;
    }

    public function getCollectionClass(): string
    {
        return TestingCollection::class;
    }

    public function getEntityClass():string
    {
        return TestingEntity::class;
    }

    protected function defineFields(): FieldCollection
    {
        return new FieldCollection([
            (new IdField('id', 'id'))->addFlags(new Required(), new PrimaryKey()),
            (new LongTextField('buy_request', 'buyRequest'))->addFlags(new Required())
        ]);
    }
}
{% endhighlight %}


<br /><br />
<h3>MichalMachovicTesting/src/Core/Content/Testing/TestingCollection.php</h3>
{% highlight php %}
<?php declare(strict_types=1);

namespace MichalMachovic\Testing\Core\Content\Testing;

use Shopware\Core\Framework\DataAbstractionLayer\EntityCollection;

/**
 * @method void              add(TestingEntity $entity)
 * @method void              set(string $key, TestingEntity $entity)
 * @method TestingEntity[]    getIterator()
 * @method TestingEntity[]    getElements()
 * @method TestingEntity|null get(string $key)
 * @method TestingEntity|null first()
 * @method TestingEntity|null last()
 */
class TestingCollection extends EntityCollection
{
    protected function getExpectedClass(): string
    {
        return TestingEntity::class;
    }
}

{% endhighlight %}



<br /><br />
<h3>MichalMachovicTesting/src/Storefront/Controller/A2cController.php</h3>
{% highlight php %}
<?php declare(strict_types=1);

namespace MichalMachovic\Testing\Storefront\Controller;

use Shopware\Core\Checkout\Cart\SalesChannel\CartService;
use Shopware\Core\Checkout\Order\SalesChannel\OrderService;
use Shopware\Storefront\Page\GenericPageLoader;
use Shopware\Core\System\SalesChannel\SalesChannelContext;
use Shopware\Core\Framework\Routing\Annotation\RouteScope;
use Shopware\Storefront\Controller\StorefrontController;
use Symfony\Component\Routing\Annotation\Route;
use Shopware\Core\Content\Product\Cart\ProductLineItemFactory;
use Symfony\Component\HttpFoundation\Request;
use Symfony\Component\HttpFoundation\Response;
use Symfony\Component\HttpFoundation\JsonResponse;
use Shopware\Core\Framework\DataAbstractionLayer\EntityRepositoryInterface;
use Shopware\Core\Framework\Uuid\Uuid;
use Shopware\Core\Framework\Context;
use Shopware\Core\Framework\DataAbstractionLayer\Search\Criteria;
use Shopware\Core\Framework\DataAbstractionLayer\Search\Filter\EqualsFilter;

/**
 * @RouteScope(scopes={"storefront"})
 */
class A2cController extends StorefrontController
{   

    /**
     *  @var EntityRepositoryInterface
     */
    private $testingRepository;
  
    private $cartService;
    private $orderService;
    private $genericPageLoader;
   
    public function __construct(
        CartService $cartService, 
        OrderService $orderService, 
        GenericPageLoader $genericPageLoader, 
        EntityRepositoryInterface $personaliseItRepository)
    {
        $this->cartService = $cartService;
        $this->orderService = $orderService;
        $this->genericPageLoader = $genericPageLoader;
        $this->personaliseItRepository = $personaliseItRepository;
    }



    /**
     * @Route("/testing/a2c/{id}", name="frontend.testing.a2c", options={"seo"="false"}, methods={"POST"})
     */
    public function a2c(SalesChannelContext $salesChannelContext, Context $context, Request $request, string $id)
    {   

        //data are coming from form with post 
        $data = json_decode($request->get('data'));
        
        //update item in cart
        if ($id) {
            $cart = $this->cartService->getCart($salesChannelContext->getToken(), $salesChannelContext);
            $product = (new ProductLineItemFactory())->create($id, ['quantity' => 1]);
            $this->cartService->add($cart, $product, $salesChannelContext);

            $lastItem = $cart->getLineItems()->last();
            $lastItem->setPayloadValue('testing', json_encode($data));
            $this->cartService->setCart($cart, $salesChannelContext);
        }
        

        //insert data into own DB table
        $insert = [
            'id' => Uuid::randomHex(),
            'buyRequest' => json_encode($data)
        ];
        $this->testingRepository->create(array($insert), $context);
                          

        return new JsonResponse(array(
            'redirect' => 'http://localhost:8000/checkout/cart',
        ));
    }

}
{% endhighlight %}


<br /><br />
<h3>MichalMachovicTesting/src/Resources/config/config.xml</h3>
{% highlight php %}
<?xml version="1.0" encoding="UTF-8"?>
<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:noNamespaceSchemaLocation="https://raw.githubusercontent.com/shopware/platform/master/src/Core/System/SystemConfig/Schema/config.xsd">

    <card>
        <title>Minimal configuration</title>
        <title lang="de-DE">Minimale Konfiguration</title>
        <input-field>
            <name>example</name>
            <label>Example Label EN</label>
            <label lang="de-DE">Beispiel Label DE</label>
        </input-field>
    </card>
</config>

{% endhighlight %}


<br /><br />
<h3>MichalMachovicTesting/src/Resources/config/routes.xml</h3>
{% highlight php %}
<?xml version="1.0" encoding="UTF-8" ?>
<routes xmlns="http://symfony.com/schema/routing"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:schemaLocation="http://symfony.com/schema/routing
        https://symfony.com/schema/routing/routing-1.0.xsd">

    <import resource="../../**/Storefront/Controller/*Controller.php" type="annotation" />
</routes>
{% endhighlight %}


<br /><br />
<h3>MichalMachovicTesting/src/Resources/config/services.xml</h3>
{% highlight php %}
<?xml version="1.0" ?>

<container xmlns="http://symfony.com/schema/dic/services"
           xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xsi:schemaLocation="http://symfony.com/schema/dic/services http://symfony.com/schema/dic/services/services-1.0.xsd">
           
    <services>
        <service id="MichalMachovic\Testing\Core\Content\Testing\TestingItDefinition">
            <tag name="shopware.entity.definition" entity="michalmachovic_testing" />
        </service>

        <service id="MichalMachovic\Testing\Storefront\Controller\A2cController" public="true">
            <argument type="service" id="Shopware\Core\Checkout\Cart\SalesChannel\CartService"/>
            <argument type="service" id="Shopware\Core\Checkout\Order\SalesChannel\OrderService"/>
            <argument type="service" id="Shopware\Storefront\Page\GenericPageLoader"/>
            <argument type="service" id="michalmachovic_testing.repository" /> 
              
            <call method="setContainer">
                <argument type="service" id="service_container"/>
            </call>
        </service>
    </services>
</container>
{% endhighlight %}


<br /><br />
<h3>MichalMachovicTesting/src/Resources/views/page/product-detail/index.html.twig</h3>
{% highlight php %}
{% raw %}
{% sw_extends '@Storefront/base.html.twig' %}

{% block base_head %}
    {% sw_include '@Storefront/page/product-detail/meta.html.twig' %}
{% endblock %}

{% block base_content %}
{% endraw %}
.....
<script>
let a2c = 'http://localhost:8000/testing/a2c/{{page.product.id}}';

var xhr = new XMLHttpRequest();
xhr.widthCredentials = true;
                                                
xhr.onreadystatechange = function() {
    if(xhr.readyState == 4) {
        if(xhr.status == 200) {
            var data = JSON.parse(xhr.responseText);
            window.top.location = data.redirect;
        } else {
            try {
                var data = JSON.parse(xhr.responseText);
                alert(data.error.message);
            } catch(e) {
                alert("An unknown error ocurred");
            }
        }
    }
};

xhr.open("POST", a2c);
                            
var fd = new FormData();
fd.append('data', JSON.stringify(body));
xhr.send(fd);
break;
</script>
.....
{% endhighlight %}


