---
layout: post
date:   2019-12-09 07:13:26 +0200
title:  "Mongo DB: Installation"
category: mongo, db
tags: [mongo, db]
---

MongoDB is a document database with the scalability and flexibility that you want with the querying and indexing that you need.

<h2>Installation - Atlas Cloud</h2>
We can use this for free and its better then local server, as its more realistic environment. Go to http://www.mongodb.com and sign up for free cluster.
<br />
1. Create new cluster <br />
2. Under `Security -> Database Access` create new user with `Read and write to any database`. You will need `user` and `password` later in your script where you will connect to db.<br />
3. Under `Security -> Network Access` add your current IP address.<br />
4. Back to `Clusters`, click on `Connect`, choose `Connect your application`<br />
<br /><br />
Now install MongoDb driver `npm install --save mongodb`.

<h3>util/database.js</h3>
{% highlight javascript %}
const mongodb = require('mongodb');
const MongoClient = mongodb.MongoClient;
const url = 'mongodb+srv://USER:PASSWORD@cluster0-gconm.mongodb.net/test?retryWrites=true&w=majority';

const mongoConnect = (callback) => {
    MongoClient.connect(
        url
    )
        .then(client => {
            console.log('Connected !');
            callback(client);
        })
        .catch(err => {
            console.log(err);
        });
}

module.exports = mongoConnect;
{% endhighlight %}

<br /><br />
<h3>app.js</h3>
{% highlight javascript %}
...
const mongoConnect = require('./util/database');
const app = express();
...
mongoConnect((client) => {
    console.log(client);
    app.listen(3000);
});
{% endhighlight %}

<br /><br /><br />

<h2>Installation - Local Server</h2>
This was tested on xubuntu 18.04.

<b>Remove previous MongoDB installations</b>
{% highlight php %}
sudo apt remove --autoremove mongodb-o
sudo rm /etc/apt/sources.list.d/mongodb*.list
sudo apt update
{% endhighlight %}
<br /><br />

<b>Import the MongoDB public key</b>
{% highlight php %}
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 9DA31620334BD75D9DCB49F368818C72E52529D4
{% endhighlight %}
<br /><br />


<b>Create mongodb-org-4.0.list file for Ubuntu 18.04 (Bionic)</b>
{% highlight php %}
echo "deb [ arch=amd64 ] https://repo.mongodb.org/apt/ubuntu bionic/mongodb-org/4.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-4.0.list
{% endhighlight %}
<br /><br />

<b>Reload local package database</b>
{% highlight php %}
sudo apt-get update
{% endhighlight %}
<br /><br />

<b>Install MongoDB packages</b>
{% highlight php %}
sudo apt-get install -y mongodb-org
{% endhighlight %}
<br /><br />

<b>Install specific release of MongoDB</b>
{% highlight php %}
sudo apt-get install -y mongodb-org=4.0.6 mongodb-org-server=4.0.6 mongodb-org-shell=4.0.6 mongodb-org-mongos=4.0.6 mongodb-org-tools=4.0.6
{% endhighlight %}
<br /><br />


<h2>Configure and Connect MongoDB</h2>
<b>Create new admin user</b>
{% highlight php %}
mongo
use admin
db.createUser({user:"admin", pwd:"password", roles:[{role:"root", db:"admin"}]})
{% endhighlight %}
<br /><br />

<b>Connect with new admin user</b>
{% highlight php %}
mongo -u admin -p password --authenticationDatabase admin
{% endhighlight %}
<br /><br />

<h2>Examples</h2>
<b>Show databases</b>
{% highlight php %}
show dbs;
{% endhighlight %}
<br /><br />


<b>Create new database</b>
{% highlight php %}
use DB_NAME;
use customer;
{% endhighlight %}
<br /><br />

<b>Create new collection</b>
{% highlight php %}
db.createCollection("users");
{% endhighlight %}
<br /><br />


<b>Show collections</b>
{% highlight php %}
show collections;
{% endhighlight %}
<br /><br />


<b>Inser new record</b>
{% highlight php %}
db.users.insert([{ first_name: "ferko", last_name: "mrkvicka", email: "ferko@gmail.com"}]);
{% endhighlight %}
<br /><br />


<b>Show all records</b>
{% highlight php %}
db.users.find();
{% endhighlight %}
<br /><br />
