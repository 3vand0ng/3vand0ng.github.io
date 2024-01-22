# 关于 Taro 使用 HOCs 的相关问题

``摘写于官方 issue``

Taro 处理生命周期的逻辑，如 onShow：

小程序触发 onShow，找到页面组件实例，调用上面的 componentDidShow 方法。

react-redux connect 后能正常触发是因为它处理了 forwardRef，Taro 拿到的是用户编写的组件，所以能找到它上面的各生命周期方法进行调用。

如果 HOC 只是注入 props，也可以使用 forwardRef 处理一下，如：
```
function withShare() {
  return function demoComponent(WrappedComponent) {
    class WithShare extends Component {...}
    React.forwardRef((props, ref) => {
      return (
        <WithShare ref={ref} {...props} />
      )
    })
  }
}
```
但你的 HOC 还注入了一些生命周期方法，处理 forwardRef 后 Taro 拿到的是 WrappedComponent 的实例，这样就不会触发注入的生命周期方法。

这种情况，可以采取你上述的继承方法。建议直接不需要写 render 函数，因为原型链上的 render 会自动被执行。
```
function demoComponent(WrappedComponent) {
  return class SomeComponent extends WrappedComponent {
    componentWillMount() {}

    // ...somecodes

    // render 方法不需要编写
    // render() {}
  }
}
```

以上 issue 的回复

奈何我项目没升级 Taro 3，在 Taro 2 里有一些 React 的 API 并没有实现，就正如我项目这个版本 v2.x 版本没有 forwardRef，还有就是 Taro 2 在热更新上经常项目大一点的时候就经常爆栈，
``Maximum call stack size exceeded``
貌似 Taro 3 没这个问题。

