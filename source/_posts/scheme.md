---
title: scheme
categories: 计算机基础
tags: scheme url
---

#### 一、是什么？

- 是一种页面内跳转协议，是一种非常好的实现机制，通过定义自己的scheme协议，可以非常方便跳转app中的各个页面


#### 二、用在什么地方？

- 通过小程序，利用Scheme协议打开原生app
- H5页面点击锚点，根据锚点具体跳转路径APP端跳转具体的页面
- APP端收到服务器端下发的PUSH通知栏消息，根据消息的点击跳转路径跳转相关页面
- APP根据URL跳转到另外一个APP指定页面
- 通过短信息中的url打开原生app

<!-- more -->

#### 三、协议格式

1. ##### URL Scheme协议格式

   ```css
   String urlStr="http://www.ycbjie.cn:80/yc?id=hello&name=cg";
   //url =           protocol + authority(host + port) + path + query
   //协议protocol=    http
   //域名authority=   www.ycbjie.cn:80
   //页面path=        /yc
   //参数query=       id=hello&name=cg
   //authority=      host + port
   //主机host=        www.ycbjie.cn
   //端口port=        80
   ```

2. ##### Scheme链接格式样式

   ```css
   [scheme:][//authority][path][?query][#fragment] 
   ```


#### 四、提示

1. URL Scheme必须能唯一标识一个APP。如果设置的URL Scheme与别的APP的URL Scheme冲突，APP不一定会被启动起来。因为当APP在安装的时候，系统里面已经注册了相同的URL Scheme。**一般情况下，是会调用先安装的app**。
2. 并不是所有的页面或者操作都有URL Schemes，如果需要的话，可以自己添加URL Schemes 相关的代码
3. **猫眼的域名maoyan.dianping.com 在腾讯白名单内，可调起下载弹窗(安卓)，建议使用maoyan.dianping.com 域名**


#### 五、“坑”点

1. 应用A配置了scheme，应用B是可以通过url scheme直接打开应用A里配置了scheme的特定页面

   ```js
   // 原生APP页面
   URL = meituanmovie://www.meituan.com/liveAudience?roomId=XXXX
   ```

   如果是浏览器用url scheme打开app就不行，浏览器调用的时候会直接打开应用A的启动页面，而不是指定页面，并且会把Uri传给启动页(即使把scheme配置在其他页面也没用，只会打开启动页)

   ```js
   //APP内嵌H5页面，需要先打开app，在app中唤起web，然后exportweb的scheme不带token等参数信息，需要跳转到http://maoyan.dianping.com/app/android-forword 先携带参数再打开web页面，注意参数中的url需要使用encodeURIComponent编码
   URL = `meituanmovie://www.meituan.com/exportweb?url=${encodeURIComponent(
           `http://m.maoyan.com/app/android-forword?to=${encodeURIComponent(
              `meituanmovie://www.meituan.com/web?url=${iurl}`,
             )}`,
          )}`
   ```

2. 可能会被app禁掉，比如微博等

3. ios9+ 禁止掉了iframe方式。

4. ios及部分安卓浏览器会提示用户是否打开App，并且ios在未安装对应App的时候，会提示“打不开网页，因为该网址无效”

5. h5无法感知是否唤醒成功

6. 大部分浏览器需要用户手动触发链接，js自动触发无效
