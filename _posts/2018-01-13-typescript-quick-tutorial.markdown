---
layout: post
title:  "Typescript: Quick Tutorial"
date:   2018-01-13 06:13:26 +0200
category: typescript
tags: [typescript]
---


<h2>Installation</h2>
{% highlight ts %}
npm install -g typescript
{% endhighlight %}
<br />


<h2>Compilation</h2>
{% highlight ts %}
tsc main.ts
{% endhighlight %}
<br />

<h2>Tsconfig - strict typing</h2>
{% highlight ts %}
tsc --init
{% endhighlight %}
If we run command above, `tsconfig.json` will be created, by default with `"strict": true`. Enabling strict mode means, that typescript will warn us for example if something can be null. <br />
If we have `tsconfig.json` file under our project, then we compile only with `tsc` command.
<br />


<h2>Static Typing</h2>
A very distinctive feature of TypeScript is the support of static typing. This means that you can declare the types of variables, and the compiler will make sure that they aren't assigned the wrong types of values. If type declarations are omitted, they will be inferred automatically from your code.
<br /><br />
Here is an example. Any variable, function argument or return value can have its type defined on initialization:
<br />
{% highlight ts %}
var burger: string = 'hamburger',     // String 
    calories: number = 300,           // Numeric
    tasty: boolean = true;            // Boolean

// Alternatively, you can omit the type declaration:
// var burger = 'hamburger';

// The function expects a string and an integer.
// It doesn't return anything so the type of the function itself is void.

function speak(food: string, energy: number): void {
  console.log("Our " + food + " has " + energy + " calories.");
}

speak(burger, calories);
{% endhighlight %}

<br /><br />



<h2>Interfaces</h2>
Interfaces are used to type-check whether an object fits a certain structure. By defining an interface we can name a specific combination of variables, making sure that they will always go together. When translated to JavaScript, interfaces disappear - their only purpose is to help in the development stage.
<br/><br/>
In the below example we define a simple interface to type-check a function's arguments:
<br />
{% highlight ts %}
// Here we define our Food interface, its properties, and their types.
interface Food {
    name: string;
    calories: number;
}

// We tell our function to expect an object that fulfills the Food interface. 
// This way we know that the properties we need will always be available.
function speak(food: Food): void{
  console.log("Our " + food.name + " has " + food.calories + " calories.");
}

// We define an object that has all of the properties the Food interface expects.
// Notice that types will be inferred automatically.
var ice_cream = {
  name: "ice cream", 
  calories: 200
}

speak(ice_cream);
{% endhighlight %}
<br /><br />

<h2>Classes</h2>
When building large scale apps, the object oriented style of programming is preferred by many developers, most notably in languages such as Java or C#. TypeScript offers a class system that is very similar to the one in these languages, including inheritance, abstract classes, interface implementations, setters/getters, and more.
<br /><br />
It's also fair to mention that since the most recent JavaScript update (ECMAScript 2015), classes are native to vanilla JS and can be used without TypeScript. The two implementation are very similar but have their differences, TypeScript being a bit more strict.
<br />
Continuing with the food theme, here is a simple TypeScript class:
<br />
{% highlight ts %}
class Menu {
  // Our properties:
  // By default they are public, but can also be private or protected.
  items: Array<string>;  // The items in the menu, an array of strings.
  pages: number;         // How many pages will the menu be, a number.

  // A straightforward constructor. 
  constructor(item_list: Array<string>, total_pages: number) {
    // The this keyword is mandatory.
    this.items = item_list;    
    this.pages = total_pages;
  }

  // Methods
  list(): void {
    console.log("Our menu for today:");
    for(var i=0; i<this.items.length; i++) {
      console.log(this.items[i]);
    }
  }

} 

// Create a new instance of the Menu class.
var sundayMenu = new Menu(["pancakes","waffles","orange juice"], 1);

// Call the list method.
sundayMenu.list();
{% endhighlight %}




<h2>Generics</h2>
Generics are templates that allow the same function to accept arguments of various different types. Creating reusable components using generics is better than using the any data type, as generics preserve the types of the variables that go in and out of them.
<br /><br />
A quick example would be a script that receives an argument and returns an array containing that same argument.
{% highlight ts %}
// The <T> after the function name symbolizes that it's a generic function.
// When we call the function, every instance of T will be replaced with the actual provided type.

// Receives one argument of type T,
// Returns an array of type T.

function genericFunc<T>(argument: T): T[] {    
  var arrayOfT: T[] = [];    // Create empty array of type T.
  arrayOfT.push(argument);   // Push, now arrayOfT = [argument].
  return arrayOfT;
}

var arrayFromString = genericFunc<string>("beep");
console.log(arrayFromString[0]);         // "beep"
console.log(typeof arrayFromString[0])   // String

var arrayFromNumber = genericFunc(42);
console.log(arrayFromNumber[0]);         // 42
console.log(typeof arrayFromNumber[0])   // number
{% endhighlight %}
The first time we called the function we manually set the type to string. This isn't required as the compiler can see what argument has been passed and automatically decide what type suits it best, like in the second call. Although it's not mandatory, providing the type every time is considered good practice as the compiler might fail to guess the right type in more complex scenarios.


