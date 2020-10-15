---
layout: post
date:   2020-09-17 13:13:26 +0200
title:  "Node.js, Express, Typescript, MongoDB"
category: nodejs, typescript, express, mongo
tags: [nodejs, typescript, express, mongo]
---


<br /><br />

{% highlight javascript %}
tsc --init
npm init
npm install mongoose
npm install --save express
npm install --save body-parser
npm install --save-dev ts-node-dev 
npm install --save-dev typescript

npm install --save-dev @types/node
npm install --save-dev @types/express
npm install --save-dev @types/body-parser
npm install --save-dev @types/mongoose
{% endhighlight %}
<br /><br />

<h3>tsconfig.json</h3>
{% highlight javascript %}
...
"compilerOptions": {
    "target": "es6",
    "module": "commonjs",
    "moduleResolution": "node",  //allows us to use new import of modules
    ...
    "outDir": "./dist",   //here will be generated javascript files
    "rootDir": "./src",   //here will be our typescript files
    ...
}
...
{% endhighlight %}
<br /><br />


<h3>package.json</h3>
{% highlight javascript %}
...
"scripts": {
    "start": "ts-node-dev src/index.ts",   //here will be generated javascript files
    ...
}
...
{% endhighlight %}
Now you can use `npm start` and code will be automatically converted from typescript to javascript during development.