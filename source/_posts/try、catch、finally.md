---
title: try、catch、finally
categories: Js
tags: try catch finally
---

1. 不管有没有异常，`finally`中的代码都会执行
2. 当`try`、`catch`中有`return`时，`finally`中的代码依然会继续执行
3. 方法的返回值在`finally`运算之前就确定了
4. `finally`代码中最好不要包含`return`，程序会提前退出，也就是说返回的值不是`try`或`catch`的值
