---
date created: 2022-05-15 00:26
date updated: 2022-05-15 00:26
---

# 什么是 JSX

React 支持 JSX 语法扩展。全称 JavaScript Syntax Extension 的缩写
JSX 可以让开发者用类似于编写 HTML 的方式去定义组件的 UI。

每个 JSX 元素只是调用 `React.createElement` 的语法糖。

```js
React.createElement(
  type,
  [props],
  [...children]
)
```

例如，下面这段 JSX 代码，就会被翻译成：

```jsx
<div className="shopping-list">
  <h1>Shopping List for {props.name}</h1>
</div>;
```

```ts
React.createElement("div", {
  className: "shopping-list"
}, React.createElement("h1", null, "Shopping List for ", props.name));
```

```ad-tip
在 React 17 版本以前，由于 JSX 会编译为 `React.createElement` 调用形式，所以 `React` 库也必须包含在 JSX 代码作用域内。
~~~tsx
import React from 'react';
~~~

In React 17, `React.createElement('div')` is deprecated, now internally using `react/jsx-runtime` to render the JSX, meaning that we will have something such as `_jsx('div', {})`. Basically, this means that you don't need to import the React object anymore in order to write JSX code.

例如，下面这段 JSX 代码在不同版本的 React 中会被编译成：
~~~tsx
function hello() {
  return <div>Hello world!</div>;
}
~~~
~~~tsx
// react 17 版本以前
function hello() {
  return React.createElement("div", null, "Hello world!");
}
~~~
~~~tsx
// react 17 版本之后
import { jsx as _jsx } from "react/jsx-runtime";

function hello() {
  return _jsx("div", {
    children: "Hello world!"
  });
}
~~~
```

# JSX 语法

```ad-info
title: 参考文档
[深入 JSX – React](https://zh-hans.reactjs.org/docs/jsx-in-depth.html)
```
