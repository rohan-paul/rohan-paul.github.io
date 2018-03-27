---
layout: post
title: Passing Data Between React Components
comments: true
author: Rohan Paul
categories: JavaScript
---
<img src="/images/fulls/React-Component-Passing-Props-To-Children.jpeg" class="fit image">

In React, props are immutable pieces of data that are passed into child components from parents (if we think of our component as the "function" we can think of props as our component's "arguments").

**Basic data flow in a React app**

The basic idea with React is whenever we have nested components – for example, ``Chat``, which has a list of ``ChatMessages`` – the parent component updates each child. The ``Chat`` component has a list of messages. Each message is given to a ``ChatMessage``, in other words, the data flows from parent (``Chat``) to child (``ChatMessage``).

We also should have a form used to add new messages. The input field within the form can be thought of as child components too. In that case, the flow is different: The input sends an event, which goes back to ``Chat``, which updates itself. Then, the data flows to the children if anything is changed.

**Parent to Child — Use Prop**

**Example-1**

<p data-height="453" data-theme-id="0" data-slug-hash="YxYbyO" data-default-tab="js" data-user="rohanpaul" data-embed-version="2" data-pen-title="YxYbyO" class="codepen">See the Pen <a href="https://codepen.io/rohanpaul/pen/YxYbyO/">YxYbyO</a> by Rohan Paul (<a href="https://codepen.io/rohanpaul">@rohanpaul</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>


So what's happening here? First, notice that we have two separate components <Parent /> and <TacosList />. In this example, we want to pass our data from <Parent /> to <TacosList /> using props. The data we want to pass from our parent to our child is a short list of taco names (the array assigned to the tacos property on our <Parent /> component).

To pass them to the child, we define a property on that child component where that data will be passed through. In this case, we define a property or "prop" in React-slang called tacos on the <TacosList /> component. Next, using the JSX braces syntax for specifying JavaScript code within our markup, we grab our array of tacos from the parent by calling this.tacos. Here, we're literraly saying to the <TacosList /> component, "here's a list of tacos, go nuts."

Realize: our <TacosList /> component couldn't care less about what data it's getting as long as 1.) that data is passed through a prop called tacos and the type of data is an Array. We could just as easily pass it a list of candy and it would work fine. If we look at the definition of our <TacosList /> component, we can see how this works.

Like we'd expect, we're pulling in our array of tacos from the <Parent /> component by calling to this.props.tacos from within our <TacosList /> (or child) component. This resolves the first requirement. Next, we call .map() on this value to iterate over it, meaning, we expect the value to be an array.

From there, we assign a variable in our map's arguments to reference each value being iterated over taco and then use it accordingly (in this case, we just print the name of the taco on to the page).


**Example-2 : Passing props from Parent to Child**

Lets workd through a extremely small app of making a profile page, taking a random image from http://lorempixel.com/. You can just copy-paste the code into an .html file and open with any browser and see the result

The structure of our components are like this

```
- App
    - Profile
    - Hobbies
```

<p data-height="1252" data-theme-id="0" data-slug-hash="GvdPJo" data-default-tab="js" data-user="rohanpaul" data-embed-version="2" data-pen-title="GvdPJo" class="codepen">See the Pen <a href="https://codepen.io/rohanpaul/pen/GvdPJo/">GvdPJo</a> by Rohan Paul (<a href="https://codepen.io/rohanpaul">@rohanpaul</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>

Lets understand the below line of code in the above first

```
ReactDOM.render(<App profileData={DATA} />, document.getElementById('root'));
```

This is how we specify where on the page we want the ``App`` component to be rendered. This is done by calling ``ReactDOM.render``, passing in the App component as the first argument and a reference to a ``div`` with the id of ``root`` as the second. So this entire app will go into this ``root`` div. Also note, how you pass the ``DATA`` into the App component, by simply changing a little bit on the ``ReactDOM.render`` method. And the curly brackets tells React that we’re escaping out of the JSX syntax in order to add a Javascript expression (``DATA``).

And with the above line of code in ``ReactDOM.render`` now we’re able to access this data from within the App component through ``this.props.profileData`` . However, the App component is simply a wrapper around the ``Profile`` and ``Hobbies`` components, so we’re going to send the data further down the hierarchy, as a prop to the child by instantiating it within the parent. 

<p data-height="320" data-theme-id="0" data-slug-hash="OjZrwj" data-default-tab="js" data-user="rohanpaul" data-embed-version="2" data-pen-title="OjZrwj" class="codepen">See the Pen <a href="https://codepen.io/rohanpaul/pen/OjZrwj/">OjZrwj</a> by Rohan Paul (<a href="https://codepen.io/rohanpaul">@rohanpaul</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>

We instantiate three variables within the top-level ``App`` component; ``imgURL`` and ``name`` for ``Profile`` component, and only ``hobbyList`` for ``Hobbies`` component. This is because the ``Hobbies`` component doesn’t need the rest of the data; it’s simply going to display a list of hobbies.

And then notice, inside the ``Profile`` component we fetch the data that we’ve passed down to it from ``App`` component by using ``this.props.name`` and ``this.props.imgURL``


And in the ``Hobbie`` component, we’re looping through the hobbies array stored in ``this.props.hobbyList``


**Example-3 : Passing props from Parent to Child**

Lets look at another example where I instantiate the data that will need to be passed to the child components, within a function in the parent, (where lets say, I am building a tic-tac-toe gameboard), and I have access to five data variables in my parent component that I need my child component to have access to. So, I need to pass those five variables (key, location, value, updateBoard, turn) from ``App.jsx`` to a child component ``Tile.jsx``:
So, I instantiate these five variables within the parent and ``App.jsx`` will include codes like below:


```
import React, { Component } from 'react';
import Tile from './Tile';

render() {
    return (
      <div className="container">       
        {this.state.gameBoard.map(function (value, i) {
            return (
              <Tile
                key={i}
                location={i}
                value={value}
                updateBoard={this.updateBoard.bind(this)}
                turn={this.state.turn} />
            );
        }.bind(this))}
      </div>

```

Now in the Tile component, if I use ``this.props.location`` I will have access to that data. So, the code in the child component Tile.jsx will include something like below:

```
  render() {
    return (
      <div className={"tile " + this.props.loc}>
        <p>{this.props.value}</p>        
      </div>
    );
  }
 ```


And in the above, note that ``Array.prototype.map()`` takes a second argument to set what ``this`` refers to in the mapping function. The (optional) second parameter is the context that the inner function is called with. So we can pass ``this`` as the second argument to preserve the current context, like so :

```
someList.map(function(item) {
  ...
}, this)
```

or we can use ``.bind(this)`` like we did above 

```
someList.map(function(item) {
  ...
}.bind(this))
```

This ``render()`` method returns a ReactElement which isn't part of the "actual DOM", but instead a description of the Virtual DOM.

React expects the method to return a single child element. It can be a virtual representation of a DOM component or can return the falsy value of null or false. React handles the falsy value by rendering an empty element (a <noscript /> tag). This is used to remove the tag from the page.


**Passing Props Child to Parent — Use a callback and states**

React's one-way data-binding model means that child components cannot send back values to parent components unless explicitly allowed to do so

However, if you want to pass data from a child to it's parent, you can use a callback function. Just pass a function as a prop to the child component. Also, the same strategy can be used to support sibling communication. So, here the steps: 


- Define a callback in parent component which takes the data I need in as a parameter.
- Pass that callback as a prop to the child.
- Call the callback using this.props.[callback] in the child (insert your own name where it says ``[callback]`` of course), and pass in the data as the argument.


<p data-height="511" data-theme-id="0" data-slug-hash="gxzBKd" data-default-tab="js" data-user="rohanpaul" data-embed-version="2" data-pen-title="gxzBKd" class="codepen">See the Pen <a href="https://codepen.io/rohanpaul/pen/gxzBKd/">gxzBKd</a> by Rohan Paul (<a href="https://codepen.io/rohanpaul">@rohanpaul</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>

**Another example to call Parent's function from Child component**

Below is a simple toggle function that I will call from Child component. So, in order to call the parent’s method ( ``doParentToggle`` ), the same line of flow of codes is followed as above example.  

- A) The callback function is first defined in the Parent component, 
- B) then within the Parent component, we instantiate a variable ``parentToggle`` to assign ``{this.doParentToggle}`` for the Child component. 
- C) And then, I will have to pass in the function as a property to Child component then trigger from child ``onClick`` method. So, inside the Child component's ``doParentToggleFromChild`` function I call the Parent's function using ``this.props.[callback]`` style. This is here, ``this.props.parentToggle``

But here we are not just calling a simple function, you are passing in some parameters from child to parent and changing some states to parent.

<script async src="//jsfiddle.net/rohanpaul/cpzxfh32/10/embed/js,html,css,result/dark/"></script>