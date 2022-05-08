# 为什么用 hook

# Hook 概览

## useState

在函数组件里调用它来给组件添加一些内部 state。

React 会在重复渲染时保留这个 state。

useState 会返回一对值：**当前**状态 & 一个让你更新它的函数。

``` tsx
import React, { useState } from 'react';

function Example() {
  // 声明一个叫 "count" 的 state 变量
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}

// 声明多个 state 变量
const [age, setAge] = useState(42);
const [fruit, setFruit] = useState('banana');
const [todos, setTodos] = useState([{ text: '学习 Hook' }]);
```

## useEffect
_Effect Hook_ 可以让你在函数组件中执行副作用操作。

通过使用这个 Hook，你可以告诉 React 组件需要在渲染后执行某些操作。React 会保存你传递的函数（我们将它称之为 “effect”），并且在执行 DOM 更新之后调用它。

不用再去考虑“挂载”还是“更新”。React 保证了每次运行 effect 的同时，DOM 都已经更新完毕。

每个 effect 都可以返回一个清除函数。React 会在组件卸载的时候执行清除操作。

```tsx
useEffect(() => {
  function handleStatusChange(status) {
    setIsOnline(status.isOnline);
  }
  ChatAPI.subscribeToFriendStatus(props.friend.id, handleStatusChange);
  // Specify how to clean up after this effect:
  return function cleanup() {
    ChatAPI.unsubscribeFromFriendStatus(props.friend.id, handleStatusChange);
  };
});
```

可以把 useEffect Hook 看做 componentDidMount，componentDidUpdate 和 componentWillUnmount 这三个函数的组合。

---

在某些情况下，每次渲染后都执行清理或者执行 effect 可能会导致性能问题。

如果某些特定值在两次重渲染之间没有发生变化，你可以通知 React **跳过**对 effect 的调用，只要传递数组作为 useEffect 的第二个可选参数即可。未来版本，可能会在构建时自动添加第二个参数。

```tsx
useEffect(() => {
  document.title = `You clicked ${count} times`;
}, [count]); // 仅在 count 更改时更新

useEffect(() => {
  function handleStatusChange(status) {
    setIsOnline(status.isOnline);
  }

  ChatAPI.subscribeToFriendStatus(props.friend.id, handleStatusChange);
  return () => {
    ChatAPI.unsubscribeFromFriendStatus(props.friend.id, handleStatusChange);
  };
}, [props.friend.id]); // 仅在 props.friend.id 发生变化时，重新订阅
```

如果想执行只运行一次的 effect（仅在组件挂载和卸载时执行），可以传递一个空数组（[]）作为第二个参数。这就告诉 React 你的 effect 不依赖于 props 或 state 中的任何值，所以它永远都不需要重复执行。

---

`React.useEffect` does NOT allow you to add `async` in the callback function.

```javascript

// This will cause your React application to break!
React.useEffect(async() => {
  // ... stuff
}, []);
```

React is expecting a regular function and not a promise.

But if you’d like to use `async/await` you can move your code into its own function, and invoke it inside `React.useEffect()`.

```javascript

async function fetchCats() {
  try {
    const response = await fetch('url/to/cat/api');
    const { cats } = await response.json();
    console.log(cats) // [ { name: 'Mr. Whiskers' }, ...cats ]
  } catch(e) {
    console.error(e);
  }
}

React.useEffect(() => {
  fetchCats();
}, []);
```

## useLayoutEffect
The React **useLayouEffect** hook is written the same way as **useEffect**, and almost behaves the same way.

One of the key differences is that it gets executed right ==after a React component **render** lifecycle, and before **useEffect** gets triggered==.

You only want to use this hook when you need to do any DOM changes directly.

This hook is optimized, to allow the engineer to ==make changes to a DOM node directly before the browser has a chance to paint==.

```ad-example

[useLayoutEffect Example - CodeSandbox](https://codesandbox.io/s/brave-pine-ktew0e)

~~~tsx
import { useEffect, useLayoutEffect, useRef, useState } from "react";

export default function App() {
  const [words, setWords] = useState("");
  const pRef = useRef(null);

  // 先执行
  useLayoutEffect(() => {
    pRef.current.innerText = `HAHA`;
  }, [words]);

  // 后执行
  useEffect(() => {
    pRef.current.innerText = `Input Value: ${words}`;
  }, [words]);

  return (
    <div className="App">
      <input onChange={(e) => setWords(e.target.value)} />
      <p ref={pRef}></p>
    </div>
  );
}
~~~
```

## useContext
React Context is a way to manage state globally.

It can be used together with the useState Hook to share state between deeply nested components more easily than with useState alone.

接收一个 context 对象（React.createContext 的返回值）并返回该 context 的当前值。当前的 context 值由上层组件中距离当前组件最近的 <MyContext.Provider> 的 value prop 决定。

当组件上层最近的 `<MyContext.Provider>` 更新时，该 Hook 会触发重渲染。

```ad-example
title: 示例 - useContext 使用
collapse: open

[useContext Example - CodeSandbox](https://codesandbox.io/s/hardcore-firefly-57zb48)

~~~tsx

import { createContext, useContext } from "react";

interface UserInfo {
  name: string;
  age: number;
}

const UserInfoContext = createContext<UserInfo>({
  name: "",
  age: 0
});

function Component3() {
  const { name, age } = useContext(UserInfoContext);
  return (
    <>
      User Info:
      <div>Name: {name}</div>
      <div>Age: {age}</div>
    </>
  );
}

function Component2() {
  return (
    <>
      <Component3 />
    </>
  );
}

function Component1() {
  return (
    <>
      <Component2 />
    </>
  );
}

export default function App() {
  const name = "liuxiang";
  const age = 100;

  return (
    <div className="App">
      <UserInfoContext.Provider value={{ name, age }}>
        <Component1 />
      </UserInfoContext.Provider>
    </div>
  );
}
~~~
```
## useMemo
```tsx
const memoizedValue = useMemo(() => computeExpensiveValue(a, b), [a, b]);
```

把“创建”函数和依赖项数组作为参数传入 useMemo，它仅会在某个依赖项改变时才重新计算 memoized 值。如果没有提供依赖项数组，useMemo 在每次渲染时都会计算新的值。

The useMemo and useCallback Hooks are similar. The main difference is that useMemo returns a memoized value and useCallback returns a memoized function.

useMemo runs during rendering. Therefore,we shouldn't runanything that we wouldn't do during rendering

用 useMemo 可以实现类似于 Vue 中的 computed 变量：

```tsx
function App (props) {
  const [message, setMessage] = useState('hello')
  const reversedMessage = useMemo(() => (
    message.split('').reverse().join('')
  ), [message])
  
  return (
    <div>
      {message}
      {reversedMessage}
    </div>
  )
}
```

## useCallback

把内联回调函数及依赖项数组作为参数传入 useCallback，它将返回该回调函数的 memoized 版本，该回调函数仅在某个依赖项改变时才会更新。

依赖项数组不会作为参数传给回调函数。

```tsx
const memoizedCallback = useCallback(
  () => {
    doSomething(a, b);
  },
  [a, b],
);
```

the useCallback Hook helps us to improve performance significantly.

Memorizes a function definition to avoid redefining it on each render.

Use it whenever a function is **passed as an effect argument**.

```tsx
useEffect(() => { printTodoList() }, [todoList, printTodoList])
```

Use it whenever a function is **passed by props** to a memorized component

```ad-example
title: 示例 - useCallback 使用

[[[useCallback Example - CodeSandbox](https://codesandbox.io/s/usecallback-example-i65yu3)

~~~tsx

import { useState } from "react";
import Todos from "./Todos";

export default function App() {
  const [todos, setTodos] = useState<string[]>([]);
  const [count, setCount] = useState(0);

  const increment = () => {
    setCount((c) => c + 1);
  };

  const addTodo = () => {
    setTodos((t) => [...t, "New Todo" + Math.floor(Math.random() * 100)]);
  };

  return (
    <>
      <div>
        Count: {count}
        <button onClick={increment}>+</button>
      </div>

      <Todos todos={todos} addTodo={addTodo} />
    </>
  );
}

import { memo } from "react";

const Todos = ({ todos, addTodo }) => {
  console.log("Todos Render");

  return (
    <>
      <h2>My Todos</h2>
      <button onClick={addTodo}>Add Todo</button>
      {todos.map((todo, index) => (
        <p key={index}>{todo}</p>
      ))}
    </>
  );
};

export default memo(Todos);
~~~
```
  

下面这个示例中，Todos 组件使用了 memo，然后接收 todos 和 addTodo 方法作为参数。

```ad-note
title: 什么是 memo
collapse: fold

Using `memo` will cause React to skip rendering a component if its props have not changed.

~~~tsx
import { memo } from "react";

const Todos = ({ todos }) => {
  console.log("child render");
  return (
    <>
      <h2>My Todos</h2>
      {todos.map((todo, index) => {
        return <p key={index}>{todo}</p>;
      })}
    </>
  );
};

export default memo(Todos);
~~~
```

外部的 count 变量的变化，应该与该组件应该没有任何关联。

现在让我们点击，`+`  号按钮，会发现 count 数字的变化，会导致 Todos 组件的重新渲染。

![](https://cdn.nlark.com/yuque/0/2022/png/271133/1652004788265-69e7ffa4-e6b5-4166-9dee-49431dce1480.png)

原因是：

count 变量的变化，会导致 App 组件重新渲染，

Every time a component re-renders, its functions get recreated. Because of this, the addTodo function has actually changed.

在这里我们用 useCallback 做个优化，

```tsx
const addTodo = useCallback(() => {
  console.log("AddTodo Init");
  return setTodos((t) => [
    ...t,
    "New Todo" + Math.floor(Math.random() * 100)
  ]);
}, [todos]);
```

![](https://cdn.nlark.com/yuque/0/2022/png/271133/1652004970458-962edb97-2c60-4369-b5dc-1ea6b5cab2c0.png)

## useReducer
是 [`useState`](https://zh-hans.reactjs.org/docs/hooks-reference.html#usestate) 的替代方案。它接收一个形如 `(state, action) => newState` 的 reducer，并返回当前的 state 以及与其配套的 `dispatch` 方法。

在某些场景下，`useReducer` 会比 `useState` 更适用，例如 state 逻辑较复杂且包含多个子值，或者下一个 state 依赖于之前的 state 等。

```ad-example
title: useReducer 示例

以下是用 reducer 重写 [`useState`](https://zh-hans.reactjs.org/docs/hooks-reference.html#usestate) 一节的计数器示例：

~~~tsx
const initialState = {count: 0};

// 这个就是 reducer
function reducer(state, action) {
  switch (action.type) {
    case 'increment':
      return {count: state.count + 1};
    case 'decrement':
      return {count: state.count - 1};
    default:
      throw new Error();
  }
}

function Counter() {
  const [state, dispatch] = useReducer(reducer, initialState);
  return (
    <>
      Count: {state.count}
      <button onClick={() => dispatch({type: 'decrement'})}>-</button>
      <button onClick={() => dispatch({type: 'increment'})}>+</button>
    </>
  );
}
~~~
```

**惰性初始化**：
你可以选择惰性地创建初始 state。为此，需要将 `init` 函数作为 `useReducer` 的第三个参数传入，这样初始 state 将被设置为 `init(initialArg)`。

这么做可以将用于计算 state 的逻辑提取到 reducer 外部。

```ad-example
title: useReducer 示例

[useReducer Example - CodeSandbox](https://codesandbox.io/s/usereducer-example-3hsvrd)

~~~tsx
import { useReducer } from "react";

interface CountState {
  count: number;
}

interface CountAction {
  type: string;
  payload?: number;
}

function init(initialCount: number = 0): CountState {
  return { count: initialCount };
}

function reducer(state: CountState, action: CountAction) {
  switch (action.type) {
    case "increment":
      return { count: state.count + 1 };
    case "decrement":
      return { count: state.count - 1 };
    case "reset":
      return init(action.payload);
    default:
      throw new Error();
  }
}

export default function App() {
  const [state, dispatch] = useReducer(reducer, 0, init);
  return (
    <div>
      Count: {state.count}
      <br />
      <button onClick={() => dispatch({ type: "reset", payload: 0 })}>
        Reset
      </button>
      <button onClick={() => dispatch({ type: "decrement" })}>-</button>
      <button onClick={() => dispatch({ type: "increment" })}>+</button>
    </div>
  );
}

~~~
```

## useRef
`useRef` 返回一个可变的 ref 对象，其 `.current` 属性被初始化为传入的参数（`initialValue`）。返回的 ref 对象在组件的整个生命周期内持续存在。

```ad-example
title: useRef 使用

[useRef Example - CodeSandbox](https://codesandbox.io/s/useref-example-rxr7fs)

~~~tsx
import { useRef, MutableRefObject } from "react";

export default function App() {
  const inputEl = useRef<HTMLInputElement>(null) as MutableRefObject<
    HTMLInputElement
  >;

  const onButtonClick = () => {
    // `current` 指向已挂载到 DOM 上的文本输入元素
    inputEl.current.focus();
  };

  return (
    <>
      <input ref={inputEl} type="text" />
      <button onClick={onButtonClick}>Focus the input</button>
    </>
  );
}
~~~
```

请记住，当 ref 对象内容发生变化时，`useRef`  并不会通知你。变更 `.current` 属性不会引发组件重新渲染。如果想要在 React 绑定或解绑 DOM 节点的 ref 时运行某些代码，则需要使用[回调 ref](https://zh-hans.reactjs.org/docs/hooks-faq.html#how-can-i-measure-a-dom-node) 来实现。

```ad-example
title: useRef 使用

[useRef Example - CodeSandbox](https://codesandbox.io/s/useref-example-rxr7fs)

~~~tsx
function MeasureExample() {
  const [clientRect, setClientRect] = useState({});

  const measuredRef = useCallback((node) => {
    if (node !== null) {
      setClientRect(node.getBoundingClientRect());
    }
  }, []);

  return (
    <>
      <h1 ref={measuredRef}>Hello, world</h1>
      <p>Client Top: {clientRect.top}</p>
    </>
  );
}
~~~
```

在这个案例中，我们没有选择使用 `useRef`，因为当 ref 是一个对象时它并不会把当前 ref 的值的 _变化_ 通知到我们。使用 callback ref。

注意到我们传递了 `[]` 作为 `useCallback` 的依赖列表。这确保了 ref callback 不会在再次渲染时改变，因此 React 不会在非必要的时候调用它。在此示例中，当且仅当组件挂载和卸载时，callback ref 才会被调用。

## useImperativeHandle
`useImperativeHandle` 可以让你在使用 `ref` 时自定义暴露给父组件的实例值。

`useImperativeHandle` 应当与 [`forwardRef`](https://zh-hans.reactjs.org/docs/react-api.html#reactforwardref) 一起使用。`React.forwardRef` 会创建一个React组件，这个组件能够将其接受的 [ref](https://zh-hans.reactjs.org/docs/refs-and-the-dom.html) 属性转发到其组件树下的另一个组件中。

```ad-example

[useImperativeHandle Example - CodeSandbox](https://codesandbox.io/s/charming-pare-cxtfus)

~~~tsx
import { useRef, useImperativeHandle, forwardRef, useEffect } from "react";

function FancyInput(props, ref) {
  const inputRef = useRef(null);

  useImperativeHandle(ref, () => ({
    focus: () => {
      inputRef.current.focus();
    },
    setValue: (value: string) => {
      inputRef.current.value = value;
    }
  }));

  return <input ref={inputRef} />;
}

export default function App() {
  const FancyInputWithRef = forwardRef(FancyInput);
  const fancyInputRef = useRef(null);

  useEffect(() => {
    console.log(fancyInputRef);
    fancyInputRef.current.focus();
    fancyInputRef.current.setValue("Hello World");
  }, [fancyInputRef]);

  return (
    <div>
      <FancyInputWithRef ref={fancyInputRef} />
    </div>
  );
}
~~~
```

# 自定义 Hook
通过自定义 Hook，可以将组件逻辑提取到可重用的函数中。

自定义的 Hook，命名规范以 use 开头，来表明其是一个 Hook。

```ad-example

[Custom Hook Example - CodeSandbox](https://codesandbox.io/s/custom-hook-example-uvs7w1?file=/src/App.tsx)

~~~tsx
import { useState, useEffect } from "react";

const useFetch = (url: string) => {
  const [data, setData] = useState(null);

  useEffect(() => {
    fetch(url)
      .then((res) => res.json())
      .then((data) => setData(data));
  }, [url]);

  return [data];
};

export default function App() {
  const [data] = useFetch("https://jsonplaceholder.typicode.com/todos");

  return (
    <div>
      {data?.map((item) => {
        return <p key={item.id}>{item.title}</p>;
      })}
      }
    </div>
  );
}
~~~
```

哈哈
   啊啊啊