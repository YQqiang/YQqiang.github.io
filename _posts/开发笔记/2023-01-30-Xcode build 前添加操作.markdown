---
layout: post
title:  "Xcode build 前添加操作"
date:   2023-01-30 09:28:31 +0800
categories: 开发笔记
---

<center class="half">
    <img src="{{site.url}}/assets/postImages/ios/202301/xcode-build-header.png"/>
</center>

# 背景

在通过 MonkeyDev [创建 MonkeyApp 工程](https://github.com/AloneMonkey/MonkeyDev/wiki/%E9%9D%9E%E8%B6%8A%E7%8B%B1App%E9%9B%86%E6%88%90)调试时，重复编译会报 `Executable Not Found` 的提示：
<center class="half">
    <img src="{{site.url}}/assets/postImages/ios/202301/ExecutableNotFound.png"/>
</center>

通过 `⌘ ⇧ K` 操作清空缓存后可正常编译运行，虽然只多了一步操作，但长时间调试还是非常影响效率的，因此我们的目的就很明确了：通过脚本在 Xcode build 前自动清空缓存。

# 尝试

明确目的后，就可以尝试实现了，主要思路如下：

1. 在哪里执行脚本？
2. 脚本如何实现？
3. 调试脚本是否正常工作

## 在哪里执行脚本

如果是通过命令行工具 `xcodebuild` 进行编译，我们可以直接在 `xcodebuild` 命令前执行清空缓存的脚本，但我们是在 Xcode IDE 中执行的 `⌘ r` 操作来编译运行到设备上的，无法直接执行脚本。

我们知道，在 Xcode IDE 中有个 `Build Phases` 菜单用来配置构建阶段，而且可以添加自定义脚本：

<center class="half">
    <img src="{{site.url}}/assets/postImages/ios/202301/add-script.png"/>
</center>

目前还不确定该阶段执行脚本是否可行，先挖个坑在这，稍后来填，继续下一步：编写脚本。

## 脚本如何实现

脚本实现逻辑比较简单，主要就两步：
1. 获取 build 的目录
2. 删除目录

### 获取 build 的目录
在 Xcode IDE 中，我们可以通过环境变量 `TARGET_BUILD_DIR` 来获取 build 目录。

> 啥？你问如何查询 Xcode 提供了哪些环境变量？   
这里有详细的说明：https://help.apple.com/xcode/mac/8.0/#/itcaec37c2a6

通过 `echo "$TARGET_BUILD_DIR"` 输出看下目录：

```
/Users/admin/Library/Developer/Xcode/DerivedData/TestBuild-corqiswhoijaynghttdmihqgdnab/Build/Products/Debug-iphoneos
```

实际我们想删除的是这个目录：

```
/Users/admin/Library/Developer/Xcode/DerivedData/TestBuild-corqiswhoijaynghttdmihqgdnab
```

因此我们对 `TARGET_BUILD_DIR` 获取 3 次上级目录即可：

```
$(dirname $(dirname $(dirname $TARGET_BUILD_DIR)))
```

### 执行删除操作

获取到目录后，使用 `rm` 命令进行删除即可：

```
echo "TARGET_BUILD_DIR:$TARGET_BUILD_DIR"
DIR=$(dirname $(dirname $(dirname $TARGET_BUILD_DIR)))
echo "DIR:$DIR"
rm -rf $DIR
```

## 调试脚本
在 Xcode Build Phases 中添加上述脚本，编译调试：

<center class="half">
    <img src="{{site.url}}/assets/postImages/ios/202301/debug-script.png"/>
</center>

哦吼，编译报错：

<center class="half">
    <img src="{{site.url}}/assets/postImages/ios/202301/debug-error.png"/>
</center>

提示说访问这个路径下的数据库失败了，仔细看下路径，这不就是我们删除的目录吗

```
error: accessing build database "/Users/admin/Library/Developer/Xcode/DerivedData/TestBuild-corqiswhoijaynghttdmihqgdnab/Build/Intermediates.noindex/XCBuildData/build.db": disk I/O error
```

# 填坑

通过上面的报错，我们知道这和我们最初的目的有点出入，我们的目的是删除上次编译后的缓存，而不是当次编译的缓存。

我们如果调整下脚本执行的时机呢？
在 `Compile Sources` 前执行脚本，(将脚本拖拽到前面)

<center class="half">
    <img src="{{site.url}}/assets/postImages/ios/202301/order-sort.png"/>
</center>

编译调试，依然报错，还是没有达到我们的预期。

通过[这篇文章](https://matheusvds.medium.com/clean-specific-folder-before-building-in-xcode-221ec6e0e004)的提示，可以在 `Scheme -> Build -> Pre-actions` 中配置脚本来实现 build 前添加操作：

<center class="half">
    <img src="{{site.url}}/assets/postImages/ios/202301/result-script.png"/>
</center>

具体步骤如下：

1. 通过 `⌘ ⇧ <` 打开 `Edit Scheme` 面板
2. 选中 `Build -> Pre-actions`
3. 点击 `+` 添加 `New Run Script Action`
4. 填入脚本内容

操作完成后进行调试，可正常编译运行，撒花🌼。

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/
