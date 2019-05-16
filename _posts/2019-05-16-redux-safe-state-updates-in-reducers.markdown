---
layout: post
date:   2019-05-16 07:13:26 +0200
title:  "Redux: Safe state updates in reducers"
category: react
tags: [react, redux, thunk]
---

Below is correct way of updating state in reducers. Reason why we are doing it this way is, that we want to create new state in memory. Then `Redux` knows that something has changed and `React` can rerender app. <br />
With examples on the left we are referencing the same array/object. <br />
With examples on the right we are creating new array/object. <br />
`_.omit` is using `lodash library`, available at `lodash.com` <br /><br />

![](http://michalmachovic.github.io/assets/2019-05-16-safe-1.png)
