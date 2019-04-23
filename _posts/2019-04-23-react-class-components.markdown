---
layout: post
date:   2019-04-23 06:13:26 +0200
title:  "React: Class components"
category: react
tags: [react, class, components]
---

<b>functional components</b> - good for simple content <br />
<b>class components </b>- good for just about everything else<br /><br />

As an example, imagine we are waiting for some callback, which can take 3 seconds and only after it finish we want to display jsx / html. We are not able to do this with functional component.

<h2>Class components</h2>
- easier code organization <Br />
- can use `state` (easier to handle user input)<Br />
- understands lifecycle events (easier to do things when the app first starts) <Br />

<br />
- must be a javascript class<br />
- must extend React.Component<br />
- must define a `render` method that return some amount of JSX<br />

<h2>src/index.js</h2>
{% highlight javascript %}
{% raw %}
// Import the React and ReactDOM libraries
import React from 'react';
import ReactDOM from 'react-dom';

class App extends React.Component {
	render() {
		window.navigator.geolocation.getCurrentPosition(
				position => console.log(position),
				err => console.log(err)
		);

		return <div>Latitude: </div>;
	}
}

// Take the react component and show it on the screen
ReactDOM.render(
	<App />,
	document.querySelector('#root')
);

{% endraw %}
{% endhighlight %}
