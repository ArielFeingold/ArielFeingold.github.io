---
layout: post
title:      "Passing props to React Router's Link component"
date:       2019-01-06 18:30:42 +0000
permalink:  passing_props_to_react_routers_link_component
---


Often times when building an app with React Router you’ll need to pass props through a Link component to the new route. In this post, we’ll break down how that process works.

There are two different ways to pass data from a Link component through to the new route that’s being rendered. The first is through URL Parameters and the second is through state.

First, let’s take a look at URL parameters. If you’ve read our URL Parameters post, you’ll be familiar with this example. Say we were in charge of building out the Route that renders Twitter’s profile page. If created with React Router, that route may look like this

```
<Route path='/:handle' component={Profile} />
```

Notice that handle is going to be dynamic. It could be anything from tylermcginnis or dan_abramov to realDonaldTrump.

So in our app we may have a Link component that looks like this

```
<Link to='/tylermcginnis'>Tyler McGinnis</Link>
```

If clicked, the user would be taken to /tylermcginnis and the Profile component would be able to access the dynamic handle (tylermcginnis) from props.match.params.handle.

```
class Profile extends React.Component {
  state = {
    user: null
  }
  componentDidMount () {
    const { handle } = this.props.match.params

    fetch(`https://api.twitter.com/user/${handle}`)
      .then((user) => {
        this.setState(() => ({ user }))
      })
  }
  render() {
    ...
  }
}
```
URL parameters are great, but they’re not really meant to serve as a way to get data from one route to another as they’re limited to just strings. What if instead of just a string, we wanted to pass something a little more complex, like an object or an array? There would be no way to do that with URL parameters. This brings us to the second way to pass data from one route to another and that’s with state.

Going back to our example from earlier, what if we wanted to pass along if the user is coming from a certain route through to the Profile component when the user clicks on the Link? React Router gives us a way to do that and the API looks like this

```
<Link to={{
  pathname: '/tylermcginnis',
  state: {
    fromNotifications: true
  }
}}>Tyler McGinnis</Link>
```

Now, the component that’s being rendered for that route (in this case, Profile) would be able to access fromNotifications by accessing props.location.state.

```
class Profile extends React.Component {
  state = {
    user: null
  }
  componentDidMount () {
    const { handle } = this.props.match.params
    const { fromNotifications } = this.props.location.state

    fetch(`https://api.twitter.com/user/${handle}`)
      .then((user) => {
        this.setState(() => ({ user }))
      })
  }
  render() {
    ...
  }
}
```

To recap, there are two ways to pass data from a Link through to the new route: URL parameters and state. URL parameters work great for strings, but break down after that. By making the Links to prop an object, you can pass along any sort of data you need under the state property and that data can be accessed in the new route under props.location.state.
