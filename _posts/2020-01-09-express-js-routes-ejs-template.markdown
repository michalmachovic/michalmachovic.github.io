---
layout: post
date:   2020-01-09 09:13:26 +0200
title:  "Express.js - Routes and Router with EJS templating engine"
category: nodejs
tags: [javascript, nodejs, express, ejs]
---

EJS is a simple templating language that lets you generate HTML markup with plain JavaScript.
<br /><br />
<h2>Examples</h2>
{% highlight javascript %}
<%= pageTitle %>  //render pageTitle variable
<%- include('includes/head.ejs') //render html/ejs file %>
<% if (prods.length > 0) { %> //for loop
{% endhighlight %}

<h2>Routes - using Router and Path - Serving HTML pages</h2>
{% highlight javascript %}
-routes
    |
    |---admin.js
    |---shop.js
-views
    |
    |---includes
    |       |
    |       |---head.ejs
    |       |---navigation.ejs
    |
    |---shop.ejs
    |---add-product.ejs
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

const products = [];

router.get('/add-product', (req, res, next) => {
    res.render('add-product', {
        pageTitle: 'Add product',
        path: '/admin/add-product',
        activeAddProduct: true
    });
});

//this will trigger for only POST requests
router.post('/add-product', (req, res, next) => {
    console.log(req.body);  //req.body is available because of using bodyParser
    products.push({ title: req.body.title });
    res.redirect('/');
});

exports.routes = router;
exports.products = products;
{% endhighlight %}

<br /><br />

<h3>routes/shop.js</h3>
{% highlight javascript %}
const path = require('path');
const express = require('express');
const router = express.Router();
const adminData = require('./admin');

router.get('/', (req, res, next) => {
    const products = adminData.products;
    res.render('shop', {
        prods: products,
        pageTitle: 'shop',
        path: '/',
        hasProducts: products.length > 0,
        activeShop: true
    });
});
module.exports = router;
{% endhighlight %}

<br /><br />

<h3>views/includes/head.ejs</h3>
{% highlight javascript %}
<!DOCTYPE html>
<html>
<head>
    <!-- we are using express.static in app.js to serve static css file from public dir -->
    <link rel="stylesheet" href="/css/main.css">
{% endhighlight %}
<br /><br />


<h3>views/includes/navigation.ejs</h3>
{% highlight javascript %}
<header>
    <nav>
        <ul>
            <li><a class="<%= path === '/' ? 'active' : '' %>" href="/">Shop</a></li>
            <li><a class="<%= path === '/admin/add-product' ? 'active' : '' %>" href="/admin/add-product">Add product</a></li>
        </ul>
    </nav>
</header>
{% endhighlight %}
<br /><br />

<h3>views/add-product.ejs</h3>
{% highlight javascript %}
<%- include('includes/head.ejs') %>
</head>
</head>
<body>
    <%- include('includes/navigation.ejs') %>

    <main>
        <form action="/admin/add-product" method="post">
            <input type="text" name="title">
            <button type="submit">Add product</button>
        </form>
    </main>
</body>
</html>
{% endhighlight %}

<br /><br />
<h3>views/404.ejs</h3>
{% highlight javascript %}
<%- include('includes/head.ejs') %>
</head>
<body>
    <%- include('includes/navigation.ejs') %>
    <h1>Page not found</h1>
</body>
</html>
{% endhighlight %}


<br /><br />
<h3>views/shop.ejs</h3>
{% highlight javascript %}
<%- include('includes/head.ejs') %>
</head>
</head>
<body>
    <%- include('includes/navigation.ejs') %>
    <main>
        <% if (prods.length > 0) { %>
            //render products
            <% for (let product of prods) { %>
                 <div>
                    <%= product.title %>
                 </div>
            <% } %>
        <% } else { %>
            <h1>No products found</h1>
        <% }  %>
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

//set template engine to EJS
app.set('view engine', 'ejs');
app.set('views', 'views');

const adminData = require('./routes/admin');
const shopRoutes = require('./routes/shop');

app.use(bodyParser.urlencoded({exteneded: false}));

//add this to make public folder available to serve static files, like css
app.use(express.static(path.joing(__dirname, 'public')));

app.use('/admin', adminData.routes);
app.use(shopRoutes);

//404 error page, when i make request to path which doesnt exists
app.use((req, res, next) => {
    res.status(404).render('404', { pageTitle: 'Page Not Found' });
});

app.listen(3000);
{% endhighlight %}
