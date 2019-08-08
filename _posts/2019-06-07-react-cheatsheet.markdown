---
layout: post
date:   2019-08-07 07:13:26 +0200
title:  "React: Cheat sheet"
category: javascript, react
tags: [javascript, react]
---

With key interpolation we can add new item to object.


<h2>Stateless component</h2>
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
<h2>Properties in stateless component</h2>
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
<h2>Class component</h2>
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
<h2>State</h2>
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
