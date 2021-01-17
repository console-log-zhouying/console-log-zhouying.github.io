---
title: TypeScript基础
categories: TypeScript
tags: TypeScript TypeScript类型
---

#### 一、作用域问题

比如直接定义一个`const a = 123`，这是定义在全局作用域上的，第二个文件再定义a会出问题

1. 第一种解决办法：立即执行函数

   ```js
   (function () {
     const a = 123;
   })
   ```

2. 第二种解决办法：模块作用域

   ```js
   const a = 123;
   export {}  // 注意：这不是导出一个空对象的意思，而是一种语法，变为模块作用域
   ```

<!-- more -->

#### 二、类型

1. ##### 原始数据类型

   ```js
   const a: string = 'foobar'
   
   const b: number = 100 // NaN Infinity
   
   const c: boolean = true // false
   
   这三种数据类型都支持值为null或者undefined
   -------
     
   const d: string = null //默认模式不会报错，严格模式不允许（配置里的strictNullChecks是检查不为空的）
   
   -------
   
   const e: void = undefined // null (严格模式下只能是undefined)
   
   const f: null = null
   
   const g: undefined = undefined
   
   const h: symbol = Symbol()
   ```

2. ##### Object类型

   - Typescript中的Object不仅仅是指对象类型，而是指所有的非原始类型，比如对象、数组、函数

     ```js
     export {} // 确保跟其它示例没有成员冲突
     
     const foo: Obejct = function () {} // [] // {}
     ```

   - 对对象的限制可以使用类似字面量的方式，但是正规的方式是使用接口

     ```js
     const obj: { foo: number, bar: string } = { foo: 123, bar: 'string' } 
     // 这种限制方式对象不能多也不能少，比如多一个参数就会报错
     ```

3. ##### 数组类型

   ```js
   const arr1: Array<number> = [1,2,3]
   
   const arr2: number[] = [1,2,3] // 更常见
   ```

4. ##### 元组类型

   每一个成员类型不必相同

   ```js
   export {} // 确保跟其它示例没有成员冲突
   
   const tuple: [number, string] = [18, 'zce']
   
   // 取元组数据
   const age = tuple[0]
   
   const [age, name] = tuple
   ```

5. ##### 枚举类型

   ```typescript
   export {} // 确保跟其它示例没有成员冲突
   
   enum PostStatus {
     Draft = 0,
     Unpublished = 1,
     Published = 2
   }
   // 注意：
   // 1. 可以不给每一个枚举写值，如果都不写的话，第一项从0开始，后面以此累加
   // 2. 如果都不写值，给第一项写值为6，后面则从6开始累加
   // 3. 枚举的值可以是值，也可以是字符串。因为字符串是不会增长的，所以必须手动写值
   // 4. 一般的类型只是运行时限制，编译后就移除了，但是枚举类型不会。除非加const，变成常量枚举
   ```

6. ##### 函数类型

   - 函数声明：

     ```js
     export {} // 确保跟其它示例没有成员冲突
     
     function func1 (a: number, b: number): string {
       return 'func1'
     }
     func1(100, 200)
     
     // 如果参数是可选的(2种写法) 
     // 注意：可选参数必须在参数列表的最后
     function func1 (a: number, b?: number): string { // 有无第二个参数都可
       return 'func1'
     }
     function func1 (a: number, b: number = 10): string { // 有无第二个参数都可
       return 'func1'
     }
     
     // 任意个数的参数
     function func1 (a: number, b: number, ...rest: number[]): string {
       return 'func1'
     }
     ```

   - 函数表达式：

     ```js
     const func1 = function(a: number, b: number): string {
       return 'fun2'
     }
     ```

7. ##### 任意类型

   ```js
   export {} // 确保跟其它示例没有成员冲突
   
   function stringify(value: any) { // 不会对any类型进行类型检查
     return Json.stringify(value)
   }
   
   // 因为不会对any类型做类型检查，所以any类型是不安全的，一般不使用，一般在兼容老代码时使用
   ```

8. ##### 隐式类型推断

   如果没有明确通过类型注解去标注一个变量的类型，那么Typescript会通过这个变量的使用情况推断变量类型

   ```js
   export {} // 确保跟其它示例没有成员冲突
   
   let age = 18 // 推断为number
   age = 'string' //报错，因为上文已推断为number
   
   // 如果无法推断，则标注为any类型
   let foo
   foo = 100
   foo = 'string'
   
   // 建议给每个变量添加明确的类型
   ```

9. ##### 类型断言

   代码无法推断是什么类型，但是人为知道是什么类型，这时候可以类型断言

   ```js
   export {} // 确保跟其它示例没有成员冲突
   
   const nums = [110, 120, 119, 112]
   
   const res = nums.find(i => i>0) //人为知道一定是number类型，但是编译器不确定，还可能找不到
   
   //断言方式1
   const num1 = res as number
   //断言方式2
   const num2 = <number>res // JSX下不能使用
   ```


#### 三、接口

- 用来约束对象的结构

  ```typescript
  export {} // 确保跟其它示例没有成员冲突
  
  interface Post {
    title: string
    content: string
    subtitle?: string // 可选成员
    readonly summary: string // 只读成员，不可修改
  }
  
  function printPost (post: Post) {
    console.log(post.title)
    console.log(post.content)
  }
  
  printPost(title: 'hello', content: 'world')
  
  // 动态成员
  interface Cache {
    [prop: string]: string
  }
  
  const cache: Cache = {}
  
  cache.foo = 'value1'
  cache.bar = 'value2'
  ```


#### 四、类

- 描述一类具体对象的抽象成员

  ```typescript
  export {} // 确保跟其它示例没有成员冲突
  
  class Person { //变量一定要有初始值
    name: string
    age: number
    
    constructor (name: string, age: number) {
      this.name = name
      this.age = age
    }
    
    sayHi (msg: string): void {
      console.log(`i am ${this.name}`)
    }
  }
  ```

- 访问修饰符

  - `private`
  - `public`
  - `protected`

- 只读属性

  ```typescript
  export {}
  
  class Person {
    name: string
    protected readonly age: number // 只读属性，初始化后不可修改
    
    constructor (name: string, age: number) {
      this.name = name
      this.age = age
    }
  }
  ```


#### 五、类与接口

```typescript
export {} // 确保跟其它示例没有成员冲突

// 一个接口定义一个能力，都定义在一起太耦合了
interface Eat {
  eat (food: string): void
}

interface Run {
  run (distance: number): void
}
  
class Person implements Eat, Run {
  eat (food: string): void {
    
  }
  
  run (distance: number) {
    
  }
}
  
class Animal implements Eat, Run {
  eat (food: string): void {
    
  }
  
  run (distance: number) {
    
  }
}
```


#### 六、抽象类

```typescript
export {} // 确保跟其它示例没有成员冲突

abstract class Animal {
  eat (food: string): void {
    console.log(`${food}`)
  }

	abstract run (distance: number): void
}

class Dog extends Animal {
  run(distance: number): void { // 必须定义抽象方法
    
  }
}

const d = new Dog()
d.eat('草')
d.run(100)
```


#### 七、泛型

- 定义：定义函数、接口、类的时候没有定义具体的类型，使用的时候再定义具体类型

- 目的：极大程度的复用代码

  ```typescript
  export {} // 确保跟其它示例没有成员冲突
  
  function createArray<T>(length: number, value: T): T[] {
    const arr = Array<T>(length).fill(value)
  }
  
  const res = createArray<string>(3, 'foo')
  ```