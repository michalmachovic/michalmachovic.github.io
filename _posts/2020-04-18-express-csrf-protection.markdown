---
layout: post
date:   2020-04-18 09:13:26 +0200
title:  "Express.js - CSRF Protection"
category: express
tags: [express, csrf]
---
Cross-site request forgery (also known as CSRF) is a web security vulnerability that allows an attacker to induce users to perform actions that they do not intend to perform. It allows an attacker to partly circumvent the same origin policy, which is designed to prevent different websites from interfering with each other.
<br /><br />
We will install `csurf` extension for this. `npm install --save csurf`. We need to pass `csrf token` through controllers to each view (we can do this once in app.js) and add hidden input with name `_csrf` with csrf token to each view, to each `post` request.

<h2></h2>

<h3>app.js</h3>
{% highlight javascript %}
const path = require('path');
const express = require('express');
const bodyParser = require('body-parser');
const app = express();
var ejs = require('ejs');

const authRoutes = require('./routes/auth');

//we will add csrf extension
const csrf = require('csurf');
const csrfProtection = csrf();

app.use(bodyParser.urlencoded({extended: false}));

//we need to add this as middleware after bodyParser
app.use(csrfProtection);

//this will push csrfToken to all rendered views, so we dont need to pass it to each view through controller separately
app.use((req, res, next) => {
    res.locals.csrfToken = req.csrfToken();
    next();
});
{% endhighlight %}

<br /><br />
<h3>routes/auth.js</h3>
{% highlight javascript %}
const path = require('path');
const express = require('express');
const router = express.Router();

const authController = require('../controllers/auth');

router.get('/login',authController.getLogin);

router.post('/login',authController.postLogin);
{% endhighlight %}

<br /><br />
<h3>controllers/auth.js</h3>
{% highlight javascript %}
exports.getLogin = (req, res, next) => {
    res.render('auth/login', {
        path: '/login',
        pageTitle: 'Login'
    });
}


exports.postLogin = (req, res, next) => {
    const email = req.body.email;
    const password = req.body.password;
   
   //user authentification
}
{% endhighlight %}

<br /><br />
<h3>views/auth/login.ejs</h3>
{% highlight javascript %}
<title><%= pageTitle %></title>
</head>

<body>
    <main>
        <div class="login">
                <div>
                    <form method="post" action="/login">
                        <div>
                            <label>email</label>
                            <input type="email" name="email" />
                        </div>

                        <div>
                            <label>password</label>
                            <input type="password" name="password" />
                        </div>

                        <div>
                            <input type="submit" value="login" />
                        </div>

                        <input type="hidden" name="_csrf" value="<%= csrfToken %>">
                    </form>
                </div>
        </div>
    </main>        
</body>
</html>
{% endhighlight %}
