---
layout: post
title:  "WebSocket"
date:   2020-04-06 14:11:31 +0800
categories: 开发笔记
---

![](http://yuqiangcoder.com/assets/postImages/ios/202004/web.jpg)

该图片由<a href="https://pixabay.com/zh/users/Pexels-2286921/?utm_source=link-attribution&amp;utm_medium=referral&amp;utm_campaign=image&amp;utm_content=1868997">Pexels</a>在<a href="https://pixabay.com/zh/?utm_source=link-attribution&amp;utm_medium=referral&amp;utm_campaign=image&amp;utm_content=1868997">Pixabay</a>上发布

## 计算机网络体系结构

![](http://yuqiangcoder.com/assets/postImages/ios/202004/OSI.png)

* 应用层
    * 任务： 通过应用进程间的交互来完成特定网络应用；
    * 应用层协议定义的是应用进程间通信和交互的规则；
    * 应用层交互的数据单元称为**报文**；
    * 对于不同的网络应用需要有不同的应用层协议；
        * 支持万维网应用的 `HTTP` 协议
        * 支持电子邮件的 `SMTP` 协议
        * 支持文件传送的 `FTP` 协议
* 运输层
    * 任务： 负责向两个主机中进程之间的通信提供通用的数据传输服务；
    * 应用层利用该服务传送应用层报文；
    * 运输层主要使用以下两种协议
        * 传输控制协议 **TCP** --- 提供面向连接的、可靠的数据传输服务，数据传输的单位是**报文段**；
        * 用户数据包协议 **UDP** --- 提供无连接的、尽最大努力的数据传输服务（不保证数据传输的可靠性），其数据传输的单位是**用户数据报**。
* 网络层
    * 任务1： 负责为分组交换网上的不同主机提供通信服务；
    * 任务2： 选择合适的路由，使源主机运输层所传下来的分组能够通过网络中的路由器找到目的主机
    * 因特网主要的网络层协议是无连接的网际协议 `IP` 和许多中路由选择协议，因此因特网的网络层也叫做**网际层**或**IP层**
* 数据链路层
    * 数据链路层将网络层交下来的IP数据报组装成帧，在两个相邻结点间的链路上传送。
    * 每一帧包括数据和必要的控制信息（如同步信息，地址信息，差错控制等）。
    * 在接收数据时，控制信息使接收端能够知道一个帧从哪个比特开始和到哪个比特结束。这样，数据链路在接收到一个帧后，就可从中提取出数据部分，上交给网络层。
* 物理层
    * 在物理层上所传输的数据单位是比特。
    * 物理层还要确定连接电缆的插头应当有多少根引脚以及各条引脚应如何连接。

**注意：传递信息所利用的一些物理媒体，如双绞线、同轴电缆、光缆、无线信道等，并不在物理层协议之内而是在物理层协议的下面。**

## socket
> `socket` 是对 `TCP/IP` 协议的封装，`socket` 本身并不是协议，而是一个调用接口， 通过 `socket`，我们才能使用 `TCP/IP`。
> `socket`编程接口在设计的时候，就希望也能适用于其他网络协议。所以说，`socket`的出现只是使得程序员更方便地使用 `TCP/IP` 协议栈而已，是对 `TCP/IP` 协议的抽象，从而形成了我们知道的一些最基本的函数接口，比如 `create`、`listen`、`connect`、`accept`、`send`、`read`和 `write` 等等。

## WebSocket

### 简介

> `WebSocket` 是一种网络传输协议，可在单个 `TCP` 连接上进行全双工通信，位于 `OSI` 模型的应用层。
> `WebSocket` 允许服务端主动向客户端推送数据。在 `WebSocket API` 中，浏览器和服务器只需要完成一次握手，两者之间就可以创建持久性的连接，并进行双向数据传输。

### 对比 HTTP 协议

![](http://yuqiangcoder.com/assets/postImages/ios/202004/websocket+http.png)

* websocket 是一个持久化的协议；HTTP 是非持久化协议；
* 在HTTP中，声明周期通过Request界定，一个 Request 对应一个 Response，且这个 Response是被动的，不能主动发起。
* websocket借用了HTTP协议来完成一部分握手🤝，

### 实时通讯
* 轮询
    * 浏览器每隔一段时间向服务器发出 HTTP 请求，然后服务器返回最新的数据给客户端。
    * 缺点： 浏览器需要不断的向服务器发出请求，然而 HTTP 请求与回复可能会包含较长的头部，其中真正有效的数据可能只是很小的一部分，所以这样会消耗很多带宽资源。
* websocket
    * 能更好的节省服务器资源和带宽，并且能够更实时地进行通讯

### 统一资源标识符（URI）
websocket使用ws或wss的统一资源标志符（URI）。其中wss表示使用了TLS的websocket。

```
ws://example.com/wsapi
wss://secure.example.com/wsapi
```

### 默认端口
* websocket与HTTP和HTTPS使用相同的TCP端口
* websocket协议使用80端口；
* 运行在 TLS 之上时，默认使用443端口。

## SocketRocket



## 使用



## 参考链接

[维基百科-WebSocket](https://zh.wikipedia.org/wiki/WebSocket)
[知乎](https://www.zhihu.com/question/20215561)





[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/

