---
layout: post
date:   2020-05-02 10:13:26 +0200
title:  "Express.js: Flash messages"
category: express
tags: [express]
---


<br />
`npm install --save connect-flash`
<br /><br />
Flash messages can be used for example when user enter wrong email or password during the logging process. Or user will enter email which is already in use during registration process. Problem is that after user enter some data, we will redirect him, so we must store information about what happened somewhere else. For that we can use flash message.


<br /><br />
<h2>Prerequisities</h2>
We are storing sessions in db and using `mongoose`.
                        
<br /><br />

<h2>app.js</h2>
{% highlight javascript %}
const flash = require('connect-flash');
...
app.use(
    session({
        secret: 'my secret', 
        resave: false, 
        saveUninitialized: false,
        store: store 
    })
);

app.use(flash());
...
{% endhighlight %}

<br /><br />

<h2>controllers/auth.js</h2>
{% highlight javascript %}
const bcrypt = require('bcryptjs');
const User = require('../models/user');

exports.getLogin = (req, res, next) => {
    let message = req.flash('error');
    if (message.length > 0) {
        message = message[0];
    } else {
        message = null;
    }
    res.render('auth/login', {
        path: '/login',
        pageTitle: 'Login',
        errorMessage: message
    });
}


exports.postLogin = (req, res, next) => {
    const email = req.body.email;
    const password = req.body.password;
    User.findOne({email: email})
        .then(user => {
            if (!user) {
                req.flash('error', 'Invalid email or password.');
                return res.redirect('/login');
            }
            bcrypt
                .compare(password, user.password)
                .then(doMatch => {
                    if (doMatch) {
                        req.session.isLoggedIn = true;
                        req.session.user = user;
                        return req.session.save(err => {
                            console.log(err);
                            res.redirect('/');
                        })            
                    }
                    req.flash('error', 'Invalid email or password.');
                    res.redirect('/login');
                })
                .catch(err => {
                    res.redirect('/login');
                })
    })
    .catch(err => console.log(err));
}
{% endhighlight %}
<br /><br />

<h2>routes/auth.js</h2>
{% highlight javascript %}
const path = require('path');
const express = require('express');
const router = express.Router();

const authController = require('../controllers/auth');

router.get('/login',authController.getLogin);
router.get('/signup',authController.getSignup);
router.post('/login',authController.postLogin);
router.post('/signup',authController.postSignup);
router.post('/logout',authController.postLogout);

module.exports = router;
{% endhighlight %}

<br /><br />


<h2>views/auth/login.ejs</h2>
{% highlight javascript %}
<%- include('../includes/head.ejs') %>
<title><%= pageTitle %></title>
</head>

<body>
    <%- include('../includes/nav.ejs') %>
    <main>
        <% if (errorMessage) { %>
            <div class="user-message user-message--error">
                <%= errorMessage %>
            </div>
        <% } %>
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