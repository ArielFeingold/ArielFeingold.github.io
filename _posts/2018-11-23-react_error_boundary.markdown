---
layout: post
title:      "React Error Boundary"
date:       2018-11-23 17:16:26 +0000
permalink:  react_error_boundary
---


# Why do we need Error Boundaries?
Before React 16 was introduced errors that occurred during the render process were very hard to handle. Due to the declarative nature of React these kinds of render based errors could not be caught and often caused unexpected behaviors in the UI and cryptic console errors that did not reference even a single line of your own code:

```
TypeError: Cannot read property 'getHostNode' of null ?
  at getHostNode(~/react/lib/ReactReconciler.js:64:0)
  at getHostNode(~/react/lib/ReactCompositeComponent.js:383:0) ?
  at getHostNode(~/react/lib/ReactReconciler.js:64:0)
  at getHostNode(~/react/lib/ReactChildReconciler.js:114:0)?
  at updateChildren(~/react/lib/ReactMultiChild.js:215:0)
  at _reconcilerUpdateChildren(~/react/lib/ReactMultiChild.js:314:0)
  at _updateChildren(~/react/lib/ReactMultiChild.js:301:0)
  at updateChildren(~/react/lib/ReactDOMComponent.js:942:0)
  at _updateDOMChildren(~/react/lib/ReactDOMComponent.js:760:0) ?
  at updateComponent(~/react/lib/ReactDOMComponent.js:718:0)
  at receiveComponent(~/react/lib/ReactReconciler.js:126:0)
  at receiveComponent(~/react/lib/ReactCompositeComponent.js:751:0) ?
  at _updateRenderedComponent(~/react/lib/ReactCompositeComponent.js:721:0)
  at _performComponentUpdate(~/react/lib/ReactCompositeComponent.js:642:0)
  at updateComponent(~/react/lib/ReactCompositeComponent.js:544:0)
  at receiveComponent(~/react/lib/ReactReconciler.js:126:0) ?
  at receiveComponent(~/react/lib/ReactCompositeComponent.js:751:0)
  at _updateRenderedComponent(~/react/lib/ReactCompositeComponent.js:721:0)
  at _performComponentUpdate(~/react/lib/ReactCompositeComponent.js:642:0)
  at updateComponent(~/react/lib/ReactCompositeComponent.js:544:0)
  at receiveComponent(~/react/lib/ReactReconciler.js:126:0)  ?
  at receiveComponent(~/react/lib/ReactCompositeComponent.js:751:0)
  at _updateRenderedComponent(~/react/lib/ReactCompositeComponent.js:721:0)
  at _performComponentUpdate(~/react/lib/ReactCompositeComponent.js:642:0)
  at updateComponent(~/react/lib/ReactCompositeComponent.js:544:0) ?
  at receiveComponent(~/react/lib/ReactReconciler.js:126:0)
  at receiveComponent(~/react/lib/ReactCompositeComponent.js:751:0) ?
  at _updateRenderedComponent(~/react/lib/ReactCompositeComponent.js:721:0)
  at _performComponentUpdate(~/react/lib/ReactCompositeComponent.js:642:0)
  at updateComponent(~/react/lib/ReactCompositeComponent.js:558:0)
  at performUpdateIfNecessary(~/react/lib/ReactReconciler.js:158:0)
  at performUpdateIfNecessary(~/react/lib/ReactUpdates.js:151:0) ?
  at call(~/react/lib/Transaction.js:138:0)
  at call(~/react/lib/Transaction.js:138:0)
  at call(~/react/lib/ReactUpdates.js:90:0)
  at perform(~/react/lib/ReactUpdates.js:173:0)
  at call(~/react/lib/Transaction.js:204:0)
  at closeAll(~/react/lib/Transaction.js:151:0)
  at perform(~/react/lib/ReactDefaultBatchingStrategy.js:63:0) ?
  at batchedUpdates(~/react/lib/ReactUpdates.js:98:0)
  at batchedUpdates(~/react/lib/ReactEventListener.js:150:0)
```

On September 2017 React 16 was officially released. React 16 is an API-compatible, under-the-hood rewrite of the React internals. One goal of the architecture is handling errors with a new, more correct and rigorous strategy that makes render errors manageable by unmounting the entire component tree when errors are thrown inside render or lifecycle methods. While this behavior makes certain errors in React components easier to understand and fix, it also means that existing problems that may have been failing silently or non-catastrophically will now cause the entire app to unmount. 

To solution to this is React’s new component- ERROR BOUNDARIES.
You use this new tool by wrapping a component that might emit errors with an Error Boundary an component that catches the error once it happens. 

Any component can become an Error Boundary component by using a new lifecycle method called- componentDidCatch:

componentDidCatch(error, info)

this method takes two arguments:

1. error: the actual error message that tells you what went wrong.
2. info: additional details about the error including the stack trace to help you debug the error.

# How to use Error Boundries
Lets create a “buggy” component:

```
    class Buggy extends React.Component {
      state = { greeting: "Hello World"};

      componentDidMount() {
        throw new Error("An error has occured in Buggy component!");
      }

      render() {
        return <h2>{this.state.greeting}</h2>;
      }
    }

```

This component will throw a new error on each render. When you try rendering this component, nothing happens, and the user gets no feedback on what is going on.

Let’s create an Error Boundary component:

```
class ErrorBoundary extends React.Component {
      state = { error: null, errorInfo: null };

      componentDidCatch(error, errorInfo) {
        this.setState({
          error: error,
          errorInfo: errorInfo
        });
      }

      render() {
        if (this.state.errorInfo) {
          return (
            <div>
              <h2>Something went wrong.</h2>
              <details style={{ whiteSpace: "pre-wrap" }}>
                {this.state.error && this.state.error.toString()}
                <br />
                {this.state.errorInfo.componentStack}
              </details>
            </div>
          );
        }

        return this.props.children;
      }
    }
```

Now let’s wrap the buggy component with the Error Boundary:

```
	const App = () => (
		<div style={styles}>

			<h2>Error Boundaries Example</h2>
			<ErrorBoundary>
				<Buggy />
			</ErrorBoundary>
		</div>
	);
```

The error Boundary component will return the “buggy” component (using the children prop) unless it catches an error. If an error is detected in one of the children the error Boundary receives the error and error info, set them in state and then renders the content to the browser.

# Error Boundary Scope and use
When you wrap a component tree with a single Error Boundary, the entire tree gets affected by errors caught in just one of the components (but not the component’s own errors). 

For that reason, it is recommended to wrap components at the granularity they can be independently useful. This means that when one errors, the others can still accomplish their purpose. Wrapping your page content will protect your header or sidebar navigation components from being unmounted on an error and will give the user an easy way to back out and return to working parts of your application. By contrast, going to the extreme and wrapping every individual input component of a form could leave someone in a UI state that still responds to their input but can’t actually be used to finish their goal.




