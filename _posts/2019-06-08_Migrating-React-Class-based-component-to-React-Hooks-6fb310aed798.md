# Migrating React Class based component to React Hooks

Let’s refactor an old class into the latest and greatest in React

![](https://cdn-images-1.medium.com/max/800/1*MNMA754TTpZTHLky5uRlLA.jpeg)

I built a simple React app that fetches the top 20 stories from HackerNews API, first with class-based component and then migrated to React hooks functional component.

*   [Source Code of the full repo using Class based component](https://github.com/rohan-paul/hacker-news-api-migrating-to-react-hooks/tree/master/Class_based-without-Hooks)
*   [Source Code of the Repo using React hooks](https://github.com/rohan-paul/hacker-news-api-migrating-to-react-hooks/tree/master/With-React-Hooks)
*   [Live app running in Netlify](https://hackhernews-top-news.netlify.com/)

The app picks 20 new articles from the [HackerNews stories API endpoint](https://hacker-news.firebaseio.com/v0/newstories.json) poll periodically (every 10,000 milliseconds) and shows the number of new stories since the last poll.

For rendering the table, I implemented the npm package [mui-datatable,](https://www.npmjs.com/package/mui-datatables) where you can sort by each heading (score, title, author)and all kinds of multi-parameter filtering.

### So Here Is the Main Class-Based Component Without Using Hooks

The component looks pretty lengthy as for the purpose of this article, I wanted to keep all the functions in the same file.

undefined
undefined

_By using Object.assign above to merge multiple sources_

_let a = Object.assign({foo: 0}, {bar: 1}, {baz: 2});_

_Console.log(a); // => Output => {foo: 0, bar: 1, baz: 2}_

In my case above, the existing state object < this.state > itself is an object with key-value pairs of each story’s parameters. So, basically here, I am merging three objects:

A> empty one {}

B> this.state

C> All the new story {fetchedData: res}

A note on the usage of _Promise.all(…) above for chaining the update of the component’s state to the resulting Promise._

The Promise.all() takes an array (or any iterable) of Promises and returns a single Promise that resolves when all of the Promises passed as an iterable have resolved or when the iterable contains no promises. If any of the passed-in Promises reject, Promise.all asynchronously rejects with the value of the Promise that rejected, whether or not the other Promises have resolved.

Promise.all() method is useful when we have multiple events and have to wait for more than one Promise to complete which is the case here.

Also, the returned values from Promise.all() will be in the order of the promises in the iterable regardless of the order of resolution of Promises. This gives us a clue that the Promise resolution follows a serial order of execution.

So in this way, the getAllNewStory() function will always wait until the data for all of the stories has been fetched before updating the app’s state in one single call.

Here’s how it will work — the getEachStoryGivenId() function returns a Promise, which when resolved gives me the story of the given ID (passed in as the argument to getEachStoryGivenId).

And then getAllNewStory() function will invoke the above as below.

topStories = storyIds.data.slice(0, 2).map(getEachStoryGivenId);

_let_ results = Promise.all(topStories);

So the variable result will wait until all the Promises have been resolved (or up to the first reject). And the result will have the PromiseValue as below (put a console.log(“RESOLVED RESULTS “, results) inside the getAllNewStory function to see this_) —_ which I am then assigning to the fetchedData state variable.

![](https://cdn-images-1.medium.com/max/800/1*UqbmUej-5TEjZRLmQJdxxA.png)
undefined

### And Now the Below Is the Same Component After Migrating to React Hooks

Again, it looks lengthy because I wanted to keep all functions in the same file.

undefined
undefined

Now let's understand a few of the key points of the migration

### Initializing All the States With a Single Liner With useState()

**_const_** \[prevStoriesIds, setPrevStoriesIds\] **=** useState(\[\]);

useState takes an initial state as an argument (in the above an empty array \[\]) and returns a pair (the current state and an updater function) as an array with two items. The first item is the current value (prevStoriesIds above ), and the second is a function ( setPrevStoriesIds ) that lets us update that initial state value.

It means that we’re making two new variables prevStoriesIds and setPrevStoriesIds, where prevStoriesIds is set to the first value returned by useState(), and setPrevStoriesIds is the second.

I could pass the initial value of the state variable as an argument directly, as I have done above. Or, in some cases, I would have to use a function to lazily initialize the variable (useful when the initial state is the result of an expensive computation) like below.

js  
const MyComponent = () => {  
 const prevStoriesIds = useState(() => expensiveComputation());  
 /\* … \*/  
};

The most important point to remember here is that the initial value will be assigned only on the initial render (if it’s a function, it will be executed only on the initial render).

In subsequent renders (due to a change of state in the component or a parent component or after invoking a setState like setCount or whatever, inside useEffect()), the argument passed to the useState hook will be ignored and the current value of the state will be retrieved.

It is important to keep this in mind because, for example, if you want to update the state based on the new properties the component receives, like in the below case, using useState alone won’t work. I have to also implement useEffect.

const Message = props => {  
 const messageState = useState(props.message);  
 /\_ … \_/;  
};

The reason that using useState alone won’t work is because its argument is used the first time only, not every time the property changes. The issue will be the state is being set upon the component being loaded the first time. But when it receives new props, the state (props.message) will not get updated.

Look at the below example for the right way to do this. We must make use of useEffect hooks to implement what you would call the componentWillReceiveProps/getDerivedStateFromProps functionality.

const Persons = props => {  
  // console.log(props.name);

const \[nameState, setNameState\] = useState(props);

useEffect(() => {  
    setNameState(props);  
  }, \[props\]);

return (  
    <div>  
      <p>  
        My name is {props.name} and my age is {props.age}  
      </p>  
      <p>My profession is {props.profession}</p>  
    </div>  
  );  
};

The \*\*setNameState\*\* function is used to update the state. It accepts a new state value as its argument and enqueues a re-render of the component.

### Special Notes on Updating States When the Updated State Depends on the Previous State

Just as with setState in a class component, you need to be careful when updating states derived from something that already is in state (i.e., your new state depends on the old state). State updates using hooks are also batched and, hence, whenever you want to update state based on the previous one it's better to use the callback pattern.

The callback pattern to update state also comes in handy when the setter doesn’t receive updated value from enclosed closure due to it being defined only once. An example of such a case is the useEffect being called only on initial render when it adds a listener that updates state on an event.

If you, for example, update a count twice in a row, it will not work as expected if you don’t use the function version of updating the state.

const { useState } = React;

function App() {  
 const \[count, setCount\] = useState(0);

// This will be bad, might lead to more bugs because such code often end up inside a closure which has an outdated value of myState.  
 function brokenIncrement() {  
  setCount(count + 1);  
  setCount(count + 1);  
 }

// The recommended way is to use a function to update the state  
 function increment() {  
  setCount(count => count + 1);  
  setCount(count => count + 1);  
 }

return (  
  <div>  
   <div>{count}</div>  
   <button onClick={brokenIncrement}>Broken increment</button>  
   <button onClick={increment}>Increment</button>  
  </div>  
 );  
}

### useEffect() Basics

First of all, you can think of useEffect() Hook as componentDidMount, componentDidUpdate, and componentWillUnmount combined.

Mutations, subscriptions, timers, logging, and other side effects are not allowed inside the main body of a function component (referred to as React’s render phase). Doing so will lead to confusing bugs and inconsistencies in the UI. According to the doc, functions passed to useEffect fire after layout and paint. The `useEffect` takes a function as an argument and it will call that function _after_ the main render cycle has completed.

React guarantees the DOM has been updated by the time it runs the effects. By using this useEffect() hook, I tell React that my component needs to do something after render. React will remember the function I passed (referred to as our effect), and call it later after performing the DOM updates.

So I do my initial data fetching by hitting the HN API inside useEffect() with the function fetchData().

Now note, in React class components, the render method itself shouldn’t cause side effects. It would be too early — we typically want to perform our effects after React has updated the DOM. This is why in React classes, we put side effects into componentDidMount() and componentDidUpdate(). The same job is done in useEffect()

Comparing with the class component, note how I had to duplicate almost similar kinds of code between the two lifecycle methods in the class-based component, componentDidMount() and componentDidUpdate().  
This is because in many cases we want to perform the same side effect regardless of whether the component just mounted or if it has been updated. Conceptually, we want it to happen after every render, but React class components don’t have a method like this. We could extract a separate method, but we would still have to call it in two places.

But with useEffect(), I take care of the componentDidUpdate with the concept called dependency array passed as the second argument to useEffect(). More about it below.

### Dependency Array Passed As the Second Argument to useEffect()

By default, useEffect() runs both after the first render and after every update. But we can customize this behaviour with useEffect. You can, however, choose to fire it only when certain values have changed, passing an array of variables as a second optional parameter.

Here’s how it works:

// Without the second parameter  
useEffect(() => {  
  console.log("I will run after every render");  
});

// With the second parameter  
useEffect(() => {  
  console.log("I will run only when valueA changes");  
}, \[valueA\]);

So when the component re-renders, useEffect will check dependencies. If the dependency values changed, useEffect will run the effect.

So here in my component, I have the following format so useEffect runs each time rowsPerPage or noOfNewStoryAfterPolling changes.

useEffect(() **_\=>_** {

 fetchData();

 doPolling();

 **return** () **_\=>_** {

  console**.**log("cleaning up");

  clearInterval(intervalRef**.**current);

};

}, \[rowsPerPage, noOfNewStoryAfterPolling\]);

This second argument to useEffect() callled dependency array, is an array of values and the following rules applies to it.

*   If any of the value in the array changes, the callback will be fired after every render.
*   When it’s not present, the callback will always be fired after every render.
*   When it’s an empty list, the callback will only be fired once, similar to componentDidMount.

#### Dependency Array’s greatest use case is when you want to run a function synchronously which is the most practical common case you will face in daily development work. That is the situation when I want to execute a function ONLY AFTER a specific state has changed.

Here’s how I would have implemented the same with class based component

![](https://cdn-images-1.medium.com/max/800/1*Lm5FOocpXClAkLcvPsKu3w.png)
undefined

And now the same implementaion with useEffect() React-Hooks

![](https://cdn-images-1.medium.com/max/800/1*PiZHgaUahEaRLr0T_DPm2Q.png)
undefined

#### [From the react docs](https://reactjs.org/docs/hooks-reference.html#conditionally-firing-an-effect) on passing empty array as second argument

Passing in an empty array \[\] of inputs tells React that your effect doesn’t depend on any values from the component, so that effect would run only on mount and clean up on unmount; it won’t run on updates.  
If you want to run an effect and clean it up only once (on mount and unmount), you can pass an empty array (\[\]) as a second argument. This tells React that your effect doesn’t depend on any values from props or state, so it never needs to re-run. This isn’t handled as a special case — it follows directly from how the dependencies array always works.

If you pass an empty array (\[\]), the props and state as inside the effect will always have their initial values. While passing \[\] as the second argument is closer to the familiar componentDidMount and componentWillUnmount mental model, there are usually better solutions to avoid re-running effects too often. Also, don’t forget that React defers running useEffect until after the browser has painted, so doing extra work is less of a problem.

#### **More explanation on why we need to pass in an empty array as the second argument to useEffect to run it just once**

This dependency array is diffed from the original creation of the effect and the new one being passed in. It will diff the array (just like it does the virtual DOM) and decide if it needs to re-apply the effect.

Passing in an empty array tells React to diff; however, there is nothing different between each render so the effect will only be run once.

#### Shallow comparison in useEffect()’s second parameter

A very important point on how the second parameter (i.e., the dependency array) is shallow compared to decide if useEffect() will run its effect again.

So by the rule, when the component re-renders, useEffect will check dependencies. If the dependency values changed, useEffect will run the effect

Now the catch is React does a shallow comparison of the dependency array. If you use an object or an array that you mutate, React will think nothing changed because objects are compared by reference.

### What Are Shallow Comparisons in React?

_Shallow compare does check for equality. When comparing scalar values (numbers, strings) it compares their values. When comparing objects, it does not compare their’s attributes — only their references are compared (e.g. “do they point to same object in memory ?).  
In other words,_ shallow comparison will check that primitives have the same value (eg, _1 equals 1_ or that _true equals true_) and that the _references_ are the same between more complex javascript values like objects and arrays.

Let’s consider the following shape of the user object:

user = {  
 name: “John”,  
 surname: “Doe”  
};

An example:

const user = this.state.user;  
user.name = “Jane”;

console.log(user === this.state.user); // true

Notice you changed users' name. Even with this change, objects are equal. The references are exactly the same—meaning no change and no re-render.

#### **What does the official doc say about shallow comparison?**

*   When shallow comparing scalar values (numbers, strings) it compares their values. When comparing objects, it does not compare their attributes, only their references are compared (e.g., do they point to the same object?).
*   Shallow comparison is when the properties of the objects being compared is done using “===” or strict equality and will not conduct comparisons deeper into the properties. So if you shallow compare a deep nested object, it will just check the reference, not the values inside that object.
*   shallowCompare performs a shallow equality check on the current props and nextProps objects as well as the current state and nextState objects.  
     It does this by iterating on the keys of the objects being compared and returning true (i.e., the component should get updated) when the values of a key in each object are not strictly equal.
*   shallowCompare returns true if the shallow comparison for props or state fails and therefore the component should update.
*   shallowCompare returns false if the shallow comparison for props and state both pass and therefore the component does not need to update.

Shallow compare is an efficient way to detect changes. It expects that you don’t mutate data.

And then React had this shallowCompare() function (which is a legacy add-on, and the React team advises to use `[React.memo](https://reactjs.org/docs/react-api.html#reactmemo)` or `[React.PureComponent](https://reactjs.org/docs/react-api.html#reactpurecomponent)` instead). Before React.PureComponent was introduced, shallowCompare was commonly used to achieve the same functionality as PureRenderMixin while using ES6 classes with React.

This shallowCompare() function in React actually works like this (just what the official doc says above), iterating on the keys of the objects being compared and returning true (i.e. saying that the objects are different, meaning a re-render is necessary) when the values of a key in each object are not strictly equal.

### How to Fire Async Function Inside useEffect()

In the above example, I am using axios.get, which returns a Promise.

But a useEffect hook should return nothing or a clean-up function. `useEffect` doesn't expect the callback function to return Promise. Rather, it expects that nothing is returned or a function is returned.

That’s why, if I did not define the fetchData() function separately and then invoke it, I could see the following warning in my developer console log: Warning: useEffect function must return a cleanup function or nothing. Promises and useEffect(async () => …) are not supported, but you can call an async function inside an effect.

That’s why using axios.get directly in the `useEffect` function isn’t the best practice.

### Replacing componentWillUnmount, Which Would Have Handled clearInterval Within useEffect()

In my component above, I have used the following inside useEffect()

**return** () **_\=>_** {

console**.**log("cleaning up");

clearInterval(intervalRef**.**current);

};

This is because, when you return a function in the callback passed to useEffect, the returned function will be called before the component is removed from the UI. In other words, whatever function we return from the useEffect will be treated as componentWillUnmount and will run either when the useEffect runs again or when the component is about to leave the UI.

So to clean up the side effects, you must return a function.

### The Custom Hook Function usePrevious() to Compare Current State and Previous State and Execute Some Function Based on the Comparison Result

The `useRef()` function returns a ref object.

```
const refContainer = useRef(initialValue);
```

Lets first understand the useRef hook by [React Official documentation](https://reactjs.org/docs/hooks-faq.html#is-there-something-like-instance-variables).

The useRef() Hook isn’t just for DOM refs. The “ref” object is a generic container whose current property is mutable and can hold any value, similar to an instance property on a class.

useRef returns a mutable ref object whose .current property is initialized to the passed argument (initialValue). The returned object will persist for the full lifetime of the component.

This works because `useRef()` creates a plain JavaScript object. The only difference between `useRef()` and creating a `{current: ...}` object yourself is that `useRef` will give you the same ref object on every render.

Keep in mind that `useRef` _doesn’t_ notify you when its content changes. Mutating the `.current` property doesn’t cause a re-render.

Even though it gives us a simple thing as a simple object, it keeps the same refs through every render phase. `useRef` can be used to store an arbitrary value. For example, you might want to use useRef to keep a mutable value for the entire life of the component. It’s similar to instance fields (e.g., this.timeoutId) in class components.

You can think of it as useState in terms of hooks, but `useState` will trigger a re-render when you update the state. `useRef` doesn't trigger a re-render; it's just a long-lived value that is held for the life of the component that can be mutated.

![](https://cdn-images-1.medium.com/max/800/1*19_s4Mpb5sR29B9c0VjHrg.png)
undefined

So in the function above, I am returning the ref, which contains the previous value, before it’s updated inside the useEffect() above.

By [Rohan Paul](https://medium.com/@paulrohan) on [June 8, 2019](https://medium.com/p/6fb310aed798).

[Canonical link](https://medium.com/@paulrohan/migrating-react-class-based-component-to-react-hooks-6fb310aed798)

Exported from [Medium](https://medium.com) on December 12, 2020.