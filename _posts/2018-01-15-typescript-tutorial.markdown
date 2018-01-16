---
layout: post
title:  "Typescript: Tutorial"
date:   2018-01-15 06:13:26 +0200
category: typescript
tags: [typescript]
---



{% highlight ts %}
//TYPES String, Number, Array
var test: String = 'ahoj';
var num: Number = 2;
var employees: String[] = ["ferko","jozko","anicka"];
employees.push("koloman");

//INTERFACES - object of predefined types
interface SuperHero {
  realName: String;
  superName: String;
}


var superman: SuperHero = {
  realName: 'Clark Kent',
  superName: 'SuperMan'
}

var spiderman: SuperHero = {
  realName: 'Niekto',
  superName: 'SpiderMan'
}


//ARRAY OF INTERFACES
var superHeroes: SuperHero[] = [];

superHeroes.push(superman);
superHeroes.push(spiderman);

console.log(employees);
console.log(superHeroes);




//LET operator - variables defined with let in blocks dont change values of variables outside of block
let sampleLet = 123;
if (true) {
  let sampleLet = 456;
}
console.log(sampleLet);   //will be 123



var sampleVar = 123;
if (true) {
  var sampleVar = 456;
}
console.log(sampleVar);   //will be 456


//LOOPS
var randArray = [4,5,6];
for (var val in randArray) {
  console.log(randArray[val]);
}


//FUNCTIONS
var getDiff = function(num1: number, num2: number, num3?: number): number {
  if (typeof num3 !== 'undefined') {
    return num1 - num2 - num3;
  }
  return num1 - num2;
}
console.log(getDiff(5,2));
console.log(getDiff(5,2,1));


//... means there is unkown count of paramaters
//reduce calls the specified callback funct ion for all elements in array. return value of callback 
//function is accumulated result and is used as an argument to next call of callback function
var sumAll = function(...nums: number[]):number {
  return nums.reduce((a,b) => a+b, 0 );
  //console.log(sum);
}
console.log(sumAll(1,2,3,4,5));


//CLASSES
//static - shared to all animal objects
class Animal {
  public favFood: string;

  static numOfAnimals: number = 0;

  //if i define type of parameter (private/public) it is autoassigned to that variable where constructor is used
  //so i dont need to do  this.name = name
  constructor(private name: string, private owner:string) {
    Animal.numOfAnimals++;
  }


  ownerInfo() {
    console.log(this.name + " is owned by " + this.owner);
  }

  static howManyAnimals(): number {
    return Animal.numOfAnimals;
  }

  private _weight: number;

  //get and set, so we can assign value to private property
  get weight(): number {
    return this._weight;
  }

  set weight(weight: number) {
    this._weight = weight;
  }
}


var spot = new Animal("Spot","Doug");
spot.ownerInfo();
spot.weight = 100;
console.log("Spot weight is " + spot.weight); 
console.log("number of animals is " + Animal.howManyAnimals());



class Dog extends Animal {
  constructor(name: string, owner:string) {
    super(name,owner);
    Dog.numOfAnimals++;
  }
}

var grover = new Dog("Grover","Jimmy");
console.log("number of animals is " + Animal.howManyAnimals());



//GENERIC FUNCTIONS
//when we want to work with multiple datatypes in similar way
function getType<T>(val: T): string {
  return typeof(val);
}
console.log(getType('string'));
console.log(getType(4));


//GENERIC CLASSES
class GenericNumber<T> {
  add: (val1: T, val2: T) => T;
}

var aNumber = new GenericNumber<number>();
aNumber.add = function(x,y) { return x+y; };


var aStrNum = new GenericNumber<string>();
aStrNum.add = function(x,y) { return String(Number(x)+Number(y));};

aNumber.add(5,4);
aStrNum.add("5","6");


//STRUCTURING
var vals = { x:1, y:2, z:3};
var {x,y,z} = vals;
console.log(x);


//TEXT
var multiString = `I go on 
for many lines`;
document.write(multiString);
document.write(`<b>${multiString}</b>`);

{% endhighlight %}

<br /><br />

