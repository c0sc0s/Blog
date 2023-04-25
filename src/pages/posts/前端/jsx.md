---
layout: "../../../layouts/MarkdownPost.astro"
title: "React(1):JSX解刨"
pubDate: 2023-4-26
description: "JSX源码级解读"
author: "cos"
cover:
  url: "https://s2.loli.net/2023/04/25/wYP4HiRk5AKT7Sa.png?resize=true&w=1920"
  square: "https://s2.loli.net/2023/04/25/wYP4HiRk5AKT7Sa.png?resize=true&w=1920"
  alt: "cover"
tags: ["前端"]
featured: true
---

# JSX

> 如果你对本段讲述的内容看的云里雾里，不要担心，读完全文回头再看一遍第一段

![|s](https://i.328888.xyz/2023/04/26/ivZcAE.jpeg)

JSX 是 React 提出的一种概念。

让开发者可以使用他们习惯的 HTML 编码方式来书写 ReactElement.

JSX 的本质是为开发者提供的一套**语法糖**。

React 借助 **Babel** 来实现 JSX 以 **简化 React 开发**。

在之前的版本中，如果要在代码中使用 JSX，必须先 import React 作为依赖

```jsx
import React from "react";

const App = () => {
  return <h1>hello world</h1>;
};
```

这是因为，

经过 Babel 的转化后，上述代码大致如下：

```jsx
import React from "react";

const App = () => {
  return React.Element("h1", null, "hello world");
};
```

可以看到，转译后的代码中使用到了 `React` 这个对象。而编译的过程并不会帮我们导入 `React` ， 因此需要我们在写代码时导入。

React17 后，对 **JSX 进行了升级**。

其中最显著的改变是，开发者在使用 JSX 时，**不需要**再导入 React 依赖。

这是因为，在更新后的版本中，上述代码经过转译后，变成了下面的格式:

```jsx
import { jsx as _jsx } from "react/jsx-runtime";

function App() {
  return _jsx("h1", { children: "Hello world" });
}
```

耶？ 他确实不再依赖 React 了，但你应该注意到了，他多了一行

```js
import { jsx as _jsx } from "react/jsx-runtime";
```

这行代码是 Babel 转译时添加进去的, 并且，改行导入 **只允许 Babel 添加，不允许开发者自己导入**

因此，新版本的 React 无需导入 React 则能使用 JSX。

## 虚拟 DOM

如果你对上面的内容不理解，不要担心，因为你缺少我接下来要说的一些知识。

虚拟 DOM 这种思想是现代前端框架中比较常见的一种技术，React 更是这个模式的先河之一，因此，我们需要了解一些虚拟 DOM 的知识。

虚拟 DOM 本质上是一个 普普通通的 JS 对象，之所以叫做 虚拟 DOM， 是因为他是用来描述 真实 DOM 的一种轻量数据。

相比于真实的 DOM, JS 对他的处理会更加高效，不同的框架，因为需求不同，虚拟 DOM 的结构不同，但他们的思想都是一样的。

同时，相比于真实 DOM 对象，虚拟 DOM 具有拓展性，可以为了实现框架内部的功能而拓展一些属性。

而且，虚拟 DOM 是最终 UI 的一层抽象，为跨平台提供了基础。

在 React 中 ，有两种数据结构，担当着虚拟 DOM 的角色, 分别是 **ReactElement** 和 **FiberNode**。

## React Element

ReactElement 是我接触 React 时了解的第一个概念，他是 React 中用于描述 DOM 的对象。

对于 React 开发者而言，使用最频繁的 JSX 本质上是声明 React Element 这种数据结构。

让我们来研究一下 React 源码中，React Element 是怎么样的，

<a href="https://github.com/facebook/react/blob/main/packages/react/src/ReactElement.js#L148" target="_blank"> 源码地址 </a>

```ts
const ReactElement = (
  type: Type,
  key: Key,
  ref: Ref,
  props: Props,
  owner: owner
): ReactElementType => ({
  $$typeof: REACT_ELEMENT_TYPE,
  type,
  key,
  ref,
  props,
  _owner: owner,
});
```

这是一段相当简单的 ts 代码，我们先忽略里面的一些类型不谈，这段代码实际上就是把我们传入的参数组装到一个对象身上，然后添加了一个 `$$typeof` 属性作为一个印记。

在 React 的源码中，REACT_ELEMENT_TYPE
比如：

```js
const App = () => {
  return <div>hello</div>;
};
```

他的本质是：

```js
const App = () => {
  return jsx("div", { children: "hello" });
};
```
