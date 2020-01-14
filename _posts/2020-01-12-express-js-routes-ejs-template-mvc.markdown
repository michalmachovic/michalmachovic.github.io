---
layout: post
date:   2020-01-12 09:13:26 +0200
title:  "Express.js - Routes and Router with EJS templating engine and MVC"
category: nodejs
tags: [javascript, nodejs, express, ejs]
---

We will add controllers and use MVC. You can have one to one mapping between routes and controllers but you can split it also differently. <br />
We will create controller for `admin` and controller for `frontend`. We will store products in filesystem.

<br />
<h2>Model</h2>
- responsible for representing your data<br />
- responsible for managing your data (saving, fetching)<br />
- doesnt matter if you manage data in memory, files, db<br />
- contains data-related logic<br />
<br /><br />

<h2>View</h2>
- what the user sees<br />
- shouldnt contain too much logic<br />
<br /><br />

<h2>Controller</h2>
- connects `Model` and `View`<br />
- should only make sure that the two can communicate (in both directions)<br />
<br /><br />

<h2>Routes - using Router and Path - Serving HTML pages</h2>
{% highlight javascript %}
-routes
    |
    |---admin.js
    |---shop.js

-controllers
    |
    |---products.js

-models
    |
    |---product.js

-views
    |
    |---includes
    |       |
    |       |---head.ejs
    |       |---navigation.ejs
    |
    |---shop.ejs
    |---add-product.ejs

-public
    |
    |---css
        |
        |---main.css
-app.js
{% endhighlight %}


<br /><br />
<h3>models/product.js</h3>
{% highlight javascript %}
//const products = []; //old approach saving products to array
const fs = require('fs');
const path = require('path');

const p = path.join(path.dirname(process.mainModule.filename), 'data', 'products.json');

const getProductsFromFile = (cb) => {
    fs.readFile(p, (err, fileContent) => {
        if (err) {
            return cb([]);
        }
        cb(JSON.parse(fileContent));
    })
}

module.exports = class Product {
    constructor(title) {
        this.title = title;
    }

    save() {
        //products.push(this);
        getProductsFromFile(products => {
            products.push(this);
            fs.writeFile(p, JSON.stringify(products), (err) => {
                console.log(err);
            });
        });
    }

    //static means that it will be not called on concrete instance of product model, but will return all products
    //cb = callback function
    static fetchAll(cb) {
        getProductsFromFile(cb);
    }
}
{% endhighlight %}




<br /><br />
<h3>controllers/products.js</h3>
{% highlight javascript %}
const Product = require('../models/product' );

exports.getAddProduct = (req, res, next) => {
     res.render('add-product', {
        pageTitle: 'Add product',
        path: '/admin/add-product',
        activeAddProduct: true
    });
}

exports.postAddProduct = (req, res, next) => {
    const product = new Product(req.body.title);
    product.save();
    res.redirect('/');
}

export.getProducts =  (req, res, next) => {
    Product.fetchAll(products => {
        res.render('shop', {
            prods: products,
            pageTitle: 'shop',
            path: '/',
            hasProducts: products.length > 0,
            activeShop: true
        });
    });
}
{% endhighlight %}
<br /><br />


<h3>routes/admin.js</h3>
{% highlight javascript %}
const path = require('path');
const express = require('express');
const router = express.Router()
const productsController = require('../controllers/products');

router.get('/add-product', productsController.getAddProduct);

//this will trigger for only POST requests
router.post('/add-product', productsController.postAddProduct );

module.exports = router;

{% endhighlight %}

<br /><br />

<h3>routes/shop.js</h3>
{% highlight javascript %}
const path = require('path');
const express = require('express');
const productsController = require('../controllers/products');

router.get('/', productsController.getProducts);

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

const adminRoutes = require('./routes/admin');
const shopRoutes = require('./routes/shop');

app.use(bodyParser.urlencoded({exteneded: false}));

//add this to make public folder available to serve static files, like css
app.use(express.static(path.joing(__dirname, 'public')));

app.use('/admin', adminRoutes);
app.use(shopRoutes);

//404 error page, when i make request to path which doesnt exists
app.use((req, res, next) => {
    res.status(404).render('404', { pageTitle: 'Page Not Found' });
});

app.listen(3000);
{% endhighlight %}
