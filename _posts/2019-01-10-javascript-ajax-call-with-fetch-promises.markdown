---
layout: post
title:  "Javascript: Ajax call with fetch and promise"
date:   2019-01-10 06:13:26 +0200
category: javascript
tags: [javascript,es6,es2015,es8,es2017,promises,fetch]
---

This is just simple example how to fetch data from Metaweather `API`. We are using `crossorigin.me` here, so we avoid `CORS`. 

{% highlight javascript %}
function getWeather(woeid) {
  fetch(`https://crossorigin.me/https://www.metaweather.com/api/location/${woeid}`)
  .then(result => {
    return result.json();
  })
  .then(data => {
    console.log(data);
  })
}
getWeather(44418);
{% endhighlight %}
<br /><br />
<br /><br />

Now we do `async` version (this will work only in ES8/ES2017) which will return data. Async function always return promise. 
<br /><br />

{% highlight javascript %}
async function getWeatherAW(woeid) {
  try {
    const result = await fetch(`https://crossorigin.me/https://www.metaweather.com/api/location/${woeid}`);
    const data = await result.json();
    
    return data;
  }

  catch(error) {
    console.log(error);
  }
}
let dataLondon;

getWeatherAW(44418).then(data => {
  dataLondon = data;
  console.log(dataLondon);
});

{% endhighlight %}