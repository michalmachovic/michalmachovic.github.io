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

const YourComponent = () => {
    return (
        <div>aaa</div>
    );
}

export default YourComponent;
{% endhighlight %}



<h2>Class component</h2>
{% highlight javascript %}
import React from 'react'

class YourComponent extends React.Component {
  render() {
    return <div>aaa</div>
  }
}

export default YourComponent
{% endhighlight %}
