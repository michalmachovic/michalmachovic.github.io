---
layout: post
date:   2020-05-09 11:13:26 +0200
title:  "Express.js: Mongoose and pagination"
category: express
tags: [express]
---


<h2>routes/shop.js</h2>
{% highlight javascript %}
const path = require('path');
const express = require('express');
const router = express.Router();

const shopController = require('../controllers/shop');

router.get('/', shopController.getProducts);
{% endhighlight %}

<br /><br />

<h2>controllers/shop.js</h2>
{% highlight javascript %}
const Product = require('../models/product');
const ITEMS_PER_PAGE = 2;
...

exports.getProducts = (req, res, next) => {
    const page = +req.query.page || 1;    //+ at the beginning makes number from string
    let totalItems;

    Product.find().countDocuments().then(numProducts => {
        totalItems = numProducts;
        return  Product.find()
            .skip((page - 1) * ITEMS_PER_PAGE)
            .limit(ITEMS_PER_PAGE)
    })
    .then(products => {
       res.render('shop/index'), {
           prods: products,
           pageTitle: 'Shop',
           path: '/',
           currentPage: page,
           hasNextPage: ITEMS_PER_PAGE * page < totalItems,
           hasPreviousPage: page > 1,
           nextPage: page + 1,
           previousPage: page - 1,
           lastPage: Math.ceil(totalItems / ITEMS_PER_PAGE)
       }
    })
}
{% endhighlight %}
<br /><br />


<h2>views/shop.ejs</h2>
{% highlight javascript %}
...
<section class="pagination">
    <% if (currentPage != 1 && previousPage != 1) { %>
        <a href="/?page=1">1</a> 
    <% } %>
    
    <% if (hasPreviousPage) { %>
        <a href="/?page=<%= previousPage %>"><%= previousPage %></a>
    <% } %> 

    <a href="/?page=<%= currentPage %>"><%= currentPage %></a>

    <% if (hasNextPage) { %>
        <a href="/?page=<%= nextPage %>"><%= nextPage %></a>
    <% } %>  

    <% if (lastPage !== currentPage && nextPage !== lastPage) { %>
        <a href="/?page=<%= lastPage %>"><%= lastPage %></a> 
    <% } %>   
</section>
...
{% endhighlight %}