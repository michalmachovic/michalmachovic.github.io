---
layout: post
date:   2020-05-14 13:13:26 +0200
title:  "Express.js - File Handling"
category: express
tags: [express]
---

{% highlight javascript %}
npm install --save multer
{% endhighlight %}
<br /><br />

`Multer` parses incoming requests for files, so it can parse text and also files.
<br /><br />

<h2>views/product-add.js</h2>
{% highlight javascript %}
<form action="/admin/product-add" enctype="multipart/form-data" method="post">
    <input type="file" name="image" id="image">
</form>
{% endhighlight %}
<br /><br />

<h2>routes/admin.js</h2>
{% highlight javascript %}
const path = require('path');
const express = require('express');
const router = express.Router();
const productsController = require('../controllers/shop');
const isAuth = require('../middleware/is-auth');

const products = [];

router.get('/product-add', isAuth, productsController.getAddProduct);
router.post('/product-add', isAuth, productsController.postAddProduct);

{% endhighlight %}
<br /><br />

<h2>controllers/shop.js</h2>
{% highlight javascript %}
exports.postAddProduct = (req, res, next) => {
    const title = req.body.title;
    const imageUrl = req.file;
    const price = req.body.price;
    const description = req.body.description;

    const product = new Product(
        {
            title: title,
            description: description, 
            price: price, 
            image: imageUrl,
            userId: req.user
        }
    );
    product.save();
    res.redirect('/');
}
{% endhighlight %}
<br /><br />



<h2>app.js</h2>
{% highlight javascript %}
...
const multer = require('multer');

app.use(multer({dest: 'images'}).single('image'));  //we are expecting single file with name image
{% endhighlight %}