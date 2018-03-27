---
layout: post
title: Understanding ReactJS Component
comments: true
author: Rohan Paul
categories: JavaScript
---
<img src="/images/fulls/React-Component.jpeg" class="fit image">

Components are the heart and soul of React. Most simpley component is just a function that accept some properties and return a UI element.

```
function Computer(props) {
	return (
	<<div>A Powerful machine</div>
	);
}
```

Components take raw data and render it as HTML in the DOM. It describes what to render. They utilize properties (props for short) and states which contribute to this raw data. Both props and states are plain Javascript objects, any changes to them trigger a render update and they are deterministic.

Components have one requirement; they must implement ``render``, a function that tells the component what to render. 

**There are three ways to create a component.**

React traditionally provided the **``React.createClass`` method to create component classes**, and then using ES6 syntax with **``extends React.Component``**, which extends the Component class instead of calling ``React.createClass``. And the third way, is applicable for creating the React Stateless Functional Components that we will discuss below. Lets look at them one by one starting with **``React.createClass``**



<p data-height="415" data-theme-id="0" data-slug-hash="PKWdEJ" data-default-tab="js" data-user="rohanpaul" data-embed-version="2" data-pen-title="firstReactComponent" class="codepen">See the Pen <a href="https://codepen.io/rohanpaul/pen/PKWdEJ/">firstReactComponent</a> by Rohan Paul (<a href="https://codepen.io/rohanpaul">@rohanpaul</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>

**And now create the same component class ES6 way as below:**


<p data-height="362" data-theme-id="0" data-slug-hash="xLgmaw" data-default-tab="js" data-user="rohanpaul" data-embed-version="2" data-pen-title="Create_React_Component-ES6Way" class="codepen">See the Pen <a href="https://codepen.io/rohanpaul/pen/xLgmaw/">Create_React_Component-ES6Way</a> by Rohan Paul (<a href="https://codepen.io/rohanpaul">@rohanpaul</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>

The ``constructor()`` method is automatically called when an object is created, and gives us chance to do any initialization work required to make the object useful. This only gets called once for each object, and you never call it directly.  As with other OOP languages, ``constructor()`` is invoked when an instance of this class is created. Furthermore, if we create a ``constructor()`` method, you almost always need to invoke super() inside of it, otherwise the parent’s constructor won’t be executed. 

The ``super()`` method is a special method that means "call this same method on whichever class I inherited from." 

So, ``class ChatComponent extends React.Component`` means our  ``ChatComponent`` gets all the functionalities provided by its parent class, ``React.Component``. It also means that it's good practise to let ``React.Component`` do its own initialization if it needs to, which is what ``super()`` does: it tells the parent class to go ahead and run the same method on itself before we can continue. And ``constructor()`` takes the component's ``props`` as its only input parameter. This must get passed on with the ``super()`` so that the parent class can act on those ``props`` as needed.


Component returns exactly one DOM element. Just like in HTML everything has to have one parent DOM element, in other words one top-level element. So, if I have a component that looks something like below:

<p data-height="304" data-theme-id="0" data-slug-hash="XapOpK" data-default-tab="js" data-user="rohanpaul" data-embed-version="2" data-pen-title="XapOpK" class="codepen">See the Pen <a href="https://codepen.io/rohanpaul/pen/XapOpK/">XapOpK</a> by Rohan Paul (<a href="https://codepen.io/rohanpaul">@rohanpaul</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>

I will need to modify by either bringing in a single wrapper element like below:

<p data-height="374" data-theme-id="0" data-slug-hash="MvJRBx" data-default-tab="js" data-user="rohanpaul" data-embed-version="2" data-pen-title="MvJRBx" class="codepen">See the Pen <a href="https://codepen.io/rohanpaul/pen/MvJRBx/">MvJRBx</a> by Rohan Paul (<a href="https://codepen.io/rohanpaul">@rohanpaul</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>

OR nesting the one of the top-level ``<div>`` inside the other like so:


<p data-height="313" data-theme-id="0" data-slug-hash="PKWgVQ" data-default-tab="js" data-user="rohanpaul" data-embed-version="2" data-pen-title="PKWgVQ" class="codepen">See the Pen <a href="https://codepen.io/rohanpaul/pen/PKWgVQ/">PKWgVQ</a> by Rohan Paul (<a href="https://codepen.io/rohanpaul">@rohanpaul</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>


**And the third way to create component is for React Stateless Functional Components**

This is a new syntax/pattern for defining components as a function of props. It was introduced in React v0.14. [From the release notes](https://facebook.github.io/react/blog/2015/10/07/react-v0.14.html#stateless-functional-components)

"In idiomatic React code, most of the components you write will be stateless, simply composing other components. We’re introducing a new, simpler syntax for these components where you can take props as an argument and return the element you want to render". Lets look at some some examples of Stateless Functional Component

<p data-height="979" data-theme-id="0" data-slug-hash="LjjqNK" data-default-tab="js" data-user="rohanpaul" data-embed-version="2" data-pen-title="LjjqNK" class="codepen">See the Pen <a href="https://codepen.io/rohanpaul/pen/LjjqNK/">LjjqNK</a> by Rohan Paul (<a href="https://codepen.io/rohanpaul">@rohanpaul</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>

This pattern is similar to the "traditional" one, except for the fact that we're using simple functions instead of methods defined in a class. This can be useful when we want to extract functions out of the class (e.g for readability and cleanliness sake).

A functional component is just a function. It's not a class. As such, there's no global ``this`` object. This means that when we're doing a ``render`` we're basically creating a new instance of a ``ReactComponent``, thus leaving out the possibility for these javascript objects to communicate with each other via some global ``this``. This also makes the use of state and any life-cycle methods impossible as a consequence.

Generally, using the new pattern is recommended whenever possible! If we only need a ``render`` method, but no life-cycle methods or state, use this pattern.


**Properties**

Props (short for properties) are a Component's configuration. They are received from above and immutable as far as the Component receiving them is concerned. That is, once a component receives its props, render may use it to display data in the HTML, but the props object will not change.
A Component cannot change its props, but it is responsible for putting together the props of its child Components. Props do not have to just be data -- callback functions may be passed in as props.
When a component is rendered, it can access its "props" using ``this.props``. In the code above, the Chat component uses ``this.props.chat``

Per React's [Thinking in React](https://facebook.github.io/react/docs/thinking-in-react.html) documentation, props are best explained as "a way of passing data from parent to child." That's it. In essence, props are just a communication channel between components, always moving from top (parent) to bottom (child). A quick example:

<p data-height="453" data-theme-id="0" data-slug-hash="YxYbyO" data-default-tab="js" data-user="rohanpaul" data-embed-version="2" data-pen-title="YxYbyO" class="codepen">See the Pen <a href="https://codepen.io/rohanpaul/pen/YxYbyO/">YxYbyO</a> by Rohan Paul (<a href="https://codepen.io/rohanpaul">@rohanpaul</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>


So what's happening here? First, notice that we have two separate components <Parent /> and <TacosList />. In this example, we want to pass our data from <Parent /> to <TacosList /> using props. The data we want to pass from our parent to our child is a short list of taco names (the array assigned to the tacos property on our <Parent /> component).

To pass them to the child, we define a property on that child component where that data will be passed through. In this case, we define a property or "prop" in React-slang called tacos on the <TacosList /> component. Next, using the JSX braces syntax for specifying JavaScript code within our markup, we grab our array of tacos from the parent by calling this.tacos. Here, we're literraly saying to the <TacosList /> component, "here's a list of tacos, go nuts."

Realize: our <TacosList /> component could care less about what data it's getting as long as 1.) that data is passed through a prop called tacos and the type of data is an Array. We could just as easily pass it a list of candy and it would work fine. If we look at the definition of our <TacosList /> component, we can see how this works.

Like we'd expect, we're pulling in our array of tacos from the <Parent /> component by calling to this.props.tacos from within our <TacosList /> (or child) component. This resolves the first requirement. Next, we call .map() on this value to iterate over it, meaning, we expect the value to be an array.

From there, we assign a variable in our map's arguments to reference each value being iterated over taco and then use it accordingly (in this case, we just print the name of the taco on to the page).


**States**

The state is a data structure that starts with a default value when a Component mounts. It may be mutated across time, mostly as a result of user events. A Component manages its own state internally. Besides setting an initial state, it has no business fiddling with the state of its children. You might conceptualize state as private to that component. For example, the user clicks a button, types in an input field, or receives new data from a WebSocket server. These events would update a property in the component's ``state`` by calling ``setState()``, and the update is ``render``ed in HTML.

Both props and state changes trigger a render update.


**Difference between Props and States**

- state are mutable, props are not
- state means internal data of a class; props means ,the data which is transferred through the components.
- If a Component needs to alter one of its attributes at some point in time, that attribute should be part of its state, otherwise it should just be a prop for that Component.



**Should this Component have state?**

The props vs state summary is beautifully given in [this guide](https://github.com/uberVU/react-guide/blob/master/props-vs-state.md). I am taking a bit of summary from that page.

State is optional. Since state increases complexity and reduces predictability, a Component without state is preferable. Even though you clearly can't do without state in an interactive app, you should avoid having too many Stateful Components.

**Component types**

**Stateless Component** -  Only props, no state. There's not much going on besides the render() function. Their logic revolves around the props they receive. This makes them very easy to follow, and to test.

This Hello World script is a good example of a stateless component:

```
class HelloWorld extends React.Component {
  render() {
    return <h1 {...this.props}>Hello {this.props.frameworkName} World!!!</h1>
  }
}
```

To have a smaller syntax for stateless components, React provides us with a function style. We create a function that takes properties as an argument and returns the view. Reqriting the HelloWorld component as a function that returns <h1> would be:

```
const HelloWorld = function(props){
  return <h1 {...props}>Hello {props.frameworkName} world!!!</h1>
}
```

And ofcourse we can use arrow functions for stateless components.
```
const HelloWorld = (props)=>{
  return <h1 {...props}>Hello {props.frameworkName} world!!!</h1>
}
```


**Stateful Component** -  Both props and state. These are used when your component must retain some state. This is a good place for client-server communication (XHR, web sockets, etc.), processing data and responding to user events. These sort of logistics should be encapsulated in a moderate number of Stateful Components, while all visualization and formatting logic should move downstream into many Stateless Components.

| | _props_ | _state_ | 
--- | --- | --- 
Can get initial value from parent Component? | Yes | Yes
Can be changed by parent Component? | Yes | No
Can set default values inside Component?* | Yes | Yes
Can change inside Component? | No | Yes
Can set initial value for child Components? | Yes | Yes
Can change in child Components? | Yes | No


It is often desirable to divide components based on their primary responsibility, into Presentational and Container components.
Presentational components are concerned only with displaying data - they can be regarded as, and are often implemented as, functions that convert a model to a view. Typically they do not maintain any internal state. 
Container components are concerned with managing data. This may be done internally through their own state, or by acting as intermediaries with a state-management library such as Redux. Initialization of the state and update of the state are handled in container component. The container component will not directly display data, rather it will pass the data to a presentational component.

**Setting the initial state**

There are two approaches - We can use ``getInitialState`` or ``this.state``

The two approaches are not interchangeable. You should initialize state in the constructor when using ES6 classes, and define the getInitialState method when using React.createClass. 

``getInitialState`` retuns an object and that object gets set to ``this.state``


```
class MyComponent extends React.Component {
  constructor(props) {
    super(props);
    this.state = { /* initial state */ };
  }
}
```

is equivalent to

```
var MyComponent = React.createClass({
  getInitialState() {
    return { /* initial state */ };
  },
});
```



**Updating state**

When a Component needs to have and manage its ``state`` we usually write the Component as a class, and have its statel live in the class ``constructor`` function.

<p data-height="307" data-theme-id="0" data-slug-hash="VzpvEJ" data-default-tab="js" data-user="rohanpaul" data-embed-version="2" data-pen-title="VzpvEJ" class="codepen">See the Pen <a href="https://codepen.io/rohanpaul/pen/VzpvEJ/">VzpvEJ</a> by Rohan Paul (<a href="https://codepen.io/rohanpaul">@rohanpaul</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>

To manage the state, React provides a special method called [``setState()``](https://facebook.github.io/react/docs/react-component.html#setstate). we use it like this:

```
class Computer {
  ... 
  increasePrice () {
    this.setState({score : this.state.price + 100});
  }
  ...
}
```

We can not update state by modifying the state object directly (i.e. ``this.state.price = 1900;``). This is wrong and won’t work, as React doesn’t watch the state object for changes. Instead, React exposes us with the ``setState`` method that we can pass updated state values to.


Only place we can directly write to ``this.state`` should be the Components constructor (or, if we’re using class-properties plugin a babel-preset, the class declaration) . In all the other places you should be using ``this.setState`` function, which will accept an Object that will be eventually merged into Components current state.
Although its possible to alter state by writing to ``this.state`` directly, it will not lead to the Component re-rendering with new data, and generally lead to state inconsistency.


Props and state are quite closely related. The state of one component will often become the props of a child component. Props are passed to the child within the render method of the parent as the second argument to ``React.createElement()`` or, if you're using JSX, the more familiar tag attributes.

``<MyChild name={this.state.childsName} />``

The parent's state value of childsName becomes the child's this.props.name. From the child's perspective, the name prop is immutable. If it needs to be changed, the parent should just change its internal state:

``this.setState({ childsName: 'New name' });``

and React will propagate it to the child for you. A natural follow-on question is: what if the child needs to change its name prop? This is usually done through child events and parent callbacks. The child might expose an event called, for example, onNameChanged. The parent would then subscribe to the event by passing a callback handler.

``<MyChild name={this.state.childsName} onNameChanged={this.handleName} />``

The child would pass its requested new name as an argument to the event callback by calling, e.g.,  this.props.onNameChanged('New name'), and the parent would use the name in the event handler to update its state.

```
handleName: function(newName) {
   this.setState({ childsName: newName });
}
```

We cannot set new state values until the component has been mounted. Lets look at another example from an app that shows an interactive 30-Day Bitcoin Price Graph. Here, the ``DataBox`` component fetches real time Bitcoin price-data from the [CoinDesk](https://www.coindesk.com/api/) API every 90 seconds. The data is formatted and then displayed to the user along with changes since last month.

<script src="https://gist.github.com/rohan-paul/7c171f35b6cbcddb4da43b620bd4092e.js"></script>

Note how setState() works. In the above we pass and object to ``setState()`` containing parts of the state we want to update. In other words, the object we pass would have keys ("currentPrice", "monthChangeD" and so on) corresponding to the keys in the component state, then setState() updates or sets the state by merging the object to the state. The object form of ``setState()`` is great when setting state that isn’t concerned with previous state. When fetching Bitcoin price date, we know the new value and don’t care what the previous value was. So, we replace it wholesale.

``componentDidMount()`` is invoked immediately after a component is mounted. And as we needed to load data from a remote endpoint, we instantiate the network request from ``componentDidMount()``


``setState()`` expects either an object or a function as the first parameter. However, you could pass both an object and a function, but React would treat it differently. If you pass an object as the first parameter and then pass a function as the second parameter, the function would be treated as a callback function, not updater function.

Think about what happens when ``setState()`` is called. React will first merge the object you passed to ``setState()`` into the current state. Then it will start that reconciliation thing. It will create a new React Element tree (an object representation of our UI), diff the new tree against the old tree, figure out what has changed based on the object you passed to ``setState()`` , then finally update the DOM. 
In other words, when setState is called, it marks the component as dirty (which means it needs to be re-rendered). The important thing to note is that although render method of the component is called, the real DOM is only updated if the output is different from the current DOM tree (a.k.a diffing between the Virtual DOM tree and document's DOM tree). 