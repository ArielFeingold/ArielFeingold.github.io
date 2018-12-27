---
layout: post
title:      "Implementing Memoization in JavaScript"
date:       2018-12-27 21:03:46 +0000
permalink:  implementing_memoization_in_javascript
---


In computing, memoization or memoisation is an optimization technique used primarily to speed up computer programs by storing the results of expensive function calls and returning the cached result when the same inputs occur again.

Programs often waste time calling functions which recalculate the same results over and over again. This is particularly true with recursive and mathematical functions. A perfect example of this is the Fibonacci number generator.

The Fibonacci sequence is a series of integers, beginning with zero and one, in which each value is the sum of the previous two numbers in the series. you can create a recursive function to calculate this:

```
function fibonacci(n) {
  if (n === 0 || n === 1)
    return n;
  else
    return fibonacci(n - 1) + fibonacci(n - 2);
}
```

This function will run well on small numbers but as our n increses performabce quickly degrades. The reason for that is that the fuction's two recursive calls repeat the same work. for example to compute the 50th Fibonacci number, the recursive function must be called over 40 billion times and doubles if we need the 51st number...

# Memoization Basics
If we have a CPU intensive operation, we can optimize the usage by storing the result of the initial operation in the cache. If the operation is bound to be carried out again, we won’t go to the hassle of boring out our CPU again, since the result of the same result was stored somewhere, we just simply return the result.

Lets implement this logic to the Fibonacci function:

```
var fibonacci = (function() {
  var memo = {};

  function f(n) {
    var value;

    if (n in memo) {
      value = memo[n];
    } else {
      if (n === 0 || n === 1)
        value = n;
      else
        value = f(n - 1) + f(n - 2);

      memo[n] = value;
    }

    return value;
  }

  return f;
})();

```
In the example, a self-executing anonymous function returns an inner function, f(), which is used as the Fibonacci function. When f() is returned, its closure allows it to continue to access the “memo” object, which stores all of its previous results. Each time f() is executed, it first checks to see if a result exists for the current value of “n”. If it does, then the cached value is returned. Otherwise, the original Fibonacci code is executed. Note that “memo” is defined outside of f() so that it can retain its value over multiple function calls. Recall that the original recursive function was called over 40 billion times to compute the 50th Fibonacci number. By implementing memoization, this number drops to 99.

# Limitations
**Only a pure function can be memoized**

The biggest limitation is that only a Pure Function can be memoized.

A pure function is a function that:

* Returns same output for the same input, every time.
* Doesn’t depend on and doesn’t modify the state of the variables out of its scope. This includes having no side-effects like logging(console.log) or any DOM manipulation.

Keep in mind that memoizing is a trade-off between added space and added speed and thus only significant for functions having a limited input range so that cached values can be made use of more frequently. It may not be ideal for infrequently called or fast executing functions.
