---
layout: post
date:   2019-05-28 07:13:26 +0200
title:  "Redux: Forms"
category: react
tags: [react, redux, forms]
---

<h2>Redux Forms</h2>
`github.com/erikras/redux-form/releases`
<br /><br />
As of March 22 2019, the maintainer of Redux Form seems to have accidentally rolled back to an extremely out of date version. To work around this, we need to install a specific version which is the latest known good release.
<br />
`npm install redux-form@8.1.0`

<br /><br />
Documentation is at:
`redux-form.com`

<br /><br />

![](http://michalmachovic.github.io/assets/2019-05-28-redux-forms-1.png)



<h2>client/src/reducers/index.js</h2>
{% highlight javascript %}
import { combineReducers } from 'redux';
import { reducer as formReducer } from 'redux-form';

export default combineReducers({
    form: formReducer
});
{% endhighlight %}


<br /><br />



<h2>client/src/components/streams/StreamCreate.js</h2>
{% highlight javascript %}
import React from 'react';
import { Field, reduxForm } from 'redux-form';

class StreamCreate extends React.Component {

    renderError(meta) {
        if (meta.touched && meta.error) {
            return (
                <div className="ui error message">
                    <div className="header">
                        {meta.error}
                    </div>
                </div>
            )
        }

    }

    renderInput = (formProps) => {
        const className = `field ${formProps.meta.error && formProps.meta.touched ? 'error' : ''}`;
        return (
            <div className={className}>
                <label>{formProps.label}</label>
                <input
                    onChange={formProps.input.onChange}
                    onFocus={formProps.input.onFocus}
                    onBlur={formProps.input.onBlur}
                    value={formProps.input.value}
                    autoComplete="off"
                />
                 {this.renderError(formProps.meta)}
            </div>
        )
    }


    /*
        //ALTERNATIVE SYNTAX - SHORTER - DESTRUCTURING OUT
        renderInput = ({input, label, meta}) => {
            const className = `field ${meta.error && meta.touched ? 'error' : ''}`;
            return (
                <div className={className}>
                    <label>{label}</label>
                    <input {...input} autoComplete="off"
                    />
                     {this.renderError(meta)}
                </div>
            )
        }
    */

    render() {
        console.log(this.props);

        return (
            <form className="ui form error" onSubmit={this.props.handleSubmit(this.onSubmit)}>
                <Field
                    name="title"
                    component={this.renderInput}
                    label="Enter Title"
                />

                <Field
                    name="description"
                    component={this.renderInput}
                    label="Enter Description"
                />

                <button className="ui button primary">Submit</button>
            </form>
        );
    }
}

const validate = (formValues) => {
    const errors = {};

    if (!formValues.title) {
        errors.title = 'You must enter a title.';
    }

    if (!formValues.description) {
        errors.description = 'You must enter a description.';
    }

    return errors;
};

export default reduxForm({
    form: 'streamCreate',
    validate: validate
})(StreamCreate);
{% endhighlight %}
