---
title: "React hook for interception observer"
timestamp: 2021-03-17T10:34:43.126Z
---

We have to build an infinite scroll component.
In order to get the job done, we have to put an element (a spinner for example) small enought to easily create a "demarcation line" to create a behavior in the easiest way.

```jsx
import { useState, useEffect } from "react";

const useIntersectionObserver = (ref, options) => {
  const [isIntersecting, setIntersecting] = useState(false);

  useEffect(() => {
    const observer = new IntersectionObserver(([entry]) => {
      return setIntersecting(entry.isIntersecting);
    }, options);

    observer.observe(ref.current);
    return () => {
      observer.disconnect();
    };
  }, [ref, options]);

  return isIntersecting;
};

export default useIntersectionObserver;
```


An example: 

```jsx
export default function App() {
  const [products, setProducts] = useState([]);
  const ref = useRef();
  const isIntersecting = useIntersectionObserver(ref, {
    root: null,
    threshold: 0
  });

  useEffect(() => {
    if (isIntersecting) {
      setProducts((old) =>
        old.concat(Array.from({ length: 10 }, () => ({ hello: "goodbye" })))
      );
    }
  }, [isIntersecting]);

  return (
    <div className="App">
      <div>
        {products.map((product, index) => (
          <Product key={index} />
        ))}
        <div ref={ref}>loading...</div>
      </div>
    </div>
  );
}
```

[Live example](https://codesandbox.io/s/competent-mayer-c9s5i?file=/src/App.js:175-774) 
[Article on medium](https://medium.com/@swatisucharita94/react-infinite-scroll-with-intersection-observer-api-db3998e52d63)
