# 关于 Taro 2 使用和定义 ref 的问题后续
## [前期提要](https://github.com/3vand0ng/3vand0ng.github.io/blob/master/20230220-taro2-define-and-use-ref-problem.md)

## 问题分析
因为项目原因，Taro 版本停留在 v2.0.5，所以在项目开发当中肯定会遇到什么坑。

又因为 搜索过 Taro v2.x 版本的 Issues 发现官方团队在 我正在使用的版本未修复 uselayoutEffect 的执行顺序，所以在使用自定义组件和操作自定义组件暴露的方法会存在问题。

之前因为以为解决了问题是因为自定义组件暴露的方法不是在页面展示就调用，所以这次碰到的是页面加载就要调用。

## 解决问题过程
通过问题的分析，既然 uselayoutEffect 有问题，我在 useEffect 中故意 set 一个值。

```
const [init, setInit] = useState(!1)

useEffect(() => {
  setTimeout(() => {
    setInit(!0) // hack
  }, 1e3)
}, [])
```

通过上述操作，触发了视图更新，当不触发使用到自定义组件暴露的方法不会 undefined。

