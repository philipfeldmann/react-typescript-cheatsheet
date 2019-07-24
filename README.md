<!-- vscode-markdown-toc -->

- 1. [Components](#Components)
     _ 1.1. [Functional Component Boilerplate](#FunctionalComponentBoilerplate)
     _ 1.2. [Functional Component with children](#FunctionalComponentwithchildren)
     _ 1.2.1. [Component Declaration](#ComponentDeclaration)
     _ 1.2.2. [Component Usage](#ComponentUsage)
     _ 1.3. [Functional Component with props](#FunctionalComponentwithprops)
     _ 1.4. [Functional Component with optional props and default values](#FunctionalComponentwithoptionalpropsanddefaultvalues)
- 2. [Templating](#Templating)
     _ 2.1. [Conditional Syntax](#ConditionalSyntax)
     _ 2.1.1. [Single conditional](#Singleconditional)
     _ 2.1.2. [Simple conditionals with ternary syntax](#Simpleconditionalswithternarysyntax)
     _ 2.1.3. [Conditions with if / else](#Conditionswithifelse)
     _ 2.1.4. [Conditions with if / else inside the "template"](#Conditionswithifelseinsidethetemplate)
     _ 2.2. [Iterating / for-loops](#Iteratingfor-loops)
     _ 2.2.1. [Simple Iteration](#SimpleIteration)
     _ 2.2.2. [Iteration with conditionals](#Iterationwithconditionals)
- 3. [State & lifecycles](#Statelifecycles)
     _ 3.1. [Simple state management example](#Simplestatemanagementexample)
     _ 3.2. [Lifecycle](#Lifecycle)
     _ 3.2.1. [Component will rerender](#Componentwillrerender)
     _ 3.2.2. [Component will mount](#Componentwillmount)
     _ 3.2.3. [Watching properties](#Watchingproperties)
     _ 3.3. [Complex state logic](#Complexstatelogic) \* 3.3.1. [useReducer and Command Pattern](#useReducerandCommandPattern)
- 4. [Tooling](#Tooling)
     _ 4.1. [Vscode snippets](#Vscodesnippets)
     _ 4.1.1. [Create a function component (type "rfc")](#Createafunctioncomponenttyperfc) \* 4.1.2. [Create a jest unit test (type "jut")](#Createajestunittesttypejut)

## 1. <a name='Components'></a>Components

### 1.1. <a name='FunctionalComponentBoilerplate'></a>Functional Component Boilerplate

```tsx
import React, { FunctionComponent } from "react";

const Hello: FunctionComponent = () => {
  return <div>Hallo</div>;
};

export default Hello;
```

### 1.2. <a name='FunctionalComponentwithchildren'></a>Functional Component with children

#### 1.2.1. <a name='ComponentDeclaration'></a>Component Declaration

```tsx
import React, { FunctionComponent } from "react";

// Children's type is inferred by the FunctionComponent interface
const Hello: FunctionComponent = ({ children }) => {
  return <div>{children}</div>;
};

export default Hello;
```

#### 1.2.2. <a name='ComponentUsage'></a>Component Usage

```tsx
<Hello>
  <span>This span will be displayed, where children is used in the 'Hello' component</span>
<Hello>
```

### 1.3. <a name='FunctionalComponentwithprops'></a>Functional Component with props

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

### 1.4. <a name='FunctionalComponentwithoptionalpropsanddefaultvalues'></a>Functional Component with optional props and default values

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

## 2. <a name='Templating'></a>Templating

While react does not have a templating engine itself, comming templating tasks can be easily achieved by using tsx.

### 2.1. <a name='ConditionalSyntax'></a>Conditional Syntax

#### 2.1.1. <a name='Singleconditional'></a>Single conditional

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

#### 2.1.2. <a name='Simpleconditionalswithternarysyntax'></a>Simple conditionals with ternary syntax

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

#### 2.1.3. <a name='Conditionswithifelse'></a>Conditions with if / else

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

#### 2.1.4. <a name='Conditionswithifelseinsidethetemplate'></a>Conditions with if / else inside the "template"

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

### 2.2. <a name='Iteratingfor-loops'></a>Iterating / for-loops

First it's important to understand that react simply renders arrays as individual elements.

`<ul>{[<li>One</li>, <li>Two</li>, <li>Three</li>]}</ul>` will be rendered as:

- One
- Two
- Three

We can make use of that by using the `map` function to transform our data into markup, as `map` always returns an array.

#### 2.2.1. <a name='SimpleIteration'></a>Simple Iteration

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

#### 2.2.2. <a name='Iterationwithconditionals'></a>Iteration with conditionals

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

## 3. <a name='Statelifecycles'></a>State & lifecycles

We are using _react hooks_ for everything that's state / lifecycle related. Because hooks only work with functional components, we are solely using `FunctionComponent` and never `React.Component` and classes.

### 3.1. <a name='Simplestatemanagementexample'></a>Simple state management example

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

### 3.2. <a name='Lifecycle'></a>Lifecycle

Functional components and hooks don't have a concept of `lifecycles` but we can get something similar using the useEffect hook, which runs every time the component is re-rendered.

#### 3.2.1. <a name='Componentwillrerender'></a>Component will rerender

```tsx
// Without dependency array
useEffect(() => {
  console.log("Component will rerender");
});
```

#### 3.2.2. <a name='Componentwillmount'></a>Component will mount

```tsx
// With empty dependency array
useEffect(() => {
  console.log("Component will mount");
}, []);
```

#### 3.2.3. <a name='Watchingproperties'></a>Watching properties

```tsx
// With dependency array
useEffect(() => {
  console.log("Watching property 'counter'");
}, [counter]);
```

### 3.3. <a name='Complexstatelogic'></a>Complex state logic

#### 3.3.1. <a name='useReducerandCommandPattern'></a>useReducer and Command Pattern

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

## 4. <a name='Tooling'></a>Tooling

### 4.1. <a name='Vscodesnippets'></a>Vscode snippets

#### 4.1.1. <a name='Createafunctioncomponenttyperfc'></a>Create a function component (type "rfc")

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

#### 4.1.2. <a name='Createajestunittesttypejut'></a>Create a jest unit test (type "jut")

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
