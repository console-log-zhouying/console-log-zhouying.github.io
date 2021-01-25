---
title: Jest
categories: 测试框架
tags: 测试框架 Jest
---

##### 一、jest优点

- `Jest `可以利用其特有的快照测试功能，通过比对 UI 代码生成的快照文件，实现对 React 等常见框架的自动测试。此外， Jest 的测试用例是并行执行的，而且只执行发生改变的文件所对应的测试，提升了测试速度。
- 安装配置简单，非常容易上手，几乎是零配置的，通过npm 命令安装就可以直接运行了。
- `Jest `内置了测试覆盖率工具istanbul，可以通过命令开启或者在 `package.json` 文件进行更详细的配置。运行 istanbul 除了会再终端展示测试覆盖率情况，还会在项目下生产一个 coverage 目录，内附一个测试覆盖率的报告，让我们可以清晰看到分支的代码的测试情况。
- 集成了断言库，不需要再引入第三方的断言库，并且非常完美的支持React组件化测试。

<!-- more -->

##### 二、jest匹配器

```js
expect(1 + 2).toBe(3); // 精确匹配
expect({one: 1}).toEqual({ one: 1 }); // 深度匹配
expect(null).toBeNull(); // Null匹配
expect(undefined).toBeUndefined(); // undefined 匹配
expect(var).toBeDefined(); // var是否定义
expect(true).toBeTruthy(); // true 匹配
expect(false).toBeFalsy(); // false 匹配
expect(false).not.toBe(true); // 取反匹配
expect(1).toBeGreaterThan(0); // 大于
expect(1).toBeLessThan(2); // 小于
expect(1).toBeGreaterThanOrEqual(1); // 大于等于
expect(1).toBeLessThanOrEqual(1); // 小于等于
expect(0.1 + 0.2).toBeCloseTo(0.3); // 浮点数
expect('toMatch').toMatch(/to/) // 是否包含
expect([1, 2, 3]).toContain(1) // 是否包含某些元素
expect(() => { throw new Error() }).toThrow(); // 异常匹配
```


##### 三、命令行工具使用

1. `f`
   - 只运行上次检测测试用例失败的文件。上次正确的文件，即使修改也不会重新检查
2. `o`
   - 只运行修改过的文件，需要配个git使用
3. `p`
   - 输入一个正则/字符串，会检查所有匹配（包含）文件的名字
4. `t`
   - 输入一个正则/字符串，会检查所有匹配（包含）的测试实例名字的文件
5. `q`
   - 退出监控
6. `enter`
   - 重新运行一遍

`package.json`配置

- `watchAll`：监控所有文件的变化
- `watch`：每次只重新测试修改过后的文件


##### 四、异步函数测试方法

- 异步函数

  ```js
  function loadData(){
    return new Promise((resolve, reject)=>{
      resolve({
        code: 200,
        data: {}
      });
    })
  }
  ```

1. 通过返回

   ```js
   test('should loadData', () => {
     expect.assertions(1); // 必须执行一个expect。否则不会等到异步函数执行完毕就测试完毕
     return loadData().then(res => {
       expect(res.code).toBe(200)
     });
   });
   
   test("fecthData3 返回结果为 404", () => {
       expect.assertions(1);
       return loadData().catch(e => {
           expect(e.toString().indexOf("404") > -1).toBe(true);
       })
   })
   ```

2. 通过调用参数

   ```js
   test('should loadData', (done) => {
     loadData().then(res => {
       expect(res.code).toBe(200)
       done(); //done函数执行了即代表执行了expect
     });
   });
   ```

3. 通过`resolves`

   ```js
   test('should loadData', () => {
     return expect(loadData()).resolves.toMatchObject({
       code: 200
     });
   });
   ```

4. `async`和`await`

   ```js
   test("should loadData", async () => {
       const res = await loadData();
       expect(res.data).toEqual({
           success: true
       })
   })
   ```


##### 五、钩子函数

```js
// 开始执行一次
beforeAll(() => {
  console.log('befaoreAll');
});

// 每个测试用例都会执行
beforeEach(() => {
  console.log('beforeEach');
});

// 每个测试用例执行后都会执行
afterEach(() => {
  console.log('afterEach');
});

// 勾子执行完毕
afterAll(() => {
  console.log('afterAll');
});
```


##### 六、对测试用例进行分组

```js
import Counter from './counter.js'
 
describe('测试Counter', () => {
    let counter = null
    beforeEach(() => {
        counter = new Counter()
    })
 
    describe('测试增加', () => {
        test('测试 Counter 中 addOne 方法', () => {
            counter.addOne()
            expect(counter.number).toBe(1)
        })
        test('测试 Counter 中 addOne 方法', () => {
            counter.addOne()
            expect(counter.number).toBe(1)
        })
    })
 
    describe('测试减少', () => {
        test('测试 Counter 中 minusOne 方法', () => {
            counter.minusOne()
            expect(counter.number).toBe(-1)
        })
        test('测试 Counter 中 minusOne 方法', () => {
            counter.minusOne()
            expect(counter.number).toBe(-1)
        })
    })
})
```


##### 七、Mock

1. 为什么使用`mock`？

   - 在项目中，一个模块的方法内常常会去调用另外一个模块的方法。在单元测试中，我们可能并不需要关心内部调用的方法的执行过程和结果，只想知道它是否被正确调用即可，甚至会指定该函数的返回值

2. `mock`的三种特性

   - 捕获函数调用情况
   - 设置函数返回值
   - 改变函数的内部实现

3. `jest.fn()`

   - 创建mock函数

   - 如果没有定义内部实现，返回值为undefined

     ```js
     test('测试jest.fn()调用', () => {
       let mockFn = jest.fn();
       let result = mockFn(1, 2, 3);
     
       // 断言mockFn的执行后返回undefined
       expect(result).toBeUndefined();
       // 断言mockFn被调用
       expect(mockFn).toBeCalled();
       // 断言mockFn被调用了一次
       expect(mockFn).toBeCalledTimes(1);
       // 断言mockFn传入的参数为1, 2, 3
       expect(mockFn).toHaveBeenCalledWith(1, 2, 3);
     })
     ```

   - 设置返回值

     ```js
     jest.fn().mockReturnValue('default');
     ```

4. `mock axios`

   ```js
   import axios from 'axios';
   import Users from './users';
   
   jest.mock('axios');
   
   test('should fetch users', () => {
     axios.get.mockResolvedValue('name'); //模拟实际调用axios后的返回值，所有请求都返回同样的结果
     // myMock.mockReturnValueOnce(10).mockReturnValueOnce('x')  多次调用返回的结果不同
     return Users.all().then(data => expect(data).toEqual('name'));
   });
   ```

5. 根目录下建`__mocks__`文件夹

   ```js
   // __mocks__  demo.js
   export const fetchData = () => {
       return new Promise((resolved, reject) => {
           resolved(
               "(function() {return '123'})()"
           )
       })
   }
   ```

   ```js
   jest.mock('./demo');
   
   import { fetchData } from './demo'; //找到__mocks__下的demo.js文件
   const { getNumber } = jest.requireActual('./demo'); //找对应文件名的test文件
   
   test("测试fetchData", () => {
       return fetchData().then(data => {
           expect(eval(data)).toBe("123");
       })
   })
   
   test("测试fetchData", () => {
       expect(getNumber()).toBe("456")
   })
   ```


##### 八、类的测试

```js
// 类
export default class Util {
    a() {
	// 逻辑异常复杂
    }

    b() {
	// 逻辑异常复杂
    }
}
```

```js
//__mocks__ util.js
import Util from './util'; 

//这里只关注a和b是否执行了即可，至于执行正确与否是util类的测试文件应该做的事情
//所以如果a，b函数的过程很耗费性能，那我们没必要让它们执行完，只需要模拟即可
const demoFunction = (a, b) => { 
    const util = new Util;
    util.a(a);
    util.b(b);
}

export default demoFunction;
```

```js
jest.mock("./util.js");
// jest.mock 发现 util 是一个类，会自动的把类的构造函数和方法变成 jest.fn()
// 背后原理如下：
// const Util = jest.fn();
// Util.a = jest.fn();
// Util.b = jest.fn();

// 由于类的方法都是一个 jest.fn() 所以我们可以对类的方法执行做追溯
import demoFunction from './demo';
import Util from './util';

test("测试 demoFunction", () => {
    // 要想测试demoFunction成功执行了，只需验证他调用了Util类创建实例，并且调用a，b 方法.
    // 但由于Util中的方法都异常复杂，全部执行，很耗性能。所以我们需要去模拟Util类
    demoFunction();
    expect(Util).toHaveBeenCalled();
    expect(Util.mock.instances[0].a).toHaveBeenCalled();
    expect(Util.mock.instances[0].b).toHaveBeenCalled();
})
```


##### 九、timer

- 测试带有定时器的文件，如果设置的定时时间是几百秒呢？难道我们要等待几百秒吗？

```js
//timer.js
export default (callback) => {
	setTimeout(() => {
		callback();
	}, 3000)
}
```

```js
//timer.test.js
import timer from './timer';

jest.useFakeTimers();
test("timer 测试", () => {
    const fn = jest.fn();
    // 这样timer还是会等待定时器设置的时长再执行
    timer(fn);
    // 所以必须配合下面的代码
    // 这种代码的意思是让timer立刻执行，我们就不必等待了
    jest.runAllTimers();
    expect(fn).toHaveBeenCalledTimes(1);
})
```

如果有这种需求，定时器的嵌套。只想测试第一层的`timer`，如果使用`runAllTimers`，内层的`timer`也会直接运行。

```js
//timer.js
export default (callback) => {
    setTimeout(() => {
        callback();
        setTimeout(() => {
            callback();
        }, 2000)
    }, 1000)
}
```

```js
import timer from './timer';

jest.useFakeTimers();
test("timer 测试", () => {
    const fn = jest.fn();
    // 这样timer还是会等待定时器设置的时长再执行
    timer(fn); 
    // 所以必须配合下面的代码
    // 这种代码的意思是让timer立刻执行，我们就不必等待了,
    // 只运行处于队列中的timer，没有被添加到队列中的timer不会被执行
    jest.runOnlyPendingTimers();
    expect(fn).toHaveBeenCalledTimes(1);
})
```


##### 十、snapshot 快照测试

- 适合测试配置文件。每次建立一个快照，下一次与上一次对比，展示差异。

```js
//demo.js
export const generateConfig  = () => {
    return {
        server: 'http://localhost',
        port: '8080'
    }
}
```

```js
//demo.test.js
import { generateConfig } from './snapshot.js'
 
test('测试 generateConfig', () => {
    expect(generateConfig()).toMatchSnapshot()  //会生成快照文件
})

//u：更新全部快照
//也可以先用i，进入调试模式，一个个展示决定是否用 u 更新


//行内快照的写法  -> 需要安装 prettier
expect(generateConfig()).toMatchInlineSnapshot();

```


##### 十一、jest测试dom节点

```js
//demo.js
import $ from 'jquery';

const addDivToBody = () => {
	$('body').append('<div/>')
};

export default addDivToBody;
```

```js
//demo.test.js
import addDivToBody from './demo';
import $ from 'jquery';

test('测试 addDivToBody', () => {
	addDivToBody();
	addDivToBody();
	expect($('body').find('div').length).toBe(2);
	console.log($('body').find('div').length);
})

//使用 jquery 创建 dom 是在 node 环境下运行的，node 是不具备 dom的。之所以能够操作 dom,是因为 jest 在 node 环境下自己模拟了一套 dom 的 api, 叫做 jsDom
```