---
layout: post
title:  "Javascript: API Calls"
date:   2019-01-14 06:13:26 +0200
category: javascript
tags: [javascript,es6,es2015,es8,es2017,api]
---

<br /><br />
We will use MVC pattern with following structure.
<br />
{% highlight javascript %}
src
 --js
   --models
     Search.js
   --views
     searchView.js
   index.js    
{% endhighlight %}
 <br /><br />

<br /><br />
We will make simple api call to food2fork.com to get some recipes and we are using `webpack` (see previous posts). We need to install axios from npm (`npm install axios --save`) - this is handling http requests better than `fetch` function, which doesnt work correctly in some older browsers. Also we are using `proxy` here, because otherwise it will not work for use from our local server because of `CORS`.
<br />
`models/Search.js`
{% highlight javascript %}
import axios from 'axios';

export default class Search {
  constructor(query) {
    this.query = query;
  }

  async getResults() {
    const proxy = 'https://cors-anywhere.herokuapp.com/';
    const key = '.......'   //register to food2fork to get api key
    try {
      const res = await axios('${proxy}http://food2fork.com/api/search?key=${key}&q=${this.query}');
      this.result = res.data.recipes;
      console.log(this.result);
    } catch (error) {
      alert(error);
    }
  }
}
{% endhighlight %}

<br /><br />

`index.js`
{% highlight javascript %}
import Search from './models/Search';

const search = new Search('pizza');
search.getResults();
{% endhighlight %}
