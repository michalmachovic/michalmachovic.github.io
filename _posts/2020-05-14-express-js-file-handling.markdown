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


<h2>Storing files</h2>
<h3>views/product-add.js</h3>
{% highlight javascript %}
<form action="/admin/product-add" enctype="multipart/form-data" method="post">
    <input type="file" name="image" id="image" />
</form>
{% endhighlight %}
<br /><br />

<h3>routes/admin.js</h3>
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

<h3>controllers/shop.js</h3>
{% highlight javascript %}
exports.postAddProduct = (req, res, next) => {
    const title = req.body.title;
    const image = req.file;
    const price = req.body.price;
    const description = req.body.description;

    if (!image) {
        return res.status(422).render('admin/edit-product', {
            hasError: true,
            errorMessage: 'Attached file is not an image'
        });
    }

    ...
        store in db
    ...
}
{% endhighlight %}
<br /><br />



<h3>app.js</h3>
{% highlight javascript %}
...
const multer = require('multer');

const fileStorage = multer.diskStorage({
    destination: (req, file, callback) => {
        callback(null, 'images');
    },
    filename: (req, file, callback) => {
        callback(null, new Date().toISOString() + '-' + file.originalname);
    }
});

const fileFilter = (req, file, callback) => {
    if (file.mimetype == 'image/png' ||
        file.mimetype == 'image/jpg' ||
        file.mimetype == 'image/jpeg'
    ) {
        callback(null, true);
    } else {
        callback(null, false);
    }    
}

app.use(multer
    ({
        storage: fileStorage,
        fileFilter: fileFilter
    })
    .single('image'));  //we are expecting single file with name image


app.use('/images', express.static(path.join(__dirname, 'images'))); //serve images as static files
{% endhighlight %}

<br /><br />

<h3>Serving files</h3>
Uploaded images can be served as public static files. But for example invoices we want to serve only for users which it belongs to. We will also generate PDF file with `pdfkit`.

<br /><br />
{% highlight javascript %}
npm install --save pdfkit
{% endhighlight %}

<br /><br />


<h3>routes/shop.js</h3>
{% highlight javascript %}
...
router.get('orders/:orderId', isAuth, shopController.getInvoice);
...
{% endhighlight %}

<br /><br />

<h3>controllers/shop.js</h3>
{% highlight javascript %}
const fs = require('fs');  //file system
const path = require('path');
const Order = require('../models/order');

const PDFDocument = require('pdfkit');

exports.getInvoice = (req, res, next) => {
    const orderId = req.params.orderId; 
    Order.findById(orderId).then(order => {
        if (!order) {
            return next(new Error('No order found'));
        }
        if (order.user.userId.toString() !== req.user._id.toString()) {
            return next(new Error('Unauthorizes'));
        }

        const invoiceName = 'invoice-' + orderId + '.pdf';
        const invoicePath = path.join('data', 'invoice', invoiceName);
       

        /*
        //readFile is good only for small files, because node read content of whole file into memory. when there are lot of request, this can cause problems
        fs.readFile(invoicePath, (err, data) => {
            if (err) {
                return next(err);
            }

            res.setHeader('Content-Type', 'application/pdf');
            res.setHeader('Content-Disposition', 'attachment; filename="' + invoiceName + '"');
            res.send(data);
        });
        */


        //streaming is good for bigger files
         
        const pdfDoc = new PDFDocument();
        res.setHeader('Content-Type', 'application/pdf');
        res.setHeader('Content-Disposition', 'attachment; filename="' + invoiceName + '"');
        pfdDoc.pipe(fs.createWriteStream(invoicePath));
        pdfDoc.pipe(res);
        pdfDoc.text('Hello this is test invoice');
        pdfDoc.end();
    })
    .catch(err => next(err));
};
{% endhighlight %}