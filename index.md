## 隐式类型转换

#### ToNumber

```markdown
  1. true 转换为1
  2. false 转换为0
  3. undefined 转换为NaN
  4. null 转换为0
```
 **解析(parseInt/parseFloat) & 转换(Number)数字字符串**
```markdown
  var a = '42'
  var b = '42px'
  Number(a) // 42
  parseInt(a) // 42
  
  Number(b) // NaN
  parseInt(b) // 42
  
 `1. 解析允许字符串中含有非数字字符，解析按照从左到右的顺序，如果遇到非数字就停止；转换不允许出现非数字字符，失败会返回NAN。
  2. parseInt(...)针对的是字符串值。向 parseInt() 传递数字和其他的参数没有用，如：true、function(){...}和[1,2]返回的结果为NaN`
```
#### ToBoolean

```markdown
  以下这些是转换结果为false
  - undefined
  - null
  - false
  - +0、-0 和 NaN
  - ""
```
#### ToPrimitive

```markdown
  1. 如果输入的是基本类型值，则直接返回它
  2. 否则，调用这个对象（数组）的 valueOf() 方法，如果 valueOf() 方法返回值是一个原始值，则返回这个原始值
  3. 否则，调用这个对象（数组）的 toString() 方法，如果 toString() 方法返回的是一个原始值，则返回这个原始值。
  4. 否则，抛出 TypeError 异常
```

#### 抽象相等
```markdown
1. 字符串和数字之间的相等比较
  (1) 如果Type(x)是数字，Type(y)是字符串，则返回 x == ToNumber(y) 的结果
  (2) 如果Type(x)是字符串，Type(y)是数字，则返回 ToNumber(x) == y 的结果
  
2. 其他类型和布尔类型之间的相等关系
  (1) 如果 Type(x) 是布尔类型，则返回 ToNumber(x) == y 的结果
  (2) 如果 Type(y) 是布尔类型，则返回 x == ToNumber(y) 的结果

3. null 和 undefined 之间的相等关系
 null == undefined

4. 对象和非对象之间的相等比较
  (1) 如果 Type(x) 是字符串或者数字，Type(y) 是对象，则返回 x == ToPrimitive(y) 的结果
  (2) 如果 Type(x) 是对象，Type(y)是字符串或数字，则返回 ToPtimitive(x) == y 的结果
```





