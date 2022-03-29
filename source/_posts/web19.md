---
title: 7 个关于 "this" 面试题，你能回答上来吗？
cover: "/img/desktop-g3f90a23ef_640.jpg"
date: 2021-1-10 19:10:50
---

在JavaScript中，this 表示函数调用上下文。this难点在于它有一个复杂的行为，这也是面试中经常被考的点。

本文列举7个关于this有趣的面试问题:

- 问题1：变量 vs 属性
- 问题2：Cat 的名字
- 问题3：延迟打招呼
- 问题4：人工方法
- 问题5：问候和告别
- 问题6：棘手的 length
- 问题7：调用参数

### 问题1：变量 vs 属性

下面的打印结果是啥：

```
const object = {
  message: 'Hello, World!',

  getMessage() {
    const message = 'Hello, Earth!';
    return this.message;
  }
};

console.log(object.getMessage()); // ??
```
答案：'Hello, World!'

object.getmessage()是一个方法调用，此时的 this 表示 object。

方法还有一个变量声明const message = 'Hello, Earth!'。这个变量都不会影响this.message的值。

### 问题2：Cat 的名字

下面代码打印什么：

```
function Pet(name) {
  this.name = name;

  this.getName = () => this.name;
}

const cat = new Pet('Fluffy');

console.log(cat.getName()); // What is logged?

const { getName } = cat;
console.log(getName());     // What is logged?

console.log(object.getMessage()); // ??
```

答案：'Fluffy' 和 'Fluffy'

当函数作为构造函数new Pet('Fluffy')调用时，构造函数内部的this等于构造的对象

Pet构造函数中的this.name = name表达式在构造的对象上创建name属性。

this.getName = () => this.name在构造的对象上创建方法getName。而且由于使用了箭头函数，箭头函数内部的this值等于外部作用域的this值, 即 Pet。

调用cat.getName()以及getName()会返回表达式this.name，其计算结果为'Fluffy'。

### 问题3：延迟打招呼

下面代码打印什么：

```
const object = {
  message: 'Hello, World!',

  logMessage() {
    console.log(this.message); // What is logged?
  }
};

setTimeout(object.logMessage, 1000);
```

答案：1秒后，打印 undefined。

尽管setTimeout()函数使用object.logMessage作为回调，但仍将object.logMessage用作常规函数，而不是方法。

在常规函数调用期间，this等于全局对象，即浏览器环境中的 window。

这就是为什么logMessage方法中的 this.message等于 window.message,即undefined。

### 问题4：人工方法

如何调用logMessage函数，让它打印 "Hello, World!" ?

```
 message: 'Hello, World!'
};

function logMessage() {
  console.log(this.message); // "Hello, World!"
}
```

答案:

至少有 3 种方式，可以做到：

```
message: 'Hello, World!'
};

function logMessage() {
  console.log(this.message); // logs 'Hello, World!'
}

// Using func.call() method
logMessage.call(object);

// Using func.apply() method
logMessage.apply(object);

// Creating a bound function
const boundLogMessage = logMessage.bind(object);
boundLogMessage();
```

### 问题5：问候和告别

下面代码打印什么：

```
const object = {
  who: 'World',

  greet() {
    return `Hello, ${this.who}!`;
  },

  farewell: () => {
    return `Goodbye, ${this.who}!`;
  }
};

console.log(object.greet());    // What is logged?
console.log(object.farewell()); // What is logged?
```
答案: 'Hello, World!' 和 'Goodbye, undefined!'。

当调用object.greet()时，在greet()方法内部，this值等于 object，因为greet是一个常规函数。因此object.greet()返回'Hello, World!'。

但是farewell()是一个箭头函数，箭头函数中的this值总是等于外部作用域中的this值。

farewell()的外部作用域是全局作用域，它是全局对象。因此object.farewell()实际上返回'Goodbye， ${window.who}!'，它的结果为'Goodbye, undefined!'。

### 问题6：棘手的 length

下面代码打印什么：

```
var length = 4;
function callback() {
  console.log(this.length); // What is logged?
}

const object = {
  length: 5,
  method(callback) {
    callback();
  }
};

object.method(callback, 1, 2);
```
答案: 4

callback()是在method()内部使用常规函数调用来调用的。由于在常规函数调用期间的this值等于全局对象，所以this.length结果为window.length。。

第一个语句var length = 4，处于最外层的作用域，在全局对象 window 上创建一个属性length。

### 问题7：调用参数

下面代码打印什么：

```
var length = 4;
function callback() {
  console.log(this.length); // What is logged?
}

const object = {
  length: 5,
  method() {
    arguments[0]();
  }
};

object.method(callback, 1, 2);
```
答案: 3

obj.method(callback, 1, 2)被调用时有3个参数:callback, 1和2。结果，method()内部的参数特殊变量是如下结构的数组类对象:

```
{
  0: callback,
  1: 1, 
  2: 2, 
  length: 3 
}
```
因为arguments[0]()是arguments对象上的回调的方法调用，所以回调内部的参数等于arguments。所以 callback()中的this.length与arguments.length相同，即3。