---
layout: post
date:   2020-05-11 11:13:26 +0200
title:  "Express.js: REST Api"
category: express
tags: [express]
---

{% highlight javascript %}
npm init
npm install --save express
npm install --save-dev nodemon   //for re-running app automatically
npm install --save body-parser   //for parsing requests
{% endhighlight %}

<br /><br />
We can test this for example through `Insomnia` to test `POST` and `GET`.

{% highlight javascript %}
GET locahost:8080/feed/posts

POST locahost:8080/feed/posts

//Example JSON data: 
{
    "title": "Test title",
    "content": "Test Content"
}
{% endhighlight %}
<br /><br />
Or we can test this on `codepen.com`:
{% highlight javascript %}
const getButton = document.getElementById('get');
const postButton = document.getElementById('post');

getButton.addEventListener('click',() => {
    fetch('http://localhost:8080/feed/posts')
    .then(res => res.json())
    .then(resData => console.log(resData))
    .catch(err => console.log(err));
});

postButton.addEventListener('click',() => {
    fetch('http://localhost:8080/feed/post',
    {
        method: 'POST',
        body: JSON.stringify({
            title: 'test post from codepen',
            body: 'body from codepen'
        }),
        headers: {
            'Content-Type': 'application/json'
        }
    })
    .then(res => res.json())
    .then(resData => console.log(resData))
    .catch(err => console.log(err));
});
{% endhighlight %}


<br /><br />


<h2>.gitignore</h2>
{% highlight javascript %}
node_modules
{% endhighlight %}

<br /><br />
<h2>package.json</h2>
{% highlight javascript %}
...
"scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "start": "nodemon app.js"
},
...
{% endhighlight %}


<br /><br />
<h2>controllers/feed.js</h2>
{% highlight javascript %}
exports.getPosts = (req, res, next) => {
    res.status(200).json({ 
        posts: [{ title: 'First Post', content: 'This is the first post' }]
    });
};

exports.createPost = (req, res, next) => {
    const title = req.body.title;
    const content = req.body.content;

    //Create post in db
    res.status(201).json({
        message: 'Post created successfully',
        post: { id: new Date().toISOString(), title: title, content:content }
    })
}
{% endhighlight %}



<br /><br />
<h2>routes/feed.js</h2>
{% highlight javascript %}
const express = require('express');
const feedController = require('controllers/feed');

const router = express.Router();

//GET /feed/posts
router.get('/posts', feedController.getPosts);


//POST /feed/post
router.post('/post', feedController.createPost);

module.exports = router;
{% endhighlight %}



<br /><br />
<h2>app.js</h2>
{% highlight javascript %}
const express = require('express');
const feedRoutes = require('./routes/feed');
const app = express();


// app.use(bodyParser.urlencoded()); //for x-www-form-urlencoded, sending requests via form
app.use(bodyParser.json()); // application/json

// we need to set headers to avoid CORS issues
app.use((req, res, next) => {
    res.setHeader('Access-Control-Allow-Origin', '*');
    res.setHeader('Access-Control-Allow-Methods', 'GET, POST, PUT, PATCH, DELETE');
    res.setHeader('Access-Control-Allow-Headers', 'Content-Type, Authorization');
    next();
})

app.use('/feed',feedRoutes);

app.listen(8080);
{% endhighlight %}