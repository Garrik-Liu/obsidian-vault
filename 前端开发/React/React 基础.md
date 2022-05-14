# 什么是 React 
```ad-info
title: 参考文档
[React 官方文档](https://zh-hans.reactjs.org/docs/getting-started.html)
```

<iframe src="https://zh-hans.reactjs.org/docs/getting-started.html" allow="fullscreen" allowfullscreen="" style="height: 100%; width: 100%; aspect-ratio: 4 / 3;"></iframe>

# Component 创建

构建 React 应用程序的基础就是组件。
可以通过如下的几种方式去创建 React 组件：

继承 `React.Component` 类去创建 [React.Component – React](https://zh-hans.reactjs.org/docs/react-component.html)

```tsx
import React from "react";

export default class HelloWorld extends React.Component {
  render() {
    return <div>Hello World</div>;
  }
}

```

编写 **Functional Components 函数组件**

```tsx
import React from 'react';

const HelloWorld = () => {
  return <div>Hello World</div>;
}
```

使用 `create-react-class` 模块。[不使用 ES6 – React](https://zh-hans.reactjs.org/docs/react-without-es6.html)

```tsx
var createReactClass = require('create-react-class');

var HelloWorld = createReactClass({
  render: function() {
    return <h1>Hello, World</h1>;
  }
});
```

`render()` 方法是 class 组件中唯一必须实现的方法。[React.Component – React](https://zh-hans.reactjs.org/docs/react-component.html#render)

---

在 React 18 版本以前，通过 `ReactDOM.render`  方法来把组件渲染到页面中：
- ReactDOM 来自 react-dom 库
- 方法需要两个参数，第一个参数是需要渲染的组件(what) ，第二个参数是渲染组件的位置(where) 

```tsx
import ReactDOM from "react-dom";
import App from "./App";

const rootElement = document.getElementById("root");
ReactDOM.render(<App />, rootElement);
```

从 React 18 版本开始，`render` 数已被 `createRoot` 所取代。[ReactDOMClient – React](https://zh-hans.reactjs.org/docs/react-dom-client.html#createroot)
- `createRoot(container[, options])`
- Create a React root for the supplied `container` and return the root. 
- The root can be used to render a React element into the DOM with `render`

```tsx
import ReactDOM from "react-dom/client";
import App from "./App";

const rootElement = document.getElementById("root");
const root = ReactDOM.createRoot(rootElement);
root.render(<App />);
```

# JSX
## 什么是 JSX
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

```



## JSX 语法
```ad-info
title: 参考文档
[深入 JSX – React](https://zh-hans.reactjs.org/docs/jsx-in-depth.html)
```

# State

# Props

# Methods

# Component Lifecycle

# Component Communication

# 受控组件 & 非受控组件

# React 应用设计
```ad-info
title: 参考文档
[React 哲学 – React](https://zh-hans.reactjs.org/docs/thinking-in-react.html)
```

# React Router
```ad-info
title: 参考文档
[[React Router]]
```

# Redux
```ad-info
title: 参考文档

```

# Higher Order Components
```ad-info
title: 参考文档
[[高阶组件入门]]
```

# React Hook
```ad-info
title: 参考文档
[[React Hook 入门]]
```