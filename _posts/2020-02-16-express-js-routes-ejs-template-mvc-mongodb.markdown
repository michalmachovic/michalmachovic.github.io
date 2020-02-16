---
layout: post
date:   2020-02-16 09:13:26 +0200
title:  "Express.js - Routes and Router with EJS templating engine and MVC and MongoDB"
category: nodejs
tags: [javascript, nodejs, express, ejs, mongodb]
---
Use `MongoDB Compass` for working with database.
<br /><br />

{% highlight javascript %}
-utils
    |
    |---db.js

-routes
    |
    |---admin.js
    |---shop.js

-controllers
    |
    |---shop.js

-models
    |
    |---product.js

-views
    |
    |--product-add.js
    |--product-edit.js
    |--product.ejs
    |--shop.ejs
    |
    |---includes
    |       |
    |       |---head.ejs
    |       |---nav.ejs
    |
    |---shop
            |
            |---cart.ejs
            |---orders.ejs

-public
    |
    |---css
        |
        |---main.css
-app.js
{% endhighlight %}
<br /><br />
<h3>models/product.js</h3>
{% highlight javascript %}
const url = 'mongodb+srv://MONGODB_USER:MONGODB_PASSWORD@cluster0-gconm.mongodb.net/MONGODB_DATABASE?retryWrites=true&w=majority';
const mongodb = require('mongodb');
const MongoClient = mongodb.MongoClient;

let _db;

const mongoConnect = (callback) => {
  MongoClient.connect(
    url
  )
    .then(client => {
      console.log('Connected!');
      _db = client.db();
      callback();
    })
    .catch(err => {
      console.log("DB NOT CONNECTED");
      console.log(err);
    });
};

const getDb = () => {
    if (_db) {
        return _db;
    }
    throw 'No db found';
}

exports.mongoConnect = mongoConnect;
exports.db = getDb;
{% endhighligt %}



<br /><br />
<h3>models/product.js</h3>
{% highlight javascript %}
const fs = require('fs');
const path = require('path');
const mongodb = require('mongodb');
const getDb = require('../util/db').db;

class Product {
    constructor(title, description, price, id, userId) {
        this.title = title;
        this.description = description;
        this.price = price;
        this._id = id ? new mongodb.ObjectId(id) : null;
        this.userId = userId;
    }

    save() {
        const db = getDb();
        db.collection('products')
        .insertOne(this)
        .then(res => {
            console.log(res);
        })
        .catch(err => {
            console.log(err);
        });
    }

    //static means that it will be not called on concrete instance of product model, but will return all products
    static fetchAll() {
        const db = getDb();
        return db
        .collection('products')
        .find()
        .toArray()
        .then(products => {
            console.log(products);
            return products;
        })
        .catch(err => {
            console.log(err);
        });
    }

    static fetchById(prodId) {
        const db = getDb();
        return db
        .collection('products')
        .find({ _id: new mongodb.ObjectId(prodId) })
        .next()
        .then(product => {
            return product;
        })
        .catch(err => {
            console.log(err);
        })
    }

    static updateById(prodId, values) {
        const db = getDb();
        db
        .collection('products')
        .updateOne(
            { _id: new mongodb.ObjectId(prodId) }, 
            { $set: values }
        );
    }
}

module.exports = Product;
{% endhighlight %}


<br /><br />
<h3>models/user.js</h3>
{% highlight javascript %}
const mongodb = require('mongodb');
const getDb = require('../util/db').db;

const ObjectId = mongodb.ObjectId;

class User {
    constructor(username, email, cart, id) {
        this.name = username;
        this.email = email;
        this.cart = cart;
        this._id = id;
    }

    save() {
        const db = getDb();
        db.collection('users').insertOne(this);
    }

    addToCart(product) {
        //is product already in cart ?
        const cartProductIndex = this.cart.items.findIndex(cp => {
          return cp.productId.toString() == product._id.toString();
        });
        let newQuantity = 1;
        const updatedCartItems = [...this.cart.items];

        //if it is greater > -1, product is already there
        if (cartProductIndex >= 0) {
          newQuantity = this.cart.items[cartProductIndex].quantity + 1;
          updatedCartItems[cartProductIndex].quantity = newQuantity;
        } else {
          updatedCartItems.push({ 
            productId: new ObjectId(product._id), 
            quantity: newQuantity})
        }

        const updatedCart = { 
          items: updatedCartItems
        };

        const db = getDb();
        return db.collection('users').updateOne(
          {_id: new ObjectId(this._id)},
          { $set: { cart: updatedCart }}
        );
    }

    getCart() {
      const db = getDb();
      const productIds = this.cart.items.map(i => {
        return i.productId;
      });
      return db
        .collection('products')
        .find({ _id: { $in: productIds } })
        .toArray()
        .then(products => {
          return products.map(p => {
            return {
              ...p,
              quantity: this.cart.items.find(i => {
                return i.productId.toString() === p._id.toString();
              }).quantity
            };
          });
        });
    }

    static findById(userId) {
        const db = getDb();
        return db
          .collection('users')
          .findOne({ _id: new ObjectId(userId) })
          .then(user => {
            console.log(user);
            return user;
          })
          .catch(err => {
            console.log(err);
          });
      }
    
      deleteItemFromCart(productId) {
        const updatedCartItems = this.cart.items.filter(item => {
          return item.productId.toString() !== productId.toString();
        })
        const db = getDb();
        return db
          .collection('users')
          .updateOne(
            { _id: new ObjectId(this._id)},
            { $set: { cart: { items: updatedCartItems} } }
          );
      }

      addOrder() {
        const db = getDb();
        return this.getCart().then(products => {
          const order = {
            items: products,
            user: {
              _id: new ObjectId(this._id),
              name: this.name
            }
          };
          return db.collection('orders')
          .insertOne(order);
        })
          .then(result => {
            this.cart = { items: [] };
            return db
              .collection('users')
              .updateOne(
                { _id: new ObjectId(this._id) },
                { $set: { cart: { items:[] } } }
              );
          });
      }

      getOrders() {
        const db = getDb();
        return db.collection('orders')
        .find({'user._id': new ObjectId(this._id)})
        .toArray();
      }
}

module.exports = User;
{% endhighlight %}

<br /><br />
<h3>controllers/shop.js</h3>
{% highlight javascript %}
const Product = require('../models/product');

exports.getAddProduct = (req, res, next) => {
    res.render('product-add', {
        pageTitle: 'Add product',
        path: '/'
    });
}

exports.postAddProduct = (req, res, next) => {
    const product = new Product(
        req.body.title,
        req.body.description, 
        req.body.price, 
        null,
        req.user._id);
    product.save();
    res.redirect('/');
}

exports.getProducts =  (req, res, next) => {
    Product.fetchAll().
        then(products => {
            res.render('shop', {
                products: products,
                pageTitle: 'shop',
                path: '/',
                hasProducts: products.length > 0,
                activeShop: true
            });
        });
}

exports.getProduct = (req, res, next) => {
    Product.fetchById(req.params.productId).
        then(product => {
            res.render('product', {
                product: product,
                pageTitle: 'product'
            });
        });
}

exports.getEditProduct = (req, res, next) => {
    Product.fetchById(req.params.productId).
        then(product => {
            res.render('product-edit', {
                product: product,
                pageTitle: 'product'
            });
        });
}

exports.postUpdateProduct = (req, res, next) => {
    Product.updateById(req.params.productId, req.body);
    res.redirect('/');
}

exports.postCart = (req, res, next) => {
    const prodId = req.body.productId;
    Product.fetchById(prodId)
    .then(product => {
        return req.user.addToCart(product);
    })
    .then(result => {
        console.log(result);
        res.redirect('/cart');
    })
}

exports.getCart = (req, res, next) => {
    req.user
        .getCart()
        .then(products => {
            res.render('shop/cart',{
                path: '/cart',
                pageTitle: 'Your cart',
                products: products
            })
        });
}

exports.postCartDeleteProduct = (req, res, next) => {
    const prodId = req.body.productId;
    req.user
        .deleteItemFromCart(prodId)
        .then(result => {
            res.redirect('/cart');
        })
        .catch(error => {
            console.log(error);
        })
}

exports.postOrder = (req, res, next) => {
    let fetchedCart;
    req.user
        .addOrder()
        .then(result => {
            res.redirect('/orders');
        })
        .catch(error => {
            console.log(error);
        })
}

exports.getOrders = (req, res, next) => {
    req.user
        .getOrders()
        .then(orders => {
            res.render('shop/orders', {
                path: '/orders',
                pageTitle: 'Your Orders',
                orders: orders
            });
        })
        .catch(error => console.log(error));
}
{% endhighlight %}
<br /><br />


<h3>routes/admin.js</h3>
{% highlight javascript %}
const path = require('path');
const express = require('express');
const router = express.Router();
const productsController = require('../controllers/shop');

const products = [];

router.get('/product-add', productsController.getAddProduct);

router.post('/product-add', productsController.postAddProduct);

router.get('/product-edit/:productId', productsController.getEditProduct);

router.post('/product-update/:productId', productsController.postUpdateProduct);

module.exports = router;
{% endhighlight %}

<br /><br />

<h3>routes/shop.js</h3>
{% highlight javascript %}
const path = require('path');
const express = require('express');
const router = express.Router();

const shopController = require('../controllers/shop');

router.get('/', shopController.getProducts);

router.get('/products/:productId', shopController.getProduct);

router.post('/cart', shopController.postCart);

router.get('/cart', shopController.getCart);

router.post('/cart-delete-item', shopController.postCartDeleteProduct);

router.post('/create-order', shopController.postOrder);

router.get('/orders', shopController.getOrders);

module.exports = router;
{% endhighlight %}

<br /><br />

<h3>views/includes/head.ejs</h3>
{% highlight javascript %}
<!DOCTYPE html>
<html>
<head>
    <!-- we are using express.static in app.js to serve static css file from public dir -->
    <link rel="stylesheet" href="/css/main.css">
{% endhighlight %}
<br /><br />


<h3>views/includes/navigation.ejs</h3>
{% highlight javascript %}
<nav>
    <ul>
        <li>
            <a href="/">
                Products
            </a>
        </li>
        <li>
            <a href="/admin/product-add">
                Add product
            </a>
        </li>
        <li>
            <a href="/cart">
                Cart
            </a>
        </li>
        <li>
            <a href="/orders">
                Orders
            </a>
        </li>
    </ul>
</nav>
{% endhighlight %}
<br /><br />

<h3>views/product-add.ejs</h3>
{% highlight javascript %}
<%- include('includes/head.ejs') %>
<title><%= pageTitle %></title>
</head>

<body>
    <%- include('includes/nav.ejs') %>
    <main>
    <h2>Add product</h2>
    <form method="post" action="/admin/product-add">
        <div>
            <label for="title">
                Title
            </label>
            <input type="text" name="title" />
        </div>    

        <div>
            <label for="description">
                Description
            </label>
            <input type="text" name="description" />    
        </div>

        <div>    
            <label for="price">
                Price
            </label>
            <input type="text" name="price" />    
        </div>
        
        <div>
            <input type="submit" value="Submit" />
        </div>
    </form>
    </main>
</body>
{% endhighlight %}



<br /><br />

<h3>views/product-edit.ejs</h3>
{% highlight javascript %}
<%- include('includes/head.ejs') %>
<title><%= pageTitle %></title>
</head>

<body>
    <%- include('includes/nav.ejs') %>
    <main>
    <h2><%= product.title %></h2>
        <div class="products">
                <div>
                    <form method="post" action="/admin/product-update/<%= product._id %>">
                        <div>
                            <input type="text" name="description" value="<%= product.description %>" />
                        </div>

                        <div>
                            <input type="text" name="price" value="<%= product.price %>" />
                        </div>

                        <div>
                            <input type="submit" value="save" />
                        </div>
                    </form>
                </div>
        </div>
    </main>        
</body>
</html>
{% endhighlight %}


<br /><br />
<h3>views/404.ejs</h3>
{% highlight javascript %}
<%- include('includes/head.ejs') %>
</head>
<body>
    <%- include('includes/navigation.ejs') %>
    <h1>Page not found</h1>
</body>
</html>
{% endhighlight %}


<br /><br />
<h3>views/shop.ejs</h3>
{% highlight javascript %}
<%- include('includes/head.ejs') %>
<title><%= pageTitle %></title>
</head>

<body>
    <%- include('includes/nav.ejs') %>
    <main>
    <h2>Products</h2>
        <% if (products.length > 0) { %>
            <div class="products">
                <% products.forEach(function(item) {  %>
                    <div>
                        <h3>
                            <%= item.title %>
                        </h3>

                        <div>
                            <%= item.description %>
                        </div>

                        <div>
                            <%= item.price %>
                        </div>

                        <div class="actions">
                            <a href="/products/<%= item._id %>">Detail</a>
                            <a href="/admin/product-edit/<%= item._id %>">Edit</a>
                            <form action="/cart" method="post">
                                <button class="btn" type="submit">Add to cart</button>
                                <input type="hidden" name="productId" value="<%= item._id %>" />
                            </form>
                        </div>
                    </div>
                <%  }); %>
            </div>    
        <% } %>    
    </main>
</body>
</html>
{% endhighlight %}



<br /><br />
<h3>views/product.ejs</h3>
{% highlight javascript %}
<%- include('includes/head.ejs') %>
<title><%= pageTitle %></title>
</head>

<body>
    <%- include('includes/nav.ejs') %>
    <main>
    <h2><%= product.title %></h2>
        <div class="products">
                <div>
                    <div>
                        <%= product.description %>
                    </div>

                    <div>
                        <%= product.price %>
                    </div>
                    
                </div>
        </div>   
    </main>     
</body>
</html>
{% endhighlight %}



<br /><br />
<h3>views/shop/cart.ejs</h3>
{% highlight javascript %}
<%- include('../includes/head.ejs') %>
<title><%= pageTitle %></title>
</head>

<body>
    <%- include('../includes/nav.ejs') %>
    <main>
    <h2>Products</h2>
        <% if (products.length > 0) { %>
            <div class="products">
                <% products.forEach(function(item) {  %>
                    <div>
                        <h3>
                            <%= item.title %>
                        </h3>

                        <div>
                            Quantity: <%= item.quantity %>
                        </div>

                        <div class="actions">
                            <form action="/cart-delete-item" method="post">
                                <button class="btn" type="submit">Delete</button>
                                <input type="hidden" name="productId" value="<%= item._id %>" />
                            </form>
                        </div>
                    </div>
                <%  }); %>
            </div>    

            <div class="actions">
                <form action="/create-order" method="post">
                    <button class="btn" type="submit">Create Order</button>
                </form>
            </div>
        <% } %>    
    </main>
</body>
</html>
{% endhighlight %}



<br /><br />
<h3>views/shop/orders.ejs</h3>
{% highlight javascript %}
<%- include('../includes/head.ejs') %>
<title><%= pageTitle %></title>
</head>

<body>
    <%- include('../includes/nav.ejs') %>
    <main>
    <h2>Orders</h2>
        <% if (orders.length > 0) { %>
            <div class="orders">
                <% orders.forEach(function(order) {  %>
                    <div>
                        <h3>
                            Order <%= order._id %>
                        </h3>

                        <% order.items.forEach(function(item) { %>
                            <div>
                                <%= item.title %>
                                Quantity: <%= item.quantity %>
                            </div>
                        <% }); %>    
                    </div>
                <%  }); %>
            </div>    
        <% } %>    
    </main>
</body>
</html>
{% endhighlight %}


<br /><br />
<h3>app.js</h3>
{% highlight javascript %}
const path = require('path');
const express = require('express');
const bodyParser = require('body-parser');
const app = express();

const shopRoutes = require('./routes/shop');
const adminRoutes = require('./routes/admin');

const mongoConnect = require('./util/db').mongoConnect;
const User = require('./models/user');

//set template engine to EJS
app.set('view engine', 'ejs');
app.set('views', 'views');

//add this to make public folder available to serve static files, like css
app.use(express.static(path.join(__dirname, 'public')));

app.use((req, res, next) => {
    User.findById('5e3ed9cee844e413f1269e18')
    .then(user => {
        req.user = new User(user.name, user.email, user.cart, user._id);
        next();
    })
    .catch(err => console.log(err));
})

app.use(bodyParser.urlencoded({exteneded: false}));
app.use(shopRoutes);
app.use('/admin', adminRoutes);

app.get('/test', (req, res, next) => {
    res.send('<h1>testing</h1>');
});

app.get('/', (req, res, next) => {
    res.send('<h1>hello</h1>');
});


mongoConnect(()  => {
        app.listen(3000);
});
{% endhighlight %}
