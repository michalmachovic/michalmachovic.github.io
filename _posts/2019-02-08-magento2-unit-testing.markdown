---
layout: post
date:   2019-02-08 10:13:26 +0200
title:  "Magento2: Unit Testing"
category: php, magento2, testing
tags: [magento2,module,frontend,testing]
---

We will do simple module which will write `Hello world` to frontend. See https://michalmachovic.github.io/php,%20magento2/2019/02/07/magento2-basic-module.html. We added `Test/Unit/SampleTest.php` file, you can see here `assertEquals` testing of `getHelloWorldTxt` method of Helloworld class defined in `Block/Helloworld.php`.

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
Test
 |
 Unit
  |
  --SampleTest.php  
registration.php
{% endhighlight %}


{% highlight php %}
//Test/Unit/SampleTest.php

<?php
 
namespace Gateway3D\Helloworld\Test\Unit;
 
use Gateway3D\Helloworld\Block\Helloworld;
 
class SampleTest extends \PHPUnit\Framework\TestCase
{
    
    protected $sampleClass;
    protected $expectedMessage;
 
    public function setUp()
    {
        $objectManager = new \Magento\Framework\TestFramework\Unit\Helper\ObjectManager($this);
        $this->sampleClass = $objectManager->getObject('Gateway3D\Helloworld\Block\Helloworld');
        $this->expectedMessage = 'Hello world!';
    }
 
    public function testGetMessage()
    {
        $this->assertEquals($this->expectedMessage, $this->sampleClass->getHelloWorldTxt());
    }
 
}
{% endhighlight %}

<br /><br />
There is `PHPUnit` configuration file shipped with Magento 2 – `{ROOT}/dev/tests/unit/phpunit.xml.dist`. Create `phpunit.xml` file in the same location and your test location (you can comment out other tests defined there, if you dont want to test everything):

{% highlight xml %}
//dev/tests/unit/phpunit.xml
<testsuite name="Magento Unit Tests">
        <directory suffix="Test.php">../../../app/code/Gateway3D/Helloworld/Test/Unit</directory>
</testsuite>
{% endhighlight %}

<br /><br />
To run test, do
{% highlight php %}
./vendor/bin/phpunit -c dev/tests/unit/phpunit.xml
{% endhighlight %}