---
layout: post
title:      "A Guide to Custom Elements for React Developers"
date:       2019-01-25 20:31:52 +0000
permalink:  a_guide_to_custom_elements_for_react_developers
---


Custom elements can offer the same general benefits of React components without being tied to a specific framework implementation. A custom element gives us a new HTML tag that we can programmatically control through a native browser API.

Let's talk about the benefits of component-based UI:
* Encapsulation - concerns scoped to that component remain in that component's implementation

* Reusability - when the UI is separated into more generic pieces, they're easier to break into patterns that you're more likely to repeat

* Isolation - because components are designed to be encapsulated and with that, you get the added benefit of isolation, which allows you scope bugs and changes to a particular part of your application easier

# Use cases
You might be wondering who is using custom elements in production. Notably:
* GitHub is using custom elements for their modal dialogs, autocomplete and display time.

* YouTube's new web app is built with Polymer and web components.

# Similarities to the Component API
When trying to compare React Components versus custom elements, I found the APIs really similar:
* They're both classes that aren't "new" and are able that extend a base class

* They both inherit a mounting or rendering lifecycle

* They both take static or dynamic input via props or attributes

# Demo 
So, let's build a tiny application that lists details about a GitHub repository.

![https://imgur.com/fN8aLAa]

If I were going to approach this with React, I would define a simple component like this:

```
<Repository name="charliewilco/obsidian" />
```

This component takes a single prop — the name of the repository — and we implement it like this:

```
class Repository extends React.Component {
  state = {
    repo: null
  };

  async getDetails(name) {
    return await fetch(`https://api.github.com/repos/${name}`, {
      mode: 'cors'
    }).then(res => res.json());
  }

  async componentDidMount() {
    const { name } = this.props;
    const repo = await this.getDetails(name);
    this.setState({ repo });
  }

  render() {
    const { repo } = this.state;

    if (!repo) {
      return <h1>Loading</h1>;
    }

    if (repo.message) {
      return <div className="Card Card--error">Error: {repo.message}</div>;
    }

    return (
      <div class="Card">
        <aside>
          <img
            width="48"
            height="48"
            class="Avatar"
            src={repo.owner.avatar_url}
            alt="Profile picture for ${repo.owner.login}"
          />
        </aside>
        <header>
          <h2 class="Card__title">{repo.full_name}</h2>
          <span class="Card__meta">{repo.description}</span>
        </header>
      </div>
    );
  }
}
```

To break this down further, we have a component that has its own state, which is the repo details. Initially, we set it to be null because we don't have any of that data yet, so we'll have a loading indicator while the data is fetched.

During the React lifecycle, we'll use fetch to go get the data from GitHub, set up the card, and trigger a re-render with setState() after we get the data back. All of these different states the UI takes are represented in the render() method.

# Defining / Using a Custom Element

Doing this with custom elements is a little different. Like the React component, our custom element will take a single attribute — again, the name of the repository — and manage its own state.

Our element will look like this:

```
<github-repo name="charliewilco/obsidian"></github-repo>
<github-repo name="charliewilco/level.css"></github-repo>
<github-repo name="charliewilco/react-branches"></github-repo>
<github-repo name="charliewilco/react-gluejar"></github-repo>
<github-repo name="charliewilco/dotfiles"></github-repo>
```

To start, all we need to do to define and register a custom element is create a class that extends the HTMLElement class and then register the name of the element with customElements.define().

```
class OurCustomElement extends HTMLElement {}
window.customElements.define('our-element', OurCustomElement);
```

And we can call it:

```
<our-element></our-element>
```

This new element isn't very useful, but with custom elements, we get three methods to expand the functionality of this element. These are almost analogous to React’s lifecycle methods for their Component API. The two lifecycle-like methods most relevant to us are the disconnectedCallBackand the connectedCallback and since this is a class, it comes with a constructor.

**constructor**

An instance of the element is created or upgraded. Useful for initializing state, settings up event listeners, or creating Shadow DOM. See the spec for restrictions on what you can do in the constructor.

**connectedCallback**

The element is inserted into the DOM. Useful for running setup code, such as fetching resources or rendering UI. Generally, you should try to delay work until this time

**disconnectedCallback**

When the element is removed from the DOM. Useful for running clean-up code.

To implement our custom element, we'll create the class and set up some attributes related to that UI:

```
class Repository extends HTMLElement {
  constructor() {
    super();

    this.repoDetails = null;

    this.name = this.getAttribute("name");
    this.endpoint = `https://api.github.com/repos/${this.name}`    
    this.innerHTML = `<h1>Loading</h1>`
  }
}
```

By calling super() in our constructor, the context of this is the element itself and all the DOM manipulation APIs can be used. So far, we've set the default repository details to null, gotten the repo name from element's attribute, created an endpoint to call so we don't have to define it later and, most importantly, set the initial HTML to be a loading indicator.

In order to get the details about that element’s repository, we’re going to need to make a request to GitHub’s API. We’ll use fetch and, since that's Promise-based, we'll use async and await to make our code more readable. You can learn more about the async/await keywords here and more about the browser's fetch API here. You can also tweet at me to find out whether I prefer it to the Axios library. (Hint, it depends if I had tea or coffee with my breakfast.)

Now, let's add a method to this class to ask GitHub for details about the repository.

```
class Repository extends HTMLElement {
  constructor() {
    // ...
  }

  async getDetails() {
    return await fetch(this.endpoint, { mode: "cors" }).then(res => res.json());
  }
}
```

Next, let's use the connectedCallback method and the Shadow DOM to use the return value from this method. Using this method will do something similar as when we called Repository.componentDidMount() in the React example. Instead, we'll override the null value we initially gave this.repoDetails — we'll use this later when we start to call the template to create the HTML.

```
class Repository extends HTMLElement {
  constructor() {
    // ...
  }

  async getDetails() {
    // ...
  }

  async connectedCallback() {
    let repo = await this.getDetails();
    this.repoDetails = repo;
    this.initShadowDOM();
  }

  initShadowDOM() {
    let shadowRoot = this.attachShadow({ mode: "open" });
    shadowRoot.innerHTML = this.template;
  }
}
```

You'll notice that we're calling methods related to the Shadow DOM. Besides being a rejected title for a Marvel movie, the Shadow DOM has its own rich API worth looking into. For our purposes, though, it's going to abstract the implementation of adding innerHTML to the element.

Now we're assigning the innerHTML to be equal to the value of this.template. Let's define that now:

```
class Repository extends HTMLElement {
  get template() {
    const repo = this.repoDetails;
  
    // if we get an error message let's show that back to the user
    if (repo.message) {
      return `<div class="Card Card--error">Error: ${repo.message}</div>`
    } else {
      return `
      <div class="Card">
        <aside>
          <img width="48" height="48" class="Avatar" src="${repo.owner.avatar_url}" alt="Profile picture for ${repo.owner.login}" />
        </aside>
        <header>
          <h2 class="Card__title">${repo.full_name}</h2>
          <span class="Card__meta">${repo.description}</span>
        </header>
      </div>
      `
    }
  }
```

That's pretty much it. We've defined a custom element that manages its own state, fetches its own data, and reflects that state back to the user while giving us an HTML element to use in our application.

After going through this exercise, I found that the only required dependency for custom elements is the browser's native APIs rather than a framework to additionally parse and execute. This makes for a more portable and reusable solution with similar APIs to the frameworks you already love and use to make your living.

There are drawbacks of using this approach, of course. We're talking about various browser support issues and some lack of consistency. Plus, working with DOM manipulation APIs can be very confusing. Sometimes they are assignments. Sometimes they are functions. Sometimes those functions take a callback and sometimes they don't. If you don't believe me, take a look at adding a class to an HTML element created via document.createElement(), which is one of the top five reasons to use React. The basic implementation isn’t that complicated but it is inconsistent with other similar document methods.

The real question is: does it even out in the wash? Maybe. React is still pretty good at the things it's designed to be very very good at: the virtual DOM, managing application state, encapsulation, and passing data down the tree. There's next to no incentive to use custom elements inside that framework. Custom elements, on the other hand, are simply available by virtue of building an application for the browser.







