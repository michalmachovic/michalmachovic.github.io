---
layout: post
date:   2019-05-27 07:13:26 +0200
title:  "Redux: Routers and Forms"
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
<h2>Structure</h2>
{% highlight javascript %}
streams
|
|---api
|
|
|---client
    |
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
<h2>JSON API SERVER</h2>
First we will do API server, which will use REST-ful conventions (standardized system for designing APIs). We will be then able to do requests to this server to create new stream, etc ...
So REST conventions is refering to standardized system of routes and request methods used to commit or operate different actions.
We will use `https://npmjs.com/package/json-server` for this.
<br /><br />
![](http://michalmachovic.github.io/assets/2019-05-27-react-router-9.png)
<br /><br />
Under `api` directory we will run following commands:
{% highlight javascript %}
npm init
npm install --save json-server
{% endhighlight %}

<br /><br />
In docuementation of `JSON server` we can find, that objects will be stored in `db.json` file. So in our case, we will store `streams`.
<h3>api/db.json</h3>
{% highlight javascript %}
{
    "streams": []
}
{% endhighlight %}

<br /><br />
<h3>api/package.json</h3>
{% highlight javascript %}
{
    ...
    "scripts": {
        "start": "json-server -p 3001 -w db.json"    //p is port, w means it is watching db.json for any changes
    }
    ...
}
{% endhighlight %}

<br /><br />
In `api` dir run `npm start`, that will start `JSON server`. We can use it now to manipulate our streams following REST convetions.
<br /><br />


<h2>REACT APP</h2>
<h3>client/index.html</h3>
After we put this inside `index.html` we should be able to write `gapi` into console.
{% highlight javascript %}
<script src="https://apis.google.com/js/api.js"></script>
{% endhighlight %}

<br /><br />
<h3>client/src/index.js</h3>
{% highlight javascript %}
import React from 'react';
import ReactDOM from 'react-dom';
import { Provider } fomr 'react-redux';
import { createStore, applyMiddleware, compose } from 'redux';
import reduxThunk from 'redux-thunk';

import App from './components/App';
import reducers from './reducers';

cosnt composeEnhancers = window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__ || compose;
const store = createStore(
    reducers,
    composeEnhancers(applyMiddleware(reduxThunk))
);

ReactDOM.render(
    <Provider store={store}>
        <App/>
    </Provider>,
document.querySelector('#root')
)
{% endhighlight %}
<br /><br />


<h2>Apis</h2>
<h3>client/src/apis/streams.js</h3>
{% highlight javascript %}
import axios from 'axios';

export default axios.create({
    baseURL: 'http://localhost:3001'
});
{% endhighlight %}


<h2>Components</h2>
<h3>client/src/components/Header.js</h3>
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
<h3>client/src/components/GoogleAuth.js</h3>
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




<h3>client/src/components/App.js</h3>
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
<h3>client/src/components/streams/StreamList.js</h3>
{% highlight javascript %}

{% endhighlight %}


<br /><br />
<h3>client/src/components/streams/StreamCreate.js</h3>
{% highlight javascript %}
import React from 'react';
import { Field, reduxForm } from 'redux-form';
import { connect } from 'react-redux';
import { createStream } from '../../actions';

class StreamCreate extends React.Component {
    renderError({error, touched}) {  //this is destructuring object meta, so we will have access only to meta.error and meta.touched
        if (touched && error) {
            return (
                <div className="ui error message">
                    <div className="header">{error}</div>
                </div>
            );
        }
    }

    renderInput = ({input, label, meta}) => {
        console.log(meta);
        const className = `field ${meta.error && meta.touched ? 'error' : ''}`;

        return (
            <div className="{className}">
                <label>{label}</label>
                <input {...input} autoComplete="off" />
                {this.renderError(meta)}
        );

        //  code above is shortcut for this
        //  <input
        //    onChange={formProps.input.onChange}
        //    value={formProps.input.value}
        //  />
    }

    onSubmit = (formValues) => {
        console.log(formValues);  //submitted values
        this.props.createStream(formValues);
    }

    render() {
        console.log(this.props);  //check available functions of redux-form
        return (
            <form
                onSubmit={this.props.handleSubmit(this.onSubmit)}
                className="ui form error"
            >
                <Field name="title" component={this.renderInput} label="Enter Title" />
                <Field name="description" component={this.renderInput} label="Enter Description" />
                <button className="ui button primary">Submit</button
            </form>
        );
    }
}

const validate = (formsValues) => {
    const errors = {};

    if (!formValues.title) {
        errors.title = 'You must enter a title';
    }

    if (!formValues.descriptin) {
        errors.title = 'You must enter a description';
    }

    return errors;
}

const formWrapped reduxForm({
    form: 'streamCreate',
    validate: validate
})(StreamCreate);

export default connect(null, { createStream })(formWrapped);
{% endhighlight %}


<br /><br />
<h3>client/src/components/streams/StreamDelete.js</h3>
{% highlight javascript %}
import React from 'react';

const StreamDelete = () => {
    return <div>StreamDelete</div>;
};

export default StreamDelete;
{% endhighlight %}

<br /><br />
<h3>client/src/components/streams/StreamEdit.js</h3>
{% highlight javascript %}
import React from 'react';

const StreamEdit = () => {
    return <div>StreamEdit</div>;
};

export default StreamEdit;
{% endhighlight %}


<br /><br />
<h3>client/src/components/streams/StreamShow.js</h3>
{% highlight javascript %}
import React from 'react';

const StreamShow = () => {
    return <div>StreamShow</div>;
};

export default StreamShow;
{% endhighlight %}

<br /><br />

<h2>Actions</h2>
<h3>client/src/actions/types.js</h3>
{% highlight javascript %}
export const SIGN_IN = 'SIGN_IN';
export const SIGN_OUT = 'SIGN_OUT';
export const CREATE_STREAM = 'CREATE_STREAM';
export const FETCH_STREAMS = 'FETCH_STREAMS';
export const FETCH_STREAM = 'FETCH_STREAM';
export const DELETE_STREAM = 'DELETE_STREAM';
export const EDIT_STREAM = 'EDIT_STREAM';
{% endhighlight %}


<h3>client/src/actions/index.js</h3>
{% highlight javascript %}
import streams from '../apis/streams';
import {
    SIGN_IN,
    SIGN_OUT,
    CREATE_STREAM,
    FETCH_STREAM,
    DELETE_STREAM,
    EDIT_STREAM
} from './types';

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

export const createStream = (formValues) => {
    //everytime we want to do async request, we are using redux-thunk
    return async (dispatch) => {
        const response = await streams.post('/streams', formValues);
        dispatch({type: CREATE_STREAM, payload: response.data });
    }
};

export const fetchStreams = () => async dispatch => {
    const response = await streams.get('/streams');
    dispatch({type: FETCH_STREAMS, payload: response.data });
};

export const fetchStream = (id) => async dispatch => {
    const response = await streams.get(`/streams/{$id}`);
    dispatch({type: FETCH_STREAM, payload: response.data });
};

export const editStream = (id, formValues) => async dispatch => {
    const response = await streams.put(`/streams/{$id}`, formValues);
    dispatch({type: EDIT_STREAM, payload: response.data });
};

export const deleteStream = (id) => async dispatch => {
    await streams.delete(`/streams/${id}`);
    dispatch({type: DELETE_STREAM, payload: id });
};
{% endhighlight %}




<br /><br />

<h2>Reducers</h2>
<h3>client/src/reducers/index.js</h3>
{% highlight javascript %}
import { combineReducers } from 'redux';
import { reducer as formReducer } from 'redux-form';
import authReducer from './authReducer';

export default combineReducers({
  auth: authReducer,
  form: formReducer
});
{% endhighlight %}

<br /><br />
<h3>client/src/reducers/authReducer.js</h3>
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
