---
title: Js性能优化
categories: JS性能优化
tags: JS性能优化
---

##### 一、慎用全局变量

- 全局变量定义在全局执行上下文，是所有作用域链的顶端
- 全局执行上下文一直存在于上下文执行栈，直到程序退出
- 如果某个局部作用域出现了同名变量则会遮蔽或污染全局

<!-- more -->

##### 二、缓存全局变量

```js
//没有缓存
function getBtn(){
  let obtn1 = document.getElementById('btn1')
  let obtn2 = document.getElementById('btn2')
  let obtn3 = document.getElementById('btn3')
}

//加缓存
function getBtn(){
  let obj = document;
  let obtn1 = obj.getElementById('btn1')
  let obtn2 = obj.getElementById('btn2')
  let obtn3 = obj.getElementById('btn3')
}
```


##### 三、通过原型新增方法

```js
// before：通过构造对象添加方法
var fn1 = function() {
  this.foo = function(){
    console.log('do someing')
  }
}
f1 = new fn1()

// after：通过原型对象添加方法
var fn2 = function() {
  fn2.prototype.foo = function(){
    console.log('do someing')
  }
}
f2 = new fn2()
```


##### 四、避开闭包陷阱

- 闭包是一种强大的语法，使用不当容易出现内存泄漏，不要为了闭包而闭包

- ```js
  function test2(){
    var name = 'test'
    return name;
  }
  
  // before
  test(function (){
    var name = 'test'
    return name;
  })
  
  // after
  test(test2())
  ```


##### 五、for循环优化

```js
// before
var arrList = []
arrList[10000] = 'icoder'
for(var i=0; i < arrList.length; i++){
  console.log(arrList[i])
}

//after
var arrList = []
arrList[10000] = 'icoder'
for(var i = arrList.length; i; i--){
  console.log(arrList[i])
}
```


##### 六、采用最优循环方式

```js
var arrList = new Array(1,2,3,4,5)

// 方式一：性能最优
arrList.forEach(item){
  console.log(item)
}

// 方式二：其次
for(var i = arrList.length; i; i--){
  console.log(arrList[i])
}

// 方式三：last
for(var i in arrList){
  console.log(arrList[i])
}
```


##### 七、节点添加优化

- 节点的添加操作必然会有回流和重绘

- ```js
  // before：方式一
  for(var i = 0; i < 10; i++){
    var oP = document.createElement('p')
    oP.innerHTML = i
    document.body.appendChild(oP)
  }
  
  // after：方式二
  const fragEle = document.createDocumentFragment()
  for(var i=0; i<10; i++){
    var oP = document.createElement('p')
    oP.innerHTML = i
    fragEle.appendChild(oP)
  }
  document.body.appendChild(fragEle)
  ```