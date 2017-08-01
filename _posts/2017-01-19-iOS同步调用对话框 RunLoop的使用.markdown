---
layout: post
title:  "iOS同步调用对话框 RunLoop的使用"
date:   2017-01-19 10:17:31 +0800
categories: jekyll update
---
![](http://upload-images.jianshu.io/upload_images/3538284-600ace94ea6c8576.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

> 更新时间: 2017-04-19
此种方案设计的弹出框, 不适用于`tableView` `Cell`点击代理方法中操作; 但是可以通过在`cell`上增加`button`, 利用`button`的点击事件调用同步弹出框. 

# 问题
> 
iOS中弹窗显示后, 代码会继续执行, 然后使用`block`获取用户操作结果;
但在开发中, 往往需要等待用户操作后再继续执行代码, 这时候我们可以借助`RunLoop`实现弹出窗的同步调用.

# 测试Demo
> 
定义了一个`Bool`的变量--`isConfirm`, 弹出对话框后, 打印`isConfirm`的值, 在用户操作后更改`isConfirm`的值, 查看打印结果和打印时机.


# 测试结果, 注意输出`isConfirm`的时机
![测试Demo.gif](http://upload-images.jianshu.io/upload_images/3538284-bd8490af18e17595.gif?imageMogr2/auto-orient/strip)

# 代码
```

        let alertVC = UIAlertController(title: "标题", message: "测试同步的弹窗", preferredStyle: .alert)
        
        let cancel = UIAlertAction(title: "取消", style: .cancel) { (action) in
            self.isConfirm = false
            CFRunLoopStop(CFRunLoopGetCurrent())
        }
        let confirm = UIAlertAction(title: "确认", style: .default) { (action) in
            self.isConfirm = true
            CFRunLoopStop(CFRunLoopGetCurrent())
        }
        alertVC.addAction(cancel)
        alertVC.addAction(confirm)
        present(alertVC, animated: true, completion: nil)
        CFRunLoopRun()
        
        print("isConfirm---\(isConfirm)")
```

# 实现原理
> 在弹出对话框后手动启动RunLoop(*`CFRunLoopRun()`*)
在用户操作后, 强行停止正在运行的RunLoop(*`CFRunLoopStop(CFRunLoopGetCurrent())`*)

# 写在最后

![](http://upload-images.jianshu.io/upload_images/3538284-2b59305066e31d68.jpeg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
本人菜鸟一枚,欢迎大神前来指(tiao)教(xi), 带我~~进坑~~入门

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/


