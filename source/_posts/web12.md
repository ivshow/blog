---
title: TypeScript
date: 2019-10-03 14:31:13
cover: http://f.dooomi.com/image/qj9102972768.jpg
---

### 1、数据类型

- 基本数据类型  
   变量命名格式为 xxx：类型，TypeScript 多了一个 void（空值）的概念，一般用于函数定义，表示不需要返回值的函数。

  由于 typescript 具有类型推论的特性，当变量初始化时没有进行类型定义，多次不同类型赋值会报错。

  ```
  //使用 string 定义字符串类型：
  let myName: string = '小明';    //编译通过

  myName = 007;
  //error 初始化myName时已定义为string类型，此时赋值为数值变会报错

  //使用 number 定义数值类型：
  let age: number = 23;

  //使用 boolean 定义布尔值类型：
  let isChild: boolean = true;

  //使用 void 定义空值类型：
  function consoleName(): void {
    console.log('My name is Ek');
  }

  //使用 null 和 undefined 来定义这两个原始数据类型：
  let person: undefined = undefined;
  let name: null = null;
  ```

- 联合类型  
   格式为 aa | bb。那按照上面的类型定义，是不是一个变量只能定义一个类型呢？其实不然，变量的取值也可以为多个类型的一种，这就是新的概念-联合类型。

  当然，没有进行类型定义的话是不能访问的。

  ```
  let number: string | number;
  number = '1';
  number = 1;
  ```

- 数组的类型  
  数组类型具有灵活的特点，可以多种类型的定义了，包括类型+方括号表示法、接口表示法等。

  ```
  //[类型 + 方括号]表示法 变量的赋值根据类型的定义决定
  let series: Number[] = [1, 2, 3, 4, 5];
  let series: (Number | String)[] = [1, '1', 2, 3];
  let series: any[]= [1, '1', true, 2];

  //接口表示法 接口可以用来描述数组
  interface NumberArr{
    [index: Number]: Number
  }
  let series: NumberArr = [1, 2, 2, 3];

  // 类数组表示法 常见的类数组都有自己的接口定义，
  // 如 IArguments, NodeList, HTMLCollection 等

  function sum(){
    let args: IArguments = arguments;
  }
  ```

- 函数的类型  
  在 TypeScript 中，常见的函数定义分为函数声明与函数的表达式。

  ```
  // 函数声明 对参数以及返回值进行类型约束，同时不允许多余或者缺少参数。
  function sum(x: Number, y: Number): Number {
    return x + y;
  }

  // 函数表达式 左边是输入类型，需要用括号；右边是输出类型，需要进行类型的约束。
  // => 在TypeScript里面指的是对函数的定义

  let mySum: (x: Number, y: Number) => function (x: Number, y: Number): Number {
    return x + y;
  };
  ```

### 2、面向对象

在 TypeScript 中，面向对象是其高级特性，亦即最重要的一个特性。  
TypeScript 除了包括所有 es6 中类的功能外，还添加了一些新的用法。

- 类

  private 私有修饰符 不能在外部访问

  ```
  class Animal{
    private name: string ;
    constructor(name) {
      this.name = name;
    }
  }
  let cat = new Animal('mike');
  cat.name = 'Tony'; //error
  ```

  public 公有修饰符 外部都可以访问

  ```
  class Animal{
    public name: string ;
    constructor(name) {
      this.name = name;
    }
  }
  let cat = new Animal('mike');
  cat.name = 'Tony'; //Tony
  ```

  protected 受保护修饰符 允许在子类访问

  ```
  class Animal{
    protected name: string ;
    constructor(name) {
      this.name = name;
    }
  }
  class Cat extends Animal{
    constructor(){
      super(name)
      console.log(this.name);
    }
  }
  ```

  abstract 抽象类,不允许实例化，但允许子类访问

  ```
  abstract class Animal {
    public name: string ;
    public constructor(name) {
      this.name = name;
    }
    public abstract sayHi();
  }

  let a = new Animal('Jack'); //error

  class Cat extends Animal {
    public sayHi(): string  {
      console.log(`Meow, My name is ${this.name}`);
    }
  }
  let cat = new Cat('Jack');
  Cat.sayHi();  //Meow, My name is Jack
  ```

  泛型 参数化的类型，用来限制集合的内容

  ```
  let keepAni: Array<person> = [];
  keepAni[0] = new Cat('jack');
  keepAni[1] = 2; //error
  ```

- 接口

  TypeScript 的接口指的是对类的一个抽象，常用来对对象的形状进行描述。  
  举个例子，当初始化一个类型为接口的变量时，变量的属性与值必须得跟接口的保持一致。

  ```
  //必选属性

  interfance Ek {   sex: Number;   weight: String}let Person: Ek {   sex: 1,   weight: '65kg'}

  //可选属性, 只对已定义的属性有效

  interfance Ek {   sex: Number;   weight?: String}let Person: Ek {   sex: 1 }

  //任意属性， 设置any类型。注意：如果props的值为string类型的话，Ek所有属性都为string类型
  interfances Ek {
    sex: String;
    weight: String;
    [propsName: string]: any
  }
  let Person: Ek {
    sex: 1,
    weight: '65kg',
    height: 'xxx'
  }

  // 只读属性, readonly 只在interface初始化时才能使用，同时初始化变量之后不允许修改该属性

  interface Person {
      readonly id: number;
      name: string;
      [propName: string]: any;
  }

  let Ek: Person = {
      id: 89757,
      name: 'Ek',
      sex: 1
  };

  tom.id = 3345; //error 此时id为只读属性，不允许修改值
  ```

### 3、代码检查

在决定使用 TypeScript 取决于代码检查这一特性，它主要是用来发现代码错误、统一代码风格的。  
例如是否使用两个空格进行缩进，是否需要禁用魔数，是否禁用 window 等等。接下来来看下基础配置 Tslint。

- 创建 tslint 的配置文件 tslint.json

  ```
  {
    "rules": {
      // 必须使用 === 或 !==，禁止使用 == 或 !=，与 null 比较时除外
      "triple-equals": [
        true,
        "allow-null-check"
      ]
    },
    "linterOptions": {
      "exclude": [
        "**/node_modules/**"
      ]
    }
  }
  ```

- 在 package.json 中添加 Tslint 的命令

  ```
  {
    "scripts": {
      "tslint": "tslint --project . src/**/*.ts src/**/*.tsx",
    }
  }
  ```

### 4、结语

以上便是对 TypeScript 的基础用法以及一些概念、配置的使用，掌握这些基本使用 TypeScript 就没啥问题了。

不过还是那句话，多敲，多看，真正学会 TypeScript 还是需要进行深入的了解与学习。
