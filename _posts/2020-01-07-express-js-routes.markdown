---
layout: post
date:   2020-01-07 09:13:26 +0200
title:  "Express.js - Routes and Router"
category: nodejs
tags: [javascript, nodejs, express]
---

<h2>Routes</h2>
<h3>app.js</h3>
{% highlight javascript %}
const express = require('express');
const app = express();

app.use('/add-product', (req, res, next) => {
    console.log('in the another middleware');
    res.send('<h1>The Add product page</h1>');
});

app.use('/', (req, res, next) => {
    console.log('in the another middleware');
    res.send('<h1>Hello from Express !</h1>');
});

app.listen(3000);
{% endhighlight %}
<br /><br />


<h2>Routes - example with form</h2>
We need to install `body-parser` for parsing incoming requests.
{% highlight javascript %}
npm install --save body-parser
{% endhighlight %}
<br /><br />

<h3>app.js</h3>
{% highlight javascript %}
const express = require('express');
const bodyParser = require('body-parser');
const app = express();

app.use(bodyParser.urlencoded({exteneded: false}));

app.get('/add-product', (req, res, next) => {
    res.send('<form action="/product" method="POST"><input type="text" name="title"><button type="submit">Add product</button></form>');
});

//this will trigger for only POST requests
app.post('/product', (req, res, next) => {
    console.log(req.body);  //req.body is available because of using bodyParser
    res.redirect('/');
});

app.use('/', (req, res, next) => {
    res.send('<h1>Hello from Express !</h1>');
});

app.listen(3000);
{% endhighlight %}
<br /><br /><br />



<h2>Routes - using Router and Path - Serving HTML pages</h2>
Typically you dont want to have all the code in single file. Using `Router` you can split routes code into separate files.
{% highlight javascript %}
-routes
    |
    |---admin.js
    |---shop.js
-views
    |
    |---shop.html
    |---add-product.html
-util
    |
    |---path.js
-public
    |
    |---css
        |
        |---main.css
-app.js
{% endhighlight %}
<br /><br />

<h3>util/path.js</h3>
{% highlight javascript %}
const path = require('path');
module.exports = path.dirname(process.mainModule.filename);
{% endhighlight %}
<br /><br />

<h3>routes/admin.js</h3>
{% highlight javascript %}
const path = require('path');
const express = require('express');
const router = express.Router()
const rootDir = require('../util/path'); //if we want to use path to dirname above

router.get('/add-product', (req, res, next) => {
    res.sendFile(path.join(__dirname,'../', 'views', 'add-product.html'));
    //__dirname return routes folder, then we need to go up one level, then to views, where is shop.html

    //OR - using rootDir and  util/path.js
    //res.sendFile(path.join(rootDir, 'views', 'add-product.html'));
});

//this will trigger for only POST requests
router.post('/add-product', (req, res, next) => {
    console.log(req.body);  //req.body is available because of using bodyParser
    res.redirect('/');
});

module.exports = router;
{% endhighlight %}

<br /><br />

<h3>routes/shop.js</h3>
{% highlight javascript %}
const path = require('path');
const express = require('express');
const router = express.Router()

router.get('/', (req, res, next) => {
    //res.send('<h1>Hello from Express !</h1>');
    res.sendFile(path.join(__dirname, '../', 'views', 'shop.html'));
    //__dirname return routes folder, then we need to go up one level, then to views, where is shop.html
});
module.exports = router;
{% endhighlight %}

<br /><br />
<h3>views/app-product.html</h3>
{% highlight javascript %}
<!DOCTYPE html>
<html>
<body>
    <header>
        <nav>
            <ul>
                <li><a href="/">Shop</a></li>
                <li><a href="/add-product">Add product</a></li>
            </ul>
        </nav>
    </header>

    <main>
        <form action="/add-product" method="post">
            <input type="text" name="title">
            <button type="submit">Add product</button>
        </form>
    </main>
</body>
</html>
{% endhighlight %}

<br /><br />
<h3>views/404.html</h3>
{% highlight javascript %}
<!DOCTYPE html>
<html>
<body>
    <h1>Page not found</h1>
</body>
</html>
{% endhighlight %}


<br /><br />
<h3>views/shop.html</h3>
{% highlight javascript %}
<!DOCTYPE html>
<html>
<head>
    <!-- we are using express.static in app.js to serve static css file from public dir -->
    <link rel="stylesheet" href="/css/main.css">
</head>
<body>
    <header>
        <nav>
            <ul>
                <li><a href="/">Shop</a></li>
                <li><a href="/add-product">Add product</a></li>
            </ul>
        </nav>
    </header>

    <main>
        <h1>My Products</h1>
        <p>List of all products...</p>
    </main>
</body>
</html>
{% endhighlight %}

<br /><br />
<h3>app.js</h3>
{% highlight javascript %}
const path = require('path');
const express = require('express');
const bodyParser = require('body-parser');
const app = express();
const adminRoutes = require('./routes/admin');
const shopRoutes = require('./routes/shop');

app.use(bodyParser.urlencoded({exteneded: false}));
app.use(express.static(path.joing(__dirname, 'public')));

app.use('/admin', adminRoutes);
app.use(shopRoutes);

//404 error page, when i make request to path which doesnt exists
app.use((req, res, next) => {
    res.status(404).sendFile(path.joing(__dirname,'views','404.html'));
});

app.listen(3000);
{% endhighlight %}
