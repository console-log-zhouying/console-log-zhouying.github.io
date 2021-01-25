---
title: node的错误处理方式
categories: node
tags: node 错误处理
---

#### 一、为什么要处理异常

1. 不处理直接导致程序奔溃，这显然不是我们想要的

2. 导致请求无法被释放，直至连接超时。用户体验体验非常差，我们要做的应该是在出错时，给用户一个友好的提示，并记录下此次异常，以便排查。

<!-- more -->

#### 二、处理方式

1. ##### try catch(只可以处理同步代码)

   ```js
   try {
       
   } catch (e) {
       /*处理异常*/
       console.log(e.message)
   }
   console.log('异常被捕获了，我可以继续执行')
   ```

2. ##### callback方式

   ```js
   fs.mkdir('/dir', function (e) {
       if (e) {
           /*处理异常*/
           console.log(e.message)
       } else {
           console.log('创建目录成功')
       }
   })
   ```

3. #### event方式

   ```js
   let events = require("events");
   //创建一个事件监听对象
   let emitter = new events.EventEmitter();
   //监听error事件
   emitter.addListener("error", function (e) {
       /*处理异常*/
       console.log(e.message)
   });
   //触发error事件
   emitter.emit("error", new Error('出错啦'));
   ```

4. #### Promise方式

   ```js
   new Promise((resolve, reject) => {
       syncError()
       /* or
       try{
           syncError()
       }catch(e){
           reject(e)
       }
       */
   })
   .then(() => {
       //...
   })
   .catch((e) => {
       /*处理异常*/
       console.log(e.message)
   })
   ```

   - `promise`同样无法处理异步代码块中抛出的异常

5. #### process方式

   `process`方式可以捕获任何异常(不管是同步代码块中的异常还是异步代码块中的异常)

   ```js
   process.on('uncaughtException', function (e) {
       /*处理异常*/
       console.log(e.message)
   });
   ```