## Hook简介

Hook是React16.8.0版本推出的api，用来解决函数组件中功能不足的问题

组件：无状态组件（函数组件）、类组件

类组件中的麻烦：

1. this指向问题
2. 繁琐的生命周期
3. 其它问题

Hook专门用于增强函数组件的功能(Hook在类组件中是不能使用的)，使之理论上可以成为类组件的替代品

官方强调：没有必要更改已经完成的类组件，目前没有计划取消类组件

Hook（钩子）本质上是一个函数（命名上总是以use开头），该函数可以挂载任何功能

Hook种类：

1. useState(解决函数组件中无法使用状态的问题)
2. useEffect(解决函数组件中无法使用生命周期的问题)
3. 其它...

## State Hook

State Hook是一个在函数组件中使用的函数（useState），用于在函数组件中使用状态

useState函数有一个参数，这个参数的值表示状态的默认值

- 函数有一个参数，这个参数的值表示状态的默认值
- 函数的返回值是一个数组，该数组一定包含两项
  - 第一项: 状态的值
  - 第二项: 一个函数，用来改变状态

基本用法1

```jsx
import React, { useState } from 'react'

export default function App() {
  // 使用一个状态，该状态的默认值是0
  const [count, setCount] = useState(0)

  return (
    <div>
      <span>{count}</span>
      <button onClick={() => setCount(count + 1)}>+</button>
    </div>
  )
}
```

一个函数组件中可以有多个状态，这种做法有利于横向切分关注点

```jsx
import React, { useState } from 'react'

export default function App() {
  // 使用一个状态，该状态的默认值是0
  const [count, setCount] = useState(0)
  // 是否可见
  const [visible, setVisible] = useState(true)
  return (
    <div>
      <p style={{display: visible ? "block" : "none"}}>
        <span>{count}</span>
        <button onClick={() => setCount(count + 1)}>+</button>
      </p>
      <button onClick={() => setVisible(!visible)}>显示/隐藏</button>
    </div>
  )
}
```
   



