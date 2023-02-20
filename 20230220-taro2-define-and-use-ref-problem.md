# 关于 Taro 2 使用和定义 ref 的问题
Taro 2 没有 `forwardRef` 方法像 React 那样包装子组件，在项目实践中父组件为类组件，子组件为函数组件。今天遇到的问题是父组件访问不了子组件的问题，举个例子：

```
// 父组件
import Taro, { Component } from '@tarojs/taro'
class MyComponent extends Component {
  handleLog () {
    this.toRefChild.log()
  }

  refChild = (node) => this.toRefChild = node // `this.toRefChild` 会变成 `ChildComponent` 组件实例的引用

  render () {
    return <ChildComponent ref={this.refChild} />
  }
}

// 子组件
import Taro, { useImperativeHandle } from '@tarojs/taro'
import { View } from "@tarojs/components"

function ChildComponent(props) {
  const { ref } = props
  
  function handleConsole() {
    console.log('child')
  }

  useImperativeHandle(ref, () => ({
    log: () => {
      handleConsole()
    }
  }))
  
  return <View>Child</View>
}

```

这样的话，会报错。

# 问题分析

1、首先子组件是函数组件 props 取值不到 ref，要自定一个 prop 去获取 ref。

2、父组件用上面例子获取子组件实例我试过在 Taro 2 不行， 我改用 `createRef` 去定义一个 ref。

代码改成如下：
```
// 父组件
import Taro, { Component, createRef } from '@tarojs/taro'
class MyComponent extends Component {
  handleLog () {
    this.refRemark.current.log() // 输出 child
  }

  refChild = createRef() // `this.refChild` 会变成 `ChildComponent` 组件实例的引用

  render () {
    return <ChildComponent childRef={this.refChild} />
  }
}

// 子组件
import Taro, { useImperativeHandle } from '@tarojs/taro'
import { View } from "@tarojs/components"

function ChildComponent(props) {
  const { childRef } = props
  
  function handleConsole() {
    console.log('输出 child')
  }

  useImperativeHandle(childRef, () => ({
    log: () => {
      handleConsole()
    }
  }))
  
  return <View>Child</View>
}

```
