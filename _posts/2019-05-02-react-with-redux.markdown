---
layout: post
date:   2019-05-01 07:13:26 +0200
title:  "React with Redux"
category: react
tags: [react, redux]
---

Go into your `React` project app folder and install following: <br />
`npm install --save redux react-redux`

<br /><br />
We will create app called Songs.
![useful image](http://michalmachovic.github.io/assets/2019-05-02-react-with-redux-1.png)

<br /><br />
To connect React with Redux, we will create two new components, `Provider` and `Connect`. `Connect` can communicate with `Provider`, which is at the top of hierarchy through `contacts` system. `Contacts` system allow parent component to communicate with any child component, even there is another component between them.

![useful image](http://michalmachovic.github.io/assets/2019-05-02-react-with-redux-2.png)
<br /><br />

<h2>Structure</h2>
![useful image](http://michalmachovic.github.io/assets/2019-05-02-react-with-redux-3.png)

<h3>src/actions/index.js</h3>
We called this file index.js because when we then import it, we dont need to enter full path, we can specify only directory, it will automatically look for index.js.
{% highlight javascript %}
//Action creator
export const selectSong = (song) => {
    //Return an action
    return {
        type: 'SONG_SELECTED',
        payload: song
    };
};
{% endhighlight %}

<br /><br />
<h3>src/reducers/index.js</h3>
{% highlight javascript %}
import { combineReducers } from 'redux';

const songsReducer = () => {
    return [
        {
            title: 'No Scrubs',
            duration: '4:05'
        },
        {
            title: 'Macarena',
            duration: '2:30'
        },
        {
            title: 'All Star',
            duration: '3:15'
        },
        {
            title: 'I want it that way',
            duration: '1:45'
        }
    ];
};

const selectedSongReducer = (selectedSong = null, action) => {
    if (action.type === 'SONG_SELECTED') {
        return action.payload;
    }
    return selectedSong;
}

export default combineReducers({
    songs: songsReducer,
    selectedSong: selectedSongReducer
});

{% endhighlight %}



<br /><br />
<h3>src/components/SongList.js</h3>
{% highlight javascript %}
import React, { Component } from 'react';
import { connect } from 'react-redux';
import { selectSong } from '../actions';

class SongList extends Component {
    renderList() {
        return this.props.songs.map((song) => {
            return (
                <div className="item" key={song.title}>
                    <div className="right floated content">
                        <button
                            className="ui button primary"
                            onClick="{() => this.props.selectSong(song)}"
                        >
                            Select
                        </button>
                    </div>

                    <div className="content">{song.title}</div>
                </div>
            );
        });
    }

    render() {
        return <div className="ui divided list">{this.renderList()}</div>;
    }
}

//here we are getting data from redux store and sending it to react component, there will be available as this.props
const mapStateToProps = (state) => {
    return {
        songs: state.songs
    };
}

//we are passing selectSong as second argument
//connect function will take selectSong action creator and pass it to our component as prop
//connect function is calling dispatch behind the scenes, each time state is updated
export default connect(mapStateToProps, {
     selectSong: selectSong
})(SongList);
{% endhighlight %}




<br /><br />
<h3>src/components/SongDetail.js</h3>
{% highlight javascript %}
import React from 'react';
import { connect } from 'react-redux';

const SongDetail = (props) => {
    if (!props.song) {
        return <div>Select a song</div>
    }

    return (
        <div>
            {props.song.title}
            {props.song.duration}
        </div>
    );
}

const mapStateToProps = (state) => {
    return { song: state.selectedSong }
}

export default connect(mapStateToProps)(SongDetail);
{% endhighlight %}





<br /><br />
<h3>src/components/App.js</h3>
{% highlight javascript %}
import React from 'react';
import SongList from './SongList';
import SongDetail from './SongDetail';

const App = () => {
    return (
        <div>
            <SongList />
        </div>

        <div>
            <SongDetail />
        </div>
    );
};

export default App;
{% endhighlight %}





<br /><br />
<h3>src/index.js</h3>
{% highlight javascript %}
import React from 'react';
import ReactDOM from 'react-dom';
import { Provider } from 'react-redux';
import { createStore } from 'redux';

import App from './componentes/App';
import reducers from './reducers';

ReactDOM.render(
    <Provider store={createStore(reducers)}>
        <App />
    </Provider>,
    document.querySelector('#root'));
{% endhighlight %}


<br /><br /><br /><br />
Při práci s reduxem potkáme tři základní konstrukce: `store`, `akci` a `reducer`.
<br />

<h3>Store</h3>
Store je objekt ve kterém jsou uložena naše data. Store poskytuje tyto 3 základní metody, v jednoduchosti je síla.
{% highlight javascript %}
store.getState() // vrací naše data (state)
store.subscribe(callback) // pokud chceme zjistit že se data změnila
store.dispatch(akce) // provádíme akci, která změní data ve Store uložená
{% endhighlight %}

<br /><br />
<h3>Akce</h3>
Pokud chceme změnit data ve store, popíšeme tuto změnu pomocí jednoduchého objektu zvaného akce. Objekt akce má jediný povinný atribut jménem `type`, ten slouží pro identifikaci. Další atributy vyplníme libovolně dle potřeby. Jinak řečeno: akce musí být jednoznačně identifikovatelná a musí obsahovat všechna data nutná k jejímu provedení.
{% highlight javascript %}
{
    type: "ADD_ITEM",
    text: "Nějaký úkol"
}
{% endhighlight %}

<br /><br />
<h3>Reducer</h3>
Posledním dílem skládačky je reducer. Funkce kterou napíšeme a vložíme do store, aby bezpečně modifikovala data podle požadavků vyjádřených akcí. Reducer tedy čeká uvnitř store až zavoláme dispatch(akce) a jakmile se tak stane, store zavolá reducer a předá mu:
 - state – současná data aplikace
 - action – celý objekt akce tak, jak jsme jej vložili do volání dispatch() (akce obsahuje identifikaci ‘type’ a jakákoliv další data potřebná k provedení)
