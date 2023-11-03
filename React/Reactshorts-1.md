# React Short Notes

## Topics

1. [Definetions](#definetions)
1. [Functional vs Class Component](#functional-vs-class-component)
1. [JSX syntax](#jsx-syntax)
1. [Props](#props)
1. [State](#state)
1. [setState](#setstate-method-in-class-components)
    - [Challenge](#challange)
1. [Destructuring props and state](#destructuring-props-and-state)
1. [Event Handling](#event-handling)
1. [Binding Event Handlers](#binding-event-handlers)
1. [Methods as Props](#methods-as-props---passing-method-from-the-parameter)
1. [Conditional Rendering](#conditional-rendering-1)

## Definetions

1. [Elements](#element)
1. [Component](#component)
1. [Expressions](#expressions)
1. [Props](#props)
1. [defaultProps](#defaultprops)
1. [Destructuring props](#destructuring-props)
1. [JSX spread attributes](#jsx-spread-attributes)
1. [Merge destructured props with other values](#merge-destructured-props-with-other-values)
1. [Short circuit operator](#short-circuit-operator)
1. [Conditional rendering](#conditional-rendering)
1. [Children types](#children-types)
1. [Array as children](#array-as-children)
1. [Function as children](#function-as-children)
1. []()
1. []()

### Element
- **Elements** are anything inside angle brackets.
    - ```jsx
      <div></div>
      <Greeting />
      ```

### Component
- A **Component**, by declaring a function that returns a React [Element](#element).
    - ```jsx
      function Greeting() {
        return <div>Hi there!</div>;
      }
      ```

### Expressions
- Use curly braces to [embed expressions]() in [JSX]().
    - ```jsx
      function Greeting() {
        let name = "chantastic";

        return <div>Hi {name}!</div>;
      }
      ```

### Props

- Take `props` as an argument to allow outside customizations of your Component.
    - ```jsx
      function Greeting(props) {
        return <div>Hi {props.name}!</div>;
      }
      ```

### defaultProps 
- Specify default values for `props` with `defaultProps`.
    - ```jsx
      function Greeting(props) {
        return <div>Hi {props.name}!</div>;
      }
      Greeting.defaultProps = {
        name: "Guest",
      };
      ```

### Destructuring props
- It is a [ES6 JS feature](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment).
    - ```jsx
      let person = { name: "chantastic" };
      let { name } = person; // Destructing object

      let things = ["one", "two"];
      let [first, second] = things; // Destructing Array
      ```
- In React we mostly use like this,
    - ```jsx
      function Greeting(props) {
        return <div>Hi {props.name}!</div>;
      }

      function Greeting({ name }) {
        return <div>Hi {name}!</div>;
      }
      ```

### JSX spread attributes
- Spread Attributes is a feature of [JSX](https://reactjs.org/docs/introducing-jsx.html)
- We can **spread** `restProps` over our `<div>`.
    - ```jsx
      <Greeting name="Fancy pants" className="fancy-greeting" id="user-greeting" />

      function Greeting({ name, ...restProps }) {
        return <div {...restProps}>Hi {name}!</div>;
      }
      ```
### Merge destructured props with other values

Components are abstractions.
Good abstractions allow for extension.

Consider this component that uses a `class` attribute for style a `button`.

```jsx
function MyButton(props) {
  return <button className="btn" {...props} />;
}
```
This works great until we try to extend it with another class.

```jsx
<MyButton className="delete-btn">Delete...</MyButton>
```
In this case, `delete-btn` replaces `btn`.

Order matters for [JSX spread attributes](#jsx-spread-attributes).
The `props.className` being spread is overriding the `className` in our component.

We can change the order but now the `className` will never be anything but `btn`.

```jsx
function MyButton(props) {
  return <button {...props} className="btn" />;
}
```
We need to use destructuring assignment to get the incoming `className` and merge with the base `className`.
We can do this simply by adding all values to an array and joining them with a space.

```jsx
function MyButton({ className, ...props }) {
  let classNames = ["btn", className].join(" ");

  return <button className={classNames} {...props} />;
}
```
To guard from `undefined` showing up as a className, you could update your logic to filter out `falsy` values:

```jsx
function MyButton({ className, ...props }) {
  let classNames = ["btn", className].filter(Boolean).join(" ").trim();

  return <button className={classNames} {...props} />;
}
```
Bear in mind though that if an empty object is passed it'll be included in the class as well, resulting in: `btn [object Object]`.

The better approach is to make use of available packages, like [classnames](https://www.npmjs.com/package/classnames) or [clsx](https://www.npmjs.com/package/clsx), that could be used to join classnames, relieving you from having to deal with it manually.

### Short circuit operator
- For example, in the expression `a && (b + c)`, if `a` is falsy, then the sub-expression `(b + c)` will *NOT* even get evaluated, even if it is grouped and therefore has higher precedence than `&&`. We could say that the logical AND operator (`&&`) is "**short-circuited**".

### Conditional rendering

You *cannot* use `if/else` statements inside a component declarations.
So conditional [(ternary) operator]() and [short-circuit](#short-circuit-operator) evaluation are your friends.

1. if
    - ```jsx
      {
        condition && <span>Rendered when `truthy`</span>;
      }
      ```
1. unless
    - ```jsx
      {
        condition || <span>Rendered when `falsy`</span>;
      }
      ```
1. if-else
    - ```jsx
      {
        condition ? (
          <span>Rendered when `truthy`</span>
        ) : (
          <span>Rendered when `falsy`</span>
        );
      }
      ```
### Children types

- React can render children from most types.
- In most cases it's either an `array` or a `string`.

<table>
  <tr>
    <th>String</th>
    <th>Array</th>
  </tr>
  <tr>
    <td>

      ```jsx
      <div>Hello World!</div>
      ```

  </td>
  <td>

      ```jsx
      <div>{["Hello ", <span>World</span>, "!"]}</div>
      ```

  </td>
  </tr>
</table>

#### Array as children

Providing an array as `children` is a very common.
It's how lists are drawn in React.

We use `map()` to create an array of React Elements for every value in the array.

```jsx
<ul>
  {["first", "second"].map((item) => (
    <li>{item}</li>
  ))}
</ul>
```
That's equivalent to providing a literal `array`.

```jsx
<ul>{[<li>first</li>, <li>second</li>]}</ul>
```

This pattern can be combined with destructuring, JSX Spread Attributes, and other components, for some serious terseness.

```jsx
<ul>
  {arrayOfMessageObjects.map(({ id, ...message }) => (
    <Message key={id} {...message} />
  ))}
</ul>
```
### Function as children

> React components *DO NOT* support functions as `children`.
However, [render props](#render-prop) is a pattern for creating components that take functions as children.

#### Render prop


## Functional vs Class Component

<table>
  <thead>
    <tr>
      <th>Functional Component</th>
      <th>Class Component</th>
  </tr>
  </thead>
  <tbody>
    <tr>
      <td>Simple functions</td>
      <td>More Rich Features</td>
    </tr>
    <tr>
      <td>Use Func components as much as Possible</td>
      <td>Maintain their own private data - state</td>
    </tr>
    <tr>
      <td>Absense of 'this' keyword. </td>
      <td>Complex UI logic</td>
    </tr>
    <tr>
      <td>Solution without using `state` </td>
      <td>Provide Lifecycle Hooks</td>
    </tr>
    <tr>
      <td>mainly responsibel for UI</td>
      <td>Stateful/ Smart/ Container</td>
    </tr>
    <tr>
      <td>Stateless/ Dumb/ Presentational</td>
    </tr>
</tbody>
</table>

## JSX Syntax

1. JavaScript XML (JSX) - Extension to the JavaScript language syntax.
1. Write XML-like code for elements and components.
1. JSX tags have a tag name, attributes, and children.
1. JSX is not a necessity to write React applications.
1. JSX makes your react code simpler and elegant.
1. JSX ultimately transpiles to pure JavaScript which is understood by the browsers.




```jsx
import React from "react";

const Hello = () = {
//  return(
//    <div className='dummyClass'>
//      <h1>Hello Vishwas</h1>
//    <div>
//  )

  return React.createElement('div',{id:'hello', className: 'dummyClass'},React.createElement('h1', null,'Hello Vishwas')
  )
  
}
export default Hello
```

***JSX differences***
- class -> className
- for -> htmiFor
- camelCase property naming convention
- onclick -> onClick
- tabindex > tabindex

## Props

- Props should be written in `{...}` *curly brackets*.
- Props are ***immutable***, *unchangable*. You cannot assign a value to `props`.
- Props can be write in both Func and Class Component
- `this` keyword is used to use **props** in *Class* component, `this.props`

1. props is a Passing parameter to the Component.
    -   ```jsx
        // Greet.jsx

        const Greet = (props) => {
          console.log(props);
          // props.name = 'Salman'  // error, props are immutabale
          return <h1>Hello {props.name} a.k.a {props.heroname}</h1> 
        }

        // App.jsx

        render(){
          return(
            <div>
              <Greet name="Bruce" heroname="Batman">
            </div>
          )
        }
        ```

1. We can pass *children* means inner Content to component
    - ```jsx
      // Greet.jsx

      const Greet = (props) => {
        console.log(props);
        return (
          <div>
            <h1>Hello {props.name} a.k.a {props.heroname}</h1>
            {props.children}
          </div>
        )
      }

      // App.jsx

      render(){
        return(
          <div>
            <Greet name="Bruce" heroname="Batman">
              Bruce Also known as Batman // this content is called children inside a component
            </Greet>
            <Greet name="Bruce" heroname="Batman" /> // we passed nothing here so nothing will be rendered
            <Greet name="Bruce" heroname="Batman">
              <button>CLick me</button> // button is rendere only this component
            </Greet>
          </div>
        )
      }
      ```


## State

- `state` - is used to *store* the value.
- `setState` - is a **method** used to *change* the current `state` value.
- We need to import `useState` and use it in function
   -  `import { useState } from 'react';`
   -  `const [index, setIndex] = useState(0);`
- `useState('...')` in this, we have to add a pre-value.



<table>
  <thead>
    <tr>
      <th>Props</th>
      <th>State</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Props get passed to the components</td>
      <td>state is managed within the component</td>
    </tr>
    <tr>
      <td>Function parameters</td>
      <td>Variables declared in the function body</td>
    </tr>
    <tr>
      <td>props are immutable</td>
       <td>State can be changed</td>
    </tr>
    <tr>
      <td>
        props- Functional Comp. <br>
        this.props - Class Comp. 
      </td>
      <td>
        useState Hook - Function comp. <br>
        this.state - Class comp. 
      </td>
    </tr>
  </tbody>
</table>

**Suppose change the Name by click on Button using props** but it gives error.

1. Take another variable called `value`, click on button. Nothing change in UI, but variable value is changed
   - ```jsx
      // Message.jsx

      import React from 'react'

      const Message = (props) => {
        let value = props.name;
        return (
          <div>
            <h1>{value}</h1>
            <button onClick={()=>{value= 'salman'; console.log(value);}}>Click</button>
          </div>
        )
      }

      export default Message;


      // App.jsx class component

      render(){
        return(
          <div>
            <Greet name="Bruce"/>
          </div>
        )
      }
      ```
1. Without using Variable, you will get *error*
   -  ```jsx
      // Message.jsx

      import React from 'react'

      const Message = (props) => {
        return (
          <div>
            <h1>{props.name}</h1>
            <button onClick={props.name = 'salman'}>Click</button> // error, props are immutable
          </div>
        )
      }

      export default Message;


      // App.jsx class component

      render(){
        return(
          <div>
            <Message name="Bruce"/>
          </div>
        )
      }
      ```
1. Using `state` **Hook**,

   -  ```jsx
      // Message class component

      import React, { Component } from 'react'

      export default class Message extends Component {

        constructor(){
          super(); // we extend parent Component, so we need call parent constructor first.
          this.state = {
            name: this.props.name
          }
        }

        changeMessage(){
          this.setState({
            name : 'Salman'
          })
        }

        render() {
          return (
            <div>
              <h1>Welcome {this.state.name}</h1>
              <button onClick={() => this.changeMessage()}>Click Me</button>
            </div>
          )
        }
      }


      // Message Functional Component

      import React, { useState } from 'react'

      const Message = (props) => {

        const [name, setName] = useState(props.name);
        const [index, setIndex] = useState(0); // you can write as many as useState, it won't affect to another useState method(state)

        return (
          <div>
            <h1>Welcome {name}</h1>
            <button onClick={() => {setName('Hello')}}>Click</button>
          </div>
        )
      }

      export default Message;

      // App.jsx

      render(){
        return(
          <div>
            <Message name="Bruce"/>
          </div>
        )
      }
      ```

## setState method in class components

1. Always make use of setState and never modify the state directly.
1. Code has to be executed after the state has been updated? Place that code in the call back
function which is the second argument to the setState method.
1. When you have to update state based on the previous state value, pass in a function as an
argument instead of the regular object.

1. `this.setState()` is *async* method, to avoid this, use **callback function** in second parameter
   - here `count` is showing `0` in *console* and `1` is showing in UI. Means *UI count* is greater than *console count*.
   -  ```jsx
      // Message class component

      import React, { Component } from 'react'

      export default class Message extends Component {

        constructor(){
          super(); // we extend parent Component, so we need call parent constructor first.
          this.state = {
            count: 0
          }
        }

        increment(){
          this.setState({
            count : this.state.count + 1
          });
          console.log('count' + this.state.count)
        }

        render() {
          return (
            <div>
              <h1>Count {this.state.count}</h1>
              <button onClick={() => this.increment()}>Increment</button>
            </div>
          )
        }
      }
      ```

   -  using *callback function*

   -  ```jsx
      this.setState({
        count : this.state.count + 1
      }, () => {console.log('callback function parameter'+ this.state.count)});
      console.log('count' + this.state.count)
      ```
### Challange

**Update 5 times increment with one click**

```jsx
// Message class component

import React, { Component } from 'react'

export default class Message extends Component {

  constructor(){
    super(); // we extend parent Component, so we need call parent constructor first.
    this.state = {
      count: 0
    }
  }

  increment(){
    this.setState({
      count : this.state.count + 1
    });
    console.log('count' + this.state.count)
  }

  incrementFive(){
    this.increment();
    this.increment();
    this.increment();
    this.increment();
    this.increment();
  }

  render() {
    return (
      <div>
        <h1>Count {this.state.count}</h1>
        <button onClick={() => this.incrementFive()}>Increment</button>
      </div>
    )
  }
}
```

Above code won't update to 5, it updates to 1;

***Solultion***

```jsx
// Message class component

import React, { Component } from 'react'

export default class Message extends Component {

  constructor(){
    super(); // we extend parent Component, so we need call parent constructor first.
    this.state = {
      count: 0
    }
  }

  increment(){
    this.setState((prevState) => {            // SOLUTION
      count: prevState + 1;                   // SOLUTION
    })
    console.log('count' + this.state.count)
  }

  incrementFive(){
    this.increment();
    this.increment();
    this.increment();
    this.increment();
    this.increment();
  }

  render() {
    return (
      <div>
        <h1>Count {this.state.count}</h1>
        <button onClick={() => this.incrementFive()}>Increment</button>
      </div>
    )
  }
}
```


## Destructuring props and state

1. Should use `{ }` *curly braces*

<table>
  <thead>
    <tr>
      <th>without Props(Destructing props)</th>
      <th>with Props</th>
    </tr>
  </thead>
  <tbody>
  <tr>
  <td>

  ```jsx
  // function 

  const Greet = ({name, heroName}) => {
    return (
      <div>
        <h1>Hello {name} a.k.a {heroName}</h1>
      <div>
    )
  }
  ```
  ```jsx
  // Class component

  render() {
    const {name, heroName} = this.props;
    return (
      <div>
        <h1>Hi {name} a.k.a {heroName}</h1>
      </div>
    )
  }
  ```

</td>
<td>

  ```jsx
  // function

  const Greet = (props) => {
    return (
      <div>
        <h1>Hello {props.name} a.k.a {props.heroName}</h1>
      <div>
    )
  }
  ```
  ```jsx
  // Class component

  render() {
    return (
      <div>
        <h1>Hi {this.props.name} a.k.a {this.props.heroName}</h1>
      </div>
    )
  }
  ```
</td>
</tr>
</tbody>
</table>

## Event Handling

1. All events should be in **camelCase** like, `onClick`, `onScroll`, etc...
1. When you are using the event handler, you should *NOT* call the function. You should write function name *with out parathesis*
    - `onClick={functionName}`    *Correct*
    - `onClick={functionName()}`  *InCorrect* it is known as **function call**

<table>
  <thead>
    <tr>
      <th>Function Component</th>
      <th>Class Component</th>
    </tr>
  </thead>
  <tbody>
  <tr>
  <td>

  ```jsx
  // function component

  function buttonEvent(){
    console.log('button is clicked of function component')
  }

  const Greet = ({name, heroName}) => {
    return (
      <div>
        <button onClick={buttonEvent}>function Click</button>
      <div>
    )
  }
  ```

</td>
<td>

  ```jsx
  // Class component

  buttonEvent(){
    console.log('button is clicked of function component')
  }

  render() {
    return (
      <div>
        <button onClick={this.buttonEvent}>Class Click</button>
      </div>
    )
  }
  ```
</td>
</tr>
</tbody>
</table>

## Binding Event Handlers

If you are `this` keyword in methods, then you should bind the `this` keyword.

The code below encounters an error. To resolve this, we have three solutions:
1. Bind methond                (NOT to use due to *performance implications*)
1. Inline Arrow function       (you can use but it might have chances of *performance issue*)
1. Bind method in constructor  (*official documention method* Best Option)
1. Arrow function              (*official documention method* but experimental)

Because `this` keyword gives an `undefined` error, if you see in `console.log(this)`, you'll get `undefined`.

```jsx
// Message class component

import React, { Component } from 'react'

export default class Message extends Component {

  constructor(){
    super(); // we extend parent Component, so we need call parent constructor first.
    this.state = {
      count: 0
    }
  }

  changeCount(){
    this.setState({
      count : this.state.count + 1
    });
  }

  render() {
    return (
      <div>
        <h1>Count {this.state.count}</h1>
        <button onClick={this.changeCount}>Increment</button>
      </div>
    )
  }
}
```

<table>
<thead>
  <tr>
    <th>using Bind method</th>
    <th>Inline Arrow function</th>
  </tr>
</thead>
<tbody>
<tr>
<td>

```jsx
render() {
  return (
    <div>
      <h1>Count {this.state.count}</h1>
      <button onClick={this.changeCount.bind(this)}>Increment</button>
    </div>
  )
}
```

</td>
<td>
        
```jsx
// here we are using paranthesis this.changeMessage() instead of this.changeMessage, so calling the function & returning it's value
render() {
  return (
    <div>
      <h1>Count {this.state.count}</h1>
      <button onClick={() => {this.changeCount()}}>Increment</button> 
    </div>
  )
}
```

</td>
</tr>

<tr>
    <th>Using Bind method in constructor</th>
    <th>Using Arrow function</th>
  </tr>
  <tr>
  <td>

```jsx
constructor(){
    super(); // we extend parent Component, so we need call parent constructor first.
    this.state = {
      count: 0
    }

    this.changeCount = this.changeCount.bind(this)
  }

  /* ... */

  render() {
    return (
      <div>
        <h1>Count {this.state.count}</h1>
        <button onClick={this.changeCount}>Increment</button>
      </div>
    )
  }
```

</td>
<td>

```jsx
changeCount = () => {
    this.setState({
      count : this.state.count + 1
    });
}

render() {
  return (
    <div>
      <h1>Count {this.state.count}</h1>
      <button onClick={this.changeCount}>Increment</button>
    </div>
  )
}
```

</td>
  </tr>
</tbody>
</table>


## Methods as Props - passing method from the parameter


<table>
<thead>
<tr>
  <th>ParentComponent.jsx</th>
  <th>ChildComponent.jsx</th>
</tr>
</thead>
<tbody>
<tr>
<td>

```jsx
import React, { Component } from 'react'

export default class ParentComponent extends Component {

  constructor(){
    super(); // 
    
    this.state = {
      parentName : 'Parent'
    }

    this.greetParent = this.greetparent.bind(this)
  }

  greetParent(){
    alert(`hello ${this.state.parentName}`); // this is ES6 syntax, Literals. NOT the reactJS syntax
  }

  render() {
    return (
      <div>
        <ChildComponent greetHandler={this.greetParent}/>
      </div>
    )
  }
}
```

</td>
<td>

```jsx
// ChildComponent
import React from 'react';

export default function ChildComponent = (props) => {
  return (
    <div>
      <button onClick={props.greetHandler}>Greet Parent</button>
    <div>
  )
}
```
</td>
</tr>
</tbody>
</table>

<table>
<thead>
  <tr>
    <th>ParentComponent.jsx</th>
    <th>ChildComponent.jsx</th>
  </tr>
</thead>
<tbody>
<tr>
<td>

```jsx
import React, { Component } from 'react'

export default class ParentComponent extends Component {

  constructor(){
    super(); // 
    
    this.state = {
      parentName : 'Parent'
    }

    this.greetParent = this.greetparent.bind(this)
  }

  greetParent(childName){
    alert(`hello ${this.state.parentName} from ${childName}`); // this is ES6 syntax, Literals. NOT the reactJS syntax
  }

  render() {
    return (
      <div>
        <ChildComponent greetHandler={this.greetParent}/>
      </div>
    )
  }
}
```
</td>
<td>

```jsx
// ChildComponent
import React from 'react';

export default function ChildComponent = (props) => {
  return (
    <div>
      <button onClick={() => props.greetHandler('child')}>Greet Parent</button>
    </div>
  )
}
```

</td>  
</tr>
</tbody>
</table>

## Conditional Rendering

We can do in four approaches,

1. if/else
1. Element Variables
1. Ternary conditional Operator
1. Short circuit operator

Here I want to render based on condition, the base code is below

```jsx
import React, { Component } from 'react'

export default class Message extends Component {

  constructor(){
    super(); // we extend parent Component, so we need call parent constructor first.
    this.state = {
      isLoggedIn: false
    }
  }

  render() {
    return (
      <div>
        <h1>Welcome Salman</h1>
        <h1>Welcome Guest</h1>
      </div>
    )
  }
}
```

1. Using `if/else` approach
    - ```jsx
      render() {
        if(this.state.isLoggedIn){
            return <h1>Welcome Salman</h1> 
        } else{
            return <h1>Welcome Guest</h1>
        }
      }
      ```
1. Using **Element variable** approach
    - ```jsx
      render() {
        let message;

        if(this.state.isLoggedIn){
            message = <h1>Welcome Salman</h1> 
        } else{
            message = <h1>Welcome Guest</h1>
        }

        return <div>{message}</div>
      }
      ```
1. Using Ternary Operator
    - ```jsx
      render() {
        return this.state.isLoggedIn ? <h1>Welcome Salman</h1> : <h1>Welcome Guest</h1>
      }
      ```

1. Short circuit operator
    - ```jsx
      render(){
        this.state.isLoggedIn && <h1>Welcome Salman</h1>
      }
      ```



