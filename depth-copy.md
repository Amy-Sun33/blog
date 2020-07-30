对于基本数据类型的拷贝，没有深浅拷贝的区别；深浅拷贝是对于引用数据类型而言的。

### 浅拷贝

浅拷贝的意思只复制引用，而未复制真正的值。

```markdown
  const originArr = [1,2,3,4,5];
  const originObj = {a:'a',b:[1,2,3]};

  const cloneArr = originArr;
  const cloneObj = originObj;

  console.log(cloneArr); // [ 1, 2, 3, 4, 5 ]
  console.log(cloneObj); // { a: 'a', b: [ 1, 2, 3 ] }

  cloneArr.push(6);
  cloneObj.a = {aa: 'aa'};

  console.log(cloneArr);  // [ 1, 2, 3, 4, 5, 6 ]
  console.log(originArr);  // [ 1, 2, 3, 4, 5, 6 ]

  console.log(cloneObj);  // { a: { aa: 'aa' }, b: [ 1, 2, 3 ] }
  console.log(originObj); // { a: { aa: 'aa' }, b: [ 1, 2, 3 ] }
```
利用 `=` 赋值操作符实现了一个浅拷贝。

### 深拷贝
深拷贝就是对目标的完全拷贝，不单单只是复制了一层引用，连值都复制了。

深拷贝实现方法：
1. 利用 `JSON.stringfy()` 和 `JSON.parse()` 
  1.1 存在问题
    1. 在对象中遇到 `undefined`、`function(){}`、`symbol` 会被自动忽略
     2. 在数组中遇到 `undefined`、`function(){}`、`symbol` 会返回null （以保证数组单元位置不变）
     3. 包含循环引用的对象执行`JSON.stringfy()` 会出错
    
2. 利用递归实现重新创建对象并赋值

```markdown
  JSON.parse(JSON.stringify({a:2, b:function(){}, c: undefined})) // {a:2}
  JSON.parse(JSON.stringify([1, undefined, function(){}, 4]))  // [1, null, null, 4]
```

### 拷贝的方法
数组有两个方法 `concat` 和 `slice` 实现对原数组的拷贝，并不会修改原数组，返回修改后的新数组 <br/>
ES6 中引入了 `Object.assign()` 和 `...` 展开运算符能实现对对象的拷贝

`concat`
```markdown
  const originArr = [1,2,3,4,5];
  const cloneArr = originArr.concat();
  
  console.log(originArr === cloneArr); // false
  cloneArr.push(6);   // [1,2,3,4,5,6]
  console.log(originArr);  // [1,2,3,4,5]
```
看上去是深拷贝。
如果这个对象是多层的？会怎样？
```markdown
  const originArr = [1,[1,2,3],{a:1}];
  const cloneArr = originArr.concat();
  console.log(originArr === cloneArr);  // false
  cloneArr[1].push(4);
  cloneArr[2].a = 2;
  console.log(originArr);   // [ 1, [ 1, 2, 3, 4 ], { a: 2 } ]
```
**结论**：`concat`、`slice(begin [,end])` 只能对数组进行第一层深拷贝

`Object.assign()`
```markdown
  const obj1 = {a: {b:1}, c: 2}
  const obj2 = Object.assign({}, obj1)

  obj1.a.b = 2
  obj1.c = 3

  console.log(obj2)  // { a: { b: 2 }, c: 2 }
```
**结论**：`Object.assign()` 属于浅拷贝，拷贝的是引用。

`... 展开运算符`
```markdown
  const originArray = [1,2,3,4,5,[6,7,8]];
  const originObj = {a:1,b:{bb:1}};

  const cloneArray = [...originArray];
  cloneArray[0] = 0;
  cloneArray[5].push(9);
  console.log(originArray); // [1,2,3,4,5,[6,7,8,9]]

  const cloneObj = {...originObj};
  cloneObj.a = 2;
  cloneObj.b.bb = 2;
  console.log(originObj); // {a:1,b:{bb:2}}
```
**结论**：`...` 属于浅拷贝，拷贝的是引用。

#### 总结
1. 赋值运算符 `=` 实现的是浅拷贝，只拷贝对象的引用值
2. javaScript 中数组(`concat`、`splice`) 和 对象 (`Object.assign()`、`...`) 用于拷贝基本类型
3. `JSON.stringfy()` 实现的是深拷贝，但是有局限性
4. 真正实现深拷贝，用递归
