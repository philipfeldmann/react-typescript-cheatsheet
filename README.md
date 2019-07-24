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

## Templating
