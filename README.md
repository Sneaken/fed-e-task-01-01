# 作业

## 选择题

1. 下面关于虚拟 DOM 的说法正确的是： A.B.C.D

   A. 使用虚拟 DOM 不需要手动操作 DOM，可以极大的提高程序的性能。

   B. 使用虚拟 DOM 不需要手动操作 DOM，可以极大的提高开发效率。

   C. 虚拟 DOM 可以维护程序的状态，通过对比两次状态的差异更新真实 DOM。

   D. 虚拟 DOM 本质上是 JavaScript 对象，可以跨平台，例如服务器渲染、Weex 开发等。

2) 下面关于 Snabbdom 库的描述错误的是： C.D

   A. Snabbdom 库是一个高效的虚拟 DOM 库，Vue.js 的虚拟 DOM 借鉴了 Snabbdom 库。

   B. 使用 h() 函数创建 VNode 对象，描述真实 DOM 结构。
   C. Snabbdom 库本身可以处理 DOM 的属性、事件、样式等操作。 X => 核心库并不能，但是可以模块来处理这些操作，并且官方提供了六个模块。

   D. 使用 patch(oldVnode, null) 可以清空页面元素 X => patch(oldVnode, '!')

## 简答题

1. 请简述 patchVnode 函数的执行过程。

### 作用

对比新旧两个节点然后更新他们的差异。

### 执行过程:

首先触发 prepatch 钩子函数(如果存在的话)

然后进入对比过程

- 新旧节点引用是否相同?

  - 相同表示未发生变化,直接返回
  - 不同则继续执行后面操作

- 此时判断新节点 data 属性是否存在?

  - 存在则触发 update 钩子函数(如果存在的话)
  - 不存在则继续执行后面操作

- 新节点 text 属性 是否存在?

  - 如果存在 text 属性且和旧节点的 text 属性不一致，则需要更新 dom 元素之间的文本内容。若旧节点存在 children 属性，还则还需要删除旧节点的子节点。

  - 如果不存在 text 属性
    - 如果老节点中有 children，新节点中没有 children，则需要移除老节点中的子节点。
    - 如果新节点存在 children 属性，但老节点中不存在，同时将新节点的 children 插入到 dom 中，若老节点存在 text 属性，则清空对应 DOM 元素的 textContent。
    - 若新老节点都存在 children 属性，则调用 updateChildren() 对比子节点，并且更新子节点的差异。
    - 如果老节点中存在 text 属性，则清空 dom 元素之间的文本内容。

- 只有老节点有 text 属性
  - 清空对应 DOM 元素中的 textContent

最后触发 postpatch 钩子函数(如果存在的话)
