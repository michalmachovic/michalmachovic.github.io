---
layout: post
date:   2019-10-02 07:13:26 +0200
title:  "React: Google Auth"
category: javascript, react, redux
tags: [javascript, react, redux, google]
---

<h2>Google Authentication with React</h2>
You need to add `<script src="https://apis.google.com/js/api.js"></script>` to `index.htm`.
Then you need to create Oauth Credentials at `http://console.developers.google.com`. Click on projects dropdown in the top, then click on `New Project`.
Enter project name and click create. After few seconds you should see your project in projects dropdown. Select your new project and choose `Credentials -> Oauth consent screen` and enter just application name.
After that go to `Credentials -> Credentials -> Create Credentials -> OAuth Client Id -> Web Application`. Add `http://localhost:3000` to `Authorized JavaScript Origins` and click `Create`. You should then see your `client ID`.

<h3>src/components/GoogleAuth.js</h3>
{% highlight javascript %}
import React from 'react';

class GoogleAuth extends React.Component {
    state = { isSignedIn: null};

    componentDidMount() {
        window.gapi.load('client:auth2', () => {
            window.gapi.client.init({
                clientId: YOUR_CLIENT_ID,
                scope: 'email'
            }).then(() => {
                this.auth = window.gapi.auth2.getAuthInstance();
                this.setState({ isSignedIn: this.auth.isSignedIn.get() });
                this.auth.isSignedIn.listen(this.onAuthChange);
            });
        });
    }

    onAuthChange = () => {
        this.setState({ isSignedIn: this.auth.isSignedIn.get() });
    };

    onSignInClick = () => {
        this.auth.signIn();
    }

    onSignOutClick = () => {
        this.auth.signOut();
    }


    renderAuthButton() {
        if (this.state.isSignedIn === null) {
            return null;
        } else if (this.state.isSignedIn) {
            return (
                <button onClick={this.onSignOutClick} className="ui red google button">
                    <i className="google icon" />
                    Sign Out
                </button>
            )
        } else {
            return (
                <button onClick={this.onSignInClick} className="ui red google button">
                    <i className="google icon" />
                    Sign In
                </button>
            )
        }
    }

    render() {
        return (
            <div>{this.renderAuthButton()}</div>
        );
    }
}

export default GoogleAuth;

{% endhighlight %}


<br /><br />

<h2>Google Authentication with React and Redux</h2>
Without Redux, authentication state is available only in `GoogleAuth` component, so for other components can be complicated to reach it. We will update it to use Redux, so it will be available in `props`. We will also catch user google id, so we can then identify which use is currently logged in.

<br /><br />
![](http://michalmachovic.github.io/assets/2019-10-02-react-google-auth.png)
<br /><br />

<h3>scr/actions/types.js</h3>
{% highlight javascript %}
export const SIGN_IN = 'SIGN_IN';
export const SIGN_OUT = 'SIGN_OUT';
{% endhighlight %}

<br /><br/ >


<h3>scr/actions/index.js</h3>
{% highlight javascript %}
import { SIGN_IN, SIGN_OUT } from './types';

export const signIn = (userId) => {
    return {
        type: SIGN_IN,
        payload: userId
    }
}

export const signOut = () => {
    return {
        type: SIGN_OUT
    }
}
{% endhighlight %}

<br /><br/ >




<h3>scr/reducers/authReducer.js</h3>
{% highlight javascript %}
import { SIGN_IN, SIGN_OUT } from '../actions/types';

const INITIAL_STATE = {
    isSignedIn: null,
    userId: null
};

export default(state = INITIAL_STATE, action) => {
    switch (action.type) {
        case SIGN_IN:
            return { ...state, isSignedIn: true, userId: action.payload };
        case SIGN_OUT:
            return { ...state, isSignedIn: false, userId: null };
        default:
            return state;
    }
};
{% endhighlight %}

<br /><br/ >



<h3>scr/reducers/index.js</h3>
{% highlight javascript %}
import { combineReducers } from 'redux';
import authReducer from './authReducer';

export default combineReducers({
    auth: authReducer
});

{% endhighlight %}

<br /><br/ >



<h3>src/components/GoogleAuth.js</h3>
{% highlight javascript %}
import React from 'react';
import { connect } from 'react-redux';
import { signIn, signOut } from '../actions';

class GoogleAuth extends React.Component {

    componentDidMount() {
        window.gapi.load('client:auth2', () => {
            window.gapi.client.init({
                clientId: '80950206281-dgiak56g74ad0bp6mnvptknrcprmloc4.apps.googleusercontent.com',
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

    onSignIn = () => {
        this.auth.signIn();
    }

    onSignOut = () => {
        this.auth.signOut();
    }


    renderAuthButton() {
        if (this.props.isSignedIn === null) {
            return null;
        } else if (this.props.isSignedIn) {
            return (
                <button onClick={this.onSignOut} className="ui red google button">
                    <i className="google icon" />
                    Sign Out
                </button>
            )
        } else {
            return (
                <button onClick={this.onSignIn} className="ui red google button">
                    <i className="google icon" />
                    Sign In
                </button>
            )
        }
    }

    render() {
        return (
            <div>{this.renderAuthButton()}</div>
        );
    }
}

const mapStateToProps = (state) => {
    return {
        isSignedIn: state.auth.isSignedIn
    };
}

export default connect(
    mapStateToProps,
    { signIn, signOut })
(GoogleAuth);


{% endhighlight %}
<br /><br />



<h3>scr/index.js</h3>
{% highlight javascript %}
import React from 'react';
import ReactDOM from 'react-dom';
import { Provider } from 'react-redux';
import { createStore } from 'redux';

import App from './components/App';
import reducers from './reducers';

const store = createStore(reducers);

ReactDOM.render(
    <Provider store={store}>
  	 <App />
    </Provider>,
  document.querySelector('#root')
);

{% endhighlight %}

<br /><br/ >
