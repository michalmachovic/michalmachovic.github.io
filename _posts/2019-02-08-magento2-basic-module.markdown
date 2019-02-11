---
layout: post
date:   2019-02-08 06:13:26 +0200
title:  "Magento2: Basic Frontend Module"
category: php, magento2
tags: [magento2,module,frontend]
---

We will do simple module which will write `Hello world` to frontend.  Copy source code under app/code/MichalMachovic/Helloworld. Activate the module, then you can see Hello world! on `[YOUR_SITE]/helloworld/index/index`.

<h2>Structure</h2>
{% highlight php %}
Block
 |
 --Helloworld.php
Controller
 |
 Index
   |
   --Index.php
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
<h2>registration.php</h2>
{% highlight php %}
<?php

\Magento\Framework\Component\ComponentRegistrar::register(
    \Magento\Framework\Component\ComponentRegistrar::MODULE,
    'MichalMachovic_Helloworld',
    __DIR__
);
{% endhighlight %}



<br /><br />
<h2>etc/module.xml</h2>
{% highlight xml %}
<?xml version="1.0"?>

<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:framework:Module/etc/module.xsd">
    <module name="MichalMachovic_Helloworld" setup_version="1.0.0.1"></module>
</config>
{% endhighlight %}

<br /><br />
<h2>etc/routes.xml</h2>
Lets take url `http://localhost/magento2/sites/m2test/helloworld/index/index`. In Magento 2, each individual module can claim a `front name`. The `front name` is the `first segment of the URL` — in our case that’s `helloworld`. When a module `claims` a front name, that is its way of saying: <br /><br />
 `Hello Magento systems code — if you see any URLs that start with /helloworld, I have controllers for them`
 <br /><br />
All frontend routes are going under
{% highlight xml %} 
<router id="standard"> 
 {% endhighlight %}

So following piece of code means: If there is url that starts with `helloworld`, module `MichalMachovic_Helloworld` will process it.

{% highlight xml %}
//etc/frontend/routes.xml
<?xml version="1.0"?>
 
<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:framework:App/etc/routes.xsd">
    <router id="standard">
        <route id="helloworld" frontName="helloworld">
            <module name="MichalMachovic_Helloworld" />
        </route>
    </router>
</config>
{% endhighlight %}


<br /><br />
<h2>Controller/Index/Index.php</h2>
Magento 2 uses a traditional PHP `transform the URL into a controller class name` approach to controller naming. Let’s take another look at our URL
<br /><br />
`/helloworld/index/index`
<br /><br />

To come up with a controller class name, Magento 2 will look at the `second` and `third` URL segments (`index`, and `index` above, respectively). That is <br /><br />

1. The class name starts with MichalMachovic\Helloworld since we configured `MichalMachovic_Helloworld` to claim the `helloworld` front name. <br />
2. Then we append Controller, because we’re defining a controller (`MichalMachovic\HelloWorld\Controller`) <br />
3. Then Magento appends the second URL segment (`index`) with an upper casing (MichalMachovic\HelloWorld\Controller\Index) <br />
4. Then Magento appends the third URL segment (`index`) with an upper casing (MichalMachovic\HelloWorld\Controller\Index\Index)
<br /><br />
That gives us a final controller name of ichalMachovic\Helloworld\Controller\Index\Index.php. Each frontend controller extends `Magento\Framework\App\Action\Action`.
<br /><br />
In Magento 2, if you want a controller to render an HTML page, you need to have the controller’s execute method return a `page` object.

{% highlight php %}
<?php
 
namespace MichalMachovic\Helloworld\Controller\Index;
use Magento\Framework\App\Action\Context;
 
class Index extends \Magento\Framework\App\Action\Action
{
    protected $_resultPageFactory;
 
    public function __construct(Context $context, \Magento\Framework\View\Result\PageFactory $resultPageFactory)
    {
        $this->_resultPageFactory = $resultPageFactory;
        parent::__construct($context);
    }
 
    public function execute()
    {
        $resultPage = $this->_resultPageFactory->create();
        return $resultPage;
    }
}
{% endhighlight %}


<br /><br />
<h2>Block/Helloworld.php</h2>
{% highlight php %}
<?php
namespace MichalMachovic\Helloworld\Block;
 
class Helloworld extends \Magento\Framework\View\Element\Template
{
    public function getHelloWorldTxt()
    {
        return 'Hello world!';
    }

    protected function _prepareLayout()
    {
      $this->setMessage('Hello');  //we can get this then in template
      $this->setName($this->getRequest()->getParam('name'));  //get parameter name
    }
}
{% endhighlight %}


<br /><br />
<h2>view/frontend/layout/helloworld_index_index.xml</h2>
If we create file `helloworld_index_index.xml`, we are telling Magento: If the handle helloworld_index_index is issue, use the layout instructions in this file. So something like: Magento, please create a block object using the class `MichalMachovic\Helloworld\Block\Helloworld`. This block should render with the template in the helloworld.phtml file, and let’s give the block a globally unique name of `helloworld`.  A block’s name should be a globally unique string, and can be used by other code to get a reference to our block object.
{% highlight xml %}
<page xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="../../../../../../../lib/internal/Magento/Framework/View/Layout/etc/page_configuration.xsd" layout="1column">
    <body>
        <referenceContainer name="content">
            <block class="MichalMachovic\Helloworld\Block\Helloworld" name="helloworld" template="helloworld.phtml" />
        </referenceContainer>
    </body>
</page>
{% endhighlight %}


<br /><br />
<h2>view/frontend/templates/helloworld.phtml</h2>
{% highlight php %}
<h1><?php echo $this->getHelloWorldTxt(); ?></h1>


<?php echo $this->escapeHtml($this->getMessage()); ?>
<?php echo $this->escapeHtml($this->getName()); ?>


{% endhighlight %}



