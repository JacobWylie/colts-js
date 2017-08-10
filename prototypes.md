<h1>Prototypes</h1>

<h3>Objectives</h3>

<li>Understand what the prototype object is</li>
<li>Describe and diagram the relationship between _proto_, prototype, and constructor</li>
<li>Add methods and properties on the protoype object to write more efficient code</li>
<li>Explain the difference between adding methods and properties to the prototype versus the constructor function</li>

<h3>Review: The 'new' Keyword</h3>

We previously used the new keyword to create objects from the constructor functions - let's recap what it does

<li>Creates an object out of thin air</li>
<li>Assigns the value of 'this' to be that object</li>
<li>Adds 'return this' to the end of the function</li>
<li>Creates a link (which we can access as _proto_) between the object created and the prototype property of the constructor function</li>

We're going to focus on the 4th point - let's see what that link looks like!

<h3>A Small Diagram</h3>
<img src="img/prototypes.png">
<li>Every constructor function has a property on it called 'prototype', which as an object</li>
<li>The prototype object has a property on it called 'constructor', which points back to the constructor function</li>
<li>Anytime an object is created using the 'new' keyword, a property called '_proto_' gets created, linking the object and the prototype property of the constructor function</li>

<h3>Code</h3>

Let's see that previous example in code

```javascript

// This is the constructor function

function Person(name) {
	this.name = name;
}

// This is an object created from the Person Constructor

const elie = new Person('Elie');
const colt = new Person('Colt');

// Since we used the new keyword, we have established a link 
// between the object and the prototype property.
// We can access that using _proto_

elie._proto_ === Person.prototype // true
colt._proto_ === Person.prototype // true

// The Person.prototype object also has a property called
// constructor which points back to the function

Person.prototype.constructor === Person; // true






















