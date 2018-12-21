---
layout: post
title:      "Recursion & JavaScript"
date:       2018-12-21 19:45:16 +0000
permalink:  recursion_and_javascript
---


# What is Recursion
Recursion is one of the main concepts in programming. It is a technique for iterating over an operation by having a function call itself repeatedly until it arrives at a result. Most loops can be rewritten in a recursive style, and in some functional languages this approach to looping is the default.

However, while JavaScript’s functional coding style does support recursive functions, we need to be aware that most JavaScript compilers are not currently optimized to support them safely.

A factorial function is a good example. It returns the factorial of a supplied integer:

```
function factorial(x) {
  if (x < 0) return;
  if (x === 0) return 1;
  return x * factorial(x - 1);
}
factorial(3);
// 6

```
As you can see, the function is actually calling itself again ( factorial(x-1) ), but with a parameter that is one less than when it was called the first time. This makes it a recursive function.

# The Three Key Features of Recursion
All recursive functions should have three key features:


**1. A Termination Condition**

The Termination Condition is our recursion fail-safe. Think of it like your emergency brake. It’s put there in case of bad input to prevent the recursion from ever running. In our factorial example,` if (x < 0) return;` is our termination condition. It’s not possible to factorial a negative number and thus, we don’t even want to run our recursion if a negative number is input.

**2. A Base Case**

The Base Case is the goal of our recursive function. Base cases are usually within an if statement .In the factorial example,` if (x === 0) return 1; `is our base case. We know that once we’ve gotten x down to zero, we’ve succeeded in determining our factorial!

**3. The Recursion**

Simply put: Our function calling itself. In the factorial example,` return x * factorial(x — 1); `is where the recursion actually happens. We’re returning the value of the number x multiplied by the value of whatever factorial(x-1) evaluates to.

Now, remember, these are all nested function calls. When you have nested function calls, the most inner nested function returns first. This initiates the ‘unwinding’ as they return in order.


# When to Use Recursion
Recursion is best applied when you need to call the same function repeatedly with different parameters from within a loop. While it can be used in many situations, it is most effective for solving problems involving iterative branching, such as fractal math, sorting, or traversing the nodes of complex or non-linear data structures.

One reason that recursion is favored in functional programming languages is that it allows for the construction of code that doesn’t require setting and maintaining state with local variables. Recursive functions are also naturally easy to test because they are easy to write in a pure manner, with a specific and consistent return value for any given input, and no side effects on external variable states.






