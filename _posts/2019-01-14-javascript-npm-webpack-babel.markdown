---
layout: post
title:  "Javascript: NPM, Webpack and Babel"
date:   2019-01-14 06:13:26 +0200
category: javascript
tags: [javascript,es6,es2015,es8,es2017,npm,webpack,babel]
---


<br /><br />
<h2>NODE.JS and NPM</h2>
`Node.js` is a javascript runtime build. Its package ecosystem `npm` is the largest ecosystem of open source libraries in the world. So first install `Node.js` and `NPM`.
<br />
Then in your project folder run `npm init`. Answer couple of questions, it will create `package.json` file in your project folder.
{% highlight javascript %}
{
  "name": "forkify",
  "version": "1.0.0",
  "description": "forkify project",
  "main": "index.js",
  "scripts": {
    "dev": "webpack --mode development",
    "build": "webpack --mode production"
  },
  "author": "Jonas Schmedtmann",
  "license": "ISC",
}
{% endhighlight %}
 <br /><br />

Now we install `webpack`, `webpack-cli` and `live-server`. `--save-dev` means that it will be installed as development dependency. It will be listed under `devDependencies` in your `package.json` file (because it is used during development). As an example `jquery` would be installed only using `--save` as it is used during project is live. <br />
`Webpack` is most commonly used asset bundler. It bundles all our javascript files into one file.
<br />
{% highlight javascript %}
  npm install webpack --save-dev
  npm install webpack-cli --save-dev
  sudo npm install live-server --global
{% endhighlight %}
<br />

Here is how our project structure will look. <br/>
{% highlight javascript %}
  dist
    --css
    --img
    --js
    index.html //(here we will include bundle.js)
  src
    --js
      --index.js
      --test.js
    index.html
  package.json
  webpack.config.js      
{% endhighlight %}

<br /><br/>
Create `webpack.config.js` in your project folder.<br/>
{% highlight javascript %}
const path = require('path');

module.exports = {
    entry: './src/js/index.js',
    output: {
        path: path.resolve(__dirname, 'dist/js'),
        filename: 'bundle.js'
    }
}
   
   
{% endhighlight %}


<br /><br/>
We create `index.js` and `test.js` files to test if webpack will bundle then into one file.
<br /><br />
`index.js`<br />
{% highlight javascript %}
//Global app controller
import num from './test';
console.log(`I imported ${num}`);
{% endhighlight %}


<br /><br />
`test.js`<br />
{% highlight javascript %}
console.log(`Imported module`);
export default 23;
{% endhighlight %}


We can run webpack from cli with `npm run dev` (or `npm run build` which will be smaller as it is minimized), which will bundle our js files to one `bundle.js` file. `dev` is defined in `package.json` file. 

<br /><br />
With this setup, we must run `npm run dev` each time we make some change. To automatize this, we will use dev server, so changes will be reloaded automatically after each change.