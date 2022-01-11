---
title: "How to create a multibrand / multitenant component"
timestamp: 2022-01-11T11:08:05.295Z
---

In this way we could render a specific component for each brand based on environment.

```tsx
import { ComponentElement, FC, useMemo } from 'react';
import App from './App';
// process.env.REACT_APP_BRAND example: gucci | armani | ck

type SpecializeProps = Partial<{
  [key in typeof process.env.REACT_APP_BRAND]: JSX.Element;
}> & {
  fallback: JSX.Element;
};

const Specialize: FC<SpecializeProps> = (props) => {
  const component = props[process.env.REACT_APP_BRAND];

  if (component) {
    return component;
  }
  return props.fallback;
};

export default Specialize;

// How to use

<Specialize gucci={<div>Hello Gucci</div>} fallback={<div>Hello generic component</div>} />;
```
