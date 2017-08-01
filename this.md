<h1>This</h1>
A reserved JavaSctipt keyword <br>
Determined by execution context <br>
Determined using four rules (global, object/implicit, explicit, new) <br>
<br>
1. Global Context(#globalcontext)
    1. Global Object(#globalobject)
    2. <a href="#strictmode">Strict Mode</a>
2. <a href="#implicitobject">Implicit Objects</a>
3. <a href="#nestedobjects">Nested Objects</a>
4. <a href="#explicitbinding">Explicit Binding</a>
5. <a href="#fixingcall">Fixing up with Call</a>
6. <a href="#callwild">Using Call in the Wild</a>
7. <a href="#apply">What about Apply?</a>
<br>
<hr>
<h2 id="globalcontext">Global Context</h2>
<h3 id="globalobject">The browser's global object is the window</h3>

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

<h3 id="strictmode">Strict Mode</h3>
When we enable strict mode and we are not inside a declared object
<br>

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

<h2 id="implicitobject">Implicit/Object</h2>
When the keyword 'this' IS inside of a declared object
<br>

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

<h2 id="nestedobjects">Nested Object</h2>
What happens when we have a nested object?
<br>

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
person.dog.sayHello(); // "Hello undefined"
person.dog.determineContext(); // false

```

<h2 id="explicitbinding">Explicit Binding</h2>
Choose what we want the context of 'this' to be using call, apply, or bind <br>
These methods can only be used by functions <br>
<br>
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
	<tr>
		<td>Apply</td>
		<td>thisArg,[a,b,c,d,...]</td>
		<td>Yes</td>
	</tr>
	<tr>
		<td>Bind</td>
		<td>thisArg,a,b,c,d,...</td>
		<td>No</td>
	</tr>
</table>

<h2 id="fixingcall">Fixing up with Call</h2>

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

person.dog.sayHello.call(person); // "Hello Colt"
person.dog.determineContext.call(person); // true

// Using call worked! Notice that we do NOT invoke sayHello or determineContext method

```

<h2 id="callwild">Using Call in the Wild</h2>
Let's examine a very common use case
<br>

```javascript

let colt = {
	firstName: "Colt",
	sayHi: function() {
		return `Hi ${this.firstName}`;
	}
}

let elie = {
	firstName: "Elie",
	// Look at all this duplication :(
	sayHi: function() {
		return `Hi ${this.firstName}`;
	}
}

colt.sayHi(); // "Hi Colt"
elie.sayHi(); // "Hi Elie" (But we had to copy and paster the function from above...)

// How can we refactor the duplication using call?

// How can we "borrow" the sayHi function from colt
// and set the value of 'this' to be elie?

```

<h5>Solution</h5>

```javascript

let colt = {
	firstName: "Colt",
	sayHi: function() {
		return `Hi ${this.firstName}`;
	}
}

let elie = {
	firstName: "Elie"
}

colt.sayHi(); // "Hi Colt"
colt.sayHi.call(elie) // "Hi Elie"

// Much better!

```

<h2 id="apply">What about Apply?</h2>
It's almost identical to call - except the parameters!
<br>

```javascript























