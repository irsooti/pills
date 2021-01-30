---
title: "Handle errors in React 17"
timestamp: 2021-01-30T16:12:27.452Z
---

There so many ways to handle errors in React 17, I prefer to not overcomplicate this operation keeping all simple and easy to debug.

In my latest project I was inclined to use an hibryd approach, atomicly controlling the data-flow in order to display the correct things in the right place (even if with some data fails) or if the api is too much unpredictable and dirty, using an error boundary component.

```jsx
import { Component, createElement } from "react";

class ErrorBoundary extends Component {
  state = { hasError: false };

  static getDerivedStateFromError(error) {
    // Update state so the next render will show the fallback UI.
    return { hasError: true, error };
  }

  componentDidCatch(error, errorInfo) {
    // Your logic Here
    // Docs advice: You can also log the error to an error reporting service https://reactjs.org/docs/error-boundaries.html#introducing-error-boundaries
    console.warn({ error, errorInfo });
  }

  render() {
    if (this.state.hasError) {
      // You can render any custom fallback UI
      return createElement(this.props.ErrorComponent, {
        error: this.state.error
      });
    }

    return this.props.children;
  }
}

export default ErrorBoundary;
```

The ol' good boundary component (exists since react 16) still does it's job and remains (at least, for me) the only worth it case to use a class component nowadays.

```jsx
import ErrorBoundary from "./ErrorBoundary";
import GenericError from "./GenericError";

const GenericError = (props) => {
  return props.error.message;
};

const TextErrorBoundary = ({ children }) => {
  return (
    <ErrorBoundary ErrorComponent={GenericError}>{children}</ErrorBoundary>
  );
};

export default TextErrorBoundary;
```

<quote>[Live Example](https://codesandbox.io/s/withered-snow-28yei?file=/src/App.js)</quote>
