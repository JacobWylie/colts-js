<h1>Introduction to Object Orientated Programming</h1>

<h3>Objectives</h3>
<li> Define what OOP (Object Orientated Programming) is</li>
<li> Revisit the 'new' keyword and understand the four things it does</li>
<li> Use constructor functions to reduce duplication in our code</li>
<li> Use call and apply to refactor constructor functions</li>
<br>

<h3>OOP Defined</h3>
<li> A programming model based around the idea of objects</li>
<li> These objects are constructed from what are called 'classes', which we can think of like a blueprint. We call these objects created from classes 'instances'</li>
<li> We strive to make our classes abstract and modular</li>
<br>

<h3>Contents</h3>

1. [OOP in JavaScript](#oop)
    1. [Object Creation](#object-creation)
    2. [Constructor Functions](#constructor-functions)
    3. [Creating an Object](#create-object)
2. [The 'new' Keyword](#new)
    1. [Our solution to the problem!](#new-solution)
    2. [Your Turn!](#new-turn)
3. [Multiple Constructors](#mulitple-constructors)
    1. [Using call/apply](#call-apply)
4. [Recap](#recap)


<hr>
<br>


<h2 id="oop">OOP in Javascript</h2>

But JavaScript does not have 'classes' built into it - so what do we do?
We use functions and objects!
<br>


<h3 id="object-creation">Object Creation</h3>

Imagine we want to make a few house objects, they will all have bedrooms, bathrooms, and numSqft

```javascript

const house = {
	bedrooms: 2,
	bathrooms: 2,
	sqFeet: 1000
}

const house = {
	bedrooms: 2,
	bathrooms: 2,
	sqFeet: 1000
}

const house = {
	bedrooms: 2,
	bathrooms: 2,
	sqFeet: 1000
}

const house = {
	bedrooms: 2,
	bathrooms: 2,
	sqFeet: 1000
}

// woof...imagine if we had to make 100 of these

```

<h3>A Solution</h3>

Instead of making an infinite number of different objects, let's see if we can create a function to construct these similiar 'house' objects.
<br>


<h3 id="constructor-functions">Constructor Functions</h3>

Let's use a function as a blueprint for what each house should be - we call these kinds of functions 'constructor' functions

```javascript

function House(bedrooms, bathrooms, numSqft) {
	this.bedrooms = bedrooms;
	this.bathrooms = bathrooms;
	this.numSqft = numSqft;
}

```

<h4>Notice a few things...</h4>
<li>Capitalization of the function name - this is convention!</li>
<li>The keyword 'this' is back!</li>
<li>We are attaching properties onto the keyword 'this'. We would like the keyword 'this' to refer to the object we will create from our constructor function, how might we do that?</li>
<br>

<h3 id="create-object">Creating an Object</h3>

So how do we use our constructor to actually construct objects?

```javascript

function House(bedrooms, bathrooms, numSqft) {
	this.bedrooms = bedrooms;
	this.bathrooms = bathrooms;
	this.numSqft = numSqft;
}

const firstHouse = House(2, 2, 1000); // does this work?
firsthouse // undefined...guess not!

```

<h4>Why is this not working?</h4>
<li>We are not returning anything from the function so our House function returns undefined</li>
<li>We are not explicitly binding the keyword 'this' or placing it inside a declared object. This means the value of the keyword 'this' will be the global object, which is not what we want!</li>
<br>
<br>


<h2 id="new">The 'new' Keyword</h2>

<h3 id="new-solution">Our solution to the problem!</h3>
<br>

```javascript

function House(bedrooms, bathrooms, numSqft) {
	this.bedrooms = bedrooms;
	this.bathrooms = bathrooms;
	this.numSqft = numSqft;
}

const firstHouse = new House(2, 2, 1000);
firstHouse.bedrooms // 2
firstHouse.bathrooms // 2
firstHouse.numSqft // 2

```

<h4>So what does the new keyword do? A lot more than we might think...</h4>
<li>It creates an empty object</li>
<li>It then sets the keyword 'this' to be that empty object</li>
<li>It adds the line 'return this' to the end of the function, which follows it</li>
<li>It adds a property onto the empty object called "_proto_", which links the prototype property on the constructor function to the empty object (more on this later)</li>
<br>

<h3 id="new-turn">Your Turn!</h3>

Create a constructor function for a Dog - each dog should have a name and an age. As a bonus, add a function for each dog called 'bark', which console.log's the name of the dog added to the string 'just barked!'
<br>

```javascript

// Your constructor function goes here

function Dog(name, age) {
	this.name = name;
	this.age = age;
	this.bark = function() {
		console.log(`${this.name} just barked`)
	}
}

// This code should work if you have implemented it correctly

const rusty = new Dog('Rusty', 3);
const fido = new Dog('Fido', 1);

rusty.bark() // Rusty just barked!
fido.bark() // Fido just barked!

```
<br>
<br>


<h2 id="multiple-constructors">Multiple Constructors</h2>

Let's create two constructor functions, one for a Car and one for a Motorcycle = here is what it might look like

```javascript

function Car(make, model, year) {
	this.make = make;
	this.model = model;
	this.year = year;
	// we can also set properties on the keyword this that are preset values
	this.numWheels = 4
}

function Motorcycle(make, model, year) {
	this.make = make;
	this.model = model;
	this.year = year;
	this.numWheels = 2
}

```

Notice how much duplication is going on in the Motorcycle function. Is there any way to 'borrow' the Car function and invoke it inside the Motorcycle function?
<br>

<h3 id="call-apply">Using call/apply</h3>

We can refactor our code quite a bit using call + apply

```javascript

function Car(make, model, year) {
	this.make = make;
	this.model = model;
	this.year = year;
	// we can also set properties on the keyword this that are preset values
	this.numWheels = 4
}

// Using call

function Motorcycle(make, model, year) {
	Car.call(this, make, model, year)
	this.numWheels = 2;
}

// Using apply

function Motorcycle(make, model, year) {
	Car.apply(this, [make, model, year])
	this.numWheels = 2;
}

// Even better using apply with arguments

function Motorcycle(make, model, year) {
	Car.apply(this, arguments)
	this.numWheels = 2;
}

```
<br>
<br>


<h2 id="recap">Recap</h2>

<li>Object Orientated Programming is a model based on objects constructed from a blueprint. W use OOP to write more modular and shareable code</li>
<li>In languages that have built-in support for OOP, we call these blueprints 'classes' and the objects created from them 'instances'</li>
<li>Since we do not have built-in class support in JavaScript, we mimic classes by using functions. These constructor functions create objects through the use of the new keyword</li>
<li>We can avoid duplication in multiple constructor functions by using call or apply</li>
<br>
<br>






























