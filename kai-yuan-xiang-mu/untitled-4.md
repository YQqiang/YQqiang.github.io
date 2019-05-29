# LocalizationTool --- 国际化工具 \| 大道至简 悟在天成

![](http://yuqiangcoder.com/assets/postImages/ios/201712/1.png)

### 下载 <a id="&#x4E0B;&#x8F7D;"></a>

下载地址: [LocalizationTool 源码](https://github.com/YQqiang/LocalizationTool)

[LocalizationTool dmg压缩包](https://github.com/YQqiang/LocalizationTool/releases/tag/v1.0)

### 前言 <a id="&#x524D;&#x8A00;"></a>

关于项目国际化, 网上一大票教程, 不在赘述; 该工具要是针对散落在代码各个角落中的`NSLocalizedString`进行检索整理. 该工具实现的主要思想内容源自 [ReadChinese](https://github.com/ashen-zhao/ReadChinese), 并在原有基础上修改并增加了一些匹配规则, 以便于适合现有的项目使用. 应用程序icon图标源自[UI中国-上古神兽](http://www.ui.cn/detail/304791.html)

### 安装 <a id="&#x5B89;&#x88C5;"></a>

1. 下载[LocalizationTool 源码](https://github.com/YQqiang/LocalizationTool), 使用Xcode 编译,运行.
2. 下载[LocalizationTool dmg压缩包](https://github.com/YQqiang/LocalizationTool/releases/tag/v1.0), 解压后双击运行.

### 使用 <a id="&#x4F7F;&#x7528;"></a>

![&#x5E94;&#x7528;&#x7A0B;&#x5E8F;&#x754C;&#x9762;](http://yuqiangcoder.com/assets/postImages/ios/201712/2.png)

1. 导入路径 选择工程项目所在路径
2. 匹配规则说明
3. 文件后缀
   * 默认检索的文件为`.h`, `.m`, `.swift`
   * 可补充需要检索的文件后缀名; 例: `xml,strings` ![&#x8865;&#x5145;&#x589E;&#x52A0;&#x68C0;&#x7D22;&#x6587;&#x4EF6;&#x540E;&#x7F00;](http://yuqiangcoder.com/assets/postImages/ios/201712/5.png)
4. 导出路径
   * 导出路径会在选择路径后创建文件夹`Localization`, 导出会有两个文件: `allKeys.txt` 和 `removedExistKey.txt`
     * `allKeys.txt` 检索到的所有key值
     * `removedExistKey.txt` 去除重复key后的文件 ![&#x5BFC;&#x51FA;&#x8DEF;&#x5F84;](http://yuqiangcoder.com/assets/postImages/ios/201712/6.png)
5. 剔除重复key

### 问题记录 <a id="&#x95EE;&#x9898;&#x8BB0;&#x5F55;"></a>

第一次写Mac程序, 记录两个小问题😂😂😂😂😂😂😂😂

1. 写文件出错

```text
Error Domain=NSCocoaErrorDomain 
Code=513 "You don’t have permission to save the file “***.txt” 
in the folder “****”." 
UserInfo={NSFilePath=/Users/dongl/Desktop/123.txt, 
NSUnderlyingError=0x608000045d90 
{Error Domain=NSPOSIXErrorDomain 
Code=1 "Operation not permitted"}}
```

解决: 关闭`TARGETS --> Capabilities --> App Sandbox` ![&#x5173;&#x95ED;App Sandbox](http://yuqiangcoder.com/assets/postImages/ios/201712/9.png)

1. 点击左上角叉号, 退出应用程序 在`AppDelegate`中处理

```text
func applicationShouldTerminateAfterLastWindowClosed(_ sender: NSApplication) -> Bool {
    return true
}
```

### shell 剔除文件中重复的行 <a id="shell-&#x5254;&#x9664;&#x6587;&#x4EF6;&#x4E2D;&#x91CD;&#x590D;&#x7684;&#x884C;"></a>

在整理国际化资源时, 国际化文件存在重复内容的行, 本来是准备撸代码遍历文件查找重复行的, 却被我们老大在终端一行脚本搞定了, 故在此记录学习下.

```text
sort Localizable.strings| uniq -d > /Users/xxxx/Desktop/d.tx
参数说明: 
| 管道符
-d 查找重复的行
-u 查找唯一的行
> 重定向, 输出到文件

或者直接使用:
awk '!a[$0]++' Localizable.strings > /Users/xxxx/Desktop/d.tx
```

## 联系我： <a id="&#x8054;&#x7CFB;&#x6211;"></a>

* 博客: http://yuqiangcoder.com/
* 邮箱: yuqiang.coder@gmail.com

