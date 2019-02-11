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
}
{% endhighlight %}


<br /><br />
<h2>view/frontend/layout/helloworld_index_index_.xml</h2>
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
{% endhighlight %}


