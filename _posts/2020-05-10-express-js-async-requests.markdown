---
layout: post
date:   2020-05-10 11:13:26 +0200
title:  "Express.js: Async Requests"
category: express
tags: [express]
---

Async request we can use when we want to do some action behind the scenes, when we dont want to reload page. Usually we are doing for example post request for deleting product - we have form with some submit button, which will redirect it to post request where product will be deleted and page will be reloaded. Here we will use async request, deleting product will happen asynchronously and page will not be reloaded, we will remove element directly from DOM.

<br /><br />

<h2>views/admin/products.js</h2>
{% highlight javascript %}
...
<article>
    ...
    <!-- product html -->
    ...
    <div class="actions">
        <input type="hidden" value="<%= product._id %>" name="productId" />
        <input type="hidden" value="<%= csrfToken %>" name="_csrf" />
        <button class="btn" type="button" onclick="deleteProduct(this)">Delete</button>
    </div>
</article>
...
{% endhighlight %}

<br /><br />


<h2>public/js/admin.js</h2>
{% highlight javascript %}
const deleteProduct = (btn) => {
    const prodId = btn.parentNode.querySelector('[name=productId]').value;
    const csrf = btn.parentNode.querySelector('[name=_csrf]').value;
    const productElement = btn.closest('article')

    fetch('/admin/product' + prodId, {
        method: 'DELETE',
        headers: {
            'csrf-token': csrf
        }
    })
    .then(result => {
        return result.json();
    })
    .then(data => {
        productElement.parentNode.removeChild(productElement);
    })
    .catch(err => {
        console.log(err);
    });
}
{% endhighlight %}


<br /><br />


<h2>routes/admin.js</h2>
{% highlight javascript %}
const path = require('path');
const express = require('express');
const router = express.Router();
const productsController = require('../controllers/shop');
const isAuth = require('../middleware/is-auth');

router.delete('/product/:productId', isAuth, productsController.deleteProduct)
module.exports = router;
{% endhighlight %}


<br /><br />


<h2>controllers/shop.js</h2>
{% highlight javascript %}
...
const Product = require('../models/product');

exports.deleteProduct = (req ,res, next) => {
    const prodId = req.params.productId;
    Product.findById(prodId)
    .then(product => {
        if (!product) {
            return next(new Error('Product not found'));
        }
        return Product.deleteOne({_id: prodId, userId: req.user._id});
    })
    .then(() => {
        res.status(200).json({ message: 'Success !'});
    })
    .catch(err => {
        res.status(500).json({ message: 'Deleting product failed !'});
    })
}
...
{% endhighlight %}