##Chapter 1

> Most programming languages contain good parts and bad parts. I discovered that I could be a better programmer by using only the good parts and avoiding the bad parts. After all, how can you build something good out of bad parts?

The best parts of Javascript include:
* functions
* loose typing (variables are declared as variables, without a type)
* dynamic objects
* object literal notation (where you can create an object already with a list of key/value pairs inside curly braces)

The worst parts include global variables - there is a common _global object_ namespace where they're all lumped together and they're essential to the language.

Javascript has a _class free_ object makeup, relying instead on objects inheriting properties directly from other objects - this is **prototypal inheritance**.

<a name="Chapter2"/>
##Chapter 2 - Grammar

Always use // for comments, even multi-line ones to avoid having to escape `/*` characters.

###Numbers

* There is a single, 64-bit floating point number type.
* `NaN` (Not-a-Number) is not equal to any value (including itself) and is essentially an **illegal number value**, but _typeOf(NaN)==number is true_
* Use `isNan(number)` to check for NaNs

Number methods are discussed in [Chapter 8](#Chapter8).

###Strings

* 16-bit character set and don't have character tpes.
* Backslashes (\\\) are used for **escaping characters** that could cause problems in strings.
* Strings are **immutable**.

String methods are discussed in [Chapter 8](#Chapter8).

###Statements

* _Inside_ a function, the var statement creates variables local to that function
* _switch, while, for_ and _do_ statements can have an optional **label**  which can be used with `break` and `continue` to provide more precise [control over exactly which statement to break or continue](http://www.tutorialspoint.com/cgi-bin/practice.cgi?file=javascript_19). Format: `labelname: statement` and then `continue labelname;`
* A _block_ (statements with {}) **do not create new scope** - the variables are local to the function, not the block. So **always define variables at the top of the function** and not inside the blocks
* _falsy_ values:
	* false
	* null
	* undefined
	* Empty string ' '
	* The number 0
	* The number NaN
* All other values are _truthy_ including all objects & the string 'false'
* If no matches are found in `case` statements, the optional default statement is executed, otherwise the matching case statement is carried out
* When using a _for in_ loop, usually a good idea to use `hasOwnProperty(variable)` to **make sure the property belongs to the object you want** and is not instead an inherited property from the prototype chain:
```javascript
for (myvariable in object) {
		if (object.hasOwnProperty(myvariable)) {
		... //statements to be executed
		}
}
```
* A _do while_ statement is always executed at least once as the while condition is only checked after the first iteration of the loop
* `catch` clause in a _try_ statement **must create a new variable** that will catch the exception object
* Scope of `thro` statement is the `try` block it's in, or the `try` of the function it's in
* If there is no `return` statement, return==undefined
* `break` exits the statement and `continue` forces a new iteration of the loop, both with the optional _label_ mentioned above

###Expressions

* For `expression ? expression2 : expression3`, if expression is _truthy_, execute expresion2; it it's _falsy_, execute expression3
*  _Invocation_ is `(expression1, expression2, etc)`
*  _refinement_ is either `.name` or `[expression]` as used in an array

###Literals

* _Names_ or _strings used for specifying new objects ([**object literals**](#Chapter3)) or arrays ([**array literals**](#Chapter6))
* Properties of the object are expressions and must be known at compile time

###Functions
* A function literal defines a function value
* More details in [Chapter 4](#Chapter4)


<a name="Chapter3"/>
##Chapter 3 - Objects

Javascript simple types:
* numbers _(has object-like methods but they are immutable)_
* strings _(has object-like methods but they are immutable)_
* booleans _(has object-like methods but they are immutable)_
* null
* undefined

**All other values are _objects_** including arrays and functions.

Objects are **class free**, can contain other objects and can inherit properties from their prototypes (which can _reduce object initialisatioin time and memory consumption_).

###Object Literals

* An object literal is _zero or more comma-separated name/value pairs surrounded by curly braces_ {}

```javascript

var empty_object = {};

var today = {
	day: "Wednesday",
	month: "April",
	year: 2014,

	weather: { //objects can contain nested objects like this one
		morning: "sunny",
		afternoon: "cloudy"
	}
}
```

###Retrieval

* Can be done with either dot notation `today.weather.morning` or with square brackets `today[month]`
* Or operand (||) can be used to fill in default values for nonexistent data to prevent and _undefined_ error: `var weath = today.weather.evening || "unknown"`


###Update

* Assigning a property value to an object **overwrites** any existing property values with that property name

###Reference

* Objects refer to each other, they don't hold duplicate copies of data

### Prototype

* Every object has a prototype object from which it inherits properties
* _Object.prototype_ comes standard with Javascript and is almost like a 'root parent'
![Object prototype diagram](https://docs.google.com/drawings/d/1NdiIkHd9Cg2j6W4QcJ1X3DEhGXD2gacMXRuURcoE5T4/pub?w=960&h=720 "Object prototype diagram from @sporto")
* The `Object.create` method is now available in ES5 (but the method is in the book if required for older versions)
* If an object does not have a property you ask it for, it will **keep looking up the prototype chain until it finds it**
	* If the property *does note exist* anywhere in the chain, it will return _undefined_
* A new property is _immediately visible_ to all of the objects below it in the chain once created

More details in [Chapter 6](#Chapter6)

###Reflection

* Determining what properties an object has
* Using `typeof` **includes all properties in the prototype chain** including functions
* To avoid inherited properties, use `hasOwnProperty(type);` which returns _true_ if that property exists only in that object itself (not the chain)
```javascript
today.hasOwnProperty('number')  //will return true
today.hasOwnProperty('constructor')   //will return false
```

###Enumeration

* Best way to enumerate all the properties you want is a for loop:
```javascript
var i;
var properties = [
	'day', 'month', 'year'
	];
for (i = 0; i < properties.length; i++) {
	document.writeIn(properties[i] + ':' + today[properties[i]]);
}
```
* This ensures you get the **properties you want** (i.e. not up the prototype chain) and in the **order you want**, as opposed to a _for in_ loop which achieves neither of these

###Delete

* Removes property from object, but also **reveals property from further up the prototype chain** if it exists
* Format: `delete today.month`

###Global Abatement

* One way to mitigate the risks of global variables is to _create a single global variable_ which then contains your whole application
```javascript
var MYAPP = {}

MYAPP.today = {
	day: "Wednesday",
	month: "April",
	year: 2014,

	weather: { //objects can contain nested objects like this one
		morning: "sunny",
		afternoon: "cloudy"
	}
}
//Making sure all other variables (like today) are contained within this one global variable (MYAPP) means none of them have global scope and therefore the risk of naming conflicts, etc in your application is reduced
```
* [Closures](#Chapter4) are also a way of mitigating the risks of global variables
* Note: **Most [Javascript MVCs](http://coding.smashingmagazine.com/2012/07/27/journey-through-the-javascript-mvc-jungle/) these days (2014) will take care of the wrapping your app for you**

<a name="Chapter4"/>
##Chapter 4 - Functions

> The best thing about JavaScript is its implementation of functions.

###Function Objects

* Functions are objects linked to _function.prototype_ (which is linked to _Object.prototype_).
* As well as usual object behaviour, they can be **invoked**.

###Function Literal

* A function literal has 4 parts:
	* The (reserved) word `function` itself
	* An _optional_ name (un-named functions are considered _anonymous_ functions)
	* Comma-seperated parameters of the function, in parentheses - `(parameters)`
	* Set of statements in curly brakcets to be carried out when the function is invoked - `{statements}`

```javascript
//Format of a function
function name (parameterA, parameterB){
	statements;
}
```
<a name="nestedFunctions"/>
* Functions can be nested within functions and the inner function can access all the parameters of the outer function as well as its own

###Invocation

* Stops the current function from running and tells the function you have invoked both to start and to use the arguments (values in parentheses) you have passed it in the invocation `function (parameters)`
	* If arguments > number of arguments expected, the **extra values will be ignored**
	* If arguments < number of arguments expected, the function will assume **undefined in place of the missing arguments**
	* No error is thrown

* **Note:** The difference between an _argument_ and a _parameter_ is that a parameter is usually what is used in the function literal, when you're setting up the function (almost like the placeholder for the actual values that the function will use when it is active) and an argument is usually the value passed to a function at the time it is invoked
* Parameters `this` and `arguments` are also passed to the function when it is invoked, but their value depends on how the function is invoked

####Method Invocation Pattern

* When a function is **stored as the property of the object** (invoked with a dot . expression) it is called on and is called a _method_ 
```javascript
myObject.incrementFunction();
```
* The method is **bound to the object** and therefore can use `this` to **retrieve or update values from the object**
* These methods are **highly reusable**
* Because their _object context_ comes from `this` they are considered _public methods_

####Function Invocation Pattern

* When a function is _not_ the property of an object, it is invoked as a _function_
```javascript
var sum = **add(3, 4);**
```
* These functions are **bound to the global object** (_a "mistake in the design of the language" according to Douglas Crockford)_
* `this` is therefore also bound to the global object [even in inner functions](#nestedFunctions)
* **Workaround:** Artificially create a new `this` ... //**NOTES TO BE FINISHED**

####Constructor Invocation Pattern

* When a function is created with `new`, that function contains a link to the function's prototype
* This means that methods that were created for the **prototype function are also available** to the function created using `new`
```javascript
//create a function Quo that takes a string - Quo will be our prototype function as we will see
var Quo = function (string){
	this.status = string;
}

//Now create a get_status method for Quo - this will be a public method
Quo.prototype.get_status = function {
	return this.status;
}

//create a new instance of Quo using the prefix NEW
var myQuo = new Quo("happy");

//because of the use of the new prefix, myQuo is an instance of Quo which means it can access the public method get_status from it's prototype
document.writeIn(myQuo.get_status());     //returns 'happy'
```
* This style of constructor pattern is not recommended, there will be better examples in [Chapter 5](#Chapter5)
* The first letter of a constructor function (in this case Quo) **must _always_ be capitalized**

####Apply Invoation Pattern

* The `apply` method lets you **choose the value to be bound to `this`**
* It also takes the parameters for a function in an array
* Format:   `function.apply(valueForThis, arrayOfParamentersForFunction);`
```javascript
var array = [5, 2]    //will be the parameters for our function
var sum = add.apply(null, array);     //value of 'this' is null and value of sum is 7 as the 'apply' method passes 5 and 2 to the 'add' method
```

###Arguments

* Another default parameter of functions is the `arguments` array which contains all the arguments that were supplied when the function was invoked
* This means you don't have to know the exact number of arguments when you build a function because you can loop through all the arguments provided at invocation with the use of the default `arguments` array
```javascript
//inside the function
for (i = 0; i <arguments.length; i++) {
	dosomething;  //e.g. sum +=arguments[i]
}
```
* `arguments` **lacks all the array methods except .length** because of a bug

###Return

* When a function gets to a `return` statement, it returns immediately **without carrying out the remaining statements in the function**
* A function **always returns a `value`** or if unspecified, it returns `undefined`
* "If the function was invoked with the `new` prefix and the `return` value is not an object, then `this` (the new object) is returned instead."

###Exceptions

* A `throw` statement interrupts the execution of the code is used to handle expected exceptions like an incorrect type of argument (e.g. a string where a number is expected)
* Each `throw` statement should have an **exception object** with a `name` holding the type of exception and a `message` with an explanation of it





<a name="Chapter6"/>
##Chapter 6

Interesting article on prototypes: http://sporto.github.io/blog/2013/02/22/a-plain-english-guide-to-javascript-prototypes/


<a name="Chapter8"/>
##Chapter 8

##Appendix A - the Awful Parts

###Global variables
These are variables that are visible throughout the code in any scope. They can be **changed at any time** by any part of the program which makes them **unreliable in larger complex programs**. This can also lead to naming conflicts which can cause your code to fail or you to accidentally overwrite a global variable.