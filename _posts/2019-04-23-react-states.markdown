---
layout: post
date:   2019-04-23 07:13:26 +0200
title:  "React: States"
category: react
tags: [react, class, components]
---

<h2>States</h2>
- only usable with class components <br />
- can be confused with Properties <br />
- state is a JS object that contains data relevant to a components <br />
- updating `state` on a component causes the component to (almost) instantly rerender <br />
- state must be initialized when a component is created <br />
- state can <b>only</b> be updated using the function `setState`


<h2>src/index.js</h2>
{% highlight javascript %}
{% raw %}
/ Import the React and ReactDOM libraries
import React from 'react';
import ReactDOM from 'react-dom';

class App extends React.Component {
	constructor(props) {
		super(props);     //we must call constructor of React.Component

		this.state = { lat: null };   // initialize state of lat, we dont know value of lat yet, so it is null

		window.navigator.geolocation.getCurrentPosition(
			position => {
				this.setState({ lat: position.cords.latitude })
			},
			err => console.log(err)
		);
	}

	render() {
		return <div>Latitude: {this.state.lat}</div>;
	}
}

// Take the react component and show it on the screen
ReactDOM.render(
	<App />,
	document.querySelector('#root')
);


{% endraw %}
{% endhighlight %}
