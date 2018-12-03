---
layout: post
title:      "React useEffect hook"
date:       2018-12-03 00:57:44 +0000
permalink:  react_useeffect_hook
---


In a previous post I’ve discussed what are hooks and why to use them and also talked about the useState hook. Today I want to talk about another hook presented by the React team: useEffect.

# What are Effects?
Even if you’ve never heard of the term before, you more than likely used effects if you programmed in React. Whenever we want to run some additional code after React has updated the code, for example do a fetch call, setting up subscriptions or manually changing the DOM in React we use effects. Up until now these actions could only be done in a Class type component using life cycle methods. useEffect provides us with a way to do so in a function component. Think of it as componentDidMount, componentDidUpdate, and componentWillUnmount combined. 

The useEffect hook tells React that the component needs to do something after render. It will remember the function you passed (that is the “effect”) and will call it by default after each time the render method is finished. It is also important to notice that the function passed to useEffect is different on every render. This is intentional, it lets us read the variables passed from inside the effect without the fear of it getting stale. This makes the effects behave more like a part of the render result — each effect “belongs” to a particular render. 

Effects are called inside a component. This puts the method inside the component scope and gives it access to the component state and props without the need for a special react API.

# Effects without cleanup
There are two common types of effect, the first type does not require cleanup, we can run it and forget about it. Example for those are network requests and DOM manipulations. 

In React class components, the render method itself shouldn’t cause side effects. It would be too early — we typically want to perform our effects after React has updated the DOM. In order to do that we use the componentDidMount & the componentDidUpdate methods. This however requires us to duplicate our code even though conceptually we want it to happen after each render, no matter if it was the first time the component was mounted or not. useEffect eliminates this and will run each time the component is rendered.

# Example using Class
```
class Example extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      data: null
    };
  }

  componentDidMount() {
    document.title = `You clicked ${this.state.count} times`;
  }

  componentDidUpdate() {
    document.title = `You clicked ${this.state.count} times`;
  }

  render() {
    return (
      <div>
        <p>You clicked {this.state.count} times</p>
        <button onClick={() => this.setState({ count: this.state.count + 1 })}>
          Click me
        </button>
      </div>
    );
  }
}
```

In this example we directly change the DOM on each click. Notice how the code is duplicated in componentDidMount & componentDidUpdate. 

Example using hooks:

```
function Example() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    document.title = `You clicked ${count} times`;
  });

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```

With hooks we don’t need to duplicate our code and it will run after each render completion. 

# Effects with Cleanup

The second type of effects are the ones that keep running in the background while the component is mounted. Examples for that are subscriptions or interval network calls. 

In a React class component we would typically set these up in the componentDidMount and then remove it using componentWillUnmount

```
class FriendStatus extends React.Component {
  constructor(props) {
    super(props);
    this.state = { isOnline: null };
    this.handleStatusChange = this.handleStatusChange.bind(this);
  }

  componentDidMount() {
    ChatAPI.subscribeToFriendStatus(
      this.props.friend.id,
      this.handleStatusChange
    );
  }

  componentWillUnmount() {
    ChatAPI.unsubscribeFromFriendStatus(
      this.props.friend.id,
      this.handleStatusChange
    );
  }

  handleStatusChange(status) {
    this.setState({
      isOnline: status.isOnline
    });
  }

  render() {
    if (this.state.isOnline === null) {
      return 'Loading...';
    }
    return this.state.isOnline ? 'Online' : 'Offline';
  }
}
```

Notice how componentDidMount and componentWillUnmount need to mirror each other. Lifecycle methods force us to split this logic even though conceptually code in both of them is related to the same effect. 

Hooks eliminates this behavior by keeping both setup and cleanup together:

```
import { useState, useEffect } from 'react';

function FriendStatus(props) {
  const [isOnline, setIsOnline] = useState(null);

  function handleStatusChange(status) {
    setIsOnline(status.isOnline);
  }

  useEffect(() => {
    ChatAPI.subscribeToFriendStatus(props.friend.id, handleStatusChange);
    // Specify how to clean up after this effect:
    return function cleanup() {
      ChatAPI.unsubscribeFromFriendStatus(props.friend.id, handleStatusChange);
    };
  });

  if (isOnline === null) {
    return 'Loading...';
  }
  return isOnline ? 'Online' : 'Offline';
}

```

