---
layout: post
date:   2019-05-30 07:13:26 +0200
title:  "Javascript: Key interpolation"
category: javascript
tags: [javascript, interpolation, object]
---

With key interpolation we can add new item to object.

{% highlight javascript %}
const animalSounds = { cat: 'meow', dog: 'bark'};
const animal = 'lion';
const sound = 'roar';
{...animalSounds, [animal]: sound};
{% endhighlight %}
