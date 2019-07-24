# React + Typescript Cheatsheet

## Components

### Functional Component Boilerplate
```tsx
import React, { FunctionComponent } from "react";

const Hello: FunctionComponent = () => {
  return <div>Hallo</div>;
}

export default Hello;
```

### Functional Component with children
#### Component Declaration
```tsx
import React, { FunctionComponent } from "react";

// Children's type is inferred by the FunctionComponent interface
const Hello: FunctionComponent = ({ children }) => {
  return <div>{children}</div>;
}

export default Hello;
```
#### Component Usage
```tsx
<Hello>
  <span>This span will be displayed, where children is used in the 'Hello' component</span>
<Hello>
```


### Functional Component with props
```tsx
import React, { FunctionComponent } from "react";

// Type definitions for our prop bindings
interface HelloProps {
  name: string
}

// Functions can infer interfaces in typescript
// We are passing props as an object with the `{}` syntax
// Inside are all properties defined by our HelloProps interface
const Hello: FunctionComponent<HelloProps> = ({ name }) => {
  return <div>Hello, {name}</div>;
}

export default Hello;
```

## Templating
While react does not have a templating engine itself, comming templating tasks can be easily achieved by using tsx.

### Conditional Syntax

#### Simple conditionals with ternary syntax
```tsx
const Hello: FunctionComponent = () => {
  const condition = 1 > 2;
  return (
    <div>
      <div>I'm always shown</div>
      {condition ? <div>Condition is true</div> : <div>Condition is false</div>}
    </div>
  );
};
```

#### Conditions with if / else
```tsx
const Hello: FunctionComponent = () => {
  let output = null;
  if (1 > 2) {
    output = <div>Condition is true</div>;
  } else {
    output = <div>Condition is false</div>;
  }
  return (
    <div>
      <div>I'm always shown</div>
      {output}
    </div>
  );
};
```

#### Conditions with if / else inside the "template"
```tsx
  return (
    <div>
      <div>I'm always shown</div>
      {/* We can use a function to put the condition inside the "template" */}
      {(() => {
        if (1 > 2) {
          return <div>Condition is true</div>;
        } else {
          return <div>Condition is false</div>;
        }
      })()}
    </div>
  );
};
```


### Iterating / for-loops
First it's important to understand that react simply renders arrays as individual elements.

`<ul>{[<li>One</li>, <li>Two</li>, <li>Three</li>]}</ul>` will be rendered as:
* One
* Two
* Three

We can make use of that by using the `map` function to transform our data into markup, as `map` always returns an array.

#### Simple Iteration
```tsx
const Hello: FunctionComponent = ({}) => {
  const todos = ["Todo 1", "Todo 2", "Todo 3"];

  return (
    <ul>
      {todos.map(todo => <li>{todo}</li>)}
    </ul>
  );
};
```

#### Iteration with conditionals
```tsx
const Hello: FunctionComponent = ({}) => {
  const todos = [
    { task: "Todo 1", done: false },
    { task: "Todo 2", done: true },
  ];

  return (
    <ul>
      {todos.map(todo => {
        if (todo.done) {
          return <li className="todo done">[DONE] {todo.task}</li>;
        } else {
          return <li className="todo">{todo.task}</li>;
        }
      })}
    </ul>
  );
};
```


## State & lifecycles
We are using *react hooks* for everything that's state / lifecycle related. Because hooks only work with functional components, we are solely using `FunctionComponent` and never `React.Component` and classes.

### Simple state management example
```tsx
const Counter: FunctionComponent = ({}) => {
  const [counter, setCounter] = useState(0);
  return (
    <div>
      {counter}
      <button onClick={() => setCounter(counter => counter + 1)}>}>+</button>
    </div>
  );
};
```

### Lifecycle
Functional components and hooks don't have a concept of `lifecycles` but we can get something similar using the useEffect hook, which runs every time the component is re-rendered.

```tsx
  // Without dependency array
  useEffect(() => {
    console.log("Component will rerender")
  })
  
  // With empty dependency array
  useEffect(() => {
    console.log("Component will mount")
  }, [])
  
  // With dependency array
  useEffect(() => {
    console.log("Watching property 'counter'")
  }, [counter])
  ```

