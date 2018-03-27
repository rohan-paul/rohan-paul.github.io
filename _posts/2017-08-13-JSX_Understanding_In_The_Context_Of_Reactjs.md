---
layout: post
title: JSX Understanding In The Context Of Reactjs
comments: true
author: Rohan Paul
categories: JavaScript
---
<img src="/images/fulls/JSX-Understanding.jpeg" class="fit image">

[JSX](http://facebook.github.io/jsx/.
) (JavaScript in XML  or JavaScript Syntax eXtension) in most simple word is how React allows us to write HTML in JavaScript, in case you prefer the readibility of HTML to raw JavaScript.

JSX is a syntax extension for JavaScript. It was written to be used by React and looks a lot like HTML. But given JSX is not valid JavaScript, web browsers cant read it directly. So, if JavaScript files contains JSX, that that file will have to be transpiled. That means that before the file gets to the web browser, a JSX compiler will translate any JSX into regular JavaScript.

JSX produces React "elements". A React element is simply an object representation of a DOM node. A React element isn’t actually the thing we see on our screen, instead, it’s just an object representation of it.
We can embed any JavaScript expression in JSX by wrapping it in curly braces. 


**Differences Between JSX and HTML**

1.	Tag attributes are camel cased.
2.	All elements must be balanced.
3.  The attribute names are based on the DOM API, not on the HTML language specs.

No 1. above is obvious in ``maxlength`` in HTML becomes ``maxLength`` in JSX.

**All Elements Must be Balanced** - Since JSX is XML, all elements must be balanced. Tags such as ``<br> and <img>``, which don’t have ending tags, need to be self-closed. So, instead of ``<br>, use <br/>`` and instead of ``<img src="...">``, use ``<img src="..." />``.


**Attribute Names are Based on the DOM API** -  When interacting with the DOM API, tag attributes may
have different names than those you use in HTML. One of such example is class and className.
For example, given this regular HTML

```
<div id="box" class="some-class"></div>
```

if you want to manipulate the DOM and change its class name using plain JavaScript, we would do
something like ``document.getElementById("box").className="some-other-class"``

As far as JavaScript is concerned, that attribute is called ``className``, not ``class``. Since JSX is just a syntax extension to JavaScript, it conforms to the attribute names as defined in the DOM. That same div should be expressed in JSX as

```
return <div id="box" className="some-class"></div>
```


<p data-height="250" data-theme-id="0" data-slug-hash="VzzzYG" data-default-tab="js" data-user="rohanpaul" data-embed-version="2" data-pen-title="VzzzYG" class="codepen">See the Pen <a href="https://codepen.io/rohanpaul/pen/VzzzYG/">VzzzYG</a> by Rohan Paul (<a href="https://codepen.io/rohanpaul">@rohanpaul</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>


 There's an abstraction layer between JSX and what’s actually going on in React land. This abstraction layer is that JSX is always going to get transpiled to ``React.createElement()`` invocations (typically) via Babel. We will not typically invoke ``React.createElement()`` directly if we are using JSX.

<p data-height="326" data-theme-id="0" data-slug-hash="WEEEKo" data-default-tab="js" data-user="rohanpaul" data-embed-version="2" data-pen-title="WEEEKo" class="codepen">See the Pen <a href="https://codepen.io/rohanpaul/pen/WEEEKo/">WEEEKo</a> by Rohan Paul (<a href="https://codepen.io/rohanpaul">@rohanpaul</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>

For the above, ``React.createElement()`` essentially creates an object like below

```
const element = {
  type: 'MyButton',
  props: {
    color: "blue",
    shadowSize: 2
  }
};
```

Checkout this [online Babel compiler](https://babeljs.io/repl/#?babili=false&evaluate=true&lineWrap=false&presets=es2015%2Creact%2Cstage-0&targets=&browsers=&builtIns=false&debug=false&code_lz=DwWQngQgrgLjD2A7ABAY3gG3gJwLwCIAjDKAU32QGcALAQwBN4B3AZQEsAvU3AbwCYAvgD4AUMmQBhDG1QBrZCFIjgAenDQ4SIUA)

Another example

<p data-height="480" data-theme-id="0" data-slug-hash="WEEKEm" data-default-tab="js" data-user="rohanpaul" data-embed-version="2" data-pen-title="WEEKEm" class="codepen">See the Pen <a href="https://codepen.io/rohanpaul/pen/WEEKEm/">WEEKEm</a> by Rohan Paul (<a href="https://codepen.io/rohanpaul">@rohanpaul</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>

The ``render()`` function in the ``HelloWorld`` component looks like it's returning HTML, but this is actually JSX and a more crisp way to write a ``React.createElement()`` function.

The ``createElement`` method takes three arguments:

```
React.createElement(type, [props], [...children])

```

The first is a tag name string (div, span, etc), the second is any attributes you want the element to have, the third is contents or the children of the element.

For React elements, ``props`` are the same as attributes. For an example, to pass in the ``src`` attribute to ``img`` element, we'd provide an object (key-value pair) with a ``src`` property set to the location of our logo.

```
var Logo = React.createElement('img', { src: './img/logo.png' });
```

Because JSX is JavaScript, we can't use JavaScript reserved words, like ``class`` and ``for``.

Thats why React gives us the attribute ``className``. We use it in ``HelloWorld`` to set the ``large`` class on our ``h1`` tag. There are a few other attributes, such as the ``for`` attribute on a label that React translates into ``htmlFor`` as ``for`` is also a reserved word. If we want to write pure JavaScript instead of rely on a JSX compiler, we can just write the ``React.createElement()`` function and not worry about the layer of abstraction. 
Odds are if you’ve been using React for any amount of time, you don’t use ``React.createElement`` to create our object representations of the DOM. JSX gives us more readibility, especially with complex components. Consider the following JSX:

<p data-height="418" data-theme-id="0" data-slug-hash="mMMjzj" data-default-tab="js" data-user="rohanpaul" data-embed-version="2" data-pen-title="mMMjzj" class="codepen">See the Pen <a href="https://codepen.io/rohanpaul/pen/mMMjzj/">mMMjzj</a> by Rohan Paul (<a href="https://codepen.io/rohanpaul">@rohanpaul</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>

And to note that, ``false``, ``null``, ``undefined``, and ``true`` are valid children. But they simply don't render.

**Writing React Components Using JSX**

<p data-height="241" data-theme-id="0" data-slug-hash="VzMrYK" data-default-tab="js" data-user="rohanpaul" data-embed-version="2" data-pen-title="VzMrYK" class="codepen">See the Pen <a href="https://codepen.io/rohanpaul/pen/VzMrYK/">VzMrYK</a> by Rohan Paul (<a href="https://codepen.io/rohanpaul">@rohanpaul</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>

``Message`` is a react component which returns the above JSX code, which is then compiled to React function code sing Babel. Using ``ReactDOM.render``, we render the component inside the HTML element div ``main``. Save the above changes and open the index.html page in a browser, and we should be able to view the message "Welcome to React World" in the browser.


**Passing Attributes to Components and display the data**

Most of the time it's required to pass data to our components which would be evaluated or modified and then rendered. Suppose we want to pass a name to our ``Message`` component and display the name in our message. First, we'll add a custom attribute (like a key) to our component.

```
ReactDom.render(<Message name="Paul" />, document.getElementById('main'));
```

The passed attribute can be read from inside the component render function using ``this.props`` on the component key.

```
var Message = React.createClass({
	render: function() {
		return <h1>Welcome to React World, {this.props.name</h1>
	}
});
```

Save the above changes and open the index.html in browser and we should be able to see the message.

```
Welcome to React World, Paul
```

Lets look at another JSX code

```
var App = (
  <div>
    <div><img src="./img/logo.png" /></div>
    <footer>2017. All rights reserved.</footer>
  </div>
);
ReactDOM.render(App, document.getElementById('renderTarget'));
```

And its transpiled version in the online [Babel compiler](https://babeljs.io/repl/#?babili=false&evaluate=true&lineWrap=false&presets=es2015%2Creact%2Cstage-2&targets=&browsers=&builtIns=false&debug=false&code_lz=G4QwTgBAggDjEF4IAoBQEIB4AmBLYAfOhlnoZrgLYDmEAzmAMYIBEAdAPRXUcA2A9tX5sYAO2osIHApg5kiJLADN-_AC4BTMAQBMABgCMAdmi9eEMLmoALNXQsa6W4BuxtZK9VoVY5-IgCUANyoAEoaIIxqACIA8gCybGAaothayLAwADQQ2PyMAK6UKWps1BpqAKK8GsWiagBCAJ4AktjIAOTJqVoAKuDlah0BwUA) 

<img src="/images/fulls/Babel-JSX-1.png" class="fit image">


In any app, to transpile JSX in the browser itself using Babel, first, include the Babel Core library in the ``<head>`` element, and add a type to the script tag like so..

```
<head>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/babel-core/5.8.23/browser.js"></script>
</head>

.....

<body>
<script type="text/babel">
    var App = (
      <div>
        <div><img src="./img/logo.png" /></div>
        <footer>© 2017 All rights reserved.</footer>
</body>
```

**Events in JSX**

Below is the non-JSX and then JSX way of adding an event to a React node 

<p data-height="545" data-theme-id="0" data-slug-hash="xLXXpe" data-default-tab="js" data-user="rohanpaul" data-embed-version="2" data-pen-title="xLXXpe" class="codepen">See the Pen <a href="https://codepen.io/rohanpaul/pen/xLXXpe/">xLXXpe</a> by Rohan Paul (<a href="https://codepen.io/rohanpaul">@rohanpaul</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>

Note that we are using the ``{ }`` brackets to connect a JS function to an event (i.e., ``onMouseOver={mouseOverHandler})``.


Now lets create a React Component for displaying Google map with JSX syntax. Create a file called index.html and put the following code and run index.html in a browser.

<p data-height="1226" data-theme-id="0" data-slug-hash="VzMoGE" data-default-tab="html" data-user="rohanpaul" data-embed-version="2" data-pen-title="VzMoGE" class="codepen">See the Pen <a href="https://codepen.io/rohanpaul/pen/VzMoGE/">VzMoGE</a> by Rohan Paul (<a href="https://codepen.io/rohanpaul">@rohanpaul</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>

Basically how the code works is - 

First we define the ``MyMap`` component and its basic structure is 

```
var MyMap = React.createClass({
    render: function() {
        return (
            <div id="map"><span></span></div>
        );
    }
});
 
ReactDOM.render(<MyMap />,document.getElementById('main'));

```

And then define the ``componentDidMount`` inside the ``MyApp`` component, and all Google map objects are initialized inside this method. The ``componentDidMount`` method is called once the component is actually mounted to the DOM.

``ReactDOM.findDOMNode`` finds a reference to the component's DOM node and creates the map object. 

``marker`` has been defined to set the marker properties like map, position, and animation.

And finally we render the map component inside the ``#main`` div. See the final result below.

<p data-height="628" data-theme-id="0" data-slug-hash="VzMoGE" data-default-tab="result" data-user="rohanpaul" data-embed-version="2" data-pen-title="VzMoGE" class="codepen">See the Pen <a href="https://codepen.io/rohanpaul/pen/VzMoGE/">VzMoGE</a> by Rohan Paul (<a href="https://codepen.io/rohanpaul">@rohanpaul</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>