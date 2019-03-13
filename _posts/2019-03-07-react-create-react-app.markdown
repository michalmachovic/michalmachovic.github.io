---
layout: post
date:   2019-03-07 06:13:26 +0200
title:  "React: Create React App"
category: react
tags: [react,create]
---


<h2>Create new app</h2>
{% highlight php %}
npm install -g create-react-app
{% endhighlight %}



<h2>Run app</h2>
{% highlight php %}
npm start
{% endhighlight %}
App will run at `localhost:3000`.


<h2>Possible errors</h2>
<b>Port is in use</b> - open new tab in terminal and run `npm start` again. You can then view your app on `localhost:3001`
<br />
`localhost:8080 doesnt't work` - go back to terminal and search for `On your network` and visit that url.

<br /><br />


<h2>index.js</h2>
{% highlight javascript %}
// Import the React and ReactDOM libraries
import React from 'react';
import ReactDOM from 'react-dom';

// Create a react component
const App = () => {
	return <div>Hi there!</div>;
}

// Take the react component and show it on the screen
ReactDOM.render(
	<App />,
	document.querySelector('#root')
);
{% endhighlight %}

<br /><br />
Div `root` is available in `index.html`.