---
layout: post
title:      "React Basics part I"
date:       2018-10-31 19:03:26 +0000
permalink:  react_basics_part_i
---


# What Is React 

React is a declarative, efficient, and flexible JavaScript library for building user interfaces. It lets you compose complex UIs from small and isolated pieces of code called “components”.
Developed by Facebook in 2011 and open-sourced in 2015, it has one of the largest communities supporting it.

It is important to understand that React is not a language. At the base of it it's JavaScript functions and classes that are usually written using a syntax extension called JSX (more on that to come). React is also not a complete MVC architecture pattern as it is only handling the V part of the MVC (Model, View, Controller). 

In this post I will go through some basic concepts in react- JSX, Virtual DOM, Elements and Components.

**JSX**

JSX is a shorthand for JavaScript XML. This is a type of file used by React which utilizes the expressiveness of JavaScript along with HTML like template syntax. Even though using JSX is not required most people find that it makes the HTML file really easy to understand. This file makes applications robust and boosts its performance by allowing React to show more useful error and warning messages. Below is an example of JSX:

```
render(){
    return(        
         <div>
            <h1> Hello World!</h1>
         </div>
    );
}
```

**Virtual DOM**

DOM manipulation is the heart of the modern, interactive web. Unfortunately, it is also a lot slower than most JavaScript operations a problem that is made worst by the fact that most JavaScript frameworks update the DOM much more than they have to because they rebuild the entire DOM with each change. To address this problem, the people at React came up with something called the virtual DOM.

A virtual DOM is a lightweight JavaScript object which originally is just the copy of the real DOM. When you render a React element it updates the entire Virtual DOM, not the real one. This process is much less resource heavy than updating the entire real DOM. Once the virtual DOM is updated React compares the new DOM to the old one (this process is called “diffing”) and only updates the parts of the real DOM that were actually changed and only those.

The use of virtual DOM makes React quicker and more efficient.

**Element**

Elements are the smallest building blocks of React apps. It describes what you see on the screen.

`const element = <h1>Hello, world</h1>;`

Unlike browser DOM elements, React elements are plain objects, and are cheap to create. ReactDOM takes care of updating the DOM to match the React elements.   

**Components**

Components are the building blocks of a React application’s UI. These components split up the entire UI into small independent and reusable pieces. Then it renders each of these components independent of each other without affecting the rest of the UI.

*Class components and functional components*

This is the simplest form of a component:

```
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}
```

This is called a “function component” because that’s what it is- a JS function.

You can also define a component using ES6 classes:

```
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```

These “class components” offer more features- mainly allows the component to be stateful and use life cycle events.  

*Components are pure functions*

All React components must act like pure functions with respect to their props. That means they do not attempt to change their inputs, and always return the same result for the same inputs.

*Composing and Extracting Components*

Components can refer to other components in their output. This lets us use the same component abstraction for any level of detail. 

```
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

function App() {
  return (
    <div>
      <Welcome name="Sara" />
      <Welcome name="Cahal" />
      <Welcome name="Edite" />
    </div>
  );
}
```

This allows us to create a palette of reusable components. A good rule of thumb is that if a part of your UI is used several times (Button, Panel, Avatar), or is complex enough on its own, it is a good candidate to be a reusable component.




