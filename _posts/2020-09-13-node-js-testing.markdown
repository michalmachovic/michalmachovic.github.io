---
layout: post
date:   2020-09-13 13:13:26 +0200
title:  "Node.js: Testing"
category: nodejs, testing
tags: [nodejs, testing]
---

`Mocha` - for running the tests <br />
`Chai` - for asserting the results <br />
`Sinon` - for managing side effects and external dependencies
<br /><br />

{% highlight javascript %}
npm install --save-dev mocha chai
{% endhighlight %}
<br /><br />

<h2>package.json</h2>
{% highlight javascript %}
"scripts": {
    "test": "mocha"
}
{% endhighlight %}
<br /><br />
Then you can run tests with `npm test`. Mocha is then looking for tests under folder `test`.

<br /><br />

<h2>Dummy test</h2>
<h3>test/start.js</h3>
{% highlight javascript %}
const expect = require('chai').expect;

it('should add numbers correctly', function(){
    const num1 = 2;  
    const num2 = 3;
    expect(num1 + num2).to.equal(5);
})

it('should not give a result of 6', function(){
    const num1 = 2;  
    const num2 = 3;
    expect(num1 + num2).not.to.equal(6);
})
{% endhighlight %}
<br /><br />
<br /><br />

<h2>Real test</h2>
<h3>middleware/is-auth.js</h3>
{% highlight javascript %}
module.exports = (req, res, next) => {
    const authHeader = req.get('Authorization');
    is (!authHeader) {
        const error = new Error('Not authenticated.');
        error.statusCode = 401;
        throw error;
    }
    ....
}
{% endhighlight %}

<br /><br />
<h3>test/auth-middleware.js</h3>
{% highlight javascript %}
const expect = require('chai').expect;
const authMiddleware = require('../middleware/is-auth');

it('should throw an error if no authorization header is present', function() {
    const req = {
        get: function(headerName) {
            return null;
        }
    };
    expect(authMiddleware.bind(req, {}, () => {})).to.throw('Not authenticated.');
});
{% endhighlight %}