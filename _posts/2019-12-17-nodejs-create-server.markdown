---
layout: post
date:   2019-12-17 09:13:26 +0200
title:  "NodeJS: Create Server"
category: nodejs
tags: [javascript, nodejs]
---
This is how we can create simple server with form where you can enter text which will be stored in message.txt

{% highlight javascript %}
const http = require('http');
const fs = require('fs');


const server = http.createServer((req, res) => {
    console.log(req.url, req.method, req.headers);

    const url = req.url;
    const method = req.method;

    if (url === '/') {
        res.setHeader('Content-Type', 'text/html');
        res.write('<html>');
        res.write('<head><title>Enter Message</title></head>');
        res.write('<body><form action="/message" method="POST"><input type="text" name="message" /><button type="submit">Send</button></form></body>');
        res.write('</html>');
        return res.end();
    }

    if (url === '/message' && method === 'POST') {
        const body = [];
        req.on('data', (chunk) => {
            body.push(chunk);
        });

        return req.on('end', () => {
            const parsedBody = Buffer.concat(body).toString();
            console.log(parsedBody);
            const message = parsedBody.split('=')[1];
            fs.writeFile('message.txt', message, err => {
                res.statusCode = 302;
                res.setHeader('Location', '/');
                return res.end();
            });
        });
    }

    res.setHeader('Content-Type', 'text/html');
    res.write('<html>');
    res.write('<head><title>My First Page</title></head>');
    res.write('<body><h1>Hello from NodeJS server ! </h1></body>');
    res.write('</html>');
    res.end();
});

server.listen(3000);

{% endhighlight %}
<br /><br />
