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

<h3>OOP in Javascript</h3>

But JavaScript does not have 'classes' built into it - so what do we do?

We use functions and objects!
<br>

<h3>Object Creation</h3>

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
<br>

<h3>A Solution</h3>

Instead of making an infinite number of different objects, let's see if we can create a function to construct these similiar 'house' objects.
<br>

<h3>Constructor Functions</h3>

Let's use a function as a blueprint for what each house should be - we call these kinds of functions 'constructor' functions

```javascript

function House(bedrooms, bathrooms, numSqft) {
	this.bedrooms = bedrooms;
	this.bathrooms = bathrooms;
	this.numSqft = numSqft;
}

```

Notice a few things...

<li>Capitalization of the function name - this is convention!</li>
<li>The keyword 'this' is back!</li>
<li>We are attaching properties onto the keyword 'this'. We would like the keyword 'this' to refer to the object we will create from our constructor function, how might we do that?</li>
<br>

<h3>Creating an Object</h3>

So how do we use our constructor to actually construct objects?

```javascript

function House(bedrooms, bathrooms, numSqft) {
	this.bedrooms = bedrooms;
	this.bathrooms = bathrooms;
	this.numSqft = numSqft;
}

const firstHouse = House(2, 2, 1000) // does this work?
firsthouse // undefined...guess not!

```

Why is this not working?

<li>We are not returning anything from the function so our House function returns undefined</li>
<li>We are not explicitly binding the keyword 'this' or placing it inside a declared object. This means the value of the keyword 'this' will be the global object, which is not what we want!</li>

<h3>Contents</h3>

1. [Global Context](#globalcontext)
    1. [Global Object](#globalobject)
    2. [Strict Mode](#strictmode)
2. [Implicit Objects](#implicitobject)
3. [Nested Objects](#nestedobjects)
4. [Explicit Binding](#explicitbinding)
5. [Fixing up with Call](#fixingcall)
6. [Using Call in the Wild](#callwild)
7. [What About Apply?](#apply)
8. [And What About Bind?](#bind)
9. [Bind in the Wild](#bind-2)
10. [The 'new' Keyword](#new)
11. [Recap](#recap)


<hr>
<br>
<h2 id="globalcontext">Global Context</h2>
<h3 id="globalobject">The browser's global object is the window</h3>

```javascript