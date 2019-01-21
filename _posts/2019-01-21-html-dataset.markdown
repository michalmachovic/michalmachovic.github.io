---
layout: post
date:   2019-01-21 06:13:26 +0200
title:  "HTML5: Dataset"
category: javascript
tags: [magento,composer]
---


<br /><br />
The dataset property on the HTMLElement interface provides read/write access to all the custom data attributes (`data-*`) set on the element. This access is available both in HTML and within the DOM.
<br /><br />

{% highlight html %}
<div id="user" data-id="1234567890" data-user="johndoe" data-date-of-birth>John Doe</div>
{% endhighlight %}

{% highlight javascript %}
const el = document.querySelector('#user');

// el.id == 'user'
// el.dataset.id === '1234567890'
// el.dataset.user === 'johndoe'
// el.dataset.dateOfBirth === ''

// set the data attribute
el.dataset.dateOfBirth = '1960-10-03'; 
// Result: el.dataset.dateOfBirth === 1960-10-03

delete el.dataset.dateOfBirth;
// Result: el.dataset.dateOfBirth === undefined

// 'someDataAttr' in el.dataset === false
el.dataset.someDataAttr = 'mydata';
// Result: 'someDataAttr' in el.dataset === true
{% endhighlight %}
