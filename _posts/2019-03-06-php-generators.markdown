---
layout: post
date:   2019-03-06 06:13:26 +0200
title:  "PHP: Generators"
category: php, generators
tags: [php,generators]
---

Added to PHP in version 5.5, generators are functions that provide a simple way to loop through data without the need to build an array in memory. Still a bit confused? An example is a good way to show generators in action.
<br /><br />
First, let's quickly create a generator.php file that we will use throughout this tutorial. After creating the file, we add this little code snippet.


{% highlight php %}
<?php

function getRange ($max = 10) {
    $array = [];

    for ($i = 1; $i < $max; $i++) {
        $array[] = $i;
    }

    return $array;
}

foreach (getRange(12) as $range) {
    echo "Dataset {$range} <br>";
}
{% endhighlight %}

<br />
When we run this code, output will look like:
{% highlight php %}
Dataset 1
Dataset 2
Dataset 3 
Dataset 4 
Dataset 5
Dataset 6
Dataset 7
Dataset 8
Dataset 9
Dataset 10
Dataset 11
Dataset 12
{% endhighlight %}

<br />
Now lets make a little change to our code:
{% highlight php %}
foreach (getRange(PHP_INT_MAX) as $range) {
    echo "Dataset {$range} <br>";
}
{% endhighlight %}

This will end in `Fatal error: Allowed memory size of ... byt exhausted ...`. Well, that's a shame, PHP ran out of memory. Possible solutions that come to mind include going into php.ini and increasing memory_limit. Let's ask ourselves these questions, is this really effective? Do we want a single script to hog all our server's memory? The answers are no and no. This is not effective, and we do not want a single script to use up all our memory.


<h2>Using Generators</h2>
{% highlight php %}
<?php

function getRange ($max = 10) {
    for ($i = 1; $i < $max; $i++) {
        yield $i;
    }
}

foreach (getRange(PHP_INT_MAX) as $range) {
    echo "Dataset {$range} <br>";
}
{% endhighlight %}

Dissecting the `getRange` function, this time, we only loop through the values and `yield` an output. `yield` is similar to return as it returns a value from a function, but the only difference is that `yield` returns a value only when it is needed and does not try to keep the entire dataset in memory.
<br /><br />
If you head over to your browser, you should see data being displayed on the page. Given the appropriate time, the browser eventually displays the data.
<br /><br />
There are times when we might want to parse a large dataset (it can be log files), perform computation on a large database result, etc. We don't want actions like this hogging all the memory. We should try to conserve memory as much as possible. The data doesn't necessarily need to be large — generators are effective no matter how small a dataset is. Don't forget, our aim is speed while using less memory.
<br /><br />

<h2>nikic/iter</h2>
https://github.com/nikic/iter <br /><br />

This library implements iteration primitives like map() and filter() using generators. To a large part this serves as a repository for small examples of generator usage, but of course the functions are also practically quite useful.
<br /><br />
All functions in this library accept arbitrary iterables, i.e. arrays, traversables, iterators and aggregates, which makes it quite different from functions like array_map() (which only accept arrays) and the SPL iterators (which usually only accept iterators, not even aggregates). The operations are of course lazy.
<br /><br />

<h3>Sources</h3>
https://honzabrecka.com/2014/10/26/generatory-v-php.html<br />
https://www.zdrojak.cz/clanky/yield-a-dalsi-novinky-php-5-5/<br />
https://scotch.io/tutorials/understanding-php-generators<br />
https://github.com/nikic/iter