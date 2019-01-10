---
layout: post
title:  "Javascript: Promises"
date:   2019-01-10 06:13:26 +0200
category: javascript
tags: [javascript,es6,es2015,es8,es2017,promises]
---


<br /><br />
<h2>ES6/ES2015</h2>
Promises were added in ES6/ES2015 in JavaScript are very similar to the promises you make in real life. 
Promise is object that keep track about whether a certain event has happened already or not. Determines what happens after the event has happened. Implement the concept of a future value that we are expecting. Promise is first `pending`. After event happened promise is `resolved`. If promise was successfull, it is `fulfilled`. But if there was an error, it is `rejected`.
We can `create` promises or `consume` it.

{% highlight javascript %}
//CREATE PROMISES
const getIDs = new Promise ((resolve, reject) => {
    setTimeout(() => {
      resolve([523, 883, 432, 974]);
    }, 1500);
});

const getRecipe = recID => {
    return new Promise((resolve,reject) => {
        setTimeout(ID => {
            const recipe = {
              title: 'Fresh tomato pasta',
              publisher: 'Jonas'
            };

            resolve(`${id}: ${recipe.title}`);
        }, 1500, recID);
    })
}

//CONSUME PROMISES
getIDs
.then(IDs => {
   console.log(IDs);
   return getRecipe(IDs[2]);
})
.then(recipe => {
   console.log(recipe);
})
.catch(error => {
   console.log(error); 
});
{% endhighlight %}
 <br /><br />


<h2>ES8/ES2017 - Async and Await</h2>
In ES8//ES2017 `async` and `await` were added to the language. This can be used only to `consume` promises. If you want to `create` promise, you will need to do it like above in ES6/ES2015.

{% highlight javascript %}
//CONSUME PROMISES
async function getRecipesAW() {
  const IDs = await getIDS;
  console.log(IDs);

  const recipe = await getRecipe(IDs[2]);
  console.log(recipe);
}
getRecipesAW();
{% endhighlight %}
<br /><br />

If we want `async` function to return something, we need to use `then`, because only after async function finish it can return result.

{% highlight javascript %}
//CONSUME PROMISES
async function getRecipesAW() {
  const IDs = await getIDS;
  const recipe = await getRecipe(IDs[2]);
  return recipe;
}
getRecipesAW().then(result => {
  console.log(result);
});
{% endhighlight %}