---
layout: post
date:   2020-09-14 13:13:26 +0200
title:  "Node.js and Typescript"
category: nodejs, typescript
tags: [nodejs, typescript]
---


<br /><br />

{% highlight javascript %}
tsc --init
npm init
npm install --save express
npm install --save body-parser

npm install --save-dev @types/node
npm install --save-dev @types/express
npm install --save-dev @types/body-parser
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