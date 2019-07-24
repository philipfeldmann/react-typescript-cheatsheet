# React + Typescript Cheatsheet
__Table of contents__

- [Components](#components)
  - [Functional Component Boilerplate](#functional-component-boilerplate)
  - [Functional Component with children](#functional-component-with-children)
    - [Component Declaration](#component-declaration)
    - [Component Usage](#component-usage)
  - [Functional Component with props](#functional-component-with-props)
  - [Functional Component with optional props and default values](#functional-component-with-optional-props-and-default-values)
- [Templating](#templating)
  - [Conditional Syntax](#conditional-syntax)
    - [Single conditional](#single-conditional)
    - [Simple conditionals with ternary syntax](#simple-conditionals-with-ternary-syntax)
    - [Conditions with if / else](#conditions-with-if--else)
    - [Conditions with if / else inside the "template"](#conditions-with-if--else-inside-the-template)
  - [Iterating / for-loops](#iterating--for-loops)
    - [Simple Iteration](#simple-iteration)
    - [Iteration with conditionals](#iteration-with-conditionals)
- [State & lifecycles](#state--lifecycles)
  - [Simple state management example](#simple-state-management-example)
  - [Lifecycle](#lifecycle)
    - [Component will rerender](#component-will-rerender)
    - [Component will mount](#component-will-mount)
    - [Watching properties](#watching-properties)
  - [Complex state logic](#complex-state-logic)
    - [useReducer and Command Pattern](#usereducer-and-command-pattern)
- [Tooling](#tooling)
  - [Vscode snippets](#vscode-snippets)
    - [Create a function component (type "rfc")](#create-a-function-component-type-rfc)
    - [Create a jest unit test (type "jut")](#create-a-jest-unit-test-type-jut)

## Components

### Functional Component Boilerplate

```tsx
import React, { FunctionComponent } from "react";

const Hello: FunctionComponent = () => {
  return <div>Hallo</div>;
};

export default Hello;
```

### Functional Component with children

#### Component Declaration

```tsx
import React, { FunctionComponent } from "react";

// Children's type is inferred by the FunctionComponent interface
const Hello: FunctionComponent = ({ children }) => {
  return <div>{children}</div>;
};

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
  name: string;
}

// Functions can infer interfaces in typescript
// We are passing props as an object with the `{}` syntax
// Inside are all properties defined by our HelloProps interface
const Hello: FunctionComponent<HelloProps> = ({ name }) => {
  return <div>Hello, {name}</div>;
};

export default Hello;
```

### Functional Component with optional props and default values

```tsx
interface ExampleProps {
  requiredProp: number;
  optionalProp?: string;
  optionalPropWithDefault?: string;
}

const Example: FunctionComponent<ExampleProps> = ({
  requiredProp,
  optionalProp,
  optionalPropWithDefault = "Default value"
}) => {
  return (
    <ul>
      <li>{requiredProp}</li>
      {optionalProp && <li>{optionalProp}</li>}
      <li>{optionalPropWithDefault}</li>
    </ul>
  );
};
```

## Templating

While react does not have a templating engine itself, common templating tasks can be easily achieved by using tsx.

### Conditional Syntax

#### Single conditional

```tsx
const Hello: FunctionComponent = () => {
  const condition = 1 > 2;
  return (
    <div>
      <div>I'm always shown</div>
      {condition && <div>Only shown if condition is true</div>}
    </div>
  );
};
```

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

- One
- Two
- Three

We can make use of that by using the `map` function to transform our data into markup, as `map` always returns an array.

#### Simple Iteration

```tsx
const Hello: FunctionComponent = ({}) => {
  const todos = ["Todo 1", "Todo 2", "Todo 3"];

  return (
    <ul>
      {todos.map(todo => (
        <li>{todo}</li>
      ))}
    </ul>
  );
};
```

#### Iteration with conditionals

```tsx
const Hello: FunctionComponent = ({}) => {
  const todos = [{ task: "Todo 1", done: false }, { task: "Todo 2", done: true }];

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

We are using _react hooks_ for everything that's state / lifecycle related. Because hooks only work with functional components, we are solely using `FunctionComponent` and never `React.Component` and classes.

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

#### Component will rerender

```tsx
// Without dependency array
useEffect(() => {
  console.log("Component will rerender");
});
```

#### Component will mount

```tsx
// With empty dependency array
useEffect(() => {
  console.log("Component will mount");
}, []);
```

#### Watching properties

```tsx
// With dependency array
useEffect(() => {
  console.log("Watching property 'counter'");
}, [counter]);
```

### Complex state logic

#### useReducer and Command Pattern

```tsx
interface Command<StateType> {
  execute(state: StateType): StateType;
}

type MyState = {
  counter: number;
  message: string;
};

class IncreaseCounterCommand implements Command<MyState> {
  execute(currentState: MyState) {
    // Always create a new object, never change existing state
    return {
      ...currentState,
      counter: currentState.counter++
    };
  }
}

class ChangeMessageCommand implements Command<MyState> {
  constructor(private msg: string) {}
  execute(currentState: MyState) {
    return {
      ...currentState,
      message: this.msg
    };
  }
}

const myStateReducer = (state: MyState, command: Command<MyState>) => command.execute(state);
const initialState: MyState = { counter: 0, message: "Hello" };

const Test: FunctionComponent = () => {
  const [state, dispatch] = useReducer(myStateReducer, initialState);

  return (
    <div>
      Counter: {state.counter}
      Message: {state.message}
      <button onClick={() => dispatch(new IncreaseCounterCommand())}>Increase Counter</button>
      <button onClick={() => dispatch(new ChangeMessageCommand("👋"))}>Change Message</button>
    </div>
  );
};
```

## Tooling

### Vscode snippets

#### Create a function component (type "rfc")

```json
{
  "Create a FunctionComponent": {
    "prefix": "rfc",
    "body": [
      "import React, { FunctionComponent } from \"react\";",
      "",
      "interface $1Props {}",
      "",
      "const $1: FunctionComponent<$1Props> = ({}) => {",
      "\treturn null;",
      "}",
      "export default $1;"
    ],
    "description": "Creates a functional React component with the corresponding interface and exports."
  }
}
```

#### Create a jest unit test (type "jut")

```json
{
  "Create a Jest Unit Test": {
    "prefix": "jut",
    "body": [
      "import React from \"react\";",
      "import { shallow } from \"enzyme\";",
      "import $1 from \"./$1\";",
      "",
      "describe(\"$1\", ()=> {",
      "  it(\"should render to snapshot\", () => {",
      "    const component = shallow(<$1 />);",
      "    expect(component).toMatchSnapshot();",
      "  });",
      "});"
    ]
  }
}
```
