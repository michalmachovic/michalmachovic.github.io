---
layout: post
title:  "Angular: New component"
date:   2017-08-07 10:13:26 +0200
category: angular
tags: [angular]
excerpt_separator: <!--more-->
---

Components can be created from cli by running `ng generate component servers` or `ng g c servers` where `servers` is name of component.

Here we will describe how to it manually.
Component is basicaly a typescript class. We will create new component called `server`.
<!--more-->

Create `src/app/server/server.component.ts`

{% highlight ts %}
import { Component } from '@angular/core';
@Component({
	selector: 'app-server',
	templateUrl: './server.component.html'
})
export class ServerComponent {
	serverId = 10;
	serverStatus = 'offline';
}
{% endhighlight %}

`import` will import component decorator<br />
`@Component` is decorator - typescript feature which will enhance our class<br />
`selector` is selector by which we will identify it and use in other component templates (html files)

<br /><br />



Create `src/app/server/server.component.html`

{% highlight html %}
{% raw %}
 <p>Server with id {{serverId}} is {{serverStatus}}</p>
{% endraw %}
{% endhighlight %}


<br /><br />


In `src/app/app.module.ts`

{% highlight ts %}
import { ServerComponent } from './server/server.component';

@NgModule({
	declarations: [
	   AppComponent,
	   ServerComponent
	]
	...
})
{% endhighlight %}

<br /><br />

In `src/app/app.component.html`
{% highlight ts %}
  <app-server></app-server>
{% endhighlight %}




