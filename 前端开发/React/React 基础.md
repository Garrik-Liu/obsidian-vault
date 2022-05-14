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
1. [**constructor()**](https://zh-hans.reactjs.org/docs/react-component.html#constructor)
2. [static getDerivedStateFromProps()](https://zh-hans.reactjs.org/docs/react-component.html#static-getderivedstatefromprops)
3. [**render()**](https://zh-hans.reactjs.org/docs/react-component.html#render)
4. [**componentDidMount()**](https://zh-hans.reactjs.org/docs/react-component.html#componentdidmount)

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
render()
</div>

`render()` 方法是 class 组件中唯一必须实现的方法。

当 `render` 被调用时，它会检查 `this.props` 和 `this.state` 的变化并返回以下类型之一：
-   **React 元素**。通常通过 JSX 创建。例如，`<div />` 会被 React 渲染为 DOM 节点，`<MyComponent />` 会被 React 渲染为自定义组件，无论是 `<div />` 还是 `<MyComponent />` 均为 React 元素。
-   **数组或 fragments**。 使得 render 方法可以返回多个元素。欲了解更多详细信息，请参阅 [fragments](https://zh-hans.reactjs.org/docs/fragments.html) 文档。
-   **Portals**。可以渲染子节点到不同的 DOM 子树中。欲了解更多详细信息，请参阅有关 [portals](https://zh-hans.reactjs.org/docs/portals.html) 的文档
-   **字符串或数值类型**。它们在 DOM 中会被渲染为文本节点
-   **布尔类型或 `null`**。什么都不渲染。（主要用于支持返回 `test && <Child />` 的模式，其中 test 为布尔类型。)

`render()` 函数应该为纯函数，这意味着在不修改组件 state 的情况下，每次调用时都返回相同的结果，并且它不会直接与浏览器交互。
如需与浏览器进行交互，请在 `componentDidMount()` 或其他生命周期方法中执行你的操作。保持 `render()` 为纯函数，可以使组件更容易思考。

<div style="font-size: 18px; color: #6d6d6d; line-height: 1.3; font-weight: 400;">
componentDidMount()
</div>

`componentDidMount()` 会在组件挂载后（插入 DOM 树中）立即调用。
依赖于 DOM 节点的初始化应该放在这里。如需通过网络请求获取数据，此处是实例化请求的好地方。

这个方法是比较适合添加订阅的地方。如果添加了订阅，请不要忘记在 `componentWillUnmount()` 里取消订阅。

你可以在 `componentDidMount()` 里**直接调用 `setState()`**。
它将触发额外渲染，但此渲染会发生在浏览器更新屏幕之前。如此保证了即使在 `render()` 两次调用的情况下，用户也不会看到中间状态。
请谨慎使用该模式，因为它会导致性能问题。通常，你应该在 `constructor()` 中初始化 state。

---

<div style="font-size: 20px; color: #6d6d6d; line-height: 1.3; font-weight: 400;">
更新：
</div>
当组件的 props 或 state 发生变化时会触发更新。组件更新的生命周期调用顺序如下：
1. [static getDerivedStateFromProps()](https://zh-hans.reactjs.org/docs/react-component.html#static-getderivedstatefromprops)
2. [shouldComponentUpdate()](https://zh-hans.reactjs.org/docs/react-component.html#shouldcomponentupdate)
3. [render()](https://zh-hans.reactjs.org/docs/react-component.html#render)
4. [getSnapshotBeforeUpdate()](https://zh-hans.reactjs.org/docs/react-component.html#getsnapshotbeforeupdate)
5. [**componentDidUpdate()**](https://zh-hans.reactjs.org/docs/react-component.html#componentdidupdate)

<div style="font-size: 18px; color: #6d6d6d; line-height: 1.3; font-weight: 400;">
componentDidUpdate()
</div>

```ts
componentDidUpdate(prevProps, prevState, snapshot)
```

`componentDidUpdate()` 会在更新后会被立即调用。首次渲染不会执行此方法。

当组件更新后，可以在此处**对 DOM 进行操作**。
可以**对更新前后的 props 进行比较**。
你也可以在 `componentDidUpdate()` 中**直接调用 `setState()`**，但请注意**它必须被包裹在一个条件语句里**，否则会导致死循环。

---

<div style="font-size: 20px; color: #6d6d6d; line-height: 1.3; font-weight: 400;">
卸载：
</div>
当组件从 DOM 中移除时会调用如下方法：
1. [componentWillUnmount()](https://zh-hans.reactjs.org/docs/react-component.html#componentwillunmount)

<div style="font-size: 18px; color: #6d6d6d; line-height: 1.3; font-weight: 400;">
componentWillUnmount()
</div>

`componentWillUnmount()` 会在组件卸载及销毁之前直接调用。
在此方法中执行必要的清理操作，例如，清除 timer，取消网络请求或清除在 `componentDidMount()` 中创建的订阅等。
`componentWillUnmount()` 中**不应调用 `setState()`**，因为该组件将永远不会重新渲染。

```ad-warning
title: 被遗弃的生命周期方法

#### componentWillMount
它在 `render()` 之前调用，因此在此方法中同步调用 `setState()` 不会触发额外渲染。
This function can be used to make ﬁnal changes to the component before it will be added to the DOM.
```

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