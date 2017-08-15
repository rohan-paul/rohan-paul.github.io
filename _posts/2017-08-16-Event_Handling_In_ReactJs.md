---
layout: post
title: Event Handling In ReactJS
comments: true
author: Rohan Paul
categories: JavaScript
---
<img src="/images/fulls/Event-Handling-ReactJS.png" class="fit image">

React implements a [synthetic event](https://facebook.github.io/react/docs/events.html) system that brings consistency and high performance to React
applications and interfaces. It achieves consistency by normalizing events so that they have the same properties across different browsers and platforms.
It achieves high performance by automatically using event delegation. React doesn't actually attach
event handlers to the nodes themselves. Instead, a single event listener is attached to the root of the document; when an event is fired, React maps it to the appropriate component element. React also
automatically removes the event listeners when a component unmounts.

HTML has always provided a beautiful and easy-to-understand event handling API for tag attributes:
onclick, onfocus, etc. The problem with this API (and the reason why it is not used in professional projects)
is that it’s full of undesirable side effects: it pollutes the global scope, it’s hard to track in the context of a big
HTML file, it can be slow, and it can lead to memory leaks, just to name a few issues.

Taking from [React Documentation](https://facebook.github.io/react/docs/handling-events.html) itself for event handling in React with JSX, you pass a function as the event handler, rather than a string as opposed to plain HTML.
For example, the HTML:

```
<button onclick="activateLasers()">
  Activate Lasers
</button>
```

This is slightly different in React:

```
<button onClick={activateLasers}>
  Activate Lasers
</button>
```

Now lets take the example given in the [React Doc](https://facebook.github.io/react/docs/handling-events.html) itself to understand the mechanism of event handling.

Here,  this Toggle component renders a button that lets the user toggle between "ON" and "OFF" states:

So, to handle that Toggling by clicking (i.e. by invoking a ``onClick`` callback), we defina a method ``handleClick()`` 

```
handleClick() {
  this.setState(prevState => ({
    isToggleOn: !prevState.isToggleOn
  }));
}
```

Now, I want to handle ``onClick`` callback. So, all what I need, is to pass my ``handleClick`` method into ``onClick`` property. But, Simply ``onClick={ this.handleClick }`` won’t work because I need to bind ``this`` value from function to our class (each function creates own environment). And React doesn't automatically bind ``this`` to the instance when using ES6 classes. We'll have to explicitly use ``.bind(this)`` in the constructor:

So, thats what we do in ``Toggle`` component definition.

```
class Toggle extends React.Component {
  constructor(props) {
    super(props);
    this.state = { isToggleOn: true };
    this.handleClick = this.handleClick.bind(this);
  }
}
```

The ``.bind()`` method simply returns a new function. When it is invoked, our new function simply invokes the original function that was passed in, setting the original value as ``this``. So, we pass our desired context, ``this``, into the ``.bind()`` function. 

And now we can finally render 

```
render() {
  return (
    <button onClick={this.handleClick}>
      {this.state.isToggleOn ? 'ON' : 'OFF'}
    </button>
  );
}

ReactDOM.render(
  <Toggle />,
  document.getElementById('root')
)
```
Or in the above ``render`` could also have written like below, if the line ``this.handleClick = this.handleClick.bind(this);`` was not included the ``Toggle`` class constructor. 

```
render() {
  return (
    <button onClick={this.handleClick.bind(this)}>
      {this.state.isToggleOn ? 'ON' : 'OFF'}
    </button>
  );
}
```

However, it's not recommended to apply the ``.bind`` within the render function since it will do it every time the component is re-rendered. you can move it to some function which runs at the start of the lifecycle i.e. withing the Component constructor. The key here is that the constructor is only called once, not on every render. This means we aren’t creating a bunch of functions and forcing the garbage collector to clean up.  The largest problem with most react apps is the render method containing too much weight, or in other cases firing unchecked.

**More on the mechanism ``.bind()`` in React event handling**

Calling ``f.bind(someObject)`` creates a new function with the same body and scope as f, but where this occurs in the original function, in the new function it is permanently bound to the first argument of bind, regardless of how the function is being used.
In JavaScript, class methods are not bound by default. If you forget to bind ``this.handleClick`` and pass it to onClick, this will be undefined when the function is actually called.

This is not React-specific behavior; it is a part of how functions work in JavaScript. Generally, if you refer to a method without () after it, such as ``onClick={this.handleClick}``, you should bind that method ( as in ``onClick={this.handleClick.bind(this)}``)

 The statement ( ``this.handleClick = this.handleClick.bind(this);``) in the constructor make sure ``this`` is always bound to current element. So when ``handleClick`` is passed down to child and called, the ``this`` is correctly defined. It is just Javascript problem. The reason it is placed in constructor is so that it is run exactly once per instance of this Component


And that was just one event hadnler for ``handleClick`` . And when we add more event handlers to our class, code should look similar to this:

<p data-height="862" data-theme-id="0" data-slug-hash="gxXWJL" data-default-tab="js" data-user="rohanpaul" data-embed-version="2" data-pen-title="gxXWJL" class="codepen">See the Pen <a href="https://codepen.io/rohanpaul/pen/gxXWJL/">gxXWJL</a> by Rohan Paul (<a href="https://codepen.io/rohanpaul">@rohanpaul</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>


There are at least 5 ways to handle the "this" context in React. And here's a working examples of all those 5 approaches:

<p data-height="1827" data-theme-id="0" data-slug-hash="QMOgLQ" data-default-tab="js" data-user="rohanpaul" data-embed-version="2" data-pen-title="QMOgLQ" class="codepen">See the Pen <a href="https://codepen.io/rohanpaul/pen/QMOgLQ/">QMOgLQ</a> by Rohan Paul (<a href="https://codepen.io/rohanpaul">@rohanpaul</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>

There's even a 6th approach by using the very nice library called [autobind-decorator](https://github.com/andreypopp/autobind-decorator)

<p data-height="345" data-theme-id="0" data-slug-hash="OjOgQQ" data-default-tab="js" data-user="rohanpaul" data-embed-version="2" data-pen-title="OjOgQQ" class="codepen">See the Pen <a href="https://codepen.io/rohanpaul/pen/OjOgQQ/">OjOgQQ</a> by Rohan Paul (<a href="https://codepen.io/rohanpaul">@rohanpaul</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>

The ``@autobind`` decorator binds the ``handleClick()`` method and we’re all set. 

And if passing a parameter then we must bind in the ``render`` function and not in the constructor. Using either the ``bind`` or an arrow function.

```
<li onClick={this.handleClick.bind(this, item.id)} />{item.name}</li>
```

or using arrow function

```
<li onClick={() => this.handleClick(item.id)} />{item.name}</li>
```