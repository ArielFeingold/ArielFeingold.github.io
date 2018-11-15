---
layout: post
title:      "Semicolons JS cheatsheet"
date:       2018-11-15 17:33:14 +0000
permalink:  semicolons_js_cheatsheet
---


Semicolons in JavaScript divide the community. Some prefer to use them always, no matter what. Others like to avoid them. The choice is possible because JavaScript does not strictly require semicolons. When there is a place where a semicolon was needed, it adds it behind the scenes.

But what are the rules of adding semicolons if you do want to use them? 

Here is a handy cheat sheet:

# Semicolons are needed at the end of JavaScript 'statements'

Many things qualify as statements in JS here are some examples:

```
// variable declaration & assigment (assigning a value, object or a function to a name)
let counter = 0;
const obj = {}; 
var doSomething = function(){ ...}; 

// method call
console.log('hello world'); 

// function call
doSomething();
```

# Semicolons are required when two statements are on the same line

it is important to note that while most of the times the semicolons are optional they are required if there are two statements on the same line of code:

```
var i = 0; i++        // <-- semicolon obligatory
                      //     (but optional before newline)
var i = 0             // <-- semicolon optional
    i++               // <-- semicolon optional
```

# Avoid semicolons after curly brackets

You shouldn’t put a semicolon after a closing curly bracket }. The only exceptions are assignment statements, such as const obj = {}; see above.

Exapmles:

```
// NO semicolons after }:
if  (...) {...} else {...}
for (...) {...}
while (...) {...}
function (arg) { /*do this*/ } 
```

# …one more thing

One more place we need a semicolon is with a for loop. We use them to separate the iteration rules within the parentheses like this: (i=0; i<array.length; i++). A semicolon comes after the first and second iteration rules but not the third.

