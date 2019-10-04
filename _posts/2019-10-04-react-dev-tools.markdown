---
layout: post
date:   2019-10-04 07:13:26 +0200
title:  "React and Redux: Dev Tools"
category: javascript, react, redux
tags: [javascript, react, redux]
---

Download dev tools from `https://github.com/zalmoxisus/redux-devtools-extension`. Then you can it to your project by following part `1.2 Advanced store setup`, example below:

<h3>src/index.js</h3>
{% highlight javascript %}
import React from 'react';
import ReactDOM from 'react-dom';
import { Provider } from 'react-redux';
import { createStore, applyMiddleware, compose } from 'redux';

import App from './components/App';
import reducers from './reducers';

const composeEnhancers = window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__ || compose;
const store = createStore(
    reducers,
    composeEnhancers(applyMiddleware())
);

ReactDOM.render(
    <Provider store={store}>
  	 <App />
    </Provider>,
  document.querySelector('#root')
);

{% endhighlight %}


<br /><br />
Normally you loose all redux store data if you refresh page. If you run your app with `localhost:3000/?debug_session=SOME_STRING`, Redux Dev Tools will hold all your store data also after refresh.
