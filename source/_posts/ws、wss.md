---
title: ws、wss
categories: 计算机基础
tags: ws wss
---

- `WS`协议和`WSS`协议两个均是`WebSocket`协议的`SCHEM`，`WS`是非安全的，`WSS`是安全的

- `WS`一般默认是80端口，而`WSS`默认是443端口

- `WSS` 就是 `WS` 基于 `SSL` 的安全传输，与 `HTTPS` 一样样的道理

- 在`https`下必须使用`WSS`，`WSS`下不支持`ip`地址的写法，写成域名形式

  ```js
  http -> new WebSocket('ws://xxx')
  
  https -> new WebSocket('wss://xxx')
  ```

> https://blog.csdn.net/hfismyangel/article/details/82758629

