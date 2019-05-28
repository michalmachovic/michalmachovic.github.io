---
layout: post
date:   2019-05-27 07:13:26 +0200
title:  "Redux: Routers"
category: react
tags: [react, redux, router]
---

We will build app for video streaming. Install it with `npm install --save react-router-dom`.

![](http://michalmachovic.github.io/assets/2019-05-27-react-router-0.png)

![](http://michalmachovic.github.io/assets/2019-05-27-react-router-1.png)

![](http://michalmachovic.github.io/assets/2019-05-27-react-router-2.png)

![](http://michalmachovic.github.io/assets/2019-05-27-react-router-3.png)

![](http://michalmachovic.github.io/assets/2019-05-27-react-router-4.png)

![](http://michalmachovic.github.io/assets/2019-05-27-react-router-5.png)

![](http://michalmachovic.github.io/assets/2019-05-27-react-router-6.png)


<br /><br />
{% highlight javascript %}
client
|
---src
    |
    --actions
    |   |
    |   --index.js
    |
    --reducers
    |   |
    |   --index.js
    |
    |--components
            |
            --App.js
            --Header.js
            |
            --streams
                    |
                    --StreamList.js
                    --StreamCreate.js
                    --StreamEdit.js
                    --StreamDelete.js
                    --StreamShow.js
{% endhighlight %}

<br /><br />

<h3>index.html</h3>
After we put this inside `index.html` we should be able to write `gapi` into console.
{% highlight javascript %}
<script src="https://apis.google.com/js/api.js"></script>
{% endhighlight %}

<br /><br />
<h3>src/index.js</h3>
{% highlight javascript %}
import React from 'react';
import ReactDOM from 'react-dom';
import { Provider } fomr 'react-redux';
import { createStore } from 'redux';

import App from './components/App';
import reducers from './reducers';

const store = createStore(reducers);

ReactDOM.render(
    <Provider store={store}>
        <App/>
    </Provider>,
document.querySelector('#root')
)
{% endhighlight %}
<br /><br />


<h2>Components</h2>
<br /><br />
<h3>src/components/Header.js</h3>
{% highlight javascript %}
import React from 'react';
import { Link } from 'react-router-dom';
import GoogleAuth from './GoogleAuth';

const Header = () => {
    return (
        <div class="ui secondary poiting menu">
            <Link to="/">
             Streams
            </Link>

            <div class="right menu">
                <Link to="/" className="item">
                    All Streams
                </Link>
                <GoogleAuth />
            </div>
        </div>

    );
}
export default Header;
{% endhighlight %}
<br /><br />



<br /><br />
<h3>src/components/GoogleAuth.js</h3>
{% highlight javascript %}
import React from 'react';
import { connect } from 'react-redux';
import { signIn, singOut } from '../actions';

class GoogleAuth extends React.Component {

    componentDidMount() {
        window.gapi.load('client:auth2', () => {
            window.gapi.client.init({
                clientId: OUR_CLIENT_ID,
                scope: 'email'
            }).then(() => {
                this.auth = window.gapi.auth2.getAuthInstance();
                this.onAuthChange(this.auth.isSignedIn.get());
                this.auth.isSignedIn.listen(this.onAuthChange);
            });
        });
    }

    onAuthChange = (isSignedIn) => {
        if (isSignedIn) {
            this.props.signIn(this.auth.currentUser.get().getId());
        } else {
            this.props.signOut();
        }
    };

    onSignInClick = () => {
        this.auth.signIn();
    }

    onSignOutClick = () => {
        this.auth.signOut();
    }

    renderAuthButton() {
        if (this.props.isSignedIn === null) {
            return null;
        } else if (this.props.isSignedIn) {
            return (
                <button onclick={this.onSignOutClick} className="ui red google button">
                    <i className="google icon" />
                    Sign out
                </button>
            );
        } else {
            return (
                <button onclick={this.onSignInClick} className="ui red google button">
                    <i className="google icon" />
                    Sign in with Google
                </button>
            );
        }
    }

    render() {
        return <div>{this.renderAuthButton()}</div>;
    }
}

const mapStateToProps = (state) => {
    return { isSignedIn: state.auth.isSignedIn }
};

export default connect(mapStateToProps, { signIn, signOut })(GoogleAuth);
{% endhighlight %}
<br /><br />




<h3>src/components/App.js</h3>
{% highlight javascript %}
import React from 'react';
import { BrowserRouter, Route } from 'react-router-dom';
import StreamCreate from './streams/StreamCreate';
import StreamEdit from './streams/StreamEdit';
import StreamDelete from './streams/StreamDelete';
import StreamList from './streams/StreamList';
import StreamShow from './streams/StreamShow';
import Header from './Header';

const App = () => {
    return (
        <div className="ui container">
            <BrowserRouter>
                <div>
                    <Header />
                    <Route path="/" exact component={StreamList} />
                    <Route path="/streams/new" exact component={StreamCreate} />
                    <Route path="/streams/edit" exact component={StreamEdit} />
                    <Route path="/streams/delete" exact component={StreamDelete} />
                    <Route path="/streams/show" exact component={StreamShow} />
                </div>
            </BrowserRouter>
        </div>
    );
};

export default App;
{% endhighlight %}




<br /><br />
<h3>src/components/streams/StreamList.js</h3>
{% highlight javascript %}

{% endhighlight %}


<br /><br />
<h3>src/components/streams/StreamCreate.js</h3>
{% highlight javascript %}
import React from 'react';

const StreamCreate = () => {
    return <div>StreamCreate</div>;
};

export default StreamCreate;
{% endhighlight %}


<br /><br />
<h3>src/components/streams/StreamDelete.js</h3>
{% highlight javascript %}
import React from 'react';

const StreamDelete = () => {
    return <div>StreamDelete</div>;
};

export default StreamDelete;
{% endhighlight %}

<br /><br />
<h3>src/components/streams/StreamEdit.js</h3>
{% highlight javascript %}
import React from 'react';

const StreamEdit = () => {
    return <div>StreamEdit</div>;
};

export default StreamEdit;
{% endhighlight %}


<br /><br />
<h3>src/components/streams/StreamShow.js</h3>
{% highlight javascript %}
import React from 'react';

const StreamShow = () => {
    return <div>StreamShow</div>;
};

export default StreamShow;
{% endhighlight %}

<br /><br />

<h2>Actions</h2>
<h3>src/actions/types.js</h3>
{% highlight javascript %}
export const SIGN_IN = 'SIGN_IN';
export const SIGN_OUT = 'SIGN_OUT';
{% endhighlight %}


<h3>src/actions/index.js</h3>
{% highlight javascript %}
import { SIGN_IN, SIGN_OUT } from './types';

export const signIn = (userId) => {
    return {
        type: SIGN_IN,
        payload: userId
    };
};

export const signOut = () => {
    return {
        type: SIGN_OUT
    };
};
{% endhighlight %}




<br /><br />

<h2>Reducers</h2>
<h3>src/reducers/index.js</h3>
{% highlight javascript %}
import { combineReducers } from 'redux';
import authReducer from './authReducer';

export default combineReducers({
  auth: authReducer
});
{% endhighlight %}

<br /><br />
<h3>src/reducers/authReducer.js</h3>
{% highlight javascript %}
import { SIGN_IN, SIGN_OUT } from './types';

const INITIAL_STATE = {
    isSignedIn: null,
    userId: null
}

export default (state = INITIAL_STATE, action) => {
    switch (action.type) {
        case SIGN_IN:
            return { ...state, isSignedIn: true, userId: action.payload };

        case SIGN_OUT:
            return { ...state, isSignedIn: false, userId: null };
        default:
            return state;
    }
}
{% endhighlight %}
