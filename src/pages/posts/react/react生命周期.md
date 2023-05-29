---
layout: "../../../layouts/MarkdownPost.astro"
title: "为什么说Fiber架构带来了更好的性能"
pubDate: 2023-5-23
description: "Fiber架构真正的优势"
author: "cos"
cover:
  url: "https://s2.loli.net/2023/04/25/wYP4HiRk5AKT7Sa.png?resize=true&w=1920"
  square: "https://s2.loli.net/2023/04/25/wYP4HiRk5AKT7Sa.png?resize=true&w=1920"
  alt: "cover"
tags: ["前端"]
featured: true
---

刚开始学习 React 源码时，接触到的第一个概念是 _Fiber_ 架构

_Fiber_ 为 React 到底带来了什么？是什么原因产生了更好的性能？所谓的更好的性能到底意味着什么？

## Why

### 帧

我们使用 css 的 animation 制作一个简单的动画

```html
<style>
  .ball {
    width: 100px;
    height: 100px;
    background-color: red;
    border-radius: 50%;
    position: relative;
    animation: move 2s infinite;
  }
  @keyframes move {
    0% {
      left: 0;
      top: 0;
    }
    25% {
      left: 200px;
      top: 0;
    }
    50% {
      left: 200px;
      top: 200px;
    }
    75% {
      left: 0;
      top: 200px;
    }
    100% {
      left: 0;
      top: 0;
    }
  }
</style>
<body>
  <div class="ball"></div>
</body>
```

我们可以看到一个流畅运动的小球

这是因为浏览器会保持 60Hz 刷新显示

具体来说，就是每 1/60 s 就要重新渲染一次页面，小球的位置在重新渲染时到了新的位置，连起来便是一次动画

更加准确来说，浏览器会在每一个帧进行重新渲染

![浏览器的一帧](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/dfe9c4516b18499ea1876c21c368e061~tplv-k3u1fbpfcp-watermark.image)

这张图表示了浏览器一帧的行为，我们做一下总结

1. 用户交互事件（wheel, click)
2. JS 引擎执行
3. 后续渲染

每一帧的时间是有限的，倘若 JS 执行的时间过长，后续的渲染没有在这一帧执行，就会出现丢帧

我们在上面的代码中添加如下脚本：

```js
setInterval(() => {
  const cur = new Date();
  while (new Date() - cur < 500) {}
}, 600);
```

每隔 600 ms 就让 js 占用线程 500 ms, 你会发现小球的动画明显出现了掉帧，不再流畅

而在早先的 React 版本中，Render 阶段是一个递归的过程，如果这棵树比较庞大，递归的层次比较深入，那么会产生一次比较长的时间开销。很可能会出现这种掉帧现象。

这就是我们所说的 React 要解决的第一个问题，**防止 JS 在一帧中执行时间的过长** 导致的掉帧

- **从 数据结构 来看**

## Why ?

## How ?
