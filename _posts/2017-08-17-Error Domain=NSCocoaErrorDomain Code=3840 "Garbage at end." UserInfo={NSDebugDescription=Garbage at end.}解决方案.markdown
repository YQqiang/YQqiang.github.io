---
layout: post
title:  "Error Domain=NSCocoaErrorDomain Code=3840 "Garbage at end." UserInfo={NSDebugDescription=Garbage at end.}解决方案"
date:   2017-08-17 17:41:31 +0800
categories: jekyll update
---
![](http://upload-images.jianshu.io/upload_images/3538284-0994ca16d7c693b9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/320)

### 使用 *NSJSONSerialization* 将字符串转字典
* 步骤一: 将字符串使用`NSUTF8StringEncoding`编码格式转换为`NSData `
* 步骤二: 将`data`使用`NSJSONSerialization `转换为对象
* 步骤三: 把对象为`NSDictionary`
```
NSData *jsonData = [result dataUsingEncoding:NSUTF8StringEncoding];
NSError *error = nil;
id  dicData = [NSJSONSerialization JSONObjectWithData:jsonData
                                              options:kNilOptions
                                                error:&error];
if ([dicData isKindOfClass:[NSDictionary class]]) {
    NSDictionary *dict = (NSDictionary *)dicData;
}
```
ps:  代码中`result` 为以下字符串
```
{"scannum":8,"list":[{"ssid":"SG-A1408190013","bssid":"C8:93:46:11:0C:7F","auth":"OPEN","channel":1,"rssi":-16},{"ssid":"MikroTik-D539F7","bssid":"4C:5E:0C:D5:39:F7","auth":"OPEN","channel":1,"rssi":-34},{"ssid":"254118","bssid":"4C:5E:0C:D6:F8:61","auth":"WPA2_PSK_AES","channel":3,"rssi":-44},{"ssid":"china-unicom","bssid":"00:36:76:1A:BE:27","auth":"WPA2_PSK_AES","channel":1,"rssi":-46},{"ssid":"Derek-Phone","bssid":"F4:5C:89:C3:2D:29","auth":"WPA2_PSK_AES","channel":6,"rssi":-50},{"ssid":"sungrow","bssid":"00:8E:F2:FF:CA:60","auth":"WPA2_PSK_AES","channel":11,"rssi":-68},{"ssid":"sungrow","bssid":"00:8E:F2:FF:C6:80","auth":"WPA2_PSK_AES","channel":11,"rssi":-68},{"ssid":"sungrow","bssid":"00:8E:F2:FF:C8:A0","auth":"WPA2_PSK_AES","channel":11,"rssi":-68}]}
```
转换结果报错, 错误如下:
```
Error Domain=NSCocoaErrorDomain Code=3840 "Garbage at end." UserInfo={NSDebugDescription=Garbage at end.}
```

### 问题分析
1. 使用json格式化工具验证json是否合格
json正确, 如下图所示

![验证json合格](http://upload-images.jianshu.io/upload_images/3538284-b0269aa4374a0c2e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

2. 既然是合法的json, 为什么转换会出错?
通过查看data数据, 发现了问题所在.
```
<7b227363 616e6e75 6d223a36 2c226c69 7374223a 5b7b2273 73696422 
3a225347 2d413134 30383139 30303133 222c2262 73736964 223a2243 
383a3933 3a34363a 31313a30 433a3746 222c2261 75746822 3a224f50 
454e222c 22636861 6e6e656c 223a312c 22727373 69223a2d 31347d2c 
7b227373 6964223a 224d696b 726f5469 6b2d4435 33394637 222c2262 
73736964 223a2234 433a3545 3a30433a 44353a33 393a4637 222c2261 
75746822 3a224f50 454e222c 22636861 6e6e656c 223a312c 22727373 
69223a2d 33307d2c 7b227373 6964223a 22323534 31313822 2c226273 
73696422 3a223443 3a35453a 30433a44 363a4638 3a363122 2c226175 
7468223a 22575041 325f5053 4b5f4145 53222c22 6368616e 6e656c22 
3a332c22 72737369 223a2d34 327d2c7b 22737369 64223a22 44657265 
6b2d5068 6f6e6522 2c226273 73696422 3a224634 3a35433a 38393a43
 333a3244 3a323922 2c226175 7468223a 22575041 325f5053 4b5f4145 
53222c22 6368616e 6e656c22 3a362c22 72737369 223a2d34 387d2c7b 
22737369 64223a22 6368696e 612d756e 69636f6d 222c2262 73736964 
223a2230 303a3336 3a37363a 31413a42 453a3237 222c2261 75746822 
3a225750 41325f50 534b5f41 4553222c 22636861 6e6e656c 223a362c 
22727373 69223a2d 35307d2c 7b227373 6964223a 2273756e 67726f77 
222c2262 73736964 223a2230 303a3845 3a46323a 46463a43 413a3630 
222c2261 75746822 3a225750 41325f50 534b5f41 4553222c 22636861 
6e6e656c 223a3131 2c227273 7369223a 2d36347d 00>
```
data 最后以00 结尾, 尝试删除最后一位,查看结果正常;
分析原因可能是转换data的字符串中含有不可见的非法字符

### 解决
过滤字符串中的非法字符
[控制字符](https://baike.baidu.com/item/%E6%8E%A7%E5%88%B6%E5%AD%97%E7%AC%A6/6913704?fr=aladdin)
```
// 去掉首尾的空白字符
result = [result stringByTrimmingCharactersInSet:[NSCharacterSet whitespaceAndNewlineCharacterSet]];
// 去除掉控制字符
result = [result stringByTrimmingCharactersInSet:[NSCharacterSet controlCharacterSet]];
```

### 完整的步骤为
```
// 去掉首尾的空白字符
result = [result stringByTrimmingCharactersInSet:[NSCharacterSet whitespaceAndNewlineCharacterSet]];
// 去除掉控制字符
result = [result stringByTrimmingCharactersInSet:[NSCharacterSet controlCharacterSet]];

NSData *jsonData = [result dataUsingEncoding:NSUTF8StringEncoding];
NSError *error = nil;
id  dicData = [NSJSONSerialization JSONObjectWithData:jsonData
                                              options:kNilOptions
                                                error:&error];
if ([dicData isKindOfClass:[NSDictionary class]]) {
    NSDictionary *dict = (NSDictionary *)dicData;
}
```

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/

