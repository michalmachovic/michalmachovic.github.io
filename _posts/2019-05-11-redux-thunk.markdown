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
![](http://michalmachovic.github.io/assets/2019-05-11-redux-thunk-1.png)

<br /><br />
![](http://michalmachovic.github.io/assets/2019-05-11-redux-thunk-2.png)

<br /><br />
![](http://michalmachovic.github.io/assets/2019-05-11-redux-thunk-3.png)

<br /><br />
![](http://michalmachovic.github.io/assets/2019-05-11-redux-thunk-4.png)

<br /><br />
![](http://michalmachovic.github.io/assets/2019-05-11-redux-thunk-5.png)

<br /><br />
![](http://michalmachovic.github.io/assets/2019-05-11-redux-thunk-6.png)


<br /><br />
![](http://michalmachovic.github.io/assets/2019-05-11-redux-thunk-7.png)

<br /><br />
<h3>src/apis/jsonPlaceholder.js</h3>
{% highlight javascript %}
import axios from 'axios';

export default axios.create({
    baseURL: 'https://jsonplaceholder.typicode.com'
});
{% endhighlight %}




<br /><br />
<h3>src/index.js</h3>
{% highlight javascript %}
import React from 'react';
import ReactDOM from 'react-dom';
import { Provider } from 'react-redux';
import { createStore, applyMiddleware } from 'redux';
import thunk from 'redux-thunk';

import App from './components/App';
import reducers from './reducers';

const store = createStore(reducers, applyMiddleware(thunk));

ReactDOM.render(
  <Provider store={store}>
  	<App />
  </Provider>,
  document.querySelector('#root	')
);

{% endhighlight %}




<br /><br />
<h3>src/actions/index.js</h3>
{% highlight javascript %}
import jsonPlaceholder from '../apis/jsonPlaceholder';

/*
//this will not work because we want to use async await
//this code is transpiled by babel to es2015
//and the it doesnt return plain object as it is required in redux actions
//you can check at babeljs.io
export const fetchPosts = async () => {
    const response = await jsonPlaceholder.get('/posts');
    return (
        type: "FETCH_POSTS",
        payload: response
    );
}
*/

//we will use redux-thunk
export const fetchPosts = async () => {
    return async (dispatch) => {
        const response = await jsonPlaceholder.get('/posts');

        dispatch({ type: 'FETCH_POSTS', payload: response.data});
    };
};

//this is the same like export above, just shorter syntax
export const fetchUser = id => async dispatch => {
  const response = await jsonPlaceholder.get(`/users/{$id}` );

  dispatch({ type: 'FETCH_USER', payload: response.data });
}
{% endhighlight %}


<br /><br />
<h3>src/components/App.js</h3>
{% highlight javascript %}
import React from 'react';
import PostList from './PostList';

const App = () => {
	return (
        <div className="ui container">
            <PostList />
        </div>
    );
}

export default App;
{% endhighlight %}


<br /><br />
<h3>src/components/PostList.js</h3>
{% highlight javascript %}
import React from 'react';
import { connect } from 'react-redux';
import { fetchPosts } from '../actions';
import UserHeader from './UserHeader';

class PostList extends React.Component {
    componentDidMoun t() {
        this.props.fetchPosts();
    }

    renderList() {
      return this.props.posts.map(post => {
        return (
            <div className="item" key={post.id}>
                <div className="content">
                    <div className="description">
                        <h2>{post.title}</h2>
                        <p>{post.body}</p>
                    </div>
                    <UserHeader userId={post.userId} />
                </div>
            </div>
        );
      });
    }

    render() {
        return <div className="ui relaxed divided">{this.renderList()}</div>;
    }
}

const mapStateToProps = (state) => {
    return {
        posts: state.posts
    };
}

//there was null at place of mapStateToProps, because we didnt have mapStateToProps function before
export default connect(mapStateToProps, { fetchPosts })(PostList);
{% endhighlight %}



br /><br />
<h3>src/components/UserHeader.js</h3>
{% highlight javascript %}
import React from 'react';
import { connect } from 'react-redux';
import { fetchUser } from '../actions';

class UserHeader extends React.Component {
    componentDidMount() {
        this.props.fetchUser(this.props.userId);
    }

    render() {
        const { user } = this.props;
        if (!user) {
          return null;
        }

        return <div className="header">{user.name}</div>;
    }
}

const mapStateToProps = (state, ownProps) => {
    return { user: state.users.find(user => user.id === ownProps.userId ) }
}
//there was null at place of mapStateToProps, because we didnt have mapStateToProps function before
export default connect(mapStateToProps, {fetchUser})(UserHeader);
{% endhighlight %}


<br /><br />
<h3>src/reducers/postsReducer.js</h3>
{% highlight javascript %}
export default (state = [], action) => {
    switch (action.type) {
        case 'FETCH_POSTS':
            return action.payload;
        default:
            return state;
    }
}
{% endhighlight %}



<br /><br />
<h3>src/reducers/usersReducer.js</h3>
{% highlight javascript %}
export default (state = [], action) => {
    switch (action.type) {
        case 'FETCH_USER':
            return [...state, action.payload];
        default:
            return state;
    }
}
{% endhighlight %}



<br /><br />
<h3>src/reducers/index.js</h3>
{% highlight javascript %}
import { combineReducers } from 'redux';
import postReducer from './postsReducer';
import usersReducer from './usersReducer';


export default combineReducers({
	posts: postsReducer,
  users: usersReducer
});
{% endhighlight %}
