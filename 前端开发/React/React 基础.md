# 什么是 React 

> [React 官方文档](https://zh-hans.reactjs.org/docs/getting-started.html)

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


render()是 React 组件唯一必需的方法。React 通过该方法的返回值来确定要渲染到页面的内容
React 组件最终渲染为浏览器中显示的 HTML。因此,组件的 render()方法需要描述视图该怎样表示为 HTML。
返回值的语法看起来和传统的 JavaScript 有些不像。该语法称为 JavaScript 扩展语法(JavaScript eXtension syntax,JSX) ,是由 Facebook 编写的 JavaScript 语法的扩展。
React 期望 render()方法返回单个子元素。 它可以是 DOM 组件的虚拟表示, 也可以返回 null 或false 这样的假值。

ReactDOM 来自 react-dom 库,我们在 index.html 中也引入了这个库。ReactDOM.render()方法需要两个参数,第一个参数是需要渲染的组件(what) ,第二个参数是渲染组件的位置(where) : ReactDOM.render([what], [where]);


---

# JSX

React 通过组件中定义的 `render()` 方法的返回值来确定要渲染的内容。

React 支持 JSX 语法扩展。
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


在 React 中，原生 HTML 元素始终以**小写字母开头**，而 React 组件名称始终以**大写字母开头**。
```tsx
const Button = ({ children }) => {
  return <button>自定义的：{children}</button>;
};

const HelloWorld = () => {
  return (
    <div>
      {/* HTML 元素 */}
      <button>点我一下</button>
      <br />
      {/* React  组件 */}
      <Button>点我一下</Button>
    </div>
  );
};

export default HelloWorld;
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