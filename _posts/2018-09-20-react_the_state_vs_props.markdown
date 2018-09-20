---
layout: post
title:      "React: The State Vs. Props"
date:       2018-09-20 15:23:21 +0000
permalink:  react_the_state_vs_props
---


In this post I would like to talk about two important elements in React: Props (short for properties) and State. 

Props and state are related. The state of one component will often become the props of a child component and passed to the child component within the render method of the parent. 

But before comparing the two let’s see what defines each.

**What are Props?**

Props are used to customize Component when it’s being created and give it different parameters. 
Components are like JavaScript functions, Props are arbitrary inputs that the component accepts and then return React elements describing what should appear on the screen. This allows us to create a component that can be customized with a new set of props every time we use it. 

Props are passed to the component and are fixed throughout its lifecycle. But there are cases when we want to use data that we know is going to change overtime. In this case we use something called state.

**What is State?**

A state in React Component is its own local state, the state cannot be accessed and modified outside the component and can only be used inside the component , similar to a function own local scope. We can define variables inside the function which can only be used inside the function block scope.

**Conclusion**

Props and State do similar things but are used in different ways. Props are used to pass data from parent to child or by the component itself. They are immutable and thus will not be changed.
State is used for mutable data, or data that will change.

Worth mentioning is that when implementing Redux in your app, the state is created once for the root component and then accessed in a read-only format by other components using mapStateToProps. Redux basically creates props out of the global state to be used as props.
