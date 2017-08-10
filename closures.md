<h1>Closures</h1>

<h3>Objectives</h3>
<li>Understand what a closure is and what it is not</li>
<li>Use a closure to emulate private variables</li>
<li>List use cases for closures in the real world</li>

<h3>Closure Defined</h3>
<li>A closure is a function that makes use of variables defined in outer functions that have previously returned</li>
<br>

<h3>Contents</h3>

1. [Our First Closure](#first)
2. [Your Turn](#your-turn)
3. [Private Variables](#private)
4. [Recap](#recap)


<hr>
<br>


<h2 id="first">Our First Closure</h2>

```javascript

function outer(a) {
	return function inner(b) {
		// The inner function is making use of the variable 'a'
		// which was defined in an outer function called 'outer'
		// and by the time this is called, that outer function has returned
		return a + b
	}
}

outer(5)(5) //10

const storeOuter = outer(5)
storeOuter(10) // 15

```

<h4>A couple of things to note here:</h4>

<li>we have to 'return' the inner function for this to work</li>
<li>We can either call the inner function right away by using an extra () or we can store the results of the function in a variable (very similiar to how bind works)</li>
<li>We do NOT have to give the inner function a name - we can make it anonymous (we just called it 'inner' for learning purposes)</li>
<br>
<br>


<h2 id="your-turn">Your Turn</h2>


<h4>Is this a closure</h4>


```javascript

function outerFn() {
	let data = 'something from outer'
	return function innerFn() {
		return 'Just returned from the inner function'
	}
}

```


<h4>What about this?</h4>


```javascript

function outerFn() {
	let data = 'something from outer'
	return function innerFn() {
		let innerData = 'something from inner'
		return `${data} ${innerData}`
	}
}

```


<h4>The first one is NOT, but the second one is. Why?</h4>

Because a closure only exists when an inner function makes use of variables defined from an outer function that has returned. If the inner function does not make use of any of the external variables all we have is a nested function
<br>
<br>


<h2 id="private">Private Variables</h2>

In other languages, there exists support for variables that can not be modified externally, we call those private variables, but in JavaScript we don't have that built in. No worries - closures can help!


```javascript

function counter() {
	let count = 0
	return function() {
		return ++count
	}
}

counter1 = counter()
counter1() // 1
counter1() // 2

counter2 = counter()
counter2 = // 1
counter2() // 2

counter1() // 3 - This is not affected by counter2

count // ReferenceError: count is not defined - because it is private

```


<h4>Another Privacy Example</h4>


```javascript

function classRoom() {
	let instructors = ['Colt', 'Elie']
	return {
		getInstructors: function() {
			return instructors;
		},
		addInstructors: function(instructor) {
			instructors.push(instructor);
			return instructors;
		} 
	}
}

course1 = classRoom()
course1.getInstructors() // ['Elie', 'Colt']
course1.addInstructor('Ian') // ['Elie', 'Colt', "Ian"]
course1.getInstructors() // ['Elie', 'Colt', 'Ian']

coure2 = classRoom()
course2.getInstructors() // ['Elie', 'Colt'] - not affected by course1

// We also have NO acess to the instructor variable
// which makes it private - no one can modify it...you're stuck with Colt and Elie

```

<br>
<br>


<h2 id="recap">Recap</h2>

<li>Closure exists when an inner function makes use of variables declared in an outer function which has previously returned</li>
<li>Closure does not exist if you do not return an inner function and if that inner function does not make use of variables returned by an outer function</li>
<li>We can use closures to create private variables and write better code that isolates our logic and application</li>


























