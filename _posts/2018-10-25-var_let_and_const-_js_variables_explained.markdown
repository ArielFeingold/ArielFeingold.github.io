---
layout: post
title:      "Var, Let and Const- JS variables explained"
date:       2018-10-25 13:52:31 +0000
permalink:  var_let_and_const-_js_variables_explained
---


In the beginning Var ruled as king in the Java Script world. But not all was good in the JS kingdom and declaring variables using Var caused issues and weaknesses that needed to be address. Fast forward to the arrival of ES2015 and the introduction of two new variable declarations- Let and Const, coming to save the day.

But what makes them different? When and where should we use them? This article will discuss each of the declarations with regards to their scope, hoisting and use. 

# VAR 
 
**Scope**

A scope in JavaScript defines what variables you have access to. There are two kinds of scopes:

* Global Scope
* Local Scope

Variables defined *outside a function* are in globally scope while variables defined *inside of a function* are in the local scope- each function when invoked creates a new scope.

Consider the following code:


```
var global = “My scope is global”

function localScope() { 
var local = "I am local"; 
console.log(local)
}

console.log(global); // “My scope is global”
console.log(local); // error: local is not defined

localScope() // “I am local”
```


Here “global” is globally scoped because it is declared and assigned outside of a function “local” on the other hand is declared within the function and only accessible from within it.

Using `let`, `const` or `var` in the Global scope will yield the same result. Things get a bit more complicated when we use `var` in Local Scope. In JavaScript, there are two kinds of local scope: *function scope* and* block scope*.


**Function Scope Vs. Block Scope**

Block is any code that’s within curly braces {}. The block scope is a subset of a function scope since functions need to be declared with curly braces (unless you're using arrow functions with an implicit return). Block statements like if and switch conditions or for and while loops, unlike functions, don't create a new scope.

Variables assigned with the `var` keyword have functional scope. The variable isn’t accessible outside of the function. However, these variables are not Block Scoped and so accessible anywhere within the function. That’s where things can get messy… 

Consider the following code:


```
Var greeting = “Hello”
Var counter = 3
If(counter > 0) { var greeting = “Goodbye”}

Console.log(greeting) // output: “Goodbye”
```

When using `var` you can reassign variables within a block, actually overriding parent assignments. That is not a problem if that is what you intended but it is easy to do it by mistake.


**Hoisting**

Hoisting is JavaScript's default behavior of moving declarations to the top of their scope before the code is executed. Note that only the declaration gets raised and not the assignment. If you’re unaware of hoisting, this can cause issues with no warning as your code won’t throw an error.

Consider the following code:


```
console.log (hello);
var hello = "Hello World"
```
Our code is actually interpreted like this:

```

Var hello 
Console.log(hello) // hello is undefined
Hello = “Hello World”
Console.log(hello) // “Hello World”
```

As you can see the variable is declared at the top and initialized with a value of undefined later to be assigned. In addition, `var` variables can be redeclared and reassigned. 

Due to the lax nature of `var` and the hoisting mechanism there is a greater chance of variable assignments and reassignments errors. That is a whole subject by itself, but it is important to keep a good practice of doing all variable assignment at the top of the scope and use the strict mode.


# LET & CONST# 
Now that we went through the behavior of Var and the possible pitfalls and gottchas. Lets turn our attention to Let & Const and how they combat those quirks and shortcomings.

**Scope**

Let & Const are blocked scoped. 

Let’s look at the example from before: 


```
let greeting = “Hello”
let counter = 3
If(counter > 0) { let greeting = “Goodbye”}

Console.log(greeting) // output: “Hello”
```


Because each assignment of “greeting” is scoped to its own block, greeting can only become “Goodbye” if there is an assignment with no new declaration.

Example:


```
let greeting = “Hello”
let counter = 3
If(counter > 0) { greeting = “Goodbye”}

Console.log(greeting) // output: “Goodbye”
```

**Hoisting**

Variables assigned with both const and let are not subject to hoisting much like those assigned with var. Variables with the var keyword are initialized with undefined. Variables assigned with const and let do not get initialized at all and will throw a Reference Error (not defined).

Consider:


```
Let man;
Man // RefrenceError {“man is not defined”};

Var man;
Man // undefined
```

The use of `const` and `let` in combination with "use strict" enforces correct variable assignment. This in turn reduces the risk of developer error.


**The difference between Let & Const**

Even though `const` and `let` share most of the same functionalities there is one quality that differentiate them and that is declaration & assignment.

`const` is short for constant. Variables assigned with `const` are read only and cannot be redeclared or reassigned:

```
const bar = “Foo”
bar = “FooBar” // output: TypeError {“Assignment to constant variable.”}
```

const variables are mutable – values of Objects and Arrays can be modified: 

```
Const fruits = [];
fruites.push(“Apple”)
fruits // Array(1)[“apple”]
```

Variables assigned with `let` cannot be redeclared, they can only be reassigned.


# Conclusion: What should I use and when?

In most cases you should try using `const` first. Using `const` helps you avoid assignment and declaration issues down the road because of const’s read-only behavior. Use let whenever you know the variable has a dynamic value. 

Even though Var is still part of the Java Script code I would recommend avoiding using it and opting for the new variables in order to avoid the pitfalls we’ve discussed before. Another added value in using let and const is code readability, it tells me if a certain variable value is supposed to change (let) or not (const).  









