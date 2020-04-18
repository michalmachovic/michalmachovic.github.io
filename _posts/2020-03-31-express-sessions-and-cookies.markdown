---
layout: post
date:   2020-03-31 09:13:26 +0200
title:  "Express.js - Session and Cookies"
category: express, sessions, cookies
tags: [express, sessions, cookies]
---

`npm install --save express-session`
<br /><br />
<h2>Storing sessions in memory</h2>
<br /><br />
This is just for development. In production we will not store sessions in memory as with lot of users it can easily crash.
<br /><br />
We need to first add sesssion middleware.
<h3>app.js</h3>
{% highlight javascript %}
const session = require('express-session');

const app = express();

app.use(
    session({secret: 'my secret', resave: false, saveUninitialized: false })
);

//my secret in production = long string value 
//resave: false and saveUninitialized: false = session will not be saved on every request, only if something change
{% endhighlight %}


<br /><br >
<h3>controllers/auth.js</h3>
{% highlight javascript %}
exports.postLogin = (req, res, next) {
    req.session.isLoggedIn = true;  //isLogged in is our name
    res.redirect('/');
}
{% endhighlight %}

After `postLogin` new cookie will be created in our browser, we can see it in developer console under Application/Cookies.

<br /><br /><br />
<h2>Storing sessions in database</h2>
`npm install --save connect-mongodb-session`
<br /><br />
This is for production. We will use MongoDB for storing sessions.

<h3>app.js</h3>
{% highlight javascript %}
const session = require('express-session');
const MongoDBStore = require('connect-mongodb-session')(session);
const MONGODB_URI = 'mongodb+srv://MONGO_USER:MONGO_PASSWORD@cluster0-gconm.mongodb.net/test?w=majority';

const app = express();
const store = new MongoDBStore({
    uri: MONGODB_URI,
    collection: 'sessions'
});

app.use(
    session({
        secret: 'my secret', 
        resave: false, 
        saveUninitialized: false,
        store: store 
    })
);
{% endhighlight %}


<br /><br >
<h3>controllers/auth.js</h3>
This remains the same, session are now stored in MongoDB.
{% highlight javascript %}
exports.postLogin = (req, res, next) {
    req.session.isLoggedIn = true;  //isLoggedIn is our name
    res.redirect('/');
}

exports.postLogout = (req, res, next) => {
    req.session.destroy((err) => {
        console.log(err);
        res.redirect('/');
    });
}

{% endhighlight %}