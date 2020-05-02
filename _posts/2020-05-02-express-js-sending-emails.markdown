---
layout: post
date:   2020-05-02 11:13:26 +0200
title:  "Express.js: Sending emails"
category: express
tags: [express]
---


<br /><br />
Builind own mailserver is very complex task, thats why we use some existing services for this. We will use Sendgrid (www.sendgrid.com) for this as they have free plan. We need to install also `nodemailer` and `nodemailer-sendgrid-transport`. <br />
`npm install --save nodemailer` <br />
`npm install --save nodemailer-sendgrid-transport`


<br /><br />


<h2>controllers/auth.js</h2>
{% highlight javascript %}

const nodemailer = require('nodemailer');
const sendgridTransport = require('nodemailer-sendgrid-transport');

const transporter = nodemailer.createTransport(sendgridTransport({
    auth: {
        api_key: SENDGRID_API_KEY
    }
}));




exports.postSignup = (req, res, next) => {
   ...
    transporter.sendEmail({
                to: email,
                from: 'shop@node-complete.com',
                subject: 'New account registration',
                html: '<h1>You successfully signed up !</h1>'
            });
   ...
}
{% endhighlight %}
<br /><br />
