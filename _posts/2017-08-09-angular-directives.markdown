---
layout: post
title:  "Angular: Directives"
date:   2017-08-09 06:13:26 +0200
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
	serverCreated = false;
	servers = ['Testserver, 'Testserver 2'];

	constructor() {
	}

	ngOnInit() {
	}

	onCreateServer() {
		this.serverCreated = true;
		this.servers.push(this.serverName);
	}
}
{% endhighlight %}

<br /><br />

<h2>ngIf and ngFor</h2>
`app/servers/servers.component.html`
{% highlight html %}
  <p *ngIf="serverCreated">Server was created</p>
  {% raw %}
<button (click)="onCreateServer()">Add Server</button>

<app-server *ngFor="let server of servers, let i = index"></app-server>
{% endraw %}
{% endhighlight %}

<br /><br />

<h2>ngIf with else condition</h2>
`app/servers/servers.component.html`
{% highlight html %}
  <p *ngIf="serverCreated; else noServer">Server was created</p>
  <ng-template #noServer>
    <p>No server was created</p>
  </ng-template>
  {% raw %}
<button (click)="onCreateServer()">Add Server</button>
{% endraw %}
{% endhighlight %}

<h2>ngStyle</h2>
`app/server/server.component.html`
{% highlight html %}
 {% raw %}
<p [ngStyle]="{backgroundColor: getColor()}"> {{serverId}} is {{getServerStatus()}} </p>
 {% endraw %}
 {% endhighlight %} 



<br /><br />
`app/servers/server.component.ts`
{% highlight ts %}
import { Component } from '@angular/core';
@Component({
	selector: 'app-server',
	templateUrl: './server.component.html',
	styles: [`
	   .online {
			color:white;
		}
	`]
})
export class ServerComponent {
	serverId: number = 10;
	serverStatus: string = 'offline';

	constructor() {
		this.serverStatus = Math.random() > 0.5 ? 'online' : 'offline';
	}

	getServerStatus() {
		return this.serverStatus;
	}

	getColor() {
		return this.serverStatus === 'online' ? 'green' : 'red';
	}
}
{% endhighlight %}



<h2>ngClass</h2>
`app/server/server.component.html`
{% highlight html %}
 {% raw %}
<p 
  [ngStyle]="{backgroundColor: getColor()}"
  [ngClass]="{'online': serverStatus==='online'}"
 > 
   {{serverId}} is {{getServerStatus()}} 
 </p>
 {% endraw %}
 {% endhighlight %} 