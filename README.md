#  What is React? 

React is a declarative, efficient, and flexible JavaScript library for building user interfaces.

#### Install React

* Via yarn
```
yarn init
yarn add react react-dom
```

* Via npm 
```
npm init
npm install --save react react-dom
```

### CDN Link

```
<script crossorigin src="https://unpkg.com/react@16/umd/react.development.js"></script>

<script crossorigin src="https://unpkg.com/react-dom@16/umd/react-dom.development.js"></script>
```

### Create React App


Create React App is the best way to start building a new React single page application(SPA).

```
npm install -g create-react-app
create-react-app my-app

cd my-app
npm start
```

### Modern Build Pipeline

* **Package Manager** such as `Yarn` or `npm`. It lets you take advantage of a vast ecosystem of third-party packages, and easily install or update them.

* **Bundler** such as `webpack` or `Browserify`. It lets you write modular code and bundle it together into small packages to optimize load time.

* **Compiler** such as `Babel`. It lets you write modern JavaScript code that still works in older browsers.

>**Note**: Instead of artificially separating technologies by putting markup and logic in separate files, React separates concerns with loosely coupled units called “components” that contain both

### Embedding Expressions in JSX


You can embed any JavaScript expression in JSX by wrapping it in curly braces.
```
function formatName(user) {
  return user.firstName + ' ' + user.lastName;
}

const user = {
  firstName: 'Harper',
  lastName: 'Perez'
};

const element = (
  <h1>
    Hello, {formatName(user)}!
  </h1>
);

ReactDOM.render(
  element,
  document.getElementById('root')
);
```

### Specifying Attributes with JSX

You may also use curly braces to embed a JavaScript expression in an attribute:

```
const element = <img src={user.avatarUrl}></img>;
```

**Note**: Don’t put quotes around curly braces when embedding a JavaScript expression in an attribute. You should either use quotes (for string values) or curly braces (for expressions), but not both in the same attribute

### JSX Prevents Injection Attacks

It is safe to embed user input in JSX:
```
const title = response.potentiallyMaliciousInput;
// This is safe:
const element = <h1>{title}</h1>;
```
By default, React DOM escapes any values embedded in JSX before rendering them. Thus it ensures that you can never inject anything that’s not explicitly written in your application. 

# Elements

Elements are the smallest building blocks of React apps.
An element describes what you want to see on the screen:
```
const element = <h1>Hello, world</h1>;
```

* React elements are immutable. 
* Once you create an element, you can’t change its children or attributes.
* With our knowledge so far, the only way to update the UI is to create a new element, and pass it to ReactDOM.render().

> **React Only Updates What’s Necessary**: 
> React DOM compares the element and its children to the previous one, and only applies the DOM updates necessary to bring the DOM to the desired state.

# Components

Components let you split the UI into independent, reusable pieces, and think about each piece in isolation.

Conceptually, components are like JavaScript functions. They accept arbitrary inputs (called “props” short form for properties ) and return React elements describing what should appear on the screen.

The simplest way to define a component is to write a JavaScript function:
```
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}
```

## Rendering a Component

Elements can also represent user-defined components:

```
const element = <Welcome name="Sara" />;
```
When React sees an element representing a user-defined component, it passes JSX attributes to this component as a single object. We call this object “props”.

```
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

const element = <Welcome name="Sara" />;
ReactDOM.render(
  element,
  document.getElementById('root')
);
```

* We call `ReactDOM.render()` with the `<Welcome name="Sara" />` element.
* React calls the Welcome component with `{name: 'Sara'}` as the **props**.
* Our Welcome component returns a `<h1>Hello, Sara</h1>`element as the result.
* React DOM efficiently updates the DOM to match `<h1>Hello, Sara</h1>`.

> **Note**: Always start component names with a capital letter.
> React treats components starting with lowercase letters as DOM tags.

## Composing Components

Components can refer to other components in their output.

```
function App() {
  return (
    <div>
      <Welcome name="Sara" />
      <Welcome name="Cahal" />
      <Welcome name="Edite" />
    </div>
  );
}
```
* Typically, new React apps have a single App component at the very top.

## Props are Read-Only

Whether you declare a component as a function or a class, it must never modify its own props.

Such functions are called “pure” because they do not attempt to change their inputs, and always return the same result for the same inputs.

```
function sum(a, b) {
  return a + b;
}
```

* All React components must act like pure functions with respect to their props.

# State and Lifecycle

State is similar to props, but it is private and fully controlled by the component.

Local state is a feature available only to classes.

### Converting a Function to a Class

You can convert a functional component like Clock to a class in five steps:

* Create an ES6 class, with the same name, that extends React.Component.

* Add a single empty method to it called render().

* Move the body of the function into the render() method.

* Replace props with this.props in the render() body.

* Delete the remaining empty function declaration.

#### Function
```
function Clock(props) {
  return (
    <div>
      <h1>Hello, world!</h1>
      <h2>It is {props.date.toLocaleTimeString()}.</h2>
    </div>
  );
}

Usage: ReactDOM.render(
    <Clock date={new Date()} />,
    document.getElementById('root')
  );
```

#### Class (with STATE)
```
class Clock extends React.Component {
  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.props.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}

Usage: ReactDOM.render(
  <Clock />,
  document.getElementById('root')
);
```

## Adding Local State to a Class

* Replace this.props.date with this.state.date in the render() method:

* Add a class constructor that assigns the initial this.state:

* Remove the date prop from the <Clock /> element:

The result looks like this:
```
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }

  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}

ReactDOM.render(
  <Clock />,
  document.getElementById('root')
);
```

## Adding Lifecycle Methods to a Class

In applications with many components, it’s very important to free up resources taken by the components when they are destroyed.

* We want to set up a timer whenever the Clock is rendered to the DOM for the first time. This is called “mounting” in React.

* We also want to clear that timer whenever the DOM produced by the Clock is removed. This is called “unmounting” in React.

* We can declare special methods on the component class to run some code when a component mounts and unmounts. These methods are called “lifecycle hooks”.

Eg: 
```
componentDidMount() {
  }

  componentWillUnmount() {
  }
```
* The componentDidMount() hook runs after the component output has been rendered to the DOM

* We will tear down the timer in the componentWillUnmount() lifecycle hook.


```
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }

  componentDidMount() {
    this.timerID = setInterval(
      () => this.tick(),
      1000
    );
  }

  componentWillUnmount() {
    clearInterval(this.timerID);
  }

  tick() {
    this.setState({
      date: new Date()
    });
  }

  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}

ReactDOM.render(
  <Clock />,
  document.getElementById('root')
);
```

Working of the above code:

* When <Clock /> is passed to ReactDOM.render(), React calls the constructor of the Clock component. Since Clock needs to display the current time, it initializes this.state with an object including the current time. We will later update this state.

* React then calls the Clock component’s render() method. This is how React learns what should be displayed on the screen. React then updates the DOM to match the Clock’s render output.

* When the Clock output is inserted in the DOM, React calls the componentDidMount() lifecycle hook. Inside it, the Clock component asks the browser to set up a timer to call the component’s tick() method once a second.

* Every second the browser calls the tick() method. Inside it, the Clock component schedules a UI update by calling setState() with an object containing the current time. Thanks to the setState() call, React knows the state has changed, and calls the render() method again to learn what should be on the screen. This time, this.state.date in the render() method will be different, and so the render output will include the updated time. React updates the DOM accordingly.

* If the Clock component is ever removed from the DOM, React calls the componentWillUnmount() lifecycle hook so the timer is stopped.

There are three things you should know about setState().

1. Do Not Modify State Directly

```
// Wrong
this.state.comment = 'Hello';
// Correct
this.setState({comment: 'Hello'});
```

2. State Updates May Be Asynchronous

Because this.props and this.state may be updated asynchronously, you should not rely on their values for calculating the next state.
```
// Wrong
this.setState({
  counter: this.state.counter + this.props.increment,
});
// Correct
this.setState((prevState, props) => ({
  counter: prevState.counter + props.increment
}));
```

3. State Updates are Merged

When you call setState(), React merges the object you provide into the current state.

For example, your state may contain several independent variables:

```
this.state = {
  posts: [],
  comments: []
};
```

Then you can update them independently with separate setState() calls:
```
fetchPosts().then(response => {
  this.setState({
    posts: response.posts
  });
});

fetchComments().then(response => {
  this.setState({
    comments: response.comments
  });
});
```

The merging is shallow, so this.setState({comments}) leaves this.state.posts intact.

* Note: Neither parent nor child components can know if a certain component is stateful or stateless. 

* This is why state is often called local or encapsulated. It is not accessible to any component other than the one that owns and sets it.

* This is commonly called a “top-down” or “unidirectional” data flow. Any state is always owned by some specific component, and any data or UI derived from that state can only affect components “below” them in the tree.