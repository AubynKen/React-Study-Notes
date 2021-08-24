## Installation

### Scaffolding

```shell
npx create-react-app [app_name]
```

### Running

```shell
npm start
```

---

## Basic Syntax

### Creation

```js
const title = React.createElement("h1", { id: tytle-element}, "hello");
```

### Rendering

```js
ReactDOM.render(title, document.getElementById('root'));
```

---

## JSX Basics

JSX allows developers to insert HTML code directly into JavaScript.

```jsx
const title = (<h1 id="title-element">Hello</h1>);
```

### Use of Parenthesis

Having parenthesis around JSX syntax avoids the automatic semicolon insertions.

### Babel

Babel internally converts the HTML syntax from JSX to JavaScript

### Attribuets

Use camelCase for tag fields just like in vanilla JS Dom manipulations. for eg.

```html
<h1 className="title">
  ... <!-- do not use class="title"-->
</h1>
```

---

## Element Embedding

```jsx
const name = "jack";

const hello = (<p>Hello, my name is { name }!</p>);
```

### Rendering an iterable

```jsx
let students = [
  { id: 1, name: "John" },
  { id: 2, name: "Jack" },
  { id: 3, name: "Johanne"}
]

const myList = (<ul>{ 
          students.map( item => <li key={item.id}> {item.name} </li>} </ul>);
                                 
```

)when rendering an iterable, we must assign an unique key to each element. The key isn't rendered in HTML. It is used for React element manipulations.



---

## Styling

### Inline Styling

```jsx
<h1 style={{ color: 'red', backgroundColor: 'blue'}}>...</h1>
```

#### Why 2 parenthesis:

- The first parenthesis indicates that we're embedding elements inside JSX syntax.
- The second parenthesis indicates that this is a JavaScript object. 

### Using CSS class

```css
.title {
  color: red,
  background-color: blue
}
```

```jsx
<h1 className="title">...</h1>
```

We use **className** instead of **class** because it's **camelCased**.

---

# Components

## function components

```jsx
function Hello() {
  return (<h1>Hello, React</h1>);
}

ReactDOM.render(<Hello/>, document.getElementById('root'));
```



- Function name follows **C**amelCase. (tag names starting with a lower case letter are recognized as html tags)
- Function component returns an element.
- To render nothing, return null. (Always have a return value.)

## Arrow function

```jsx
const Hello = () => <h1>Hello, React</h1>;
```

### Class components

```jsx
class Hello extends React.Component {
    render() {
        return ( <h1>Hello, React</h1>);
    }
}
```

- extend from React.Component
- haev a render function, that has a return value.

### Separate file for storing a component

It is best practice to store a component in an independent file.

```jsx
import React from 'react';

class Hello extends React.Component { ... }

export default Hello
```

```jsx
import Hello from '.Hello.js';
ReactDom.render(<hello/>, root);
```

---

## Event Binding

The exeamples below show an h1 element which, when clicked on, console logs Hello.

### class component

- Define a class method for handling the event.

```jsx
class Hello extends React.Component {
    handleClick() {
        console.log("hello");
    }
    render() {
        return (<h1 onClick={this.handleClick}>Hello React</h1>);
    }
}
```

### function component

- Define a nested function inside the function

```jsx
function Hello() {
    function handleClick() {
        console.log("Hello");
    }
    return (<h1 onClick={handleClick}>Hello React</h1>)
}
```

### event.preventDefault

You can prevent the default behavior by passing in an event elements in the handler function and use preventDefault

```jsx
export default class Hello extends React.Component {
    1andleClick(e) {
        e.preventDefault(); <!-- this prevents triggering a redirection when anchor tag is clicked -->
        console.log("anchor tag clicked")
    }
  <!-- We render an anchor tag with an href fields -->
    render() {
        return (<a href="http://www.google.com" onClick={this.handleClick}>Hello</a>);
    }
}
```

---

## State

A function component is also called a **stateless** component because it can't have a state.

A class component is a **stateful** component.

```jsx
class Hello extends React.Component {
  <!-- we can set the state as an object in the constructor -->
  constuctor() {
    super();
    this.state = {
      count: 0
    };
  }
  <!-- alternatively, we can use the shortcut field initialization syntax -->
  state = { count: 0 };
  render() {
    return <div>Current count: {this.state.count}</div>
  }
}
```



### setState

- updates the state
- updates the DOM Nodes using the values

```js
this.setState({
  field1: someValue,
  field2: someValue,
  ...
})
```



## this keyword caveat

In callback functions, the this keyword isn't the component instance.

e.g.

```js
class SomeClass {
  someMethod() {
    console.log(this);
  }
}

const myInstance = new SomeClass();
setTimeOut(myInstance.someMethod(), 500); // Prints out 'undefined'
```



### solutions:

- Use arrow functions 

```js
someMethod = () => {
  console.log(this);
}
```

* Wrap the function to an arrow function

```js
setTimeOut(() => myInstance.someMethod(), 500)
```

* Bind the this keyword in someMethod

```js
class SomeCclass {
  constructor() {
    this.someMethod = this.someMethod.bind(this); // this creates a copy of someMethod, with this always pointing to its SomeClass instance
  }
  someMethod() {...}
}
```



### Tip: How can you use the same handler function for different state attributes corresponding to different tags?

You set a different name attribute for each html tag.

The example below shows a handler that can update the textInput and the textArea fields of the state, depending on which one's value changed.

```jsx
// here we have a text input and a text area 
render() {
  return (<div>
      <!-- same on-change handler, different behavior depending on the name attribute -->
      <input name="textInput" onChange={this.handleChange} type="text"></input>
      <textarea name="textArea" onChange={this.handleChange}></textarea>
    </div>)
}

state = {
  textInput: "",
  textArea: ""
}

handleChange = e => { 
  // retrieve the target, ie. the input tag or the textarea tag
  const target = e.target;
  // retrieve value of the name attribute, ie. "textInput" or "textArea"
  const name = target.name;
  this.setState({
    [name] = target.value;
    // [] transforms a string into a variable name, for eg. this can be equivalent to:
    // textInput: target.value
    // or:
    // textArea: targe.value
  })
}
```

## 

---

# Refs

**ref** provides a way to access DOM nodes/ React Elements created in the render function.

## Syntax

### Creating a ref

```jsx
constuctor() {
  super();
  this.myRef = React.createRef();
}
```

### Associating a ref to a DOM node

```jsx
render() {
  return <h1 ref={this.myRef}>...</h1>;
}
```

### Accessing the DOM node

```jsx
this.myRef.current
```



## Example

The code below shows a text input with a button.

Whenever the button is clicked, the value in the text input gets printed out.

```jsx
class PrintText extends React.Component {
    constructor() {
        super();
        this.textRef = React.createRef();
    }

    printInput = () => {
        const inputValue = this.textRef.current.value;
        console.log(`Current Value: ${inputValue}`);
    };

    render() {
        return (
            <p>
                <input type="text" ref={this.textRef}></input>
                <button onClick={this.printInput}>Print Text</button>
            </p>
        );
    }
}
```

