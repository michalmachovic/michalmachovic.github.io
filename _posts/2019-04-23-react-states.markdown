---
layout: post
date:   2019-04-23 07:13:26 +0200
title:  "React: States and Lifecycle methods"
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

		this.state = { lat: null, errorMessage: '' };   // initialize state of lat, we dont know value of lat yet, so it is null

		window.navigator.geolocation.getCurrentPosition(
			position => {
				this.setState({ lat: position.cords.latitude })
			},
			err => {
				this.setState({ errorMessage: err.message });
			}
		);
	}

	render() {
		if (this.state.errorMessage && !this.state.lat) {
			return (
				<div>Error: {this.state.errorMessage}</div>
			);
		}

		if (!this.state.errorMessage && this.state.lat) {
			return (
				<div>Latitude: {this.state.lat}</div>
			);
		}

		return <div>Loading!</div>

	}
}

// Take the react component and show it on the screen
ReactDOM.render(
	<App />,
	document.querySelector('#root')
);


{% endraw %}
{% endhighlight %}

<br /><br /><br />

<h2>Lifecycle methods</h2>
<b>constructor</b> - good place to do one-time setup <br />
<b>render</b> - avoid doing anything besides returning JSX<br />
<b>componentDidMount</b> - good place to data-loading<br />
<b>componenDidUpdate</b> - good place to do more data-loading when state/props change<br />
<b>componentWillUnmount</b> - good place to do cleanup (especially for non-React stuff<br />
	<br /><br />
See that we are not using `contructor` in following code. The result is the same, as code is transformed by Babel, which will add `constructor` automatically.

<h2>src/index.js</h2>
{% highlight javascript %}
{% raw %}
/ Import the React and ReactDOM libraries
import React from 'react';
import ReactDOM from 'react-dom';
import SeasonDisplay from './SeasonDisplay';

class App extends React.Component {
	state = { lat: null, errorMessage: '' };

	componentDidMount() {
		window.navigator.geolocation.getCurrentPosition(
			position => {
				this.setState({ lat: position.cords.latitude });
			},
			err => {
				this.setState({ errorMessage: err.message });
			}
		);
	}

	componentDidUpdate() {

	}

	render() {
		if (this.state.errorMessage && !this.state.lat) {
			return (
				<div>Error: {this.state.errorMessage}</div>
			);
		}

		if (!this.state.errorMessage && this.state.lat) {
			return (
				<SeasonDisplay lat={this.state.lat} />
			);
		}

		return <div>Loading!</div>

	}
}

// Take the react component and show it on the screen
ReactDOM.render(
	<App />,
	document.querySelector('#root')
);

{% endraw %}
{% endhighlight %}
<br /><br />

<h2>src/SeasonDisplay.js</h2>
{% highlight javascript %}
{% raw %}
import './SeasonDisplay.css';
import React from 'react';

const getSeason = (lat, month) => {
	if (month > 2 && month < 9) {
		return lat > 0 ? 'summer' : 'winter';
	} else {
		return lat > 0 ? 'winter' : 'summer';
 	}
}

const SeasonDisplay = (props) => {
	const season = getSeason(props.lat, new Date().getMonth());
	const text = season === 'winter' ? 'Burr, it is chilly' : 'Lets hit the beach';
	return (
		<div>
			{text}
		</div>
	);
};

export default SeasonDisplay;
{% endraw %}
{% endhighlight %}
