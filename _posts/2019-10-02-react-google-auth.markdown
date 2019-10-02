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
