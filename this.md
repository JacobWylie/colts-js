<h1>This</h1>
A reserved JavaSctipt keyword <br>
Determined by execution context <br>
Determined using four rules (global, object/implicit, explicit, new) <br>

<h2>Global Context</h2>
<h3>The browser's global object is the window</h3>

```javascript

console.log(this); // window

function whatIsThis() {
	return this;
}

function variablesInThis() {
	// since the value of this is the window
	// all we are doing here is creating a global variable
	this.person = "Ellie";
}

console.log(person); // Ellie

whatIsThis(); // window

```

<h3>Strict Mode</h3>
When we enable strict mode and we are not inside a declared object

```javascript

"use strict"

console.log(this); // window

function whitIsThis() {
	return this;
}

function variablesInThis() {
	// since we are in strict mode this is undefined
	// so what happens if we add a property on undefined?
	// let's see what happens when we call the function ...
	this.person = "Elie";
}

variableInThis(); // TypeError, can't set person on undefined

whatIsThis() // undefined

```

<h2>Implicit/Object</h2>
When the keyword 'this' IS inside of a declared object

```javascript

// strict mode does NOT make a difference here

let person = {
	firstName: "Ellie",
	sayHi: function() {
		return `Hi ${this.firstName}`;
	},
	determineContext: function() {
		return this === person
	}
}

person.sayHi(); // "Hi Ellie"
person.determineContext() // true

```

<h2>Nested Object</h2>
What happens when we have a nested object?

```javascript

let person = {
	firstName: "Colt",
	sayHi: function() {
		return `Hi ${this.firstName}`;
	},
	determineContext: function() {
		return this === person;
	},
	dog: {
		sayHello: function() {
			return `Hello ${this.firstName}`;
		},
		determineContext: function() {
			return this === person;
		}	
	}
}

person.sayHi(); // "Hi Colt"
person.determineContext(); // true

// But what is the value of the keyword this now?
person.dog.sayHello(); // Hello undefined
person.dog.determineContext(); // false

```

<h2>Explicit Binding</h2>
Choose what we want the context of 'this' to be using call, apply, or bind <br>
These methods can only be used by functions <br>

<table>
	<tr>
		<th>Name of Method</th>
		<th>Parameters</th>
		<th>Invoke Immediately?</th>
	</tr>
	<tr>
		<td>Call</td>
		<td>thisArg a,b,c,d,...</td>
		<td>Yes</td>
	</tr>
</table>






























