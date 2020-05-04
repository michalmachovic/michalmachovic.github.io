---
layout: post
date:   2020-05-03 11:13:26 +0200
title:  "Express.js: Validation"
category: express
tags: [express]
---


`npm install --save express-validator` <br /><br />
Typically we want to validate on post routes, because that is the case when user is sending some data. <br />
We will check if on signup page, entered email is in correct format.



<br /><br />


<h2>routes/auth.js</h2>
{% highlight javascript %}
const express = require('express');
const { check } = require('express-validator/check');

router.get('/signup', authController.getSignup);
router.post(
    '/signup', 
    [
        check('email')
            .isEmail(),
            .withMessage('Please enter a valid email.')
            .normalizeEmail(),  //sanitazing
        body(
            'password',
            'Please make sure your password has at least 5 characters and is alphanumeric'   //this is like withMessage but for both validators
        )
            .isLength({min: 5})
            .isAlphanumeric()
            .trim(),        
        body('confirmPassword').custom((value, { req }) => {      //custom validator
            if (value !== req.body.password) {
                throw new Error('Passwords have to match! ');
            }
            return true;
        })
    ],
    authController.postSignup);
{% endhighlight %}
<br /><br />

<h2>controllers/auth.js</h2>
{% highlight javascript %}
const express = require('express');
const { validationResult } = require('express-validator/check');

...
exports.postSignup = (req, res, next) => {
    const errors = validationResult(req);
    if (!errors.isEmpty()) {
        return res.status(422).render('auth/signup', {
            path: '/signup',
            pageTitle: 'Sign up',
            errorMessage: errors.array()[0].msg,
            validationErrors: errors.array(),
            oldInput: {
                email: req.body.email
                password: req.body.password,
                confirmPassword: req.body.confirmPassword
            }
        })
    }
}

...
{% endhighlight %}
<br /><br />


<h2>view/auth/signup.ejs</h2>
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
                    <form method="post" action="/signup">
                        <div>
                            <label>email</label>
                            <input 
                                class="<%=  validationErrors.find(e => e.param === 'email') ? 'invalid' : '' %>"
                                type="email" 
                                name="email" 
                                id="email"
                                value="<%= oldInput.email %>"
                            />
                        </div>

                        <div>
                            <label>password</label>
                            <input 
                                class="<%=  validationErrors.find(e => e.param === 'password') ? 'invalid' : '' %>"
                                type="password" 
                                name="password" 
                                id="password"
                                value="<%= oldInput.password %>"
                             />
                        </div>

                        <div>
                            <label>confirm password</label>
                            <input 
                                class="<%=  validationErrors.find(e => e.param === 'confirmPassword') ? 'invalid' : '' %>"
                                type="password" 
                                name="confirmPassword" 
                                id="confirmPassword"
                                value="<%= oldInput.confirmPassword %>"
                             />
                        </div>

                        <div>
                            <input type="hidden" name="_csrf" value="<%= csrfToken %>">
                            <input type="submit" value="signup" />
                        </div>
                    </form>
                </div>
        </div>
    </main>        
</body>
</html>
{% endhighlight %}
<br /><br />