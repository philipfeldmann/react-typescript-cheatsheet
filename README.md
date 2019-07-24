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
