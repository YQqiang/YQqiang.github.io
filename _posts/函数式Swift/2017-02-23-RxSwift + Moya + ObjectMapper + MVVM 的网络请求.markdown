---
layout: post
title:  "RxSwift + Moya + ObjectMapper + MVVM 的网络请求"
date:   2017-02-23 10:17:31 +0800
categories: 函数式Swift
---
![](http://yuqiangcoder.com/assets/postImages/ios/201702/13.jpg)

> 2016年底的时候, 项目经理决定把我们的项目以`MVVM`架构方式进行重构; 当时`Swift`已经是3.0版本了, 也是时候转战Swift了.  之所以选择使用`RxSwift`实现`MVVM`, 而没有使用 `RAC`, 是因为我觉得`RxSwift`更符合`Swift`的编程思想.

---

# 首先送上 ***Demo*** 地址
[Demo地址](https://github.com/YQqiang/MoyaDemo)

---

# MVVM浅析
| M| Model|负责数据层|
|:-:|:-:|:-:|
| V| ViewController|负责View|
| VM| ViewModel|负责业务逻辑|
* `ViewModel` 负责网络请求和数据解析, 可以再抽出一层网络请求层`APIService`层; 
* `ViewModel`负责把数据解析为对应的`Model`;
*  `ViewController`从`ViewModel`中读取数据;
* `ViewController` 和 `Model` 之间是不接触的, 相当于`ViewModel`是它们之间的桥梁.

---

# 构建 ***Service*** 层
1. 新建一个`enum` 遵循 `TargetType`协议, 枚举的`case`值, 为每一个接口
```
enum AppService: TargetType {
    case login(username: String, pwd: String)
    case video
}
```

2. 在枚举的扩展中定义一个网络请求的必要参数
```
extension AppService {
    var baseURL: URL {
        return URL(string: API_PRO)!
    }
    
    var path: String {
        switch self {
        case .login(username: _, pwd: _):
            return "/login"
        case .video:
            return "/video"
        }
    }
    
    var method: Moya.Method {
        switch self {
        case .login(username: _, pwd: _):
            return .get
        case .video:
            return .post
        }
    }
    
    var parameters: [String: Any]? {
        switch self {
        case .login(username: let username, pwd: let pwd):
            return ["username": username, "pwd": pwd]
        case .video:
            return ["type": "JSON"]
        }
    }
    
    var parameterEncoding: ParameterEncoding {
        return URLEncoding.default
    }
    
    var sampleData: Data {
        return "".data(using: String.Encoding.utf8)!
    }
    
    var task: Task {
        return .request
    }
}
```

3. 可以把`baseUrl` , 请求头, 公共参数, 定义在一个单独的文件中;
```
let API_PRO = "http://120.25.226.186:32812"
let headerFields: [String: String] = ["system": "iOS","sys_ver": String(UIDevice.version())]
let publicParameters: [String: String] = ["language": "_zh_CN"]
```

4. 创建`RxMoyaProvider`用于发送网络请求, 可以在创建的时候传入请求头和公共参数
```
let appServiceProvider = RxMoyaProvider<AppService>.init()
```

---

# 构建 ***Model*** 层
使用`ObjectMapper`库转模型
1. 登录接口的`Model`
```
class LoginModel: Mappable {
    var error: String?
    var success: String?
    
    required init?(map: Map) {
    }
    
    func mapping(map: Map) {
        error <- map["error"]
        success <- map["success"]
    }
}
```

2. 保存视频信息的`Model`;
```
class VideoModel: Mappable {
    var videos: [Video]?
    
    required init?(map: Map) {
    }
    
    func mapping(map: Map) {
        videos <- map["videos"]
    }
}
 ```

  ```
  class Video: Mappable {
    var id: String?
    var length: Float?
    var name: String?
    var url: String?
    
    required init?(map: Map) {
    }
    
    func mapping(map: Map) {
        id <- map["id"]
        length <- map["length"]
        name <- map["name"]
        url <- map["url"]
    }
}
  ```
---

# 构建 ***ViewModel*** 层 
`ViewModel`层发送网络请求, 获取数据, 获取到的数据保存在`Model`中, 通过回调刷新UI, 并且返回一个可观察者.
```
class ViewModel {
    
    func login(username: String, pwd: String) -> Observable<LoginModel> {
        return appServiceProvider.request(.login(username: username, pwd: pwd))
            .filterSuccessfulStatusCodes()
            .mapJSON()
            .showAPIErrorToast()
            .mapObject(type: LoginModel.self)
    }
    
    func video() -> Observable<VideoModel> {
        return appServiceProvider.request(.video)
            .filterSuccessfulStatusCodes()
            .mapJSON()
            .showAPIErrorToast()
            .mapObject(type: VideoModel.self)
    }
}
```

函数中的`showAPIErrorToast()`是自己定义的一个`Observable`的扩展函数, 用于在网络请求错误的时候需要做的一些操作
```
extension Observable {
    func showAPIErrorToast() -> Observable<Element> {
        return self.do(onNext: { (event) in
        }, onError: { (error) in
            // TODO: 可以在此处做一些网络错误的时候的提示信息
            print("\(error.localizedDescription)")
        }, onCompleted: {
        }, onSubscribe: {
        }, onDispose: {
        })
    }
}
```

函数中的 `mapObject(type:)` 是自定义的`Observable`扩展函数, 用于`json` 转模型, 需要自己根据实际项目中的`json`格式, 做相应的解析.
```
extension Observable {
    func mapObject<T: Mappable>(type: T.Type) -> Observable<T> {
        return self.map { response in
            //if response is a dictionary, then use ObjectMapper to map the dictionary
            //if not throw an error
            guard let dict = response as? [String: Any] else {
                throw RxSwiftMoyaError.ParseJSONError
            }
            return Mapper<T>().map(JSON: dict)!
        }
    }
    
    func mapArray<T: Mappable>(type: T.Type) -> Observable<[T]> {
        return self.map { response in
            //if response is an array of dictionaries, then use ObjectMapper to map the dictionary
            //if not, throw an error
            guard let array = response as? [[String: Any]] else {
                throw RxSwiftMoyaError.ParseJSONError
            }
            return Mapper<T>().mapArray(JSONArray: array)!
        }
    }
}
```

`RxSwiftMoyaError`是自定义的一个错误类型枚举值, 可以返回一些错误信息, 用在`showAPIErrorToast()`中提示用户的信息
```
enum RxSwiftMoyaError : Swift.Error {
    case ParseJSONError
    case NoRepresentor
    case NotSuccessfulHTTP
    case NoData
    case CouldNotMakeObjectError
    case BizError(resultCode: String, resultMsg: String)
}

extension RxSwiftMoyaError: LocalizedError {
    public var errorDescription: String? {
        switch self {
        case .ParseJSONError:
            return "数据解析失败"
        case .NoRepresentor:
            return "NoRepresentor."
        case .NotSuccessfulHTTP:
            return "NotSuccessfulHTTP."
        case .NoData:
            return "NoData."
        case .CouldNotMakeObjectError:
            return "CouldNotMakeObjectError."
        case .BizError(resultCode: let resultCode, resultMsg: let resultMsg):
            return "错误码: \(resultCode), 错误信息: \(resultMsg)"
        }
    }
}
```

---

# ***ViewController*** 中就可以直接操作 ***viewModel*** 发送网络请求, 并且获取到模型数据
`ViewController` 中包含一个  `ViewModel` 对象, `View`需要变化的时候, 直接让这个对象调用自己的函数, 获取到`Model`数据, 刷新UI
```
let disposeBag = DisposeBag()
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        let viewModel = ViewModel()
        viewModel.login(username: "520it", pwd: "520it").subscribe(onNext: { (loginModel) in
            print("---\(loginModel.success)")
        }).addDisposableTo(disposeBag)
        
        viewModel.video().subscribe(onNext: { (videoModel) in
            guard let videos = videoModel.videos else {
                return
            }
            for video in videos {
                print("----id:\(video.id)---length:\(video.length)---name:\(video.name)---url:\(video.url)")
            }
        }).addDisposableTo(disposeBag)
    }
```
结果如下所示: 
```
---Optional("登录成功")
----id:nil---length:Optional(10.0)---name:Optional("小黄人 第01部")---url:Optional("resources/videos/minion_01.mp4")
----id:nil---length:Optional(12.0)---name:Optional("小黄人 第02部")---url:Optional("resources/videos/minion_02.mp4")
----id:nil---length:Optional(14.0)---name:Optional("小黄人 第03部")---url:Optional("resources/videos/minion_03.mp4")
----id:nil---length:Optional(16.0)---name:Optional("小黄人 第04部")---url:Optional("resources/videos/minion_04.mp4")
----id:nil---length:Optional(18.0)---name:Optional("小黄人 第05部")---url:Optional("resources/videos/minion_05.mp4")
----id:nil---length:Optional(20.0)---name:Optional("小黄人 第06部")---url:Optional("resources/videos/minion_06.mp4")
----id:nil---length:Optional(22.0)---name:Optional("小黄人 第07部")---url:Optional("resources/videos/minion_07.mp4")
----id:nil---length:Optional(24.0)---name:Optional("小黄人 第08部")---url:Optional("resources/videos/minion_08.mp4")
----id:nil---length:Optional(26.0)---name:Optional("小黄人 第09部")---url:Optional("resources/videos/minion_09.mp4")
----id:nil---length:Optional(28.0)---name:Optional("小黄人 第10部")---url:Optional("resources/videos/minion_10.mp4")
----id:nil---length:Optional(30.0)---name:Optional("小黄人 第11部")---url:Optional("resources/videos/minion_11.mp4")
----id:nil---length:Optional(32.0)---name:Optional("小黄人 第12部")---url:Optional("resources/videos/minion_12.mp4")
----id:nil---length:Optional(34.0)---name:Optional("小黄人 第13部")---url:Optional("resources/videos/minion_13.mp4")
----id:nil---length:Optional(36.0)---name:Optional("小黄人 第14部")---url:Optional("resources/videos/minion_14.mp4")
----id:nil---length:Optional(38.0)---name:Optional("小黄人 第15部")---url:Optional("resources/videos/minion_15.mp4")
----id:nil---length:Optional(40.0)---name:Optional("小黄人 第16部")---url:Optional("resources/videos/minion_16.mp4")
```

到此, 我们已经完成了使用`RxSwift` 进行` MVVM` 架构, 以及使用`Moya` 封装网络请求层, 和使用 `ObjectMapper` 进行 `json` 转模型. 不知道小伙伴有没有感觉到 使用 `Swift` 编程, 的确是很优雅呢?

---

#  写在最后
[Demo地址](https://github.com/YQqiang/MoyaDemo)
> 自己写的博客, 内容略显粗浅, 大家可以看下面列出的博客.   

* RxSwift
[RxSwift 中的 Observable 详解 （翻译一）](http://www.jianshu.com/p/348bbb20b435)

[RxSwift 中的 Subject 详解 （翻译二）](http://www.jianshu.com/p/07266f30ce5f)

[RxSwift 上手详解 —— 入门篇（翻译三）](http://www.jianshu.com/p/1a8531894562)

[RxSwift 中的 Units——一个富有哲学意味的概念（翻译四）](http://www.jianshu.com/p/bd8eb2f6d407)

[【iOS开发】RxSwift入坑解读-你所需要知道的各种概念](http://www.codertian.com/2016/11/27/RxSwift-ru-keng-ji-read-document/#more)

[【iOS开发】RxSwift入坑解读-那些难以理解的细节](http://www.codertian.com/2016/12/01/RxSwift-ru-keng-ji-learn-the-difficulty/#more)

[【iOS开发】RxSwift实战教程-核心用法](http://www.codertian.com/2016/12/10/RxSwift-shi-zhan-jie-du-base-demo/#more)

* Moya 网络请求
[如何写出最简洁优雅的网络封装 Moya + RxSwift](http://www.jianshu.com/p/c1494681400b)

[【iOS开发】Moya入坑记-用法解读篇](http://www.codertian.com/2017/01/21/iOS-moya-ru-keng-usage/#more)

[【iOS开发】使用RxSwift+Moya进行优雅的网络请求](http://www.codertian.com/2017/02/04/iOS-Moya-RxSwift-better-networking/#more)

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/


