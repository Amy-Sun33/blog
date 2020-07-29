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
  对象（包含数组）转换成基本类型值，会首先检查是否有valueOf（）方法，如果有并且返回基本类型值，就会进行强类型转换。
  如果没有使用toString()的返回值进行强类型转换。如果valueOf() 和 toString()均不返回基本类型值，会产生TypeError 错误.
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





