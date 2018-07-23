---
layout: post
title:      "Hoisting, Scope, Closures, and Anonymous vs Named Functions in JavaScript"
date:       2018-07-19 16:32:22 -0400
permalink:  js_hoisting_and_scope
---



From the beginning of JavaScript until the release of ES6, there was only one way to declare a variable: `var`. 
For those new to JavaScript, `var` has some unusual characteristics.

When you define a variable using `var`, it is defined in **function scope**. `var` declarations respect function boundaries, but not block boundaries.

```
var y = 5
var x = function(){
   console.log(y);
   var y = 1;
}

x() => undefined
```

`y` returns undefined. But why? Because of hoisting. It becomes clear once you manually hoist the defined variable to the top of the function.
## Hoisting
The term hoisting is a made-up term which doesn't appear in the offical JavaScript specification. Instead it's a metaphor to describe how the complier works when interpreting the source code. 

```
var y = 5
var x = function(){
  
  var y; // hoisted variable declaration. The compilier is always doing this in the background on its first pass through the code.
 
  console.log(y);
	 
  y = 1; // value assignment
}

x() => undefined
```

`var` returns undefined instead of an error when it has been defined but not yet assigned a value, which complicates debugging. The definition of `var` is always hoisted to the top of the function with the default value of undefined, and only assigned a value on the line of code where the value is defined. The definition of the variable is hoisted, the value assignment is not. Therefore, it's a best practice to move `var` definitions to the top of a function manually to make it clear when in the function the variable actually has a value. 
## Const and Let

ES 6 brought us two new ways to define variables: `const` and `let`.
Before const, you were able to define constants using `Object.defineProperty()`, but when you tried to reassign the constant, it would give you no error, and the constant would go on retaining its initial definition. 

`const` on the other hand only allows a variable to be declared once. 

`let` has block scope and doesn't hoist but have it's value reassigned like `var`. 

## Closure

> Closure is when a function is able to remember and access its lexical scope even when that function is executing outside its lexical scope. - Kyle Simpson

That succient quote requires unpacking. JavaScript has lexical scoping which means that variables declared outside of functions and blocks are available inside. Or to put it the opposite way, variables are only accessable within the scope they are defined, otherwise know as private variables to the function or block. 

An example of the use of closures in JavaScript are callback functions, which the MDN webdocs defines accordingly:

> A callback function is a function passed into another function as an argument, which is then invoked inside the outer function to complete some kind of routine or action.

```
function greeting(name) {
  alert('Hello ' + name);
}

function processUserInput(callback) {
  var name = prompt('Please enter your name.');
  callback(name);
}

processUserInput(greeting);
```
This is an example of a synchronous callback. 

## Named vs Anonymous Functions

### Anonymous functions
Anonymous functions are functions without a name identifier, for example:

```
const thisFunctionHasNoName = function() {
  console.log('Hello, I'm anonymous')
}
```

They are not pre-compiled, but rather are only compiled when called. 

### Named functions 

The first benefit to naming functions is that it can make debugging easier.  In a nasty debugging situation, it can help track down the source of an error rather than seeing a number of `anonymous` funcitons in the call stack.
If you need to call the function from inside the function itself, as is the case with recursive functions, then the function needs to be named. 

