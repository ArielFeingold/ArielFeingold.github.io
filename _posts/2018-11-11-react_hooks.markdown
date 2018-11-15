---
layout: post
title:      "React useState Hook"
date:       2018-11-11 10:16:33 -0500
permalink:  react_hooks
---


Hooks are a new feature proposal that lets you use state and other React features without writing a class.

# Why Hooks?
We know that components and top-down data flow help us organize a large UI into small, independent, reusable pieces. However, we often can’t break complex components down any further because the logic is stateful and can’t be extracted to a function or another component. These cases are very common and include: animations, form handling, connecting to external data sources, and many other things we want to do from our components.

In addition, JavaScript classes are really complicated to work with. The reason for that is that JavaScript’s this works a lot different than any other language’s this and leads to many bugs which are hard to debug and fix.

Hooks apply the React philosophy (explicit data flow and composition) inside a component, rather than just between the components to solve some of these problems.


# What are hooks?
Hooks make it possible to take a React function component and add state to it (using the `useState` hook), or hook into lifecycle methods like componentDidMount and componentDidUpdate ((using `useEffect` hook) without the need to turn it to a class component.


# The { useState }  hook
Let's start with an example, a simple class component:

```
import React from 'react'

class Counter extends Component {
  
	constructor(props) {
    super(props);
    this.state = {
      counter: 0
    }
    this.handleCounter =this.handleCounter.bind(this);
  }

  handleCounter (value)  {
    this.setState({
      counter: value
    });
  }

  render() {
    return (
      <div>
        <div>
  <button onClick={() => this.handleCounter(this.state.counter + 1)}> 	Increment
  </button>
        </div>
        <div>
          Count: {this.state.counter}
        </div>
      </div>
    )
  }
}

Export default Counter;
```


When using hooks our code will look like this:

```
import React, { useState } from 'react';

function counter() {
  
	const [count, setCount] = useState(0);
  
	return (
    <div>
      <div>
        <button onClick={() => setCount(count + 1)}> + Increment</button>
      </div>
      <div>
        Count: {count}
      </div>
    </div>
  );
}

export default counter;
```

the first thing you notice is that the code is much shorter and simple to read. We don’t need to create an event handler, no need to use setState and no need for a constructor.

useState lets you use mutable state within a component without the need for it to be a class component. This method requires only one argument- the initial state (in our case 0). Unlike with classes, the state doesn’t have to be an object. We can also keep a number or a string if that’s all we need.

`setState `returns a pair of values: 

1.	The current state
2.	A function that updates the state

That is why we write `const [ count, setCount ]`. They are equivalent to `“this.state.count”` & `this.setState ({ count: })`

Now we have an initial value set in a state object provided by React and a way to update that state and all that without the need to use a JS class.







