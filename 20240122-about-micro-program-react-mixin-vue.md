# 关于小程序 Taro 混入 UniApp 开发
## 前言
项目里需要 Taro 混入 UniApp 做子包开发出现了一个问题，UniApp 和 Taro 框架存在差异，UniApp 跳转到 Taro 会白屏或载入页面卡顿和爆栈问题。
## 发现问题
经过发现 UniApp 如果做首次打开的页面，UniApp 会修改 wx 里的属性，导致打开 Taro 页面时报错白屏。
## 解决问题
监听 wx 对象变化，如果没有 `webpackJsonp` 这个属性就把它添加回去。
```
if (wx.webpackJsonp && !getApp()) {
  let timer
  wx = new Proxy(wx, {
    get (target, value) {
      return target[value]
    },
    set (target, key, newValue) {
      if (key === 'webpackJsonp') {
        clearTimeout(timer)
        timer = setTimeout(() => {
          getApp().webpackJsonp = newValue
        }, 1e2)
      }

      target[key] = newValue
      return !0
    }
  })
}
```
在 Taro app.js 里保存 Taro 的 getApp，如果切换回 Taro 就删掉 UniApp 的 $vm。
```
const $getApp = Taro.getApp
Taro.getApp = () => {
  let result = $getApp()
  delete result.$vm
  Reflect.deleteProperty(result, '$vm')
  return result
}
```
