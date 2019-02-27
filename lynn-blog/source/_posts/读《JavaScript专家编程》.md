---
title: 读《JavaScript专家编程》
date: 2018-08-22 20:02:50
category:
  - Notes
tags:
  - Reading
  - Software Engineering
---

# 读《JavaScript 专家编程》

[《JavaScript 专家编程》](https://www.amazon.cn/dp/B01251LYTW)这本书刚开始读的时候有种能读一个月的错觉。但是实际上不到一周时间就读完了，原因在于这本书有点“头重脚轻”，前面讲的挺透彻，越到后面越放飞自我……再加上年代久远，有些工具已经不适用了，而且其中的一些有关于测试的内容还是在软件工程类别的书里深入了解吧。

## 第 1 章 对象和原型

### 对象概述

- configurable：设置为 false 的时候，属性的描述符会被锁定，无法修改。
- enumberable：设置为 false 的时候，属性不可以被遍历（阻止遍历，不是被隐藏起来）
- writable：设置为 false 的时候，属性的值不可被改变
- 检视对象：

```javascript
// 1. 返回对象属性特性的配置
Object.getOwnPropertyDescriptor(o,'foo')

// 2. 返回对象全部属性的名字，包括那些不能枚举的
Object.getOwnPropertyNames(o)

// 3. 用来返回特定对象的原型，有时可以使用__proto__代替，但是仅可访问，设置时需要__proto__
Object.getPrototypeOf(o)

// 4. 分辨某个属性是否存在与对象实例中
Object.hasOwnProperty('foo')

// 5. 返回可枚举属性
Object.keys()

// 6. 如果对象不能扩展，属性也不能修改，返回true
Object.isFrozen()

// 7. 这个方法在对象额整个原型链中检查每一环，看传入的对象是否存在于其中
Object.isPrototypeOf()

// 8. 使用Object.preventExtensions('o')来禁止扩展
Object.isExtensible()

// 9. 对象不可扩展&&属性都不可配置
Object.isSealed()

// 10. 使用一个基本类型的值来描述对象
Object.valueOf()

// 11. 与严格相等运算符类似，不需要强制转换时是否具有相同的值
// console.log(NaN===0/0) false
// Object.is(NaN,0/0) true
Object.is(foo,bar)
```

- 修改对象

区分几个概念：

freeze：~~增/删/改~~  
seal：~~增/删~~  
preventExtensions：~~增~~

- 创建对象

```javascript
var foo = {}

/** new的四个步骤
 * 1. 创建新的对象，相当于创建字面量{}
 * 2. 构造函数指向new的函数this.constructor = foo
 * 3. 该对象的原型链接到Foo.prototype
 * 4. 传入的参数赋给新对象
 * 之前看过new的三个步骤版本，以后整理
 */

var bar = new Object()

// 可以接收参数作为新创建对象的属性
var baz = Object.create()
```

- 原型的访问方式

```javascript
Foo.prototype
Object.getPrototypeOf(foo)
Foo.__proto__
```

## 第 2 章 函数

## 第 3 章 闭包

## 第 4 章 强转

- JavaScript 尝试将对象转换成字符串时，首先调用 toString()方法。如果不反悔基础类型，会调用 valueOf()函数。否则抛出 Typeerror 异常。转为数字时顺序相反。

```javascript
console.log("1" + 1) // "11"
console.log(+"11") // 11

console.log(+new Date())
console.log(new Date() + "")
```

- 在对象的原型链上定义 toString()和 valueOf()来定义强转时使用的函数，但是会出现先调用 valueOf()的情况。原因在于内部的机制：`如果一个对象能够转成多个基础类型，那么会使用可选参数PreferredType来指定类型。`由于没有指定内部默认值（DefaultValue）函数，JS 假设你想要一个数字。

- 数组的 valueOf()方法返回一个对象。

- '||'默认值、!!获得 Boolean 类型

- 立即调用函数表达式，前面加上一元运算符会把函数声明转成函数表达式，可以在后面接上括号进行调用。在写类库框架的时候常常把分号前置，以防止解析器意想不到的错误。

```javascript
function foo(){
  console.log('这是一个函数声明')
}() //报错

!function bar(){
  console.log('OK')
}()
```

- \>>>逻辑右移运算符，正负均补 0 ，详见计算机组成原理一书。

-

```javascript
;(false + [])[0] === "f" // true
```

- 堆：堆是在内存中的顺序无关的容器，存放正在使用或未被垃圾回手清理的变量和对象的地方
- 帧：帧是时间循环周期中需要被执行的连续工作单元。帧包含把函数对象和堆中的变量链接在一起的执行上下文。
- 栈：事件循环栈包含了执行一个消息所需的所有连续的帧，事件循环自顶向下处理帧，有以来的帧把他们所依赖的帧添加到上面。
- 队列：队列是等待处理的消息的列表，每条消息都引用一个函数。当栈为空时，队列中最旧的消息作为底部帧被添加到栈中。

有关宏任务微任务可以看[这个](https://zhuanlan.zhihu.com/p/25407758)简单了解一下。

## 第 7 章 风格

- 变量名、函用 camelCase，类使用 PascalCase，匈牙利命名法不是必须的，可用于表达对象是依赖于一个库或者框架构造的。

## 多线程

[web worker](http://www.ruanyifeng.com/blog/2018/07/web-worker.html)
