---
layout: post
date:   2019-04-23 07:13:26 +0200
title:  "React: Event Handlers"
category: react
tags: [react, event]
---

We will build simple app which will have SearchBar. You can enter there some term and based on that, images will be loaded from external api.
To fetch data we can use built-in `fetch` function (but we need to write more code) or use 3rd party `axios` (with less code).
`npm install --save axios`

<br /><br />
{% highlight javascript %}
src/index.js
src/components/App.js
src/components/SearchBar.js
src/components/ImageList.js
src/api/unsplash.js //updated version
{% endhighlight %}

<br /><br />

<h2>src/components/App.js</h2>
{% highlight javascript %}
{% raw %}
import React from 'react';
import SearchBar from './SearchBar';
import ImageList from './ImageList';
import axios from 'axios';

class App extends React.Component {
	state = { images: [] };

	onSearchSubmit = async (term) => {
		const response = await axios.get('https://api.unsplash.com/search/photos',{
			params: { query: term },
			headers: {
				Authorization: 'Client-Id CLIENTID'
			}
		});

		this.setState({ images: response.data.results });
	}

	/*
	//ALTERNATIVE SYNTAX WITH then
	onSearchSubmit(term) {
		axios.get('https://api.unsplash.com/search/photos',{
			params: { query: term },
			header: {
				Authorization: 'Client-Id CLIENTID'
			}
		}).then((response) => {
			console.log(response.data.results);
		});
	}
	*/

	render() {
		return (
			<div style={{ marginTop: '10px'}}>
				<SearchBar onSubmit={this.onSearchSubmit} />
				<ImageList images={this.state.images} />
			</div>
		);
	}
}

export default App;
{% endraw %}
{% endhighlight %}

<br /><br />


<h2>src/components/SearchBar.js</h2>
{% highlight javascript %}
{% raw %}
import React from 'react';

class SearchBar extends React.Component {
	state: {
		term: ''
	};

	/*
	//onFormSubmit(event) is shortcut for onFormSubmit: function(event)
	//THIS WILL LEAD TO ERROR. this IS UNDEFINED

	onFormSubmit(event) {
		event.preventDefault();
		console.log(this.state.term);
	}
	*/

	//arrow function will make sure that value of 'this' is always equal to our instance of SearchBar
	onFormSubmit = (event) => {
		event.preventDefault();
		this.props.onSubmit(this.state.term);
	}


	return(
		<div>
			<form onSubmit={this.onFormSubmit}>
		
				// or we can do <form onSubmit={event => this.onFormSubmit(event)}>
				// if we want to use first version of onFormSubmit above, without arrow function
			
				<label>Image Search</label>
				<input
					type="text"
					onChange={(e) => this.setSate({ term: e.target.value })}
					value={this.state.term}
				/>
			</form>
		</div>
	);
};

export default SearchBar;
{% endraw %}
{% endhighlight %}

<br /><br />




<h2>src/components/ImageList.js</h2>
See each image has `key` property. React recommend each list of components to have this property.
{% highlight javascript %}
{% raw %}
import React from 'react';

const ImageList = (props) => {
	const images = props.images.map((image) => {
		return <img key={image.id} alt={image.description} src="{image.urls.regular}" />
	});

	return(
		<div>
			{images}
		</div>
	);
};

export default ImageList;
{% endraw %}
{% endhighlight %}

<br /><br />




<h2>src/index.js</h2>
{% highlight javascript %}
{% raw %}
/ Import the React and ReactDOM libraries
import React from 'react';
import ReactDOM from 'react-dom';
inmport App from './components/App';


// Take the react component and show it on the screen
ReactDOM.render(
	<App />,
	document.querySelector('#root')
);


{% endraw %}
{% endhighlight %}

<br /><br /><br />


<h2>UPDATE</h2>
In `onSearchSubmit` we are now loading `unsplash` api with `axios`. We can put this configuration away from our `App` component into `api/unsplash.js`, so code will be cleaner.

<h2>src/api/unsplash.js</h2>
{% highlight javascript %}
{% raw %}
import axios from 'axios';

export default axios.create({
	baseURL: 'https://api.unsplash.com',
	headers: {
		Authorization: 'Client-Id CLIENTID'
	}	
});

{% endraw %}
{% endhighlight %}

<br /><br />

<h2>src/components/App.js</h2>
{% highlight javascript %}
{% raw %}
import React from 'react';
import SearchBar from './SearchBar';
import unsplash from '../api/unsplash';

class App extends React.Component {
	state = { images: [] };

	onSearchSubmit = async (term) => {
		const response = await unsplash.get('/search/photos',{
			params: { query: term },
		});

		this.setState({ images: response.data.results });
	}

	
	render() {
		return (
			<div style={{ marginTop: '10px'}}>
				<SearchBar onSubmit={this.onSearchSubmit} />
			</div>
		);
	}
}

export default App;
{% endraw %}
{% endhighlight %}

<br /><br />




