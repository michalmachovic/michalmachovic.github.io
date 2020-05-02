---
layout: post
date:   2020-05-01 10:13:26 +0200
title:  "Express.js: Using middleware to protect routes"
category: express
tags: [express]
---


<br /><br />
We are usually protecting routes which shouldnt be accessible for not logged users, for example orders, cart, ... 
We can use our own middleware for this.

<br /><br />
<h2>Prerequisities</h2>
We have some login page, we have default authenticate system, user will enter email and password, app will look into db, if there is that user with email and password, it will set `req.session.isLoggedIn = true;`. We are using sessions which are stored in db and we are using `mongoose`.
                        
<br /><br />

<h2>middleware/is-auth.js</h2>
If somebody wants to visit protected route, we redirect him to login page.
{% highlight javascript %}
module.exports = (req, res, next) => {
    if (!req.session.isLoggedIn) {
        return res.redirect('/login');
    }
    next();
}
{% endhighlight %}
<br /><br />


<h2>routes/shop.js</h2>
{% highlight javascript %}
const path = require('path');
const express = require('express');
const router = express.Router();

const shopController = require('../controllers/shop');
const isAuth = require('../middleware/is-auth');

router.post('/cart', isAuth, shopController.postCart);
{% endhighlight %}

<br /><br />


<h2>controllers/shop.js</h2>
{% highlight javascript %}
exports.postCart = (req, res, next) => {
    //code which will add product to cart
}  
{% endhighlight %}

