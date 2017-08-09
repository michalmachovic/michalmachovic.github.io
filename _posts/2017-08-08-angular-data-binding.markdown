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
	serverName = '';

	constructor() {
		setTimeOut(() => {
			this.allowNewServer = true;
		},2000);
	}

	ngOnInit() {

	}

	onCreateServer() {
		//do something
	}

	onUpdateServerName(event: Event) {
		this.serverName = (<HTMLInputElement>event.target).value;
	}
}
{% endhighlight %}


In template [] means property binding, () means event binding.

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




<br /><br />
<h2>Event Binding</h2>
`app/servers/servers.component.html`
{% highlight ts %}
{% raw %}
<button 
	[disabled]="!allowNewServer"
	(click)="onCreateServer()">Add Server</button>
{% endraw %}
{% endhighlight %} 




<br /><br />
<h2>Passing and Using Data with Event Binding</h2>
`app/servers/servers.component.html`
{% highlight ts %}
{% raw %}
<input 
   type="text"
   (input)="onUpdateServerName($event)">
{% endraw %}
{% endhighlight %} 

`$event` is reserved variable, it contains data which comes from input, so in this case what you type.




<br /><br />
<h2>Two-Way Data Binding</h2>
We combine property and event binding. Important: For Two-Way-Binding to work, you need to enable the `ngModel`  directive. This is done by adding the `FormsModule`  to the `imports[]`  array in the `AppModule`.

You then also need to add the `import from @angular/forms`  in the `app.module.ts` file:

`import { FormsModule } from '@angular/forms';`


Following code will update value of serverName, when you type inside the input. But it will also update value of input if you change value of serverName from somewhere else. 
`app/servers/servers.component.html`
{% highlight ts %}
{% raw %}
<input 
   type="text"
   [(ngModel)]="serverName">
{% endraw %}
{% endhighlight %} 



How do you know to which `Properties` or `Events` of HTML Elements you may bind? You can basically bind to all Properties and Events - a good idea is to `console.log()`  the element you're interested in to see which properties and events it offers.

Important: For events, you don't bind to onclick but only to click (=> (click)).

The MDN (Mozilla Developer Network) offers nice lists of all properties and events of the element you're interested in. Googling for YOUR_ELEMENT properties  or YOUR_ELEMENT events  should yield nice results.