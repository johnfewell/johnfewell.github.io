---
layout: post
title:      "JS Hoisting and Scope"
date:       2018-07-19 16:32:22 -0400
permalink:  js_hoisting_and_scope
---



From the beginning of Javascript until the release of ES6, there was only one way to declare a variable: `var`. 
For those new to Javascript, `var` has some unusual characteristics.

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

```
var y = 5
var x = function(){
   
	 var y; // hoisted variable declaration. Javascript is always doing this in the background!
	 
   console.log(y);
	 
   y = 1; // value assignment
}

x() => undefined
```

`var` returns undefined instead of an error when it has been defined but not yet assigned a value, which complicates debugging. The definition of `var` is always hoisted to the top of the function with the default value of undefined, and only assigned a value on the line of code where the value is defined. The definition of the variable is hoisted, the value assignment is not. Therefore, it's a best practice to move `var` definitions to the top of a function manually to make it clear when in the function the variable actually has a value. 

JS has lexical scoping. Variables declared outside of functions and blocks are available inside. 

## Const and Let

ES 6 brought us two new ways to define variables: `const` and `let`.
Before const, you were able to define constants using `Object.defineProperty()`, but when you tried to reassign the constant, it would give you no error, and the constant would go on retaining its initial definition. 

`const` on the other hand only allows a variable to be declared once. 
Other variable have block scope.

`let` has block scope and doesn't hoist. 
