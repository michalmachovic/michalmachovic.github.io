---
layout: post
date:   2019-05-28 07:13:26 +0200
title:  "Redux: Routers"
category: react
tags: [react, redux, router]
---

<h2>Redux Devtools Extension</h2>
`github.com/zalmoxisus/redux-devtools-extension`

<br /><br />

<h3>index.js</h3>
{% highlight javascript %}
import { createStore, applyMiddleware, compose } from 'redux';
...
cons composeEnhancers = window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__ || compose;
const store = createStore(
    reducers,
    composeEnhancers(applyMiddleware())
);
{% endhighlight %}



<h2>Debug Sessions</h2>
`localhost:3000/?debug_session=logged_in`, where `logged_in` is my name of debug sessions. With this you can create checkpoints / states and then you can easily go back to that state. You can easily check actual state through `Redux Devtools Extension`.
