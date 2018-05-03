---
layout: post
title:  "Angular: Lifecycle hooks"
date:   2017-09-12 06:13:26 +0200
category: Angular
tags: [angular]
---

A component has a lifecycle managed by Angular.
<br /><br />
Angular creates it, renders it, creates and renders its children, checks it when its data-bound properties change, and destroys it before removing it from the DOM.
<br /><br />
Angular offers lifecycle hooks that provide visibility into these key life moments and the ability to act when they occur.
<br /><br />
A directive has the same set of lifecycle hooks, minus the hooks that are specific to component content and views.
<br /><br />
So this is usefull if you want to run some specific piece of code after component was created, or destroyed or something changed.

<br /><br />

`ngOnChanges()`
<br/>
Respond when Angular (re)sets data-bound input properties. The method receives a SimpleChanges object of current and previous property values.
Called before ngOnInit() and whenever one or more data-bound input properties change
<br /><br />

`ngOnInit()`
<br/>
Initialize the directive/component after Angular first displays the data-bound properties and sets the directive/component's input properties.
Called once, after the first ngOnChanges().
<br /><br />

`ngDoCheck()`
<br/>
Detect and act upon changes that Angular can't or won't detect on its own.
Called during every change detection run, immediately after ngOnChanges() and ngOnInit().
<br /><br />

`ngAfterContentInit()`
<br/>
Respond after Angular projects external content into the component's view.
Called once after the first ngDoCheck().
A component-only hook.
<br /><br />

`ngAfterContentChecked()`
<br/>
Respond after Angular checks the content projected into the component.
Called after the ngAfterContentInit() and every subsequent ngDoCheck().
A component-only hook
<br /><br />

`ngAfterViewInit()`
<br/>
Respond after Angular initializes the component's views and child views.
Called once after the first ngAfterContentChecked().
A component-only hook.
<br /><br />

`ngAfterViewChecked()`
<br/>
Respond after Angular checks the component's views and child views.
Called after the ngAfterViewInit and every subsequent ngAfterContentChecked().
A component-only hook.
<br /><br />

`ngOnDestroy()`
<br/>
Cleanup just before Angular destroys the directive/component. Unsubscribe Observables and detach event handlers to avoid memory leaks.
Called just before Angular destroys the directive/component.
<br /><br />

