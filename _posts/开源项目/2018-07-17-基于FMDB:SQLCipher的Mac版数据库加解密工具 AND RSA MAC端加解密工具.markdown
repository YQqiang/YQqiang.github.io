---
layout: post
title:  "基于FMDB/SQLCipher的Mac版数据库加解密工具 AND RSA MAC端加解密工具"
date:   2018-07-17 16:02:31 +0800
categories: 开源项目
---
![](http://yuqiangcoder.com/assets/postImages/ios/201807/1.svg)

### 前言
项目中使用到的数据库进行了加密操作, 每次在调试数据库相关功能时,很不方便. 于是便撸了一个Mac 桌面版数据库加解密工具, 方便查看加密的数据库内容及对已有数据库进行加密操作.

### 下载
下载地址: 
[EncryptDBUI 源码](https://github.com/YQqiang/EncryptDBUI)

### 使用
![应用程序界面](http://yuqiangcoder.com/assets/postImages/ios/201807/1.png)

1. 拖拽数据库到工具页面中;
2. 输入加解密密码;
3. 进行加密/解密

![应用程序界面](http://yuqiangcoder.com/assets/postImages/ios/201807/2.png)

* 可点击右上角`清空重选`按钮进行清空已选的数据库,重新选择数据库文件

### 拓展
[sqlcipherDemo](https://github.com/zhengbomo/sqlcipherDemo)

[命令行加解密数据库](https://www.zetetic.net/sqlcipher/sqlcipher-api/#sqlcipher_export)


### 2018-09-27 更新
RSA 加解密工具
[RSAUI 源码](https://github.com/YQqiang/RSAUI)
![应用程序界面](http://yuqiangcoder.com/assets/postImages/ios/201807/3.png)

## 联系我：
- 博客: http://yuqiangcoder.com/
- 邮箱: yuqiang.coder@gmail.com

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/


