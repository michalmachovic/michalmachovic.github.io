---
layout: post
title:  "Angular: Data Binding"
date:   2017-08-08 06:13:26 +0200
category: angular
tags: [angular]
---

`app/servers/servers.component.ts`
{% highlight ts %}
import { Component, OnInit } from '@angular/core';
@Component({
	selector: 'app-servers',
	templateUrl: './servers.component.html'
})
export class ServersComponent implements OnInit {
	
	allowNewServer = false;
	serverId = 10;
	serverStatus = 'offline';

	constructor() {
		setTimeOut(() => {
			this.allowNewServer = true;
		},2000);
	}

	ngOnInit() {

	}
}
{% endhighlight %}


<br /><br />
<h2>String interpolation</h2>
This is one way data binding, we are just printing out `serverId` and `serverStatus` property.
<br /><br />
`app/servers/servers.component.html`
{% highlight ts %}
{% raw %}
{{serverId}} is {{serverStatus}}
{% endraw %}
{% endhighlight %}

<br /><br />
<h2>Property Binding</h2>
`app/servers/servers.component.html`
{% highlight ts %}
{% raw %}
<button [disabled]="!allowNewServer">Add Server</button>
{% endraw %}
{% endhighlight %}