---
layout: post
date:   2019-08-07 07:13:26 +0200
title:  "React and Redux: Cheat sheet"
category: javascript, react, redux
tags: [javascript, react, redux]
---

With key interpolation we can add new item to object.

<h2>React</h2>
<h3>Stateless component</h3>
{% highlight javascript %}
import React from 'react'
import ReactDOM from 'react-dom';

const MyComponent = () => {
    return (
        <div>
            test
        </div>
    );
}

export default MyComponent;
{% endhighlight %}



<br /><br />
<h3>Properties in stateless component</h3>
{% highlight javascript %}
const MyComponent = ({ propExample1, example2 }) => (
  <div>
    <h1>properties from parent component:</h1>
    <ul>
      <li>{propExample1}</li>
      <li>{example2}</li>
    </ul>
  </div>
)

// <MyComponent propExample1="aaa" example2="bbb" />
{% endhighlight %}


<br /><br />
<h3>Class component</h3>
{% highlight javascript %}
import React from 'react'
import ReactDOM from 'react-dom';

class MyComponent extends React.Component {
  render() {
    return (
        <div>
            test
        </div>
    );
  }
}

export default MyComponent;
{% endhighlight %}



<br /><br />
<h3>State</h3>
{% highlight javascript %}
class CountClicks extends React.Component {
  state = {
    clicks: 0
  }

  onButtonClick = () => {
    this.setState(prevState => ({
      clicks: prevState.clicks + 1
    }))
  }

  render() {
    return (
      <div>
        <button onClick={this.onButtonClick}>
          Click me
        </button>
        <span>
          The button clicked
          {this.state.clicks} times.
        </span>
      </div>
    )
  }
}
{% endhighlight %}

<br /><br />
<h2>React and Redux</h2>
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
  document.querySelector('#root')
);
{% endhighlight %}


<br /><br />
<h3>src/components/App.js</h3>
{% highlight javascript %}
import React from 'react';
import SongList from './SongList';

const App = () => {
	return (
        <div>
            <SongList />
        </div>;
    );
}

export default App;
{% endhighlight %}



<br /><br />
<h3>src/reducers/index.js</h3>
{% highlight javascript %}
import { combineReducers } from 'redux';

const songListReducer = () => {
	return [
		{
			title: 'song1',
			duration: '2:20'
		},
		{
			title: 'song2',
			duration: '3:20'
		}
	];
};

const selectedSongReducer = (song = null, action) => {
	if (action.type == 'SONG_SELECTED') {
		return action.payload;
	}
	return song;
}


export default combineReducers({
	songs: songListReducer,
	selectedSong: selectedSongReducer
});

{% endhighlight %}



<br /><br />
<h3>src/actions/index.js</h3>
{% highlight javascript %}
export const selectSong = (song) => {
    return {
        type: "SONG_SELECTED",
        payload: song
    }
}
{% endhighlight %}



<br /><br />
<h3>src/components/SongList.js</h3>
{% highlight javascript %}
import React from 'react';
import { connect } from 'react-redux';
import { selectSong } from '../actions';

class SongList extends React.Component {
    render() {
        return (
                this.props.songs.map((song) => {
                    return (
                        <div key={song.title}>
                            {song.title} - {song.duration}
                            <button
                                onClick={() => this.props.selectSong(song)}
                            >
                                SELECT
                            </button>
                        </div>
                    )
                })
        );

    }
}

const mapStateToProps = (state) => {
    return {
        songs: state.songs
    }
}

export default connect(mapStateToProps, {
    selectSong: selectSong
})(SongList);
{% endhighlight %}
