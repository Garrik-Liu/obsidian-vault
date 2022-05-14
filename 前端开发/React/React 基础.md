# 什么是 React 
```ad-info
title: 参考文档
[React 官方文档](https://zh-hans.reactjs.org/docs/getting-started.html)
```

<iframe src="https://zh-hans.reactjs.org/docs/getting-started.html" allow="fullscreen" allowfullscreen="" style="height: 100%; width: 100%; aspect-ratio: 4 / 3;"></iframe>

# Component

## 创建方式

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

## 组件渲染
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

## JSX 语法
```ad-info
title: 参考文档
[深入 JSX – React](https://zh-hans.reactjs.org/docs/jsx-in-depth.html)
```

# State

# Props

# Methods

# Component Lifecycle
每个组件都包含 “生命周期方法”，你可以重写这些方法，以便于在运行过程中特定的阶段执行这些方法。
根据创建组件的方式不同，组件的生命周期也不相同。

## Class 组件
[React.Component – React](https://zh-hans.reactjs.org/docs/react-component.html)
通过继承 `React.Component` 创建的组件。生命周期如下：

<iframe src="https://projects.wojtekmaj.pl/react-lifecycle-methods-diagram/" allow="fullscreen" allowfullscreen="" style="height:100%;width:100%; aspect-ratio: 16 / 9; "></iframe>

<div style="font-size: 20px; color: #6d6d6d; line-height: 1.3; font-weight: 400;">
    挂载：
</div>
当组件实例被创建并插入 DOM 中时，其生命周期调用顺序如下：.
1. [constructor()](https://zh-hans.reactjs.org/docs/react-component.html#constructor)
2. [static getDerivedStateFromProps()](https://zh-hans.reactjs.org/docs/react-component.html#static-getderivedstatefromprops)
3. [render()](https://zh-hans.reactjs.org/docs/react-component.html#render)
4. [componentDidMount()](https://zh-hans.reactjs.org/docs/react-component.html#componentdidmount)

<div style="font-size: 18px; color: #6d6d6d; line-height: 1.3; font-weight: 400;">
constructor()
</div>
在 React 组件挂载之前，会调用它的构造函数。
应在其他语句之前调用 `super(props)`。否则，`this.props` 在构造函数中可能会出现未定义的 bug。

通常，在 React 中，构造函数仅用于以下两种情况：
-   通过给 `this.state` 赋值对象来初始化[内部 state](https://zh-hans.reactjs.org/docs/state-and-lifecycle.html)。
-   为[事件处理函数](https://zh-hans.reactjs.org/docs/handling-events.html)绑定实例

要避免在构造函数中引入任何副作用或订阅。如遇到此场景，请将对应的操作放置在 `componentDidMount` 中

<div style="font-size: 18px; color: #6d6d6d; line-height: 1.3; font-weight: 400;">
constructor()
</div>

```ad-warning
title: 被遗弃的生命周期方法
#### `componentWillMount` 
它在 `render()` 之前调用，因此在此方法中同步调用 `setState()` 不会触发额外渲染。
This function can be used to make ﬁnal changes to the component before it will be added to the DOM.

```

<div style="font-size: 20px; color: #6d6d6d; line-height: 1.3; font-weight: 400;">
更新：
</div>

<div style="font-size: 20px; color: #6d6d6d; line-height: 1.3; font-weight: 400;">
卸载：
</div>




## 函数组件

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

# Redux & Mobx
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