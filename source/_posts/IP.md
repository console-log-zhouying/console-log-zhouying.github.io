---
title: IP
categories: 计算机基础
tags: IP
---

#### 一、X-forwarded-for

1. ##### 定义

   - X-Forwarded-For 是一个扩展header头，用来表示 HTTP 请求端真实 IP，在HTTP/1.1（RFC 2616）协议中没有定义，但是现在已经成为事实上的标准，被各大 HTTP 代理、负载均衡等转发服务广泛使用，并被写入 RFC 7239（Forwarded HTTP Extension）标准之中。

   - 一般来说，`X-Forwarded-For`是用于记录代理信息的，每经过一级代理(匿名代理除外)，代理服务器都会把这次请求的`来源IP`追加在`X-Forwarded-For`中

<!-- more -->

2. ##### 由人为设置

   - 一些代理服务器会设置一些消息头，比如nginx会在转发请求的时候可以带上这个消息头，向应用服务传递客户端的真实IP;

     使用下面的配置在nginx设置反向代理转发的X-Forwarded-For：

     ```js
     proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for; // 常用
     proxy_set_header X-Forwarded-For "$http_x_forwarded_for, $remote_addr" // 不常用，$http_x_forwarded_for是读取到的消息头，如果请求头没有x-Forwarded-for,这个值就是空的。
     ```

     这样就能在应用服务器拿到消息头 X-Forwarded-For；

3. ##### 可以伪造

   - 因为是人为设置，伪造一个假的很简单，如下：

     ```js
     proxy_set_header X-Forwarded-For "121.12.12.12"; 
     ```

     所以接收到的不可信任的服务器传递回来的header，或者客户端伪造了header，其中x-forwarded-for是不能当做客户端IP的。

4. ##### 格式

   - 如果一个 HTTP 请求到达服务器之前，经过了三个代理 Proxy1、Proxy2、Proxy3，IP 分别为 IP1、IP2、IP3，用户真实 IP 为 IP0，代理服务器会把前一个网络设备的IP地址追加到X-Forwarded-For 上面，经过层层追加，服务端最终会收到以下信息：

     ```js
     X-Forwarded-For: IP0, IP1, IP2
     ```

     同时注意到IP3是不会追加上到这个列表上的。

   - 来自`4.4.4.4`的一个请求，header包含这样一行

     ```
     X-Forwarded-For: 1.1.1.1, 2.2.2.2, 3.3.3.3
     ```

     代表 请求由`1.1.1.1`发出，经过三层代理，第一层是`2.2.2.2`，第二层是`3.3.3.3`，而本次请求的来源IP`4.4.4.4`是第三层代理。而`X-Real-IP`，没有相关标准，上面的例子，如果配置了`X-Read-IP`，可能会有两种情况：

     - ```js
       // 最后一跳是正向代理，可能会保留真实客户端IP
       X-Real-IP: 1.1.1.1
       ```

     - ```js
       // 最后一跳是反向代理，比如Nginx，一般会是与之直接连接的客户端IP
       X-Real-IP: 3.3.3.3
       ```

       所以 ，如果只有一层代理，这两个头的值就是一样的

5. ##### 实际验证

   在本地电脑开启一个nodejs服务，端口为9090；开启nginx反向代理，端口为8062；

   - 通过8062访问nginx服务的时候，req.headers['x-forwarded-for']值为

     ```js
     192.168.1.105 //nginx通过X-Forwarded-For将客户端的地址转发了过来
     ```

   - 通过9090直接访问nodejs访问，req.headers['x-forwarded-for']值为

     ```js
     undefined //直接请求的时候，没有设置消息头，自然为undefined
     ```

6. ##### 结论

   - 有没有X-Forwarded-For和代理服务器的设置有关；
   - 正确与否也和代理服务器有关；


#### 二、remoteAddress

- 如果没有X-Forwarded-For，应用服务器可以通过与服务端建立 TCP 连接获取到。在nodejs中可以通过req.socket.remoteAddress获取到IP3；
- remoteAddress有没有可能是假的呢？因为tcp链接需要三次握手，所以无法伪造这个ip。


#### 三、X-Real-IP

- 是一个自定义的消息头，目前并不属于任何标准，完全由用户控制。


#### 四、结论

- 没有代理：直接使用remoteAddress获取客户端IP，因为header中x-forwarded-for不可靠，可能有也可能没有，甚至可能是伪造的。
- 有代理的情况下，获取到的remoteAddress是代理服务器的IP，如果代理服务器是可信赖的，那么能通过x-forwarded-for来获取客户端IP。