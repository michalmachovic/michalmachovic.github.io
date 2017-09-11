---
layout: post
title:  "Angular: Binding to custom properties"
date:   2017-08-15 06:13:26 +0200
category: Angular
tags: [angular]
---

We can use property binding to bind to our own properties, properties of our components.
By default all properties of components are available only inside these components. What to do if we want access some properties in another component ?
We need to add decorator `@Input`.

<br />
So in other words, we have parent component and child component. We have array of objects defined in parent component and we want to display items of this array in child component in a loop.
<br /><br />

`app/server-element/server-element.component.ts`
{% highlight ts %}
import { Component, OnInit, Input} from '@angular/core';

@Component({
	selector: 'app-server-element',
	templateUrl: './server-element.component.html'
})

export class ServerElementComponent implements OnInit {
	@Input() element: {type:string, name: string, content: string};

	constructor() {}

	ngOnInit() {

	}

}
{% endhighlight %}

<br /><br />
`app/server-element/server-element.component.html`
{% highlight ts %}
{% raw %}
 {{element.type}}
 {% endraw %}
{% endhighlight %} 

<br /><br />

`app/app.component.ts`
{% highlight ts %}
import { Component } from '@angular/core';

@Component({
	selector: 'app-root',
	templateUrl: './app.component.html'
})

export class AppComponent {
	serverElements = [{type: 'server', name: 'Testserver', content: 'test'}];
}
{% endhighlight %} 

<br /><br />

`app/app.component.html`
{% highlight ts %}
<app-server-element 
	*ngFor="let serverElement of serverElements"
	[element]="serverElement"></app-server-element>
{% endhighlight %} 


<br /><br />
<h1>Binding to custom properties with alias</h1>
If we want to bind to different name, you can bind with an alias.

`app/server-element/server-element.component.ts`
{% highlight ts %}
@Input('srvElement') element: {type:string, name: string, content: string};
{% endhighlight %} 

<br /><br />
`app/app.component.html`
{% highlight ts %}
<app-server-element 
	*ngFor="let serverElement of serverElements"
	[srvElement]="serverElement"></app-server-element>
{% endhighlight %} 