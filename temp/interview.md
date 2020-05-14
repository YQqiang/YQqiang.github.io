---
layout: post
title:  "interview"
date:   2019-09-02 15:29:31 +0800
categories: 开发笔记
---

# OC

## copy 和 strong 修饰 NSSting 属性的区别
 
## Block 及 内存管理
* 应用程序内存分配
    * 程序区域
    * 数据区域
    * 堆
    * 栈
* 什么是 Block？
* block 语法 和 block 类型变量
* 下面代码输出结果是什么？
    
    ```
    void autoGetValueForBlock() {
    
        int val = 10;
        
        const char *fmt = "val = %d\n";
        
        void (^blk)(void) = ^ { printf(fmt, val); } ;
        
        val = 2;
        
        fmt = "These values were changed. val = %d\n";
        
        blk();
    }
    ```
* 如何在 block 中更改变量值？
* __block 变量赋值原理?
* Block 在什么情况下会从 栈 复制到 堆？为什么？
* __block 变量在栈和堆上如何保证正确访问？
* 如何解决 Block 循环引用？

## GCD 多线程

## 动画相关

# cocoapods 
* 有发布过自己的仓库吗？
* 有开发过私有库吗？
    * lib库如何获取bundle？
    * lib 如何添加 libraries 依赖 和 frameworks 依赖？
* podfile 和 podspec 的区别和作用？
* Podfile.lock 的作用？
* pod repo push 命令的执行过程？

# Git
1. 如何切换分支、合并分支、删除本地及远程分支？
2. 本地文件的状态？
    * 未追踪、未修改、已修改、已暂存
    * 如何从 已修改状态 更改为 已暂存状态？
    * 如何从 已暂存状态 更改为 已修改状态？
    * 如何移除已被追踪的文件？
    * 如何添加 .gitignore?
    
3. 本地仓库和远程仓库之间的操作？
    * 如何把本地仓库分支 b_local 推送到远程仓库分支 b_remote?
    * 有使用过 pr 吗？
4. 如何把 某个commit 合并到 另外一个 分支?
5. 如何保存当前的工作进度？
6. git 提交规范

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/

