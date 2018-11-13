---
layout: post
title:  "Xcode10 New Build System And CocoaPods"
date:   2018-11-13 15:29:31 +0800
categories: 开发笔记
---

![](http://yuqiangcoder.com/assets/postImages/ios/201811/1.png)

## 前言

更新了 `Xcode 10` 后, 遇到一些坑, 为了使用新版本有些坑忍忍就算啦~~
但是在更改了源文件, 编译运行,代码居然不生效, 这能忍?????

别着急, clean 一下试试

1. 打开 Xcode 偏好设置

    `Xcode -> Preference -> Locations`
    
    ![](http://yuqiangcoder.com/assets/postImages/ios/201811/2.png)
    
2. 打开 `Derived Data` 文件夹, 并删除文件夹下的内容
    
    ![](http://yuqiangcoder.com/assets/postImages/ios/201811/3.png)
    
重新 编译运行, 修改的代码生效了(还好我机智😎)

等等.....每次改完代码都要 `clean` 一遍, 想想都可怕.....

## Google
`google` 一下吧, 看下别人时如何解决的

 果然, 已经有人在 `Stack overflow` 提问了
 
 [Xcode 10 build 10A255 requires clean build folder for all changes](https://stackoverflow.com/questions/52382648/xcode-10-build-10a255-requires-clean-build-folder-for-all-changes)
 
![](http://yuqiangcoder.com/assets/postImages/ios/201811/4.png)

## New Build System

因为 `Xcode 10` 默认的编译系统是  `New Build System`

而在 `Xcode 9.4` 时默认的编译系统还是 `Legacy Build System`(传统编译系统)

那么把编译系统改为`Legacy Build System`试下

`File -> Workspace Settings -> Build System`

![](http://yuqiangcoder.com/assets/postImages/ios/201811/5.png)

更改为`Legacy Build System`后, 完美运行.

但是该更改操作只对当前项目工程生效.

## 什么是 New Build System ?

> Xcode 10 uses a new build system. The new build system provides improved reliability and build performance, and it catches project configuration problems that the legacy build system does not.

* 提供更高的可靠性
* 捕获项目配置错误
* 提高整体构建性能

更多关于`New Build System`的内容可查看以下 

Apple 官方文档 [Build System Release Notes for Xcode 10](https://developer.apple.com/documentation/xcode_release_notes/xcode_10_release_notes/build_system_release_notes_for_xcode_10)

[Xcode New Build System for Speedy Swift Builds](https://medium.com/xcblog/xcode-new-build-system-for-speedy-swift-builds-c39ea6596e17)

## 担忧

既然苹果会在`Xcode 10`上默认为新的编译系统, 那么有可能在下个版本放弃 `Legacy Build System`, 强制使用 `New Build System`, 因此更改 `Build System`并不是最好的解决方案.

同样有人抛出了这种顾虑.
[Xcode 10 - New Build System](https://www.reddit.com/r/iOSProgramming/comments/9l2u8a/xcode_10_new_build_system/)

> 
> My uninformed guess is that Xcode 11 will eliminate the legacy build system.
>
> * Xcode 9: new build system introduced, off by default
> 
> * Xcode 10: new build system on by default, legacy system still available
> 
> * Xcode 11: legacy system removed
> 
> Edit: that would mean roughly this time next year

## 继续 Google

检索了多篇内容, 都把源头指向了 `CocoaPods`

![](http://yuqiangcoder.com/assets/postImages/ios/201811/6.png)

![](http://yuqiangcoder.com/assets/postImages/ios/201811/7.png)

## CocoaPods
果不其然, 在 [CocoaPods](https://github.com/CocoaPods/CocoaPods/issues?utf8=%E2%9C%93&q=new+build+system+xcode+10) `Issues` 页面有相关问题

[Embed Pods Frameworks build phase not reliably executed with Xcode 10 / New Build System](https://github.com/CocoaPods/CocoaPods/issues/8151)

[XCode10 (new build system) - if incremental build, Embed script doesn't run](https://github.com/CocoaPods/CocoaPods/issues/8073)

[Provide an installation option to disable usage of input/output paths](https://github.com/CocoaPods/CocoaPods/pull/8105)

通过以上不难找到解决方案

在 `Build Phase` -> `[CP] Embed Pods Frameworks` 
移除 `Input Files` 和 `Output Files`

或者在 `Podfile` 文件中增加禁用指令

```
install! 'cocoapods', :disable_input_output_paths => true
```

注: `disable_input_output_paths` 参数在 `pod v1.6.0` 以上版本才可用
可使用 `gem install cocoapods --pre` 命令升级

更过 `CocoaPods` 内容可查看 

[Podfile Syntax Reference](https://guides.cocoapods.org/syntax/podfile.html#install_bang)

## 总结

1. 解决方案一:

    更改 Xcode 的编译系统为 `Legacy Build System`


2. 解决方案二:

    在 `podfile` 文件中增加 
    
    ```
    install! 'cocoapods', :disable_input_output_paths => true
    ```


[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/

