---
layout: post
title:  "Nunchakus -- 个人APP"
date:   2017-04-19 10:17:31 +0800
categories: 开源项目
---
![](http://upload-images.jianshu.io/upload_images/3538284-e1bc33c34e201e2f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 前言
最近利用业余时间, 写了一个非常简陋的APP(Nunchakus): 该APP只有一个功能, 那就是视屏播放, 虽然功能就一个, 但是开发中坑却不少, 所以在此做下总结.

# Nunchakus 预览及简介

![Nunchakus](http://upload-images.jianshu.io/upload_images/3538284-62c5d1bbc283b0b7.gif?imageMogr2/auto-orient/strip)

Nunchakus 是一款专门播放双节棍视频的APP, Nunchakus中所有的视屏资源均来自[双节棍吧](http://www.sjg8.com)

# 技术实现
### 网络层的代码实现
观察网站的视屏结构可以清楚整理出网页的路径`http://www.sjg8.com/###/xxx`
其中`###`代表的视频的类型, `xxx`代表页数.       
```
enum VideoType {
    case zipai      // 棍友自拍
    case biaoyan    // 舞台表演
    case jiaoxue    // 棍法教学
    case media      // 媒体关注
    case movie      // 影视动画
    case guowai     // 国外聚焦
    case paoku      // 极限跑酷
    case v          // 播放视屏
}
```
整理好网站结构可以很容易地使用`moya`写出网络请求`Service`, 不太会使用`moya`的同学可以参考我的这篇文章[RxSwift + Moya + ObjectMapper + MVVM 的网络请求](http://www.jianshu.com/p/2e0dfba02ae5)
首页虽然有七大类(`v`是用来播放视频的不作为视频类型, 但是`URL`路径类似, 所以写在同一个枚举里面), 但是我们只需要实现某一类型的视频播放, 然后设置不同的类型即可.

### 解析HTML
`html`的解析使用到了第三方库[Kanna](https://github.com/tid-kijyun/Kanna)
`XPath`语法:

|表达式|描述|
|:-:|:-:|
| nodename |选取此节点的所有子节点|
|/|从根节点选取|
|//|从匹配选择的当前节点选择文档中的节点，而不考虑它们的位置|
|.|选取当前节点|
|..|选取当前节点的父节点|
|@|选取属性|

`XPath`更多其他用法, 可以去[w3school](http://www.w3school.com.cn/xpath/xpath_syntax.asp)查看
至此, 可以完成解析`html`并转成`model`的步骤, 接下来的开发就和常规开发模式相同了, 在次不再赘述.

以下是填坑部分

---

# 填坑
### 坑1 -  获取到优酷视频的VID, 如何解析出真实的视频播放地址
使用以下`js`代码,可以解析优酷视频地址:
```
console.log("request")

var requestURL = function(window, encodeid){
    window.BuildVideoInfo = {
        encodeid:encodeid,
        _type:"m3u8",
        _url:"http://play.youku.com/play/get.json?vid=" + encodeid + "&ct=12&callback=BuildVideoInfo.response",
        _vid:encodeid,
    };
    BuildVideoInfo.response = function(a) {
        this.init(a);
    };
    BuildVideoInfo.m3u8src = function(a) {
        return YK.password = this._password, YK.m3u8src_v2(this.encodeid, a)
    };
    BuildVideoInfo.init = function(a) {
        console.log(a);
        this._v = a;
        var b = a.data, c = b.stream;
        if (this.encodeid = b.video.encodeid, !b.security ||!b.security.encrypt_string ||!b.security.ip)
//            location.href = "yuqiang://encodeidfailed"
            return YKP.sendErrorReport(2004), void YKP.showError(null, "数据解析错误");
        if (!c&&!b.error)
//            location.href = "yuqiang://encodeidfailed"
            return void YKP.showError(null, "该视频暂不能播放");
        var d = [19, 1, 4, 7, 30, 14, 28, 8, 24, 17, 6, 35, 34, 16, 9, 10, 13, 22, 32, 29, 31, 21, 18, 3, 2, 23, 25, 27, 11, 20, 5, 15, 12, 0, 33, 26], e = rc4(translate(YK.mk.a3 + "o0b" + YKP.userCache.a1, d).toString(), decode64(b.security.encrypt_string)), f = e.split("_");
        YKP.userCache.sid = e.split("_")[0];
        YKP.userCache.token = e.split("_")[1];
        YK.v = a;
        var url = YK.m3u8src_v2(BuildVideoInfo.encodeid,"mp4");
        $("#media").attr("src",url);
        location.href = url
//        alert(requestCallback)
//        alert(requestCallback.callback)
//        alert(url);
        requestCallback.callback(url)
    };
    
    var YK = {}, YKU = {}, YKP = {
        playerType: "",
        userCache: {
            a1: "4",
            a2: "1"
        },
        playerState: {
            PLAYER_STATE_INIT: "PLAYER_STATE_INIT",
            PLAYER_STATE_READY: "PLAYER_STATE_READY",
            PLAYER_STATE_AD: "PLAYER_STATE_AD",
            PLAYER_STATE_PLAYING: "PLAYER_STATE_PLAYING",
            PLAYER_STATE_END: "PLAYER_STATE_END",
            PLAYER_STATE_ERROR: "PLAYER_STATE_ERROR"
        },
        playerCurrentState: "PLAYER_STATE_INIT"
    };
    YK.m3u8src = function(a, b) {
        var c = "http://v.youku.com/player/getM3U8/vid/" + a + "/type/" + b + "/ts/" + parseInt((new Date).getTime() / 1e3);
        return  (c += "/useKeyFrame/0"), c += "/v.m3u8"
    };
    YK.m3u8src_v2 = function(a, b) {
        var c = {
            vid: a,
            type: b,
            ts: parseInt((new Date).getTime() / 1e3),
            keyframe: YKP.isIPHONE ? 0: 1
        };
        YK.password && (c.password = YK.password), YK.password && YK.initConfig.client_id && YK.config.partner_config && 1 == YK.config.partner_config.status && 1 == YK.config.partner_config.passless && (c.client_id = YK.initConfig.client_id);
        var d = [19, 1, 4, 7, 30, 14, 28, 8, 24, 17, 6, 35, 34, 16, 9, 10, 13, 22, 32, 29, 31, 21, 18, 3, 2, 23, 25, 27, 11, 20, 5, 15, 12, 0, 33, 26], e = encodeURIComponent(encode64(rc4(translate(YK.mk.a4 + "poz" + YKP.userCache.a2, d).toString(), YKP.userCache.sid + "_" + a + "_" + YKP.userCache.token)));
        c.ep = e, c.sid = YKP.userCache.sid, c.token = YKP.userCache.token, c.ctype = "12", c.ev = "1", c.oip = YK.v.data.security.ip;
        var f = "http://pl.youku.com/playlist/m3u8?" + urlParameter(c);
        return f;
    };
    YK.mk = {}, YK.mk.a3 = "b4et", void(YK.mk.a4 = "boa4")
    function decode64(a) {
        if (!a)
            return "";
        a = a.toString();
        var b, c, d, e, f, g, h, i = new Array( - 1, - 1, - 1, - 1, - 1, - 1, - 1, - 1, - 1, - 1, - 1, - 1, - 1, - 1, - 1, - 1, - 1, - 1, - 1, - 1, - 1, - 1, - 1, - 1, - 1, - 1, - 1, - 1, - 1, - 1, - 1, - 1, - 1, - 1, - 1, - 1, - 1, - 1, - 1, - 1, - 1, - 1, - 1, 62, - 1, - 1, - 1, 63, 52, 53, 54, 55, 56, 57, 58, 59, 60, 61, - 1, - 1, - 1, - 1, - 1, - 1, - 1, 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25, - 1, - 1, - 1, - 1, - 1, - 1, 26, 27, 28, 29, 30, 31, 32, 33, 34, 35, 36, 37, 38, 39, 40, 41, 42, 43, 44, 45, 46, 47, 48, 49, 50, 51, - 1, - 1, - 1, - 1, - 1);
        for (g = a.length, f = 0, h = ""; g > f;) {
            do
                b = i[255 & a.charCodeAt(f++)];
            while (g > f&&-1 == b);
            if ( - 1 == b)
                break;
            do
                c = i[255 & a.charCodeAt(f++)];
            while (g > f&&-1 == c);
            if ( - 1 == c)
                break;
            h += String.fromCharCode(b<<2 | (48 & c)>>4);
            do {
                if (d = 255 & a.charCodeAt(f++), 61 == d)
                    return h;
                d = i[d]
            }
            while (g > f&&-1 == d);
            if ( - 1 == d)
                break;
            h += String.fromCharCode((15 & c)<<4 | (60 & d)>>2);
            do {
                if (e = 255 & a.charCodeAt(f++), 61 == e)
                    return h;
                e = i[e]
            }
            while (g > f&&-1 == e);
            if ( - 1 == e)
                break;
            h += String.fromCharCode((3 & d)<<6 | e)
        }
        return h
    }
    function rc4(a, b) {
        for (var c, d = [], e = 0, f = "", g = 0; 256 > g; g++)
            d[g] = g;
        for (g = 0; 256 > g; g++)
            e = (e + d[g] + a.charCodeAt(g%a.length))%256, c = d[g], d[g] = d[e], d[e] = c;
        g = 0, e = 0;
        for (var h = 0; h < b.length; h++)
            g = (g + 1)%256, e = (e + d[g])%256, c = d[g], d[g] = d[e], d[e] = c, f += String.fromCharCode(b.charCodeAt(h)^d[(d[g] + d[e])%256]);
        return f
    }
    function translate(a, b) {
        for (var c = [], d = 0; d < a.length; d++) {
            var e = 0;
            e = a[d] >= "a" && a[d] <= "z" ? a[d].charCodeAt(0) - "a".charCodeAt(0) : a[d] - "0" + 26;
            for (var f = 0; 36 > f; f++)
                if (b[f] == e) {
                    e = f;
                    break
                }
            e > 25 ? c[d] = e - 26 : c[d] = String.fromCharCode(e + 97)
        }
        return c.join("")
    }

    var encode64 = function(a) {
        if (!a)
            return "";
        a = a.toString();
        var b, c, d, e, f, g, h = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/";
        for (d = a.length, c = 0, b = ""; d > c;) {
            if (e = 255 & a.charCodeAt(c++), c == d) {
                b += h.charAt(e>>2), b += h.charAt((3 & e)<<4), b += "==";
                break
            }
            if (f = a.charCodeAt(c++), c == d) {
                b += h.charAt(e>>2), b += h.charAt((3 & e)<<4 | (240 & f)>>4), b += h.charAt((15 & f)<<2), b += "=";
                break
            }
            g = a.charCodeAt(c++), b += h.charAt(e>>2), b += h.charAt((3 & e)<<4 | (240 & f)>>4), b += h.charAt((15 & f)<<2 | (192 & g)>>6), b += h.charAt(63 & g)
        }
        return b
    }
    var urlParameter = function(a) {
        var b = [];
        for (var c in a)
            b.push(c + "=" + a[c]);
        return b.join("&")
    }

    $.ajax(
        {
            type:'get',
            url : BuildVideoInfo._url,
            dataType : 'jsonp',
            jsonp:"callback",
            success  : function(data) {
            },
            error : function(request, msg, e) {
            }
        }
    );
    return "requestJS";
}
```

### 坑2 - 如何调用这段 js 代码
使用`webView`的`stringByEvaluatingJavaScript(from:)` 调用, 并且传入解析出来的视频`v_id`
注: js中传入的字符串参数需要使用单引号 `' '` 括起来

### 坑3 - 如何获取到 js 中解析出来的 url
`js`中是异步解析`url` , 不能立即返回结果.
解决办法: 在`js`中获取到`URL`后, 使用`location.href = url`资源重定向, 然后在`webView` 的代理方法`webView(_ webView: UIWebView, shouldStartLoadWith request: URLRequest, navigationType: UIWebViewNavigationType) -> Bool`中根据自己定义的规则进行拦截, 最后将获取到的`URL`回调出去.

### 坑3 - tableView上播放视屏, 会出现复用问题
说明: 项目中的视频播放使用了库[BMPlayer](https://github.com/BrikerMan/BMPlayer), 感谢开源社区的贡献.
因为tableViewCell可以复用, 所以滑动后, 会有多个`cell`同时播放视频
解决办法: 在`tableView`的代理方法`tableView(_ tableView: UITableView, willDisplay cell: UITableViewCell, forRowAt indexPath: IndexPath) {}`中移除不复用`cell`上的视频播放器
```
func tableView(_ tableView: UITableView, willDisplay cell: UITableViewCell, forRowAt indexPath: IndexPath) {
        guard let videoCell = cell as? VideoCell else {
            return
        }
        let view = videoCell.bgView.subviews.last
        
        if currentIndexPath == indexPath {
            // 当前cell需要播放视屏
            if (view as? BMPlayer) == nil, player.avPlayer != nil {
                videoCell.bgView.addSubview(player)
                player.snp.remakeConstraints { (make) in
                    make.edges.equalTo(videoCell.imgV)
                }
            }
        } else {
            // 该cell为复用的cell, 需要移除播放器
            if let playerV = view as? BMPlayer {
                playerV.removeFromSuperview()
            }
        }
    }
```

### 坑4 - 播放某一个视频后, 滑动 tableView , 正在播放视频的 Cell 虽然已经不在屏幕上显示了, 但是视频仍在继续播放
解决办法: 在`tableView`的代理方法`tableView(_ tableView: UITableView, willDisplay cell: UITableViewCell, forRowAt indexPath: IndexPath) {}`中加入如下代码, 如果当前`Cell` 不在显示, 则暂停播放
```
       if let visibleIndexPath = tableView.indexPathsForVisibleRows,visibleIndexPath.contains(currentIndexPath) {
            if !player.isPlaying {
                player.play()
            }
        } else {
            player.pause()
        }
```

### 坑5 - tableView 的点击代理方法中, 不能使用同步的弹出框
注: 弹出框的同步调用可以参考我的这篇文章[iOS同步调用对话框 RunLoop的使用](http://www.jianshu.com/p/95df9f07b5f2)

网络状态监控, 使用到了库[ReachabilitySwift](https://github.com/ashleymills/Reachability.swift), 感谢开源社区的贡献.

当用户点击播放视频时, 需要判断当前用户的网络状态. 如果使用的是`WiFi`则可以播放视频; 如果使用的是移动网络, 则需要提示用用户"当前正在使用移动网络, 是否继续播放?".

`tableView`的点击代理方中使用此方式, 对话框弹不出来, 并且界面处于阻塞状态, 退回桌面, 再次进入APP, 出现弹出框; 点击取消或确定, 同样不能结束对话框, 退回桌面, 再次进入APP, 对话框才消失.

解决办法: 在`tableView`的 `cell`上加按钮, 通过点击按钮的回调使用同步对话框.

### 坑6 - 视频的全屏播放
点击全屏播放视频, 需要模态出一个全屏播放的控制器.
在`window`的主控制器中, 把所有页面的状态都设置为横屏
```
override var supportedInterfaceOrientations: UIInterfaceOrientationMask {
        return UIInterfaceOrientationMask.portrait
    }
```
在模态出的全屏播放控制器, 旋转屏幕
```
override var supportedInterfaceOrientations: UIInterfaceOrientationMask {
        return [.landscapeRight, .landscapeLeft]
    }
```
更多屏幕旋转的知识请参考[如何用代码控制以不同屏幕方向打开新页面【iOS】](https://lvwenhan.com/ios/458.html)

### 坑7 - tableView 的头部下拉放大效果实现
网上参考教程很多, 不再赘述

# 总结
项目中还有很多bug, 及很多待优化的地方; `Nunchakus `作为自己练手的一个小项目, 我会对它持续改进, 并添加一些其他的功能.另外希望各路大神可以提一些改进的意见或建议.

# 还有------就是项目git地址在这[猛戳这里](https://github.com/YQqiang/Nunchakus)

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/


