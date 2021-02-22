---
title: Flow静态类型检查方案
categories: TypeScript
tags: Flow
---

#### 一、工作原理

```js
// 在代码中添加类型注解的方式去标记变量类型，并检查

// 包： flow-bin

1. // @flow   //必须要写
function sum (a:number, b:number){ 2.
  return a + b;
}

3. yarn flow init
4. yarn flow 
```

<!-- more -->

#### 二、移除flow的2种方式

1. flow-remove-types

   ```js
   1. 安装 flow-remove-types  // 编译时移除，既用不上js语法又报错
   2. yarn flow-remove-types . -d dist (当前目录，输出目录)
   ```

2. 结合`babel`

   ```js
   1. yarn add @babel/core @babel/cli @babel/preset-flow --dev
   2. .babelrc
   	{
       "presets": ["@babel/preset-flow"]
     }
   ```


#### 三、作用

- 弥补`JavaScript`弱类型带来的弊端


#### 四、特点

- 生产环境这些类型注解可以自动去除
- 不需要给每一个变量添加类型注解，可根据需求灵活添加


#### 五、插件

- Flow Language Support
  - 更直观的体现问题


#### 六、类型推断

```js
// 此时未进行类型注解，但是flow可以根据乘法推断n必须是number类型
function square(n) { 
  return n * n;
}

square('100'); // 报错
```


#### 七、类型注解

```js
function square(n: number) {
  
}

let num: number = 100;

function foo(): number {
  return 100;
}

function foo(): void { // 没有返 回值的函数标记为void
 
}
```


#### 八、原始类型

```js
const a: string = 'a'

const b: number = Infinity //NaN //100

const c: boolean = false //true

const d: null = null

const e: void = undefined

const f: symbol = Symbol()
```


#### 九、数组类型

```js
const arr1: Array<number> = [1,2,3]

const arr2: number[] = [1,2,3]

// 元组
const foo: [string, number] = ['foo', 123]

```


#### 十、对象类型

```js
const obj1: { foo: string, bar: number } = { foo: 'string', bar: 100 }

const obj2: { foo?: string, bar: number } = { bar: 100 } // ？代表可有可无

//代表这个对象可以添加任意多个键值对，但是值和类型都必须是string
const obj3: { [string]: string } = {} 
obj3.key1 = 'value1'
obj3.key2 = 'value2'
```


#### 十一、函数类型

```js
function foo (callback: (string, number) => void) {
  callback('string',100)
}

foo(function (str, n) {
  //不可以有返回值
})
```


#### 十二、特殊类型

```js
const a: 'foo' = 'foo' //只能是foo

const type: 'success' | 'warning' | 'danger' = 'success' // 只能是这三种类型中的一种

type StringOrNumber = string | number
const b: StringOrNumber = 'string' // 可以传字符串或者数字

const gender: ?number = undefined //在类型前加问号代表，除了可以是number类型，还多了null和undefined类型
```


#### 十三、Mixed & Any

```js
function passMixed (value: mixed) { //任意类型
  // 没有明确mixed类型是不可以当成任意类型使用的
  // 正常判断类型再使用
  if(typeof value === 'string') {
    value.substr(1)
  }
}

------

function passAny (value: any) { //任意类型
  // 可以把value当作任意类型使用
}

------
区别：
	mixed是强类型，any是弱类型。
```


#### 十四、类型手册

https://www.saltycrane.com/cheat-sheets/flow-type/latest/