---
title: 为什么要用 setTimeout 模拟 setInterval ？
cover: "/img/science-g49fdd3b92_640.jpg"
date: 2020-11-20 18:20:01
---

在 JS 事件循环之宏任务和微任务中讲到过，setInterval 是一个宏任务。
用多了你就会发现它并不是准确无误，极端情况下还会出现一些令人费解的问题。

下面我们一一罗列..

### 推入任务队列后的时间不准确

定时器代码：

```
setInterval(fn(), N);
```

上面这句代码的意思其实是 fn()将会在 N 秒之后被推入任务队列。

所以，在 setInterval 被推入任务队列时，如果在它前面有很多任务或者某个任务等待时间较长比如网络请求等，那么这个定时器的执行时间和我们预定它执行的时间可能并不一致。

比如：

```
let startTime = new Date().getTime();
let count = 0;
//耗时任务
setInterval(function() {
  let i = 0;
  while (i++ < 1000000000);
}, 0);
setInterval(function() {
  count++;
  console.log(
    "与原设定的间隔时差了：",
    new Date().getTime() - (startTime + count * 1000),
    "毫秒"
  );
}, 1000);
// 输出：
// 与原设定的间隔时差了：699 毫秒
// 与原设定的间隔时差了：771 毫秒
// 与原设定的间隔时差了：887 毫秒
// 与原设定的间隔时差了：981 毫秒
// 与原设定的间隔时差了：1142 毫秒
// 与原设定的间隔时差了：1822 毫秒
// 与原设定的间隔时差了：1891 毫秒
// 与原设定的间隔时差了：2001 毫秒
// 与原设定的间隔时差了：2748 毫秒
// ...
```

### setInterval 缺点 与 setTimeout 的不同

再次强调，定时器指定的时间间隔，表示的是何时将定时器的代码添加到消息队列，而不是何时执行代码。所以真正何时执行代码的时间是不能保证的，取决于何时被主线程的事件循环取到，并执行。

```
setInterval(function, N)
//即：每隔N秒把function事件推到消息队列中
```

![编码](/img/20220324233618.png)

上图可见，setInterval 每隔 100ms 往队列中添加一个事件；100ms 后，添加 T1 定时器代码至队列中，主线程中还有任务在执行，所以等待，some event 执行结束后执行 T1 定时器代码；又过了 100ms ， T2 定时器被添加到队列中，主线程还在执行 T1 代码，所以等待；又过了 100ms ，理论上又要往队列里推一个定时器代码，但由于此时 T2 还在队列中，所以 T3 不会被添加（T3 被跳过），结果就是此时被跳过；这里我们可以看到，T1 定时器执行结束后马上执行了 T2 代码，所以并没有达到定时器的效果。

综上所述，setInterval 有两个缺点：

- 使用 setInterval 时，某些间隔会被跳过；
- 可能多个定时器会连续执行；

可以这么理解：每个 setTimeout 产生的任务会直接 push 到任务队列中；而 setInterval 在每次把任务 push 到任务队列前，都要进行一下判断(看上次的任务是否仍在队列中，如果有则不添加，没有则添加)。

因而我们一般用 setTimeout 模拟 setInterval ，来规避掉上面的缺点。

来看一个经典的例子来说明他们的不同：

```
for (var i = 0; i < 5; i++) {
  setTimeout(function() {
    console.log(i);
  }, 1000);
}
```

做过的朋友都知道：是一次输出了 5 个 5 ; 那么问题来了：是每隔 1 秒输出一个 5 ？还是一秒后立即输出 5 个 5 ？答案是：一秒后立即输出 5 个 5 因为 for 循环了五次，所以 setTimeout 被 5 次添加到时间循环中，等待一秒后全部执行。

为什么是一秒后输出了 5 个 5 呢？简单来说，因为 for 是主线程代码，先执行完了，才轮到执行 setTimeout 。

当然为什么输出不是 1 到 5 ，这个涉及到作用域的问题了，这里就不解释了。

### setTimeout 模拟 setInterval

综上所述，在某些情况下，setInterval 缺点是很明显的，为了解决这些弊端，可以使用 setTimeout() 代替。

- 在前一个定时器执行完前，不会向队列插入新的定时器（解决缺点一）
- 保证定时器间隔（解决缺点二）

具体实现如下：

1.写一个 interval 方法

```
let timer = null
interval(func, wait){
    let interv = function(){
        func.call(null);
        timer=setTimeout(interv, wait);
    };
    timer= setTimeout(interv, wait);
 },
```

2.和 setInterval() 一样使用它

```
interval(function() {}, 20);
```

3.终止定时器

```
if (timer) {
  window.clearSetTimeout(timer);
  timer = null;
}
```

### 参考

- 为什么要用 setTimeout 模拟 setInterval ？
- 用 settTimeout()代替 setInterval()
