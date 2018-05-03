---
layout: post
title:  "Angular: Binding to custom events"
date:   2017-08-15 06:13:26 +0200
category: Angular
tags: [angular]
---

We can use event binding to pass data from our child component to parent component.
We need to add decorator `@Output` and also custom event binding. In following case we pass data from cockpit child component to parent app component. 


<br /><br />

`app/app.component.ts`
{% highlight ts %}
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  serverElements = [{ type: 'server', name: 'TestServer', content: 'Just a test'}];

 onServerAdded(serverData: {serverName: string, serverContent: string}) {
    
    this.serverElements.push({
      type: 'server',
      name: serverData.serverName,
      content: serverData.serverContent
    });
    
  }

  onBlueprintAdded(blueprintData: {serverName: string, serverContent: string}) {
   
    this.serverElements.push({
      type: 'blueprint',
      name: blueprintData.serverName,
      content: blueprintData.serverContent
    });
   
  }


}

{% endhighlight %}


<br /><br />

In app component html file we are using our own event binding called `serverCreated` and `blueprintCreated`. It means that if we click button in app cockpit html file, serverCreated will run and it will pass data to onServerAdded method, which is defined in app ts file.

`app/app.component.html`
{% highlight ts %}
<div class="container">
  <app-cockpit 
  (serverCreated)="onServerAdded($event)"
  (blueprintCreated)="onBlueprintAdded($event)"

  ></app-cockpit>
  
  <hr>
  <div class="row">
    <div class="col-xs-12">
      <app-server-element 
       *ngFor="let serverElement of serverElements"
       [srvElement]=serverElement
       ></app-server-element>    
    </div>
  </div>
</div>

{% endhighlight %}



<br /><br />
See `EventEmitter` here. It means it is listening to event. Inside <> is which data we are passing and () and the end means it is executed immediately.
<br /><br />
`cockpit.component.ts`
{% highlight ts %}
import { Component, OnInit, EventEmitter, Output } from '@angular/core';

@Component({
  selector: 'app-cockpit',
  templateUrl: './cockpit.component.html',
  styleUrls: ['./cockpit.component.css']
})
export class CockpitComponent implements OnInit {
  @Output() serverCreated = new EventEmitter<{serverName: string, serverContent: string}>();
  @Output() blueprintCreated = new EventEmitter<{serverName: string, serverContent: string}>();

  newServerName = '';
  newServerContent = '';

  constructor() { }

  ngOnInit() {
  }

  onAddServer() {
    this.serverCreated.emit({
      serverName: this.newServerName,
      serverContent: this.newServerContent
    })
  }

  onAddBlueprint() {
    this.blueprintCreated.emit({
      serverName: this.newServerName,
      serverContent: this.newServerContent
    })
  }
}
{% endhighlight %}


<br /><br />
`cockpit.component.html`
{% highlight ts %}
<div class="row">
  <div class="col-xs-12">
      <p>Add new Servers or blueprints!</p>
      <label>Server Name</label>
      <input type="text" class="form-control" [(ngModel)]="newServerName">
      <label>Server Content</label>
      <input type="text" class="form-control" [(ngModel)]="newServerContent">
      <br>
      <button
        class="btn btn-primary"
        (click)="onAddServer()">Add Server</button>
      <button
        class="btn btn-primary"
        (click)="onAddBlueprint()">Add Server Blueprint</button>
    </div>
</div>
{% endhighlight %}