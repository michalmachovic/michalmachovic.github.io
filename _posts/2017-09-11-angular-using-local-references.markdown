---
layout: post
title:  "Angular: Using local references"
date:   2017-08-15 06:13:26 +0200
category: Angular
tags: [angular]
---

There is also another way how to pass data from template to component


<br /><br />

`cockpit.component.html`
{% highlight ts %}
<input type="text" #serverNameInput />
<button (click)="onAddServer(serverNameInput)">Add server</button>
{% endhighlight %}

