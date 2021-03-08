---
layout: post
date:   2020-03-08 13:13:26 +0200
title:  "Selenium and Javascript"
category: javascript
tags: [selenium, javascript]
---
`Selenium` is automation tool for testing web applications. 
We will create test with Javascript, Firefox, Node.js. This test will open Firefox browser and search for string `Selenium`.
<br /><br />

<h2>Installation</h2>
{% highlight javascript %}
//Install Node.js and Npm

//Install Gecko Driver for Firefox
wget https://github.com/mozilla/geckodriver/releases/download/v0.29.0/geckodriver-v0.29.0-linux64.tar.gz
tar -xvzf geckodriver*
chmod +x geckodriver
sudo mv geckodriver /usr/local/bin/
{% endhighlight %}

<br /><br />
Create new folder and inside that `index.js`.
<br />

{% highlight javascript %}
npm init
npm install selenium-webdriver@3.6.0  //latest version didnt work for me
{% endhighlight %}


<h2>index.js</h2>
{% highlight javascript %}
const { Builder, By, Key, util } = require('selenium-webdriver');

async function example() {
  let driver = await new Builder().forBrowser("firefox").build();
  await driver.get("http://google.com");
  await driver.findElement(By.name("q")).sendKeys("Selenium", Key.RETURN);
}

example();
{% endhighlight %}

<br /><br />
Run it with:
{% highlight javascript %}
node index.js
{% endhighlight %}