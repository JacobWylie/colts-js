<h1>Keyword 'This'</h1>
<li> A reserved JavaSctipt keyword </li>
<li> Determined by execution context </li>
<li> Determined using four rules (global, object/implicit, explicit, new) </li>
<br>
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

console.log(this) // window

function whatIsThis() {
	return this
}

function variablesInThis() {
	// since the value of this is the window
	// all we are doing here is creating a global variable
	this.person = "Ellie"
}

console.log(person) // Ellie

whatIsThis() // window

```


<h3 id="strictmode">Strict Mode</h3>
When we enable strict mode and we are not inside a declared object
<br>
<br>

```javascript

"use strict"

console.log(this) // window

function whitIsThis() {
	return this
}

function variablesInThis() {
	// since we are in strict mode this is undefined
	// so what happens if we add a property on undefined?
	// let's see what happens when we call the function ...
	this.person = "Elie"
}

variableInThis() // TypeError, can't set person on undefined

whatIsThis() // undefined

```
<br>
<br>


<h2 id="implicitobject">Implicit/Object</h2>
When the keyword 'this' IS inside of a declared object
<br>
<br>

```javascript

// strict mode does NOT make a difference here

const person = {
	firstName: "Ellie",
	sayHi: function() {
		return `Hi ${this.firstName}`
	},
	determineContext: function() {
		return this === person
	}
}

person.sayHi() // "Hi Ellie"
person.determineContext() // true

```
<br>
<br>


<h2 id="nestedobjects">Nested Object</h2>
What happens when we have a nested object?
<br>
<br>

```javascript

const person = {
	firstName: "Colt",
	sayHi: function() {
		return `Hi ${this.firstName}`
	},
	determineContext: function() {
		return this === person;
	},
	dog: {
		sayHello: function() {
			return `Hello ${this.firstName}`
		},
		determineContext: function() {
			return this === person;
		}	
	}
}

person.sayHi() // "Hi Colt"
person.determineContext() // true

// But what is the value of the keyword this now?
person.dog.sayHello() // "Hello undefined"
person.dog.determineContext() // false

```
<br>


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
<br>
<br>


<h2 id="fixingcall">Fixing up With Call</h2>

```javascript

const person = {
	firstName: "Colt",
	sayHi: function() {
		return `Hi ${this.firstName}`
	},
	determineContext: function() {
		return this === person
	},
	dog: {
		sayHello: function() {
			return `Hello ${this.firstName}`
		},
		determineContext: function() {
			return this === person
		}	
	}
}

person.sayHi() // "Hi Colt"
person.determineContext() // true

person.dog.sayHello.call(person) // "Hello Colt"
person.dog.determineContext.call(person) // true

// Using call worked! Notice that we do NOT invoke sayHello or determineContext method

```
<br>
<br>


<h2 id="callwild">Using Call in the Wild</h2>
Let's examine a very common use case
<br>
<br>

```javascript

const colt = {
	firstName: "Colt",
	sayHi: function() {
		return `Hi ${this.firstName}`
	}
}

const elie = {
	firstName: "Elie",
	// Look at all this duplication :(
	sayHi: function() {
		return `Hi ${this.firstName}`
	}
}

colt.sayHi() // "Hi Colt"
elie.sayHi() // "Hi Elie" (But we had to copy and paster the function from above...)

// How can we refactor the duplication using call?

// How can we "borrow" the sayHi function from colt and set the value of 'this' to be elie?

```
<br>


Solution
<br>

```javascript

const colt = {
	firstName: "Colt",
	sayHi: function() {
		return `Hi ${this.firstName}`
	}
}

const elie = {
	firstName: "Elie"
}

colt.sayHi() // "Hi Colt"
colt.sayHi.call(elie) // "Hi Elie"

// Much better!

```
<br>
<br>


<h2 id="apply">What About Apply?</h2>
It's almost identical to call - except the parameters!
<br>
<br>

```javascript

const colt = {
	firstName: "Colt",
	sayHi: function() {
		return `Hi ${this.firstName}`
	},
	addNumbers: function(a, b, c, d) {
		return `${this.firstName} just calculated ${a + b + c + d}`
	}
}

const elie = {
	firstName: "Elie"
}

colt.sayHi() // Hi Colt
colt.sayHi.apply(elie) // Hi Elie

// well this seems the same...but what happens when we start adding arguments?

colt.addNumbers(1, 2, 3, 4) // Colt just calculated 10
colt.addNumbers.call(elie, 1, 2, 3, 4) // Elie just calculated 10
colt.addNumbers.apply(elie, [1, 2, 3, 4]) // Elie just calculated 10

```
<br>
<br>


<h2 id="bind">And What About Bind?</h2>
The parameters work like call, but bind returns a function with the context of 'this' bound already!
<br>
<br>

```javascript

const colt = {
	firstName: 'Colt',
	sayHi: function() {
		return `Hi ${this.firstName}`
	},
	addNumbers: function(a, b, c, d) {
		return `${this.firstName} just calculated ${a + b + c + d}`
	}
}

const elie = {
	firstName: 'Elie'
}

const elieCalc = colt.addNumbers.bind(elie, 1, 2, 3, 4) // function() {}...
elieCalc() // Elie just calculated 10

// With bind - we do not need to know all the arguments up front!

const elieCalc2 = colt.addNumbers.bind(elie, 1, 2); // function() {}...
elieCalc2(3, 4) // Elie just calculated 10

```
<br>
<br>


<h2 id="bind-2">Bind in the Wild</h2>
Very commonly we lose context of 'this', but in functions that we do not want to execute right away!
<br>
<br>

```javascript

const colt = {
	firstName: 'Colt',
	sayHi: function() {
		setTimeout(function() {
			console.log(`Hi ${this.firstName`)
		}, 1000)
	}
}

colt.sayHi() // Hi undefined (1000 milliseconds later)
// setTimeout() is a window method so 'this' is in the global scope

```

Use bind to set the correct context of 'this'
<br>

```javascript

const colt = {
	firstName: 'Colt',
	sayHi: function() {
		setTimeout(function() {
			console.log(`Hi ${this.firstName`)
		}.bind(this), 1000)
	}
}

colt.sayHi() // Hi Colt (1000 milliseconds later)

```
<br>
<br>


<h2 id="new">The 'new' keyword</h2>
We can set the context of the keyword 'this' using the 'new' keyword - it does quite a bit more as well which we will discuss further when we talk about OOP
<br>
<br>

```javascript

function Person(firstName, lastName) {
	this.firstName = firstName
	this.lastName = lastName
}

const elie = new Person('Elie', 'Schoppik');

elie.firstName // 'Elie'
elie.lastName // 'Schoppik'

```
<br>
<br>


<h2 id="recap">Recap</h2>
<br>
<li>The keyword 'this' is a reserved keyword in JavaScript and its value is determined at execution</li>
<li>It is either set using the global context, object binding, explicit binding, or the new keyword</li>
<li>When set in the global context in a function, it is either the global object(window if in the brwoser) or undefined (if we are using 'strict mode')</li>
<li>When the keyword 'this' is inside of a declared object, the value of the keyword 'this' will always be the closest parent object</li>
<li>To explicitly set the value of the keyword 'this', we use call, apply, or bind</li>
<li>We can also use the 'new' keyword to set the context of 'this', which we will discuss when we talk about Object Orientated Programming</li>























