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