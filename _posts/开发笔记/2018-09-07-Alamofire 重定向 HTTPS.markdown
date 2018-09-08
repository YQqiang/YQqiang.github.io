---
layout: post
title:  "Alamofire 重定向 HTTPS"
date:   2018-09-07 15:29:31 +0800
categories: 开发笔记
---

![](http://yuqiangcoder.com/assets/postImages/ios/201809/0.jpg)

## 前言
> 在多数情况下，我们做的网络请求是返回200状态码的，但也有返回302的时候，比如使用基于Oauth2认证协议的API时.

目前的项目是支持接入私有云, 即用户可以自定义服务器地址;
但是前端并不知道用户输入的`host`是否支持`HTTPS`; 
最开始的设计是页面提供选项,由用户自己决定是使用`HTTP`还是`HTTPS`, 但是这种方式的用户体验不够友好, 很可能用户自己也不清楚到底该选择哪一种.

## 问题
是否可以自动重定向`HTTPS`?

## 尝试
使用`Alamofire`以`HTTP`的方式请求支持`HTTPS`的服务地址, 使用`Charles`工具查看相关请求.
如下图所示:
![](http://yuqiangcoder.com/assets/postImages/ios/201809/1.png)

由此可见, `Alamofire` 支持重定向`HTTPS`, 但是重定向后的请求走不通, 需要进一步调试.

## 拦截重定向
> `Alamofire`在构造 `SessionManager`时, 可以传入代理`SessionDelegate`, 在代理中拦截重定向内容

```
let sessionDelegate: SessionDelegate = SessionDelegate()
    sessionDelegate.taskWillPerformHTTPRedirection = { (session: URLSession, task: URLSessionTask, response: HTTPURLResponse, request: URLRequest) -> URLRequest? in
   var rqt = request
   rqt.httpMethod = task.originalRequest?.httpMethod
   rqt.httpBody = task.originalRequest?.httpBody
   return rqt
}
```

调试发现, 重定向后的`Request`中的请求方式和请求参数丢了, 因此需要重新设置

```
var rqt = request
rqt.httpMethod = task.originalRequest?.httpMethod
rqt.httpBody = task.originalRequest?.httpBody
```

## 安全策略
设置无条件信任

```
let policy = ServerTrustPolicy.disableEvaluation
let serverTrushPolicy = ServerTrustPolicyManager(policies: [(URL(string: host)?.host ?? "").lowercased(): policy])
Alamofire.SessionManager(configuration: configuration, serverTrustPolicyManager: serverTrushPolicy)
```

## SessionManager
当使用 Alamofire.SessionManager 的实例时, 必须保证该实例的生命周期, 否则实例对象释放**会话结束**,引发错误`code=-999`
可以使用下面的方式解决 [Alamofire Error Domain=NSURLErrorDomain Code=-999 “cancelled”](https://stackoverflow.com/questions/40447436/alamofire-error-domain-nsurlerrordomain-code-999-cancelled)

```
sessionManager.request(url).session.finishTasksAndInvalidate()
```

> `finishTasksAndInvalidate()` 此方法立即返回，无需等待任务完成。 会话失效后，无法在会话中创建新任务，但现有任务会一直持续到完成。 在最后一个任务完成并且会话进行与这些任务相关的最后一个委托调用之后，会话在其委托上调用`urlSession(_:didBecomeInvalidWithError:)`方法，然后中断对委托和回调对象的引用。 失效后，会话对象无法重用。
[finishTasksAndInvalidate() 苹果官方文档](https://developer.apple.com/documentation/foundation/urlsession/1407428-finishtasksandinvalidate)


## 链接
[Alamofire Issues](https://github.com/Alamofire/Alamofire/issues/876)

[Alamofire源码解读系列(六)之Task代理(TaskDelegate)](http://www.cnblogs.com/machao/p/6544153.html)

[Alamofire源码解读系列(八)之安全策略(ServerTrustPolicy)](https://www.cnblogs.com/machao/p/6605794.html)

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/

