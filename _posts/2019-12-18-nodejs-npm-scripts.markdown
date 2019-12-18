---
layout: post
date:   2019-12-18 09:13:26 +0200
title:  "NodeJS: NPM Scripts and Nodemon"
category: nodejs
tags: [javascript, nodejs, npm, nodemon]
---
This process will create file `package.json` file under your project, where you can define scripts. <Br />
Run `npm init` under your project. Following file will be generated:

{% highlight javascript %}
{
  "name": "server",
  "version": "1.0.0",
  "description": "",
  "main": "app.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC"
}

{% endhighlight %}
<br /><br />

Now you can update this file.

{% highlight javascript %}
  "scripts": {
    "start": "node app.js"
  },
 {% endhighlight %}

You can then run your app with just `npm start`.
But if you make some change to the app, you must still quit the app and restart it.
To automate this process we can use `nodemon`. `--save-dev` means that this package is used during development only.


{% highlight javascript %}
npm install nodemon --save-dev
{% endhighlight %}
<br /><br />

After installing `nodemon` you can change `package.json`:

{% highlight javascript %}
  "scripts": {
    "start": "nodemon app.js"
  },
 {% endhighlight %}

<br /><br />

Then run your app with `npm start` and it will now look for any change you make and restart app automatically.
