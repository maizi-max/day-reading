# 前端状态管理之 overmind

> 前端状态管理库层出不穷，在不同的使用场景下也各有利弊，本文主要介绍 overmind这个状态管理库，所使用的前端框架是react (overmind也可搭配vue `||` angular使用), 主要介绍核心的概念和基础用法及思想，后续会对其源码进行分析。

## 核心概念

overmind的核心概念有：`state`,  `actions`, `effects`

其职责可以解释为：

- state: 应用程序内部的状态
- actions: 用户的行为，会导致state的变更
- effects: 一些副作用，比如http请求等

三种关系通过`overmind`关联,定义方式如下:

```ts
import { createOvermind } from 'overmind'
import { Provider } from 'overmind-react'
import * as React from 'react'
import { render } from 'react-dom'

const rootElement = document.getElementById('root')

export const overmind = createOvermind(
  config,
  {
    devtools: true,
  }
)
render(
  <Provider value={overmind}>
    <TodoApp />
  </Provider>,
  rootElement
)

```


以下对`state`,  `actions`, `effects`的定义及消费方式作具体介绍

### `state`

#### 定义
`state`形式是一个对象, 键为string, 值为 object | array | number| string | null | boolean | classInstance | derived
- > 支持classInstance,
- > 支持计算属性(派生属性)，使用 `derived` 关键字。derived((state) => somethind)
#### Demo
```ts
import { derived } from 'overmind'

interface Company {
  size: 'sm' | 'md' | 'lg'
}
class Company {
  size = 'large'
  getSize () {
    return this.size
  }
}
interface State {
  name: string
  age: number
  addredd: string[]
}
const state: State {
  name: 'lei',
  age: 999,
  address: ['中国', '上海', 'somewhere'],
  company: new Company(),
  indetity: derived({ state } => `${state.name}-${state.age}-${state.address}`)
}
```
#### 消费 `state`
```tsx
import { createHook } form 'overmind'
const useApp = createHook<typeof config>()
const Demo = () => {
  const { state } = useApp()
  return <div>{state?.name}</div>
}

```
