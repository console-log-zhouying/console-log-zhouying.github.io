---
title: deeplink
categories: 计算机基础
tags: deeplink url
---

#### 一、deeplink是什么

- `Deeplink`，简单讲，就是你在手机上点击一个链接之后，可以直接链接到`app`内部的某个页面，而不是`app`正常打开时显示的首页。不似`web`，一个链接就可以直接打开`web`的内页，`app`的内页打开，必须用到deeplink技术。


#### 二、deeplink的调用过程

假设从`APP-F`调用`APP-T`

1. `APP-T`要进行自定义`scheme`的配置（`iOS`是`info`文件，`Android`是`activity`），并进行参数处理的`coding`。
2. `APP-F`进行调用，首先判断设备是否安装`APP-T`。
3. 如果未安装，则跳转到`APP-T`的`web`版应用（假设他提供`web`版）或者是跳转到`AppStore`等应用市场进行下载。
4. 如果已安装，则调用`APP-T`配置好的`URL SCHEME`，直接打开`APP-T`的相关界面。

<!-- more -->

#### 三、是否使用deeplink的过程差异

![3-1](3-1.png)

#### 四、deeplink与scheme的关系

- `URI Schemes`其实就是实现`deeplink`的第一代解决方案。利用它就可以在移动开发中实现从`web`页面或者别的`app`中唤起自己的`app`的功能，然而开发者们很快就发现，这样也还有很多限制：
  - ==当要被唤起的`app`没有安装时，这个链接就会出错。==
  - 当注册有多个`scheme`相同的时候，目前没有办法区分。
- 因此为了解决以上问题，苹果和安卓都有了自己的第二套解决方案，分别是`iOS`的`Universal Link`，和安卓的`App Link`。