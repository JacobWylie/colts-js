<h1>This</h1>
<li>Reserved JavaSctiptKeyword
<li>Determined by execution context</li>
<li>Determined using four rules (global, object/implicit, explicit, new)</li>

<h3>The browser's global object is the window</h3>
``` javascript

console.log(this) //window

function whatIsThis() {
	return this
}

function variablesInThis() {
	// since the value of this is the window
	// all we are doing here is creating a global variable
	this.person = "Ellie"
}

console.log(person) //Ellie

whatIsThis() // window

```