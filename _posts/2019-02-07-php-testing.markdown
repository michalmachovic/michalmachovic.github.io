---
layout: post
date:   2019-02-07 06:13:26 +0200
title:  "PHP: Testing"
category: php
tags: [php,testing]
---


<h2>Unit tests</h2>
We are using these for testing class methods. These tests are assigning different inputs to methods and testing if they get correct output. Example is validator of phone numbers, which we can use in different apps. Unit test can test for example 20 different phone nubmers if they are valid or no.
<br /><br />

<h2>Acceptance/blackbox tests</h2>
Usually test some functionality, for example for article writing we need to test different use cases (write article, approve article, deny article, publish article...) These test usually use Selenium framework, which can simulate real use by clicking in browser.
<br /><br />

<h2>Integration tests</h2>
If we have complex app which is using different services, integration tests are testing if everything fit together.
<br /><br />

<h2>System tests</h2>
Used for example in live environment, where app must handle thousands of users.
<br /><br />


<h2>Unit tests - Example</h2>
We will create simple calculator and we will use `Codeception` framework, which includes `PHPUnit`, acceptance wrapper for `Selenium` and other frameworks.
<br />
In your project folder:<br />
1. Download codecept.phar from `http://codeception.com/quickstart`
<br />
2. Install codecept framework `composer require "codeception/codeception" --dev`
<br />
3. Create following php file Kalkulacka.php
<br /><br />


{% highlight php %}
//Kalkulacka.php
<?php

/**
 * Reprezentuje jednoduchou kalkulačku
 */
class Kalkulacka
{

    /**
     * Sečte 2 čísla
     * @param int|float $a První číslo
     * @param int|float $b Druhé číslo
     * @return int|float Součet 2 čísel
     */
    public function secti($a, $b)
    {
        return $a + $b;
    }

    /**
     * Odečte 2 čísla
     * @param int|float $a První číslo
     * @param int|float $b Druhé číslo
     * @return int|float Rozdíl 2 čísel
     */
    public function odecti($a, $b)
    {
        return $a - $b;
    }

    /**
     * Vynásobí 2 čísla
     * @param int|float $a První číslo
     * @param int|float $b Druhé číslo
     * @return int|float Součin 2 čísel
     */
    public function vynasob($a, $b)
    {
        return $a * $b;
    }

    /**
     * Vydělí 2 čísla
     * @param int|float $a První číslo
     * @param int|float $b Druhé číslo
     * @return int|float Podíl 2 čísel
     */
    public function vydel($a, $b)
    {
        if ($b == 0)
            throw new \InvalidArgumentException("Nelze dělit nulou!");
        return $a / $b;
    }

}    
{% endhighlight %}

<br /><br />
Now run `php codecept.phar bootstrap`. That should generate `tests` folder for you. Under `tests\unit` create tile `_bootstrap.php` which will load all classes which will be tested.
<Br /><br />
{% highlight php %}
//_bootstrap.php
<?php

function autoloader($trida)
{
    if (!file_exists(__DIR__ . '/../../' . $trida . '.php'))
        return false;
    require(__DIR__ . '/../../' . $trida . '.php');
}

spl_autoload_register('autoloader');
{% endhighlight %}
<br /><br />

Also you need to add `settings: bootstrap: _bootstrap.php` to `codeception.yml`.

{% highlight php %}
paths:
    tests: tests
    output: tests/_output
    data: tests/_data
    support: tests/_support
    envs: tests/_envs
actor_suffix: Tester
settings:
    bootstrap: _bootstrap.php
extensions:
    enabled:
        - Codeception\Extension\RunFailed
{% endhighlight %}

<br /><br />

So your project folder should look like this:
<br />

{% highlight php %}
-tests
-vendor
codeception.tml
codecept.phar
composer.json
composer.lock
Kalkulacka.php
{% endhighlight %}

<Br /><br />
To generate unit tests run `php codecept.phar generate:test unit Kalkulacka`. This will create following file under `tests/unit`:
{% highlight php %}
<?php

class KalkulackaTest extends \Codeception\Test\Unit
{
    /**
     * @var \UnitTester
     */
    protected $tester;

    protected function _before()
    {
    }

    protected function _after()
    {
    }

    // tests
    public function testSomeFeature()
    {

    }
}
{% endhighlight %}

<br /><Br />
We need to update this file for testing:

{% highlight php %}
class KalkulackaTest extends \Codeception\Test\Unit
{
    /**
     * @var \UnitTester
     */
    protected $tester;

    private $kalkulacka;

    protected function _before()
    {
        $this->kalkulacka = new Kalkulacka();
    }

    protected function _after()
    {
    }


    public function testScitani()
    {
        $this->assertEquals(2, $this->kalkulacka->secti(1, 1));
        $this->assertEquals(1.42, $this->kalkulacka->secti(3.14, -1.72), '', 0.001);
        $this->assertEquals(2/3, $this->kalkulacka->secti(1/3, 1/3), '', 0.001);
    }

    public function testOdcitani()
    {
        $this->assertEquals(0, $this->kalkulacka->odecti(1, 1));
        $this->assertEquals(4.86, $this->kalkulacka->odecti(3.14, -1.72), '', 0.001);
        $this->assertEquals(2/3, $this->kalkulacka->odecti(1/3, -1/3), '', 0.001);
    }

    public function testNasobeni()
    {
        $this->assertEquals(2, $this->kalkulacka->vynasob(1, 2));
        $this->assertEquals(-5.4008, $this->kalkulacka->vynasob(3.14, -1.72), '', 0.001);
        $this->assertEquals(0.111, $this->kalkulacka->vynasob(1/3, 1/3), '', 0.001);
    }

    public function testDeleni()
    {
        $this->assertEquals(2, $this->kalkulacka->vydel(4, 2));
        $this->assertEquals(-1.826, $this->kalkulacka->vydel(3.14, -1.72), '', 0.001);
        $this->assertEquals(1, $this->kalkulacka->vydel(1/3, 1/3));
    }

    /**
    * @expectedException InvalidArgumentException
    */
    public function testDeleniVyjimka()
    {
        $this->kalkulacka->vydel(2, 0);
    }
}
{% endhighlight %}

`assertEquals()` parameters: <br />
1 - expected value <br/>
2 - actual value
3 - error message
4 - result tolerance

<Br /><br />
Using `@expectedException InvalidArgumentException` in `testDeleniVyjimka` is testing it really happens exception with zero. 

<br /><br />
Run test with `php codecept.phar run unit`. You should see something like:

{% highlight php %}
==== Redirecting to Composer-installed version in vendor/codeception ====
Codeception PHP Testing Framework v2.5.3
Powered by PHPUnit 6.5.14 by Sebastian Bergmann and contributors.
Running with seed: 


Unit Tests (6) ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
✔ KalkulackaTest: Some feature (0.00s)
✔ KalkulackaTest: Scitani (0.00s)
✔ KalkulackaTest: Odcitani (0.00s)
✔ KalkulackaTest: Nasobeni (0.00s)
✔ KalkulackaTest: Deleni (0.00s)
✔ KalkulackaTest: Deleni vyjimka (0.00s)
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
{% endhighlight %}


