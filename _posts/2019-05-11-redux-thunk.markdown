---
layout: post
date:   2019-05-11 07:13:26 +0200
title:  "Redux: Thunk"
category: react
tags: [react, redux, thunk]
---

We are going to do simple blog app. We will fetch data from `jsonplaceholder.typicode.com`.

<h2>What is Redux Thunk ? </h2>
- redux = redux library <br />
- react-redux = integration layer between react and redux <br />
- axios = help us to make network requests <br />
- redux-thunk = middleware to help us make requests in a redux applications <br />

<br /><br />
<h3>src/index.js</h3>
{% highlight javascript %}
import React from 'react';
import ReactDOM from 'react-dom';
import { Provider } from 'react-redux';
import { createStore } from 'redux';

import App from './components/App';
import reducers from './reducers';


ReactDOM.render(
  <Provider store={createStore(reducers)}>
  	<App />
  </Provider>,
  document.querySelector('#root	')	
);

{% endhighlight %}



<br /><br />
<h3>src/components/App.js</h3>
{% highlight javascript %}
import React from 'react';

const App = () => {
	return <div className="ui container">App</div>;
}

export default App;
{% endhighlight %}


<br /><br />
<h3>src/reducers/index.js</h3>
{% highlight javascript %}
import { combineReducers } from 'redux';


export default combineReducers({
	test: () => 'test'
});
{% endhighlight %}