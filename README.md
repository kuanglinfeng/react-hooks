# Hook简介

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

### State Hook使用

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

### State Hook原理

![avatar](/img/state-hook.png)
   
### 注意的细节

1. useState最好写到函数的起始位置，便于阅读，如果某些状态之间没什么必然的联系，应该分化为不同的状态（解耦）
2. **useState严禁出现在判断、循环的代码块中（这样会导致状态表对应不上）**
3. useState返回的函数，为节约内存空间，引用不会变化
4. **和类组件setState不同，useState中使用函数改变数据，若数据和之前的数据完全相等（使用Object.is比较），不会导致重新渲染，这样可以提升性能**
5. **和类组件setState不同，使用函数改变数据，传入的值不会和原来的数据进行合并，而是直接替换**
6. 和类组件一样，千万不要直接改变状态，而应该使用相应的api
7. **和类组件一样，函数组件中改变状态可能是异步的（在DOM事件中），多个状态变化会合并，以提高性能。此时不能信任之前的状态，而应该使用回调函数的方式改变状态。**
8. 如果要实现强制刷新组件
   1. 类组件：使用forceUpdate函数（不会运行shouldComponentUpdate）
   2. 函数组件：可以使用空对象的useState 

异步问题
```jsx
import React, { useState } from 'react'

export default function App() {
  const [count, setCount] = useState(0)

  return (
    <div>
      <span>{count}</span>
      <button
        onClick={() => {
          // 异步 不会立即改变 事件运行完成之后一起改变
          setCount(count + 1)
          // 此时 count的值仍然是0 
          setCount(count + 1)
          // 点击事件执行完成后 发现多个setCount调用 合并多个setCount为一个 
          // 也就会导致后面的状态改变覆盖前面的 最终使得点击一次 count只会加1
        }}
      >+</button>
    </div>
  )
}
```

处理异步问题
```jsx
import React, { useState } from 'react'

export default function App() {
  const [count, setCount] = useState(0)

  return (
    <div>
      <span>{count}</span>
      <button
        onClick={() => {
          // 使用回调函数 解决异步问题
          // 这里调用两次setCount 但是最终只会render一次（调用setCount一次）
          // 原因还是会合并这两次状态 只不过隐式使用中间变量存储了每次改变count的结果
          // 最后进行一次setCount 最终使得点击一次 count只会加2
          setCount(prevCount => prevCount + 1)
          setCount(prevCount => prevCount + 1)
        }}
      >+</button>
    </div>
  )
}
```

强制刷新
```jsx
import React, { useState } from 'react'

export default function App() {
  const [, forceUpdate] = useState({})
  return (
    <div>
      <button onClick={() => forceUpdate({})}>强制刷新</button>
    </div>
  )
}
```



