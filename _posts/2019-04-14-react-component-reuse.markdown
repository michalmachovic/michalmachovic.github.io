---
layout: post
date:   2019-04-14 07:13:26 +0200
title:  "React: Component Reuse"
category: react
tags: [react, props]
---

<h2>src/index.js</h2>
{% highlight javascript %}
{% raw %}
// Import the React and ReactDOM libraries
import React from 'react';
import ReactDOM from 'react-dom';
import faker from 'faker';   //lib for fake data
import CommentDetail from './CommentDetail'
import ApprovalCard from './ApprovalCard'

// Create a react component
const App = () => {
	return (
		<div className="ui container comments">
			<ApprovalCard>
				<div>
					<h4>Warning</h4>
					Are you sure you want to do this ?
				</div>
			</ApprovalCard>

			<ApprovalCard>
				<CommentDetail 
					author = "Sam" 
					content = "some content"
					avatar = {faker.image.avatar()}
				/>
			</ApprovalCard>

			<ApprovalCard>
				<CommentDetail 
					author = "Alex" 
					content = "another content"
					avatar = {faker.image.avatar()}
				/>
			</ApprovalCard>
		</div>
	);
}

// Take the react component and show it on the screen
ReactDOM.render(
	<App />,
	document.querySelector('#root')
);

{% endraw %}
{% endhighlight %}



<h2>src/CommentDetail.js</h2>
{% highlight javascript %}
{% raw %}
import React from 'react';

const CommentDetail = props => {
	return (
		<div class="comment">
			<a href="/" className="avatar">
				<img src="{props.avatar}"/>
			</a>

			<div className="content">
				{props.author}
				{props.content}
			</div>
		</div>

	);
}

export default CommmentDetail;
{% endraw %}
{% endhighlight %}

<h2>src/ApprovalCard.js</h2>
{% highlight javascript %}
{% raw %}
import React from 'react';

const ApprovalCard = props => {
	return (
		<div className="ui card">
			<div className="content">
				{props.children}
			</div>
			<div className="ui basic button green">Approve</div>
			<div className="ui basic button red">Reject</div>
		</div>
	);
}

export default ApprovalCard;
{% endraw %}
{% endhighlight %}