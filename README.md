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
#### Component Code
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
