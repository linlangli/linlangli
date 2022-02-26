@import "./image/JS基础TOC.jpeg"

<!-- @import "[TOC]" {cmd="toc" depthFrom=1 depthTo=6 orderedList=false} -->

<!-- code_chunk_output -->
# 目录
- [目录](#目录)
- [类型](#类型)
  - [Symbol](#symbol)
  - [String](#string)
- [变量](#变量)
- [常量](#常量)
- [对象](#对象)
  - [对象的创建](#对象的创建)
  - [对象的引用](#对象的引用)
  - [对象的属性](#对象的属性)
    - [获取属性值](#获取属性值)
    - [操作属性](#操作属性)
    - [属性的特征](#属性的特征)
  - [对象的原型](#对象的原型)
- [函数](#函数)
  - [函数的声明](#函数的声明)
  - [函数的调用](#函数的调用)
  - [函数的参数](#函数的参数)
  - [函数的方法](#函数的方法)

<!-- /code_chunk_output -->

～ 🚀  ㅤ💪 ㅤ学习笔记，第三次更新，修改内容：闭包 ㅤ🚗  ㅤ~

# 类型

> JS 共7种数据类型，包括基本数据类型[数字（整树或浮点数），字符串，布尔值，null，undefine，Symbol]和引用类型

## Symbol

* 声明

  ```js
  let s1 = Symbol()
  // Symbol(s1)
  s1
  // 为Symbol变量添加des描述
  let s2 = Symbol('des')
  // Symbol(s2)
  s2
  // 获取描述信息
  s2.description
  let s3 = Symbol();
  let s4 = Symbol('des');
  // false
  s1 === s3
  // false
  s2 === s4
  ```

  Symbol变量不是字符串也不是对象，每一个Symbol变量都是独一无二的。

* 原型

  ```js
  // Symbol的原型
  Symbol.prototype;
  ```

  > 1. *Symbol {Symbol(Symbol.toStringTag): "Symbol", constructor: \*ƒ*, toString: \*ƒ*, valueOf: \*ƒ*, …}*
  >
  > 2. 1. constructor: *ƒ Symbol()*
  >    2. **description**: [Exception: TypeError: Symbol.prototype.description requires that 'this' be a Symbol at Symbol.get description [as description] (<anonymous>) at Symbol.o (<anonymous>:1:83)]
  >    3. **toString**: *ƒ toString()*
  >    4. valueOf: *ƒ valueOf()*
  >    5. Symbol(Symbol.toPrimitive): *ƒ [Symbol.toPrimitive]()*
  >    6. Symbol(Symbol.toStringTag): "Symbol"
  >    7. get description: *ƒ description()*
  >    8. __proto__: Object
  

* 转换

  Symbol变量不能任何运算，但可以进行转换为字符串和布尔型

  ```js
  let s = Symbol('des');
  // "Symbol('des')"
  String(s)
  // "Symbol('des')"
  s.toString()
  // true
  Boolean(s);
  ```

* 作用

  1. 作为对象属性名

     ```js
     let obj = {};
     let s = Symbol('des');
     // 只能使用[]方式创建属性，若用点运算符的方式则是一个普通的字符串
     obj[s] = 1;
     // {Symbol(des):1}
     obj;
     obj.s = 2
     // {Symbol(des):1, s:2}
     obj;
     ```

  2. 作为判断条件变量

     ```js
     const s1 = Symbol('s1')
     const s2 = Symbol('s2')
     const s3 = Symbol('s3')
     switch(s) {
       case s1:
       case s2:
       case s3:
     }
     ```

* 获取Symbol类型属性名

  获取Symbol类型属性只能通过`getOwnPropertySymbols`和`eflect.ownKeys()`
  ```js
  let obj = {a: 1};
  let s1 = Symbol('s1');
  let s2 = symbol('s2');
  obj[s1] = 2;
  obj[s2] = 3;
  // [Symbol(s1),Symbol(s2)]
  Object.getOwnPropertySymbols(obj)
  // ["a", Symbol(s1),Symbol(s2)]
  Reflect.ownKeys(obj)
  ```

## String

* 字符串的转译

  对于\，"等特殊字符，需要使用反斜杠（\）转译

  ```js
  let a = "\\"
  // \
  a
  a = "\""
  // "
  a
  ```

  或使用Unicode表示法：

  ```js
  let a = "\u005c"
  // \
  a
  ```

  Unicode表示法默认只表示`\u0000 ~ \uFFFF`之间的字符，但可以使用`{}`改变这种限制：

  ```js
  // 只会将005c转译为\，1则不转译
  let a = "\u005c1"
  // \1
  a
  // 会将005c1当成整体一起转译
  a = "\u{005c1}"
  ```

* 字符串的遍历

  ```js
  // 你
  let str = String.fromCodePoint(0x2F804);
  for(let i = 0; i < str.length; i++) {
    console.log('str:', str[i]);
  }
  ```

  > str: �
  > str: �

  ```js
  for(let s of str) {
    // 你
    console.log('str:', s)
  }
  ```

* Json格式转换

  ```js
  // 将JS对象转换成JSON格式字符串
  JSON.stringify({ x: 1, y: 2 })
  // 将Json格式字符串转换成JS对象
  JSON.parse('{"x":5,"y":6}')
  ```

* 字符串模版

  使用``` ` 包裹字符串，在字符串中可通过将变量写到`${}`中来获取变量的值作为字符串的一部分，`${}`里面不仅支持任一表达式，还支持在里面调用函数

  ```js
  let a = 'a';
  function f() {
    return 'f'
  }
  let r = `我是字符串${a} ${f()}模版`;
  // 我是字符串a f模版"
  r
  ```

  以上代码输出结果字符串中不仅读到了变量a的值，也获取到了函数f返回来的值，同时也保留了空格

* 标签模版

  标签模版并不是一种模版，而是函数调用的一种方式，标签即函数后面紧跟着字符串模版，其实就是以该字符串模版为参数调用该函数

  ```js
  // error
  alert '你好吗？'
  // success
  alert `你好吗？`
  ```

  若后面的字符串模版带有变量，则会将该字符串拆分为：`function (string, ...value) `的形式

  ```js
  let a = '我很好。'
  let b = '我很不好。'
  f `你好吗？ ${a} 你呢？ ${b} `
  // 等同于
  function f('你好吗？', '你呢？', a, b);
  ```

# 变量

* 变量的作用域

  ```js
  // 除了函数内声明是局部变量外，都是全局变量
  var a;
  if(true) {
    var b = 1;
  }
  // 1
  b
  // 若在块状区域内声明a,则只能在块状区域内使用
  let a;
  if(true) {
    let b = 1;
  }
  // error
  b
  ```

  使用`let`对比`var`定义变量有以下优势：

  1. 避免内部变量覆盖外部同名变量

  2. 避免泄漏块级作用域中的循环计数器

     ```js
     for(var i = 0; i < 10; i++) {}
     // 10
     i
     for(let j = 0; j < 10; j++) {}
     // error
     j
     ```

* 变量的提升

  ```js
  // undefine
  console.log(a);
  // var定义的a变量提示到代码开头
  var a = 1;
  // error
  console.log(b);
  // let定义的变量并不存在提升特性
  let b = 2;
  ```

* 暂时性死区

  ```js
  var a = 1;
  function f() {
    // error
    console.log(a);
    let a = 2;
  }
  ```

  若在函数f中使用`let`声明a，会将a与该区域绑定，导致无法使用全局变量a，故在声明前就使用局部变量a就会报错。

* 块级作用域中声明变量

  ```js
  {
    // error，说明 a 没有作用域提升
    a;
    a = 1;
  }
  // 1
  a
  {
    // undefine
    b;
    var b = 1;
  }
  // 1
  b
  {
    // error
    c;
    let c = 1
  }
  // error
  c;
  ```

# 常量

> 使用 const 声明一个不可改变的常量。

**注意：**

1. 由于常量一旦声明就不可改变，所以若声明一个不赋任何值的常量则会报错

2. 和`let`声明的变量一样其同样不存在作用域提升且具有暂时性死区。

3. 对于一些复合类型的数据，比如具有属性的对象，却可以改变其属性值：

   ```js
   const obj = {a: 1}
   // success
   obj.a = 2
   // error
   obj = {a: 2}
   ```

   > 常量不可改变，是指指向常量内存地址的指针不可变，但改变该内存地址对象的属性并不会受影响，但当obj重新指向另一个对象，则会报错。

# 对象

> 在javascript中，一个对象可以是一个单独的拥有属性和类型的实体

## 对象的创建

* 通过对象直接量创建对象

  ``` js
    // 创建一个 name 属性为 n 的对象
    let person = {"name": "n"}
  ```

  其中name为健名，n是键值

  键名可以不加引号，若不加会自动将其转换成字符串类型，但若不符合变量的基本命名规范会导致报错：

  ```js
  	// error
    let person = {1name: "n"}
    // success
    let person = {"1name": "n"}
  ```

* 通过new创建对象

  ```js
    // 通过 new 创建一个空对象
    let person = new Object()
    let animal = new Object
    // 使用构造函数
    function bird(name) {
      this.name = name;
      this.fly = function() {
        console.log("fly");
      }
    }
    let myBird = new bird("n");
    myBird.fly();
  ```

* 通过 Object.create 创建对象

  ```js
  // error
  let bird = Object.create()
  // 创建一个普通的空对象，等同于 new Object，{}
  let bird = Object.create(Object.prototype);
  // 创建一个 name 属性为 n 的对象
  let bird = Object.create({name:"n"});
  // 创建一个不继承任何对象的对象，即该对象没有原型
  let bird = Object.create(null);
  // error
  console.log("bird:", bird.toString());  
  ```

## 对象的引用

```js
let o1 = {p: 1};
// o2指向o1的内存地址
let o2 = o1;
// 更改o2的p属性，其实是更改o1所在内存地址的p属性值，即o1.p为2
o2.p = 2;
// o1更改并不会影响o2的值，此时o2仍为：{p:1}
o1 = {p: 2};
```

`this`指的是当前区域对象的引用：

```js
let o = {p: 1}
o.f = function() {
  // 1
  this.p;
}
```



## 对象的属性

### 获取属性值

* `Object.property`

  ```js
    let persion = {
    	name: "n",
        1: "1"
    }
    persion.name
    // error，数字键名只能使用数组的形式获取
    persion.1
  ```

* `Object[property]`

  ```js
    persion["name"]
  ```

  Object[property]称为关联数组，也称为散列，映射或字典，每一个对象都是关联数组 ，其中括号内的索引必须加上引号，否则会被当作变量处理。对象做了字符串到值的映射，而数组做的是数字到值的映射

**注意：**

```js
let p = 1
let persion = {
	p: 2,
  1: “1”
}
// 2
persion["p"]
// 指向变量p，结果为1
persion[p]
// 1，可不加引号，数字会自动转换为字符串
persion[1]
```

### 操作属性

* 新增属性

  除了可在创建对象的时候定义属性，还支持动态增加属性

  ```js
  // 动态增加 name 属性
  dog.name = "dog"
  ```

* 删除属性

  ```js
    let dog = {name: "dog"};
    // 删除name属性，删除成功返回true，删除不存在的属性也返回true
    delete dog.name
  ```

    delete 无法删除属性中的属性，该特性会导致内存泄漏， 如：

    ```js
    let dog = {name:{first:"",second:""}}
    let cat.name.first = dog.name.first;
    delete dog.name;
    // fist
    console.log(cat.name.first);
    ```

* 检查属性

  1. 若对象属性不存在，则会返回 `undefine`

  2. 通过 in 判断对象属性是否存在

     ```js
     // 若 dog 存在 name 属性，则返回 true
     "name" in dog
     ```

     3. 通过 hasProperty 判断对象是否含有自有属性，即非继承属性

     ```js
     // 若 dog 对象存在 name 属性则返回true
     dog.hasProperty("name")
     // toString 非该对象的自有属性，故返回 false
     dog.hasProperty("toString")
     ```

* 枚举属性

  ```js
    // 无法判断属性是否是对象自身的
    for(p in dog) {
      console.log("p:", p);
    }
  ```

* 修改属性

  ```js
    let o1 = {p1: 1, p2: 2};
    o1.p1 = 3;
    o1.p2 = 4
  ```

    可使用以下语句代替：

    ```js
    let o1 = {p1: 1, p2: 2};
    with(o1) {
      p1 = 3;
      p2 = 4;	
    }
    ```

* getter 和 setter

  ```js
    let cat = {
      age: 0,
      get name() {
      	this.age = 10;
      	console.log("get name");
      },
      set name(value) {
      	this.age = 12;
      	console.log("set", value);
      }
    };
    // get name
    cat.name
    // 10
    cat.age
    // set name
    cat.name = "name";
    // 12
    cat.age
  ```

### 属性的特征

1. 值 [value]
2. 可写 [writable]，属性是否可修改
3. 可配置 [configurable]，特性是否可配置
4. 可枚举 [enumerable]，属性是否可枚举

```js
// {value: 1, writable: true, enumerable: true, configurable: true}
Object.getOwnPropertyDescriptor({ x: 1 }, "x");
```

**设置属性特征：**

* 不可修改

  ```js
  let o = {};
  Object.defineProperty(o, "x", {
    value: 1,
    writable: false,
    enumerable: true,
    configurable: true
  });
  o.x = 2;
  // 可写性为false，故值仍为1
  console.log(o.x);
  Object.defineProperty(o, "x", {
    value: 1,
    writable: true,
    enumerable: false,
    configurable: true
  });
  ```

* 不可枚举

  ```js
  Object.defineProperty(o, "x", {
    value: 1,
    writable: true,
    enumerable: false,
    configurable: true
  });
  o.y = 2;
  // 由于x设置为不可枚举，所以只输出y属性
  for (let p in o) {
    console.log(p);
  }
  ```

* 不可配置

  ```js
  Object.defineProperty(o, "x", {
    value: 1,
    writable: true,
    enumerable: true,
    configurable: false
  });
  // error
  Object.defineProperty(o, "x", {
    value: 1,
    writable: true,
    enumerable: false,
    configurable: false
  });
  ```

## 对象的原型

> JS是一种基于原型的编程语言，每一个对象都有一个原型对象，而原型对象又有其自身的原型对象，这种关系称为原型链。

* 继承

  ```js
  function Person() {};
  Person.p1 = 'walk';
  Person.prototype.p2 = 'walk';
  Person.prototype;
  ```

  > 1. *{p: "walk", constructor: \*ƒ*}*
  >
  > 2. 1. **p2: "walk"**
  >
  >    2. constructor: *ƒ person()*
  >
  >    3. 1. **p1: "walk"**
  >       2. arguments: null
  >       3. caller: null
  >       4. length: 0
  >       5. name: "person"
  >       6. prototype: {p: "walk", constructor: *ƒ*}
  >       7. __proto__: *ƒ ()*
  >       8. *[[FunctionLocation]]*: VM2863:1
  >       9. *[[Scopes]]*: Scopes[1]
  >
  >    4. __proto__: Object

  ```js
  let student = new Person();
  student.p = 'study';
  student;
  
  let teacher = new Person();
  teacher.p = 'teach';
  ```

  > 1. *Person {p: "study"}*
  >
  > 2. 1. **p: "study"**
  >
  >    2. __proto__:
  >
  >    3. 1. **p2: "walk"**
  >
  >       2. constructor: *ƒ Person()*
  >
  >       3. 1. **p1: "walk"**
  >          2. arguments: null
  >          3. caller: null
  >          4. length: 0
  >          5. name: "Person"
  >          6. prototype: {p2: "walk", constructor: *ƒ*}
  >          7. __proto__: *ƒ ()*
  >          8. *[[FunctionLocation]]*: VM3025:1
  >          9. *[[Scopes]]*: Scopes[2]
  >
  >       4. __proto__: Object
  >

* 扩展

  ```js
  function Person() {};
  Person.p1 = 'walk';
  Person.prototype.p2 = 'walk';
  let student = new Person();
  student.p = 'study';
  Person.prototype.cry = function(){console.log('cry')}
  // cry
  student.cry();
  student;
  ```

  > 1. *Person {p: "study"}*
  >
  > 2. 1. p: "study"
  >
  >    2. __proto__:
  >
  >    3. 1. **cry: *ƒ ()***
  >
  >       2. p2: "walk"
  >
  >       3. constructor: *ƒ Person()*
  >
  >       4. 1. p1: "walk"
  >          2. arguments: null
  >          3. caller: null
  >          4. length: 0
  >          5. name: "Person"
  >          6. prototype: {p2: "walk", cry: *ƒ*, constructor: *ƒ*}
  >          7. __proto__: *ƒ ()*
  >          8. *[[FunctionLocation]]*: VM2965:1
  >          9. *[[Scopes]]*: Scopes[2]
  >
  >       5. __proto__: Object

# 函数

> 当一个函数为一个对象的属性时，则称该函数为方法。

## 函数的声明

* 声明语句

  ```js
  function f() {}
  ```

* 函数表达式

  ```js
  let f = function(p){
    console.log(p);
  };
  // f
  f("f");
  ```

* 构造函数

  ```js
  let f = new function(r) {
    return r
  }
  // 等同于：
  function f(r) {
    return r;
  }
  ```

* 在块级作用域中声明函数

  在ES5中，允许在块级作用域中声明函数，由于函数提升，可在作用域外调用该函数：

  ```js
  function f() {
    console.log('outside')
  };
  (function() {
    if(true) {
  		function f() {
       console.log('inside');
      } 
  	}
    // inside
    f();
  })()
  ```

* 函数声明语句提升

  ```js
  f();
  // f函数声明会被提升到调用前
  function f() {};
  ```

  但函数表达式无法提升：

  ```js
  // error
  f();
  let f = function() {};
  ```

* 获取参数个数

  ```js
  function f(a, b) {}
  // 2
  f.length
  ```

* 获取函数名称

  ```js
  function f() {}
  // f
  f.name
  let f1 = function f2() {}
  // f2
  f1.name
  ```

* `eval`命令

  该命令的作用是将字符串当作语句执行，在严格模式下使用eval声明的变量不会对外部造成影响。

  ```js
  eval('let a = 1');
  // error
  a;
  ```

## 函数的调用

* 立即执行的函数表达式

  > 使用()可立即执行函数表达式，被称为立即执行的函数表达式（IIFE）

  ```js
  // 立刻调用f函数
  (function f(() {})()
  ```

  以上f函数使用（）将其包裹了起来，因为js将function开头的默认解析为语句，若不加（），则会报以下错误：

  > VM3801:1 Uncaught SyntaxError: Unexpected token ')'

  ```js
  // 语句
  function f1() {};
  // 表达式
  let f = f2() {};
  ```

  作用： 1. 避免污染全局变量； 2. 创建一些外部无法访问的私有变量

  ```js
  // 避免污染全局变量
  var a = 1;
  (function() {
    var a = 2;
    // 2
    a;
  })()
  // 1
  a;
  
  // 经过一些逻辑运算后得出结果再赋值给r
  let r = (function() {
    let a = 1;
    let b = 2;
    return a + b;
  })
  // 3
  r;
  ```

**注意：**

函数定义在条件语句中并不合法，但仍然不报错：

```js
if (true) {
  function f1(){}
}
// 由于函数上升，所以在if语句外可调用f1函数
f1()
```

## 函数的参数

* 参数省略

  函数中的参数与实际传入的参数并无关系

  ```js
  function f(a, b){}
  f(1,2,3,4);
  // 2
  f.length
  ```

  但通过arguments对象可获取传入的参数值：

  ```js
  function f(a, b){
    console.log(arguments[0]);
    console.log(arguments[1]);
    console.log(arguments[2]);
  }
  // 1 2 3
  f(1,2,3,4);
  ```

  在正常模式下，可修改参数：

  ```js
  function f(a, b){
    arguments[0] = 2;
    arguments[1] = 3;
    return a + b;
  }
  // 返回5，而不是3
  f(1,2,3,4);
  ```

  但在严格模式下，修改参数不会产生效果。

* 参数同名

  若存在同名参数，则无论最后一个参数值是什么都以取该参数值

  ```js
  function f(a, a) {
    return a;
  }
  // undefine
  f(1, undefine);
  ```

* 默认参数

  ```js
  function f(a, b = 1) { 
    return a + b
  }
  // 2
  f(1);
  ```

* 剩余参数

  ```js
  function f(a, ...b) {
    return b;
  }
  // [2, 3, 4]
  f3(1, 2, 3, 4);
  ```

  

## 函数的方法 {ignore=true}

## 闭包

### 闭包的概念

> 闭包指的是可以访问非本作用域变量的函数。

当无法访问某个函数作用域时，通常可以在要访问的函数内再创建一个函数，根据作用域链特性，该函数是可以访问到其上层变量的，此时只需要将创建的内部函数作为返回值返回，便可让外部作用域访问到函数作用域内的变量。

```js
function f1() {
    let a = 1;
    function f2() {
        console.log('a:', a);
    }
    return f2;
}
let f3 = f1();       // 1
```

而这个在函数作用域中创建的内部函数（f2）即为闭包。


### 闭包的作用 {ignore=true}

1. 获取内部函数的变量

   ```js
   function f1() {
     // 对 f2 可见
     let p1 = "p1";
     function f2() {
       // 对 f1 不可见， 对 f3 可见
       let p2 = "p2";
       console.log(p1)
       function f3() {
         // p2
         console.log(p2);
       }
     }
     return f2;
   }
   let f4 = f1();
   // p1
   f4();
   ```

   虽然p1不作用于函数f1外，但函数f4通过函数f2仍然能返回函数f1内p1的值。

2. 保存内部函数的变量

   ```js
   let add;
   function f1() {
     let n = 0
     add = function () {
       n = n + 1;
     }
     function f2() {
       console.log(n)
     }
     return f2;
   }
   let f3 = f1();
   // 0
   f3();
   add();
   // 1
   f3();
   ```

   由以上结果可知n变量值再f3函数调用结束后并未销毁回收，而是被保存了下来，这是因为f2被赋值到全局变量f3，即f3持有f2，而f2又携有f1引用，故并未被回收。
   
3.  避免被全局污染

    函数作用域中的变量无法被其它地方访问到，这样可避免变量被串改。
    
4. 模块化编程

    ```js
    const obj = (function () {
      let name = 'xiaoli';
      return {
        getName: function () {
          return name;
        },
        setName: function (value) {
          name = value;
        },
      };
    }());
    obj.name;
    obj.getName();
    obj.setName('xiaohuang');
    ```
   以上代码将获取和设置name变量的方式独立成一个模块，只需要访问obj对象的方法便可以简单清晰的操作name变量。
