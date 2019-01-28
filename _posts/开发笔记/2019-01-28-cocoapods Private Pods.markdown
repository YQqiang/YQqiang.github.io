---
layout: post
title:  "cocoapods Private Pods"
date:   2019-01-28 16:29:31 +0800
categories: 开发笔记
---

![](http://yuqiangcoder.com/assets/postImages/ios/201901/2.png)

# cocoapods Private Pods 

### 前言
[2018年底遗留篇补充系列1](http://yuqiangcoder.com/2019/01/28/VPS-%E6%90%AD%E5%BB%BA-GOGS.html)

2018年底遗留篇补充系列2

### spec repo ?

> `spec repo` 是所有的`Pods`的一个索引,就是一个容器,所有公开的`Pods`都在这个里面，他实际是一个`Git`仓库`remote`端

当你使用了`Cocoapods`后他会被`clone`到本地的`~/.cocoapods/repos`目录下

### 创建 spec repo
可在私有Git服务器上, 创建 `spec` 仓库 (和正常的创建仓库没有任何区别)

### 添加 spec repo

```
pod repo add YQSpecs http://m24.ink:3000/yuqiang/YQSpecs.git
```

注: 格式为
pod repo add 仓库名 仓库地址

### 创建私有仓库/组件/工具代码/项目工程代码

```
pod lib create MyLibrary
```
注: 格式为
pod lib create 库/组件名称

执行命令后需要回答几个问题进行简单的配置

1. 想使用的平台? [iOS / macOS ]

    ```
    What platform do you want to use?? [ iOS / macOS ]
    ```

2. 想使用的开发语言? [ Swift / Objc]
    
    ```
    What language do you want to use?? [ Swift / ObjC ]
    ```

3. 是否需要包含一个示例工程? [ Yes / No ]   **建议选 YES**
    
    ```
    Would you like to include a demo application with your library? [ Yes / No ]
    ```

4. 选择一个测试框架. [ Specta / Kiwi / None ]
    
    ```
    Which testing frameworks will you use? [ Specta / Kiwi / None ]
    ```
    
5. 是否基于`View`测试? [ Yes / No ]
    
    ```
    Would you like to do view based testing? [ Yes / No ]
    ```

6. 类的前缀
    
    ```
    What is your class prefix?
    ```

我这里选择的是:

    1. iOS
    2. Objc
    3. Yes
    4. None
    5. No
    6. YQ

配置结束后, 会自动打开工程


### 仓库结构预览

```
$ tree LibDemo -L 2
LibDemo
├── Example                     # Demo工程
│   ├── LibDemo
│   ├── LibDemo.xcodeproj
│   ├── LibDemo.xcworkspace
│   ├── Podfile
│   ├── Podfile.lock
│   ├── Pods
│   └── Tests
├── LICENSE
├── LibDemo                     # 源文件
│   ├── Assets                  # 源文件之资源(xml, png, db, plist...)
│   └── Classes                 # 源文件之代码(.h, .m, .Swift...)
├── LibDemo.podspec
├── README.md
└── _Pods.xcodeproj -> Example/Pods/Pods.xcodeproj
```

* `LibDemo` -> `Example` 目录为 Demo工程
* `LibDemo` -> `LibDemo` 目录为源代码及资源文件
* `LibDemo` -> `LibDemo.podspec` 文件为库的配置信息文件, 需要上传至 `spec repo`

### PodSpec

```
Pod::Spec.new do |s|
  s.name             = 'LibDemo'
  s.version          = '0.1.0'
  s.summary          = 'A short description of LibDemo.'
  s.description      = <<-DESC
TODO: Add long description of the pod here.
                       DESC

  s.license          = { :type => 'MIT', :file => 'LICENSE' }
  s.author           = { 'yuqiang' => 'yuqiang@yuqiang.com' }
  s.source           = { :git => 'https://m24.ink:3000/yuqiang/LibDemo.git', :tag => s.version.to_s }

  s.ios.deployment_target = '8.0'
  s.source_files = 'LibDemo/Classes/**/*'
end
```

* `s.name` 仓库名称
* `s.version` 仓库版本 对应 Git管理的 tag 值
* `s.summary` 仓库简介
* `s.description` 仓库描述
* `s.license` 开源协议
* `s.author` 作者
* `s.source` 源代码仓库
* `s.ios.deployment_target` 最低支持的系统版本
* `s.source_files` 源代码仓库下的具体源码文件路径

更过信息请查看官方文档 [Podspec Syntax Reference](https://guides.cocoapods.org/syntax/podspec.html)

### Bundle

在 `iOS` 中, 主工程中获取的图片, file文件等都是默认使用的 
`[NSBundle mainBundle]`, 

在仓库里需要获取指定`Framework`对应的 `Bundle`

```
#import "NSBundle+LibDemo.h"
#import "xxxxx.h"

@implementation NSBundle (LibDemo)

+ (NSBundle *)libDemo_bundle {
    return [NSBundle bundleForClass:[xxxxx class]];
}

@end
```

注: `#import "xxxxx.h"`, 为当前库的任意一个`Class`, 目的是为了根据该类获取当前库的`Bundle`

获取 `bundle` 时 使用 `[NSBundle LibDemo_bundle]` 即可

### 资源图片

在`xxx.podspec` 文件中配置资源文件的`Bundle`

```
s.resource_bundles = {
     'xxx_Images' => ['LibDemo/Assets/Assets.xcassets']
   }

```

增加`UIImage`扩展

```
#import "UIImage+LibDemo.h"
#import "NSBundle+LibDemo.h"

@implementation UIImage (LibDemo)

+ (UIImage *)libDemo_imageNamed:(NSString *)name {
    NSBundle *bundle = [NSBundle bundleWithURL:[[NSBundle libDemo_bundle] URLForResource:@"xxx_Images" withExtension:@"bundle"]];
    return [self libDemo_imageNamed:name inBundle:bundle];
}


+ (UIImage *)libDemo_imageNamed:(NSString *)name inBundle:(NSBundle *)bundle {
    return [self imageNamed:name inBundle:bundle compatibleWithTraitCollection:nil];
}

@end

```

加载图片时使用`[UIimage libDemo_imageNamed:@"xxx"]` 方法即可

### Swift 实现上述扩展功能

以下为 `Swift` 实现的上述功能扩展

针对具体的`podspec`配置, 需要做部分修改(切勿copy使用)

```
extension Bundle {
    class func sgftf_bundle() -> Bundle {
        return Bundle(for: SGFloatingLabelTextField.self)
    }
    
    class func sgftf_imagsBundle() -> Bundle {
        let urlOption = Bundle.sgftf_bundle().url(forResource: "SGFTF_Images", withExtension: "bundle")
        if let url = urlOption {
            if let bundle = Bundle(url: url) {
                return bundle
            }
        }
        return Bundle.main
    }
}

extension UIImage {
    class func sgftf_imageNamed(_ name: String, in bundle: Bundle = Bundle.sgftf_imagsBundle()) -> UIImage? {
        let image = UIImage(named: name, in: bundle, compatibleWith: nil)
        return image
    }
}
```

### Tag

开发完成, 需要增加 `tag`

注意: `tag` 值 需要和 `podSpec` 文件中的 `s.version` 对应

```
// 查看 tag 列表
git tag

// 新增 tag
git tag 0.0.1

// 推送 tag 到远端
git push --tags
```

### pod lib lint 

验证 lib

```
pod lib lint YQSpecs --sources=http://m24.ink:3000/yuqiang/YQSpecs.git,master LibDemo.podspec --allow-warnings
```

* `YQSpecs` 为文章开头时的 `spec repo`
* `--sources=xxx.git,master` 为 `YQSpecs` 的仓库地址
* `LibDemo.podspec` 为制作的仓库的配置文件
* `--allow-warnings` 忽略警告

### pod repo push

添加 Podspec 到 spec repo

```
pod repo push YQSpecs --sources=http://m24.ink:3000/yuqiang/YQSpecs,master LibDemo.podspec --allow-warnings --verbose
```

* `YQSpecs` 为文章开头时的 `spec repo`
* `--sources=xxx.git,master` 为 `YQSpecs` 的仓库地址
* `LibDemo.podspec` 为制作的仓库的配置文件
* `--allow-warnings` 忽略警告
* `--verbose` 显示详细信息

### 使用 制作的 pod

在项目的`Podfile` 文件中增加 `Source`

```
# cocoapods 的 repo url
source 'https://github.com/CocoaPods/Specs.git'

# private pod 的 repo url
source 'http://m24.ink:3000/yuqiang/YQSpecs.git'

target 'LibDemo_Example' do
  pod 'LibDemo'
end
```

### 参考文章 

[Private Pods](https://guides.cocoapods.org/making/private-cocoapods.html)

[使用Cocoapods创建私有podspec](http://blog.wtlucky.com/blog/2015/02/26/create-private-podspec/)

[给 Pod 添加资源文件](http://blog.xianqu.org/2015/08/pod-resources/)

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/

