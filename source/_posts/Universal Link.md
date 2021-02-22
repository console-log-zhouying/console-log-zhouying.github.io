---
title: Universal Link
categories: 计算机基础
tags: Universal-Link
---

#### 一、Universal Link是什么？

- `iOS 9`之后苹果推出的一个替代之前的`custom URL Scheme`的新概念
- `Universal Link`，就可以让网站或者`web view`中的内容在用户点击跳转或安装了`app`之后仍然能够直接在这个`app`中被找到。比如，用户在官网上点击了“在`app`中浏览该商品”的链接，这个时候就可以通过`Universal Link`去唤起这个`app`，同时直接定位到该商品页面。
- 它的实现机制与`deeplink`相似，只不过它不是只定义一个`custom URL scheme`，而是匹配了多个`web`页面到`app`中相应的位置，当用户打开某个匹配的页面时，`iOS`会自动地将其重定向到`app`内。

<!-- more -->

#### 二、优势

1. 之前的Custom URL scheme是自定义的协议，因此在没有安装该app的情况下是无法直接打开的。而Universal Links本身也就是一个能够指向一个web页面或者app中的内容页的标准的web link（形如[https://example.com](https://link.jianshu.com?t=https://example.com)） 因此能够很好的兼容其他情况。也就是说，当已经安装了这个app的时候，不需要加载任何web页面，app就会立即启动；当这个app没有安装的时候，就会默认地从当前浏览器中重定向到App Store中引导用户去下载安装这个app。
2. Universal links是从服务器上查询是哪个app需要被打开，因此不存在Custom URL scheme那样名字被抢占、冲突的情况。
3. Universal links支持从其他app中的UIWebView中跳转到目标app
4. 安全性，用universl link去打开的时候，只有你（开发这个app的人）可以通过创建和上传一个允许这个网页去通过这个URL去打开你的app的文件。
5. 隐私性，提供Universal link给别的app进行app间的交流，然而对方并不能够用这个方法去检测你的app是否被安装。（之前的custom scheme URL的canOpenURL方法可以，具体可以看这里[iOS Review-DetectScheme](https://www.jianshu.com/p/97a0e708a6b2)。）


#### 三、“坑”点

1. 跨域问题，IOS 9.2 以后，必须要触发跨域才能支持 Universal Link 唤端。IOS 有判断，如果要打开的 Universal Link 和 当前页面是同一域名，ios 尊重用户最可能的意图，直接打开链接所对应的页面。如果不在同一域名下，则在APP 中打开链接，执行具体的唤端操作。
2. Universal Link 是空页面，Universal Link 本质上是个空页面，如果未安装 APP，Universal Link 被当做普通的页面链接，会跳到 404 页面，所以我们需要将它绑定到我们的中转页或者下载页。