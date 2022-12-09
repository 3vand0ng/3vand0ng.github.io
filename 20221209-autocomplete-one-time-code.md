# 实现提示填充验证码
原以为前几年苹果推出可以 autofill 手机验证码时是苹果提供的 API，原来HTML是可以这样实现的。
只要在 input 加入以下属性就可以。awesome!!!

``
<input autocomplete="one-time-code" type="text" />
``
