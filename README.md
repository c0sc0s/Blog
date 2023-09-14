## COSINE BLOG

过程驱动 --> 状态驱动

## 过程驱动

开发者 --> 直接调用宿主环境的 API --> 更新 UI

## 状态驱动

开发者 --> 描述 UI(jsx,模板语法) --> 框架的编译优化（可能） -->
框架运行时核心库 --> 调用宿主环境 API --> 更新 UI

react 是没有编译优化的，react 是一个纯运行时的前端框架

## Reconciler

Reconciler 能直接操作 JSX 吗？

不行，因为 JSX 存在下面的问题：

- 不能表达节点之间的关系
- 字段有限，不能表达一些其他数据，比如 status

JSX -> DOM ❌
JSX -> FiberNode -> DOM ✅

整理目前的数据结构：be

- JSX
- React Element
- FiberNode
- DOM Element
# Blog
