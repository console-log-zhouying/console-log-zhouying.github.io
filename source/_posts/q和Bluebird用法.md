---
title: q 和 Bluebird 用法
categories: Js
tags: q、Bluebird
---

#### 一、这两个类库是什么？有什么作用？

1. 两个类库都是基于`promise`封装的库，方便我们在项目实战中能够方便采用`promise`处理异步
2. 可以提高`promise`的性能，也扩展了`promise`的一些用法


#### 二、Q

安装: `npm install q --save`

```react
let Q = require('q');
let p = new Promise(function(resolve,reject){
    resolve('hello,promise')
})
```

1. `Q`的`all`方法 和`Promise.all`方法一样，都是等待多个异步请求结果，在做出操作

```react
Q.all([p,p,p]).then(([a,b,c])=>{console.log(a,b,c)});
Promise.all([p,p,p]).then(([a,b,c])=>{console.log(a,b,c)})
```

2. `Q`的`fcall`方法，将一个同步的函数，转换为一个成功的`promise`对象，支持调用`then`方法，并且可以把同步函数的返回值，传入`then`的成功回调中作为参数。
   它和`Promise`对象的`resolve`方法，不太一样。`Promise`的`resolve`方法，也是生成一个成功的`promise`对象，但是确是将`resolve`的参数传入`promise`对象成功回调中作为成功回调参数。

```react
Q.fcall(function(){return '123'}).then(res=>{console.log(res)});
Promise.resolve(function(){return '123'}).then(res=>{console.log(res)})
```

3. `defer`对象和 `promise`的`defer`处理方式和调用方式相同。

```react
let fs = require('fs');
function readFile(path){
    let defer = Q.defer();
    fs.readFile(path,'utf8',function(err,data){
        err?defer.reject(err): defer.resolve(data);
    })
    return defer.promise;
}
readFile('./1.txt').then(res=>{console.log(res)});
```


#### 三、Bluebird

- 上面`q`库主要是对原有`promise`方法 进行了封装和改造，`bluebird`库 主要是对`promise`原有功能进行了扩展，主要是添加了 `promisify`和`promisifyAll`两个方法，可以将异步方法`promise`化。

安装 `npm intall bluebird --save`

1. `promisify` 将一个异步方法`promise`化

```react
let blueBird = require('bluebird');
let read = blueBird.promisify(fs.readFile);
read('./2.txt','utf8').then(res=>{console.log(res)});
```

`promisify` 的实现原理，就是利用高阶函数返回一个函数的特性，完成一个`promise`实例的封装

```react
function myPrimisify (fn){
    return function (...args){
       return new Promise(function(resolve,reject){
                fn(...args,function(err,data){
                    err?reject(err):resolve(data)
                })
        })
    }
}
let read2 = myPrimisify(fs.readFile);
read2('./2.txt','utf8').then(res=>{console.log(res)});
```

2. `promisifyAll` 将一个对象上面所有的方法，都`promise`化，生成一个属性名为‘原有方法’+‘`Async`’的方法，都可以支持异步方法

```react
blueBird.promisifyAll(fs);
fs.readFileAsync('./2.txt','utf8').then(res=>{console.log(res)});
```

实现原理:

```react
function myPromisifyAll(obj){
    Object.keys(obj).forEach((item,index)=>{
        if(typeof obj[item]=='function')
        obj[item+'Async']  =myPrimisify(obj[item])
    })
}
myPromisifyAll(fs);
fs.readFileAsync('./2.txt','utf8').then(res=>{console.log(res)});
```