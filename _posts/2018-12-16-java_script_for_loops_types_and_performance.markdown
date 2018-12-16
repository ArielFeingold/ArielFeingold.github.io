---
layout: post
title:      "Java Script For Loops. Types and Performance "
date:       2018-12-16 19:10:08 +0000
permalink:  java_script_for_loops_types_and_performance
---


Loops are used in programming to automate repetitive tasks. The most basic types of loops used in JavaScript are the while and do...while statements, which you can review in "How To Construct While and Do...While Loops in JavaScript."

Because while and do...while statements are conditionally based, they execute when a given statement returns as evaluating to true. Similar in that they are also conditionally based, for statements also include extra features such as a loop counter, allowing you to set the number of iterations of the loop beforehand.

In this tutorial, we will learn about the for statement, including the for...of and for...in statements, which are essential elements of the JavaScript programming language.

# Classic For Loop

The classic for statement is a type of loop that will use up to three optional expressions to implement the repeated execution of a code block.

Let's take a look at an example of what that means:

```
for (let i = 0; i < 4; i++) {
    // Print each iteration to the console
    console.log(i);
}
```

**First Expression: Initialization**

```
let i = 0;
```

Our first expression is the initialization. We're declaring a variable called i with the let keyword (the keyword var may also be used) and giving it a value of 0. While the variable can be named anything, i is most frequently used. The variable i stands for iteration, is consistent, and keeps the code compact.

**Second Expression: Condition** 

```
 i < 4;
```

Just as in the while and do...while loops, for loops usually contain a condition.  We already established that our iteration variable, i, represents 0 to start. Now we are saying that the condition is true as long as i is less than 4 in this example.

**Final Expression**

```
i++
```

The final expression is a statement that is executed at the end of each loop. It is most often used to increment or decrement a value, but it can be used for any purpose.

In our example, we are incrementing the variable by one, with i++. This is the same as running i = i + 1.
Unlike the initialization and condition expressions, the final expression does not end with a semicolon.

# ES6 For...in & For...of Loops
ES6 introduced a new syntex for For loops. for...of & for...in. These loops give us a very clean and concise syntax to iterate over all kinds of iterables and enumerables like strings, arrays and object literals.

they both work the same way but used for different type of JS objects.

**For...In Loop**

The for...in statement iterates over the properties of an **object**. To demonstrate, we will make a simple sandwich object with a few name:value pairs.

```
const sandwich = {
    bread: "rye",
    spread: "mayo",
		meat: "balogna"
}
```

Using the for...in loop, we can easily access each of the property names and values.

```
for (attribute in sandwich) {
    console.log(`${attribute}`: ${sandwich[attribute]}`);
}
```

**For...of Loop**

The for...in statement is useful for iterating over object properties, but to iterate over iterable objects like arrays and strings, we can use the for...of statement.

```
let str = 'Turn the page';
let animals = ['🐔', '🐷', '🐑', '🐇'];
let names = ['Gertrude', 'Henry', 'Melvin', 'Billy Bob'];

for (let animal of animals) {
  // Random name for our animal
  let nameIdx = Math.floor(Math.random() * names.length);

  console.log(`${names[nameIdx]} the ${animal}`);
}

// Henry the 🐔
// Melvin the 🐷
// Henry the 🐑
// Billy Bob the 🐇
```

# Performance 
While the new ES6 loops provide a more concise and simple syntex a test done by [Incredible Web](https://www.incredible-web.com/blog/performance-of-for-loops-with-javascript/) found that in terms of performace they require much more resources and take longer to finish. This is probably due to the fact that the loop control mechanism is more coplicated and needs to guess the leangth of the string. In fact thier test concliuded that a reverse for loop is the most efficiant ( a loop that is initialized at the array length and then decriments down). It is important to note that the larger/longer the iterated object is the less impact the loop control mechanism has. Therefor I would say that if you are working with small objects it would be better to use the classic loop but if we are dealing with large arrays it might be better to use the new loops for their better syntex. 





