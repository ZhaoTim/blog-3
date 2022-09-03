---
title: 笔记
description: Learn how to rename fields of a GraphQL object by using aliases.
date: 2020-07-05
tags: [GraphQL, Today I Learned]
---


> **面完作业帮以后，沉默了，脑袋里始终都出现初中数学老师经常说的一句话：读懂题就做对了一半了。**

```js
/* 不使用async await 实现一个函数createFlow,使得以下代码输出方式如下：
// 延迟1s
1
2
// 延迟3s
3
4
*/
const delay = ms => new Promise(resolve => setTimeout(resolve, ms));
const log = console.log;
createFlow([
  () => delay(1000).then(() => log(1)),
  () => log(2),
  () => delay(3000).then(() => log(3)),
  () => log(4),
]);
```

正常人看到这道题都知道题里的**_延迟 1s 1 2 延迟 3s 3 4_**，是在描述一条时间线，我却理解成了：**_代码开始执行了，一秒后输出 1，2、三秒后输出 3,4。_**

彻底偏了，出题人想让候选人实现的是`createFlow`函数接收的数组，能够**按照数组顺序执行**，且如果函数执行返回的是 promise，那么则等到 promise 变为 fulfilled（成功态），再执行数组的下一项函数。

最可笑的是，面完试没过 3 分钟，我就写出了正确答案：

```js
const createFlow = promises => {
  promises.reduce((result, fn) => {
    return result.then(() => {
      return fn();
    });
  }, Promise.resolve());
};
```

### 实现 strBuilder

```js
function strBuilder(str) {}

var hello = strBuilder("Hello, ");
var kyle = hello("Kyle");
var susan = hello("Susan");
var question = kyle("?")();
var greeting = susan("!")();

// todo 实现 strBuilder, 使以下输出都为 true
console.log(strBuilder("Hello, ")("")("Kyle")(".")("")() === "Hello, Kyle.");
console.log(hello() === "Hello, ");
console.log(kyle() === "Hello, Kyle");
console.log(susan() === "Hello, Susan");
console.log(question === "Hello, Kyle?");
console.log(greeting === "Hello, Susan!");
```

1. strBuilder 生成的函数支持无限次调用，直到参数为空（undefined）
2. 调用了两次`hello`方法，是相互**独立的**

```js
function strBuilder(str1) {
  return str2 => typeof str2 === 'string' ?  strBuilder(`${str1}${str2}` : str1
}
```

