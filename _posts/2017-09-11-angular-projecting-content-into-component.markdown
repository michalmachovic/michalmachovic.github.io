---
layout: post
title:  "Angular: Projecting content into component"
date:   2017-09-12 06:13:26 +0200
category: Angular
tags: [angular]
---

Sometimes you have complex html code which you want to past into component from outside. By default everything what you put between opening and closing tag of your own component is lost, angular ignore it. Unless you use special directive `<ng-content></ng-content>`.


<br /><br />

`app.component.html`
{% highlight html %}
<app-server-element
  *ngFor="let serverElement of serverElements"
  [srvElement]="serverElement">
  <strong *ngIf="serverElement.type==='server'">{{serverElement.content}}</strong>
</app-server-element>
{% endhighlight %}


<br /><br />

`server-element.component.html`
{% highlight html %}
 <div>
  <ng-content></ng-content>
 </div> 
{% endhighlight %}


