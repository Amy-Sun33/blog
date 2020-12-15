### 异步控制台

在某些浏览器的 console.log(...) 并不会把传入的内容立即输出。主要原因是，在许多程序（不只是javascript）中，I/O是非常低速的阻塞部分。

下面场景不是很常见：
```javascript
  var a = { index: 1 };
  
  console.log(a);
  
  a.index++
```
我们认为在执行到 console.log(...) 语句的时候会看到 a 对象的快照，打印出来 {index: 1} ，然后在语句 a.index++ 执行的时候将其修改；

多数情况下，控制台输出的对象和期望是一致的。但是，这段代码运行的时候，浏览器可能会认为需要把控制台I/O延迟到后台，这种情况下，浏览器控制台输出对象内容时，a.index++ 可能已经执行，显示 {index : 2};
当遇到在意料之外的结果，可能就是 `I/O的异步化` 造成的。

**解决方案：**
1. 在javascript调试器中使用断点，不依赖控制台输出。
2. 把对象序列化到一个字符串中，以强制执行一次 ‘快照’，比如通过 JSON.stringfy(...).

### 事件循环 [先进先出]
javascript 一次只能处理一个事件。

伪代码：
```javascript
  var eventLoop = [];
  var event;
  
  while(true) {
    // 一次tick
    if (eventLoop.length > 0) {
      // 获取队列中的下一个事件 
      event = eventLoop.shift();
      
      // 执行
      event()
    }
  }
```

循环的每一轮称为一个tick。对于 tick 而言，如果在队列中有等待事件，那么就会从队列中拿出一个事件并执行。

**`settimeout(..)` 并没有把回调函数挂在事件循环队列中。它所作的是设置一个定时器。当定时器到时，环境会把回调函数放在事件循环中，这样在某个时刻的tick会执行这个回调函数。 `settimeout`只能确保
回调函数不会在指定的时间间隔之前执行运行，可能在那个时刻运行，也可能在那之后运行**