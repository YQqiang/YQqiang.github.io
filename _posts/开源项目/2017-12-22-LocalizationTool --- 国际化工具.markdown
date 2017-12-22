---
layout: post
title:  "LocalizationTool --- 国际化工具"
date:   2017-12-22 09:48:31 +0800
categories: 开源项目
---
![](http://upload-images.jianshu.io/upload_images/3538284-22a1a8c2a367d199.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 下载
下载地址: 
[LocalizationTool 源码](https://github.com/YQqiang/LocalizationTool)
[LocalizationTool dmg压缩包](https://github.com/YQqiang/LocalizationTool/releases/tag/v1.0)

### 前言
关于项目国际化, 网上一大票教程, 不在赘述; 该工具要是针对散落在代码各个角落中的`NSLocalizedString`进行检索整理.
该工具实现的主要思想内容源自 [ReadChinese](https://github.com/ashen-zhao/ReadChinese), 并在原有基础上修改并增加了一些匹配规则, 以便于适合现有的项目使用.
应用程序icon图标源自[UI中国-上古神兽](http://www.ui.cn/detail/304791.html)

### 安装
1. 下载[LocalizationTool 源码](https://github.com/YQqiang/LocalizationTool), 使用Xcode 编译,运行. 
2. 下载[LocalizationTool dmg压缩包](https://github.com/YQqiang/LocalizationTool/releases/tag/v1.0), 解压后双击运行.

### 使用
![应用程序界面](http://upload-images.jianshu.io/upload_images/3538284-c0724452b26c8a54.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

1. 导入路径
选择工程项目所在路径

2. 匹配规则说明
* 默认匹配规则为 检索 `NSLocalizedString` 包裹的字符串, 可自动根据`OC` 或 `Swift`文件切换匹配规则
* 自定义匹配规则, 可填写正则表达式进行匹配; 注: 匹配中若包含`(` `)`, 需要转义 `\(` `\)`
![自定义匹配规则](http://upload-images.jianshu.io/upload_images/3538284-651cde3d341bf9d4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
* 使用前缀&后缀匹配, 输入要匹配的前缀和后缀即可自动匹配; 注: 匹配中若包含`(` `)`, 需要转义 `\(` `\)`
![输入前缀&后缀匹配](http://upload-images.jianshu.io/upload_images/3538284-11fe6e69c431d904.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

3. 文件后缀
* 默认检索的文件为`.h`, `.m`, `.swift`
* 可补充需要检索的文件后缀名; 例: `xml,strings`
![补充增加检索文件后缀](http://upload-images.jianshu.io/upload_images/3538284-5b890af99c57623c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

4. 导出路径
* 导出路径会在选择路径后创建文件夹`Localization`, 导出会有两个文件: `allKeys.txt` 和 `removedExistKey.txt`
  * `allKeys.txt`  检索到的所有key值
  * `removedExistKey.txt`  去除重复key后的文件
![导出路径](http://upload-images.jianshu.io/upload_images/3538284-7d6344725dca42bd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

5. 剔除重复key
* 默认剔除项目中所有`strings`文件中存在`key`
![默认剔除所有String文件中存在的key](http://upload-images.jianshu.io/upload_images/3538284-26c8ecff35185fdd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
* 剔除指定`strings`文件中存在的`key`
![剔除指定strings文件中存在的key](http://upload-images.jianshu.io/upload_images/3538284-50f1394ee3035359.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 问题记录
第一次写Mac程序, 记录两个小问题😂😂😂😂😂😂😂😂
1. 写文件出错

```
Error Domain=NSCocoaErrorDomain 
Code=513 "You don’t have permission to save the file “***.txt” 
in the folder “****”." 
UserInfo={NSFilePath=/Users/dongl/Desktop/123.txt, 
NSUnderlyingError=0x608000045d90 
{Error Domain=NSPOSIXErrorDomain 
Code=1 "Operation not permitted"}}
```
解决: 关闭`TARGETS --> Capabilities --> App Sandbox`
![关闭App Sandbox](http://upload-images.jianshu.io/upload_images/3538284-48c1413ac208041c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
2. 点击左上角叉号, 退出应用程序
在`AppDelegate`中处理

```
func applicationShouldTerminateAfterLastWindowClosed(_ sender: NSApplication) -> Bool {
    return true
}
```

### shell 剔除文件中重复的行
在整理国际化资源时, 国际化文件存在重复内容的行, 本来是准备撸代码遍历文件查找重复行的, 却被我们老大在终端一行脚本搞定了, 故在此记录学习下.

```
sort Localizable.strings| uniq -d > /Users/xxxx/Desktop/d.tx
参数说明: 
| 管道符
-d 查找重复的行
-u 查找唯一的行
> 重定向, 输出到文件

或者直接使用:
awk '!a[$0]++' Localizable.strings > /Users/xxxx/Desktop/d.tx
```


## 联系我：
- 博客: http://yuqiangcoder.com/
- 邮箱: yuqiang.coder@gmail.com

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/


