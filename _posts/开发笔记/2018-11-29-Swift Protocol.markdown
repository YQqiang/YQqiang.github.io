---
layout: post
title:  "Swift Protocol"
date:   2018-11-29 15:29:31 +0800
categories: 开发笔记
---

![](http://yuqiangcoder.com/assets/postImages/ios/201811/8.jpg)

## 协议?
> 协议，规定了用来实现某一特定任务或者功能的方法、属性，以及其他需要的东西。类、结构体或枚举都可以遵循协议，并为协议定义的这些要求提供具体实现。

如果某个具体类型想要遵守一个协议，那它需要实现这个协议所定义的所有这些内容

## Swift Protocol 语法
1. 协议定义
    
    * `protocol` 关键字
    * `SomeProtocol` 自定义协议的名称

    ```swift
    protocol SomeProtocol {
        // 协议定义部分
    }
    ```

2. 自定义类型遵循协议

    ```swift
    protocol FirstProtocol {
    }
    
    protocol SecondProtocol {
    }
    
    /// 在定义类型时，需要在类型名称后加上协议名称，中间以冒号（:）分隔
    class ClassA: FirstProtocol {
    }
    
    /// 遵循多个协议时，各协议之间用逗号（,）分隔
    struct StructA: FirstProtocol, SecondProtocol {
    }
    
    class SuperClass {
    }
    
    /// 拥有父类的类在遵循协议时，应该将父类名放在协议名之前，以逗号分隔：
    class SubClass: SuperClass, FirstProtocol, SecondProtocol {
    }
    ```
    
3. 属性要求
    协议总是用 `var` 关键字来声明变量属性
    
    * 可读可写属性 `{ set get }`
    * 只读属性 `{ get }`
    
    ```swift
    protocol SomeProtocol {
        // 协议定义部分
        
        /// 在类型声明后加上 `{ set get }` 来表示属性是可读可写的
        var mustBeSettable: Int { set get}
        
        /// 可读属性则用 `{ get }` 来表示
        var doesNotNeedToBeSettable: String { get }
    }
    
    class ClassIM: SomeProtocol {
        var mustBeSettable: Int = 0
        var doesNotNeedToBeSettable: String {
            return "\(mustBeSettable)"
        }
    }
    ```
    
    * `class` 修饰的类属性, 只能是 类 `class` 类型遵循该协议
    * `static` 修饰的类属性, 可以是 类 `class` 或 结构体 `struct`等类型遵循该协议
    
    ```swift
    protocol ProtocolD {
        /// 协议中定义类型属性时，总是使用 static 关键字作为前缀
        static var staticProperty: String { get set }
        
        /// 当类类型遵循协议时，除了 static 关键字，还可以使用 class 关键字来声明类型属性
    //    class var staticProperty: String { get set }
    }
    
    struct StructD: ProtocolD {
        static var staticProperty: String = ""
    }
    
    class ClassD: ProtocolD {
        static var staticProperty: String = "D"
    }
    ```
    
4. 方法要求
    协议可以要求遵循了协议的具体类型, 实现一些指定的实例方法和类方法;
    这些方法放在协议的定义中, 不需要大括号和方法体;
    不能为协议中方法的参数提供默认值.
    
    * 类方法 
        * 在协议中声明使用`static`关键字做前缀;
        * 具体实现时,当<font color=red>类</font>类型遵循协议时,还可使用`class`关键字
        
       ```swift
        protocol ProtocolE {
            static func staticFunc() -> Void
        }
        
        struct StructE: ProtocolE {
            static func staticFunc() {
                print("\(self)--\(#function)")
            }
        }
        
        class ClassE: ProtocolE {
            class func staticFunc() {
                print("\(self)--\(#function)")
            }
        }
        
        StructE.staticFunc()
        ClassE.staticFunc()
       ```
        
    * 实例方法
        
        ```swift
        protocol ProtocolF {
            func instanceFunc() -> String
        }
        
        struct structF: ProtocolF {
            func instanceFunc() -> String {
                return "\(#function)"
            }
        }
        ```
        
    * `mutating`方法
        在方法中改变方法所属的实例;
        在值类型（即结构体和枚举）的实例方法中，将 `mutating` 关键字作为方法的前缀，
        写在 `func` 关键字之前，表示可以在该方法中修改它所属的实例以及实例的任意属性的值。
        
        > 实现协议中的 `mutating` 方法时, 
        > 若是 **类** 类型, 则<font color=red>不用写</font> `mutating` 关键字
        > 对于 **结构体** 和 **枚举**, 则<font color=red>必须写</font> `mutating` 关键字
        
        ```swift
        protocol ProtocolG {
            mutating func changeState()
        }
        
        enum StateG {
            case on
            case off
        }
        
        struct StructG: ProtocolG {
            
            var state: StateG = .off
            
            /// 结构体或枚举实现 `mutating` 方法
            /// 必须加关键字 `mutating`
            mutating func changeState() {
                if state == .off {
                    state = .on
                } else {
                    state = .off
                }
            }
        }
        
        class ClassG: ProtocolG {
            
            var state: StateG = .off
            
            /// 类 实现 `mutating` 方法
            /// 不用加关键字
            func changeState() {
                if state == .off {
                    state = .on
                } else {
                    state = .off
                }
            }
        }
        ```
        
    * 构造器
        协议可以要求遵循协议的类型实现指定的构造器。
        你可以像编写普通构造器那样，在协议的定义里写下构造器的声明，
        但不需要写花括号和构造器的实体
        
        ```swift
        protocol ProtocolH {
            init(someValue: Int)
        }
        
        class ClassH: ProtocolH {
            
            required init(someValue: Int) {
                
            }
        }
        
        final class ClassH2: ProtocolH {
            init(someValue: Int) {
                
            }
        }
        
        ```
        
        * 使用 `required` 修饰符可以确保所有子类也必须提供此构造器实现，从而也能符合协议
        * 如果类已经被标记为 `final`，那么不需要在协议构造器的实现中使用 `required` 修饰符，因为 `final` 类不能有子类          

        > 如果一个子类重写了父类的指定构造器，
        > 并且该构造器满足了某个协议的要求，
        > 那么该构造器的实现需要同时标注 `required` 和 `override` 修饰符
        
        ```swift
        protocol ProtocolH {
            init(someValue: Int)
        }
        
        class ClassH {
            init(someValue: Int) {
                
            }
        }
        
        class ClassSubH: ClassH, ProtocolH {
            required override init(someValue: Int) {
                super.init(someValue: someValue)
            }
        }
        ```
    
5. 协议作为类型
   协议可以像其他普通类型一样使用
   
   * 作为函数、方法或构造器中的参数类型或返回值类型
   * 作为常量、变量或属性的类型
   * 作为数组、字典或其他容器中的元素类型
    
    > 协议是一种类型，因此协议类型的名称应与其他类型（例如 Int，Double，String）的写法相同，使用大写字母开头的驼峰式写法
    
6. 在扩展中遵循协议
    即便无法修改源代码，依然可以通过扩展令已有类型遵循并符合协议。
    扩展可以为已有类型添加属性、方法、下标以及构造器，因此可以符合协议中的相应要求
    
    > 当一个类型已经符合了某个协议中的所有要求，却还没有声明采纳该协议时，可以通过空扩展体的扩展采纳该协议
    
    ```swift
    protocol ProtocolI {
        var protocolValue: String { set get}
        func desc() -> String
    }
    
    class ClassI {
        var classValue: String = ""
    }
    
    extension ClassI: ProtocolI {
        
        var protocolValue: String {
            set {
                classValue = newValue
            }
            
            get {
               return classValue
            }
        }
        
        func desc() -> String {
            return "desc = \(protocolValue)"
        }
    }
    
    let classI = ClassI()
    classI.protocolValue = "TEST"
    classI.desc()
    ```
    
7. 有条件地遵循协议
    泛型类型可能只在某些情况下满足一个协议的要求，
    比如当类型的泛型形式参数遵循对应协议时。
    你可以通过在扩展类型时列出限制让泛型类型有条件地遵循某协议。
    在你采纳协议的名字后面写泛型 `where` 分句
    
    ```swift
    extension Array: ProtocolI where Element: ProtocolI {
        var protocolValue: String {
            set {
                
            }
            
            get {
                return "TEST"
            }
        }
        
        func desc() -> String {
            let values = map({$0.protocolValue})
            return "\(protocolValue): \(values.joined(separator: ","))"
        }
    }
    
    let classI1 = ClassI()
    classI1.protocolValue = "one"
    
    let classI2 = ClassI()
    classI2.protocolValue = "two"
    
    [classI1, classI2].desc()
    ```
    
8. 协议类型的集合
    协议类型可以在数组或者字典这样的集合中使用
    
    ```swift
    protocol ProtocolJ {
        var testString: String { get }
    }
    
    class ClassJ1: ProtocolJ {
        var testString: String {
            return "ClassJ1"
        }
    }
    
    class ClassJ2: ProtocolJ {
        var testString: String {
            return "ClassJ2"
        }
    }
    
    let protocolJs: [ProtocolJ] = [ClassJ1(), ClassJ2()]
    protocolJs.forEach { print("\($0.testString)") }
    ```
    
9. 协议的继承
    协议能够继承一个或多个其他协议，
    可以在继承的协议的基础上增加新的要求。
    协议的继承语法与类的继承相似，多个被继承的协议间用逗号分隔
    
    ```swift
    protocol ProtocolHJ: ProtocolH, ProtocolJ {
        // 协议定义部分
    }
    ```
    
10. 类专属的协议
    通过添加关键字: `class`
    当协议定义的要求需要遵循协议的类型必须是引用语义而非值语义时，应该采用类类型专属协议
    
    ```swift
    protocol ProtocolK {
        
    }
    
    protocol ClassPortocolK: class, ProtocolK {
        
    }
    
    protocol ProtocolM: class {
        
    }   
    ```
    
11. 协议合成
    要求一个类型同时遵循多个协议
    协议组合使用 `SomeProtocol & AnotherProtocol` 的形式。用和符号（`&`）分开。
    
    ```swfit 
    protocol NameProtocol {
        var name: String { get }
    }
    
    protocol AgeProtocol {
        var age: Int { get }
    }
    
    struct NameAgeStruct: NameProtocol, AgeProtocol {
        var name: String
        var age: Int
    }
    
    func logNameAge(_ value: NameProtocol & AgeProtocol) {
        print("name = \(value.name)")
        print("age = \(value.age)")
    }
    
    let nameAgeStruct = NameAgeStruct(name: "yuqiang", age: 24)
    logNameAge(nameAgeStruct)
    ```
    
12. 检查协议一致性
    使用 `is` 和 `as` 来检查协议一致性
    检查和转换到某个协议类型在语法上和类型的检查和转换完全相同
    
    * `is` 用来检查实例是否符合某个协议，若符合则返回 `true`，否则返回 `false`
    * `as?` 返回一个可选值，当实例符合某个协议时，返回类型为协议类型的可选值，否则返回 `nil`
    * `as!` 将实例强制向下转换到某个协议类型，如果强转失败，会引发运行时错误

13. 可选的协议要求
    在协议中使用 `optional` 关键字作为前缀来定义可选要求。
    可选要求用在你需要和 `Objective-C` 打交道的代码中。
    协议和可选要求都必须带上 `@objc` 属性。
    标记 `@objc` 特性的协议只能被继承自 `Objective-C` 类的类或者 `@objc` 类遵循，其他类以及结构体和枚举均不能遵循这种协议。
    
    ```swift
    @objc protocol protocolN {
        @objc optional var optionalValue: String { get }
    }
    
    class ClassN: protocolN {
        
    }
    ```
    
14. 协议扩展
    协议可以通过扩展来为遵循协议的类型提供属性、方法以及下标的实现。
    通过这种方式，你可以基于协议本身来实现这些功能，
    而无需在每个遵循协议的类型中都重复同样的实现，也无需使用全局函数
    
    ```swift
    protocol RandomNumberProtocol {
        
    }
    
    extension RandomNumberProtocol {
        func randomNumber() -> Int {
            return Int(arc4random_uniform(100))
        }
    }
    
    class RandomNumberClass: RandomNumberProtocol {
        
    }
    
    let randomNumberClass = RandomNumberClass()
    randomNumberClass.randomNumber()
    ```
    
15. 为协议扩展添加限制条件
    限制条件写在协议名之后，使用 `where` 子句来描述
    
    ```swift
    struct TestStruct {
        var value: String = ""
    }
    
    extension TestStruct: Equatable {
        public static func == (lhs: TestStruct, rhs: TestStruct) -> Bool {
            return lhs.value == rhs.value
        }
    }
    
    extension Collection where Element: Equatable {
        func allEqual() -> Bool {
            for element in self {
                if element != self.first {
                    return false
                }
            }
            return true
        }
    }
    
    let testStruct1 = TestStruct(value: "a")
    let testStruct2 = TestStruct(value: "a")
    
    [testStruct1, testStruct2].allEqual()
    ```
    
## 通用协议
> Swift 语言是类型安全的, 即必须在编译之前定义类型.

* 熟悉泛型

    * 显式声明
    * 隐式推断
    
    ```swift
    struct GenericStruct<T> {
        var property: T?
    }
    /// 明确说明 `T` 的类型 为 `Bool`
    let explictStruct = GenericStruct<Bool>()
    
    /// 让 Swift 根据值 推断类型 `T` 为 `String`
    let implicitStruct = GenericStruct(property: "yq")
    ```
* 协议关联类型
    在通用协议中, 要创建 `<T>` 泛型的东西, 需要添加 `associatedtype`
    
    ```swift
    protocol GenericProtocol {
        associatedtype GenericType
        var genericProperty: GenericType { get set }
    }
    ```
    
    > 关联类型 = 类型别名 + 泛型
    
    现在, 任何遵循该协议的类或结构体都必须对类型定义
    
   * 显式定义
   * 隐式定义
    
   ```swift
   /// 显式定义关联类型
   class SomeClass: GenericProtocol {
       typealias GenericType = String
       
       var genericProperty: GenericType = "qiang"
   }
   
   /// 隐式定义关联类型
   struct SomeStruct: GenericProtocol {
       var genericProperty = 1995
   }
   ```

* 协议扩展
    协议扩展提供了默认实现, 无需定义所需的方法和属性
    
    ```swift
    extension GenericProtocol {
        func testProtocolExtension() {
            print("\(#function)")
        }
    }
    ```
    
* 类型约束
    指定为某种类型添加扩展
    
    ```swift
    /// 当 GenericType 为 `String` 类型时,该扩展才有效
    extension GenericProtocol where GenericType == String {
        func testStringType() {
            print("\(#function)")
        }
    }
    
    /// 当 GenericType 为 `Int` 类型时,该扩展才有效
    extension GenericProtocol where GenericType == Int {
        func testIntType() {
            print("\(#function)")
        }
    }
    
    /// SomeClass 定义了 GenericType 类型为 `String`
    /// 因此可以调用 `testStringType()`
    let someClass = SomeClass()
    someClass.testStringType()
    
    /// SomeStruct 定义了 GenericType 类型为 `Int`
    /// 因此可以调用 `testIntType()`
    let someStruct = SomeStruct()
    someStruct.testIntType()
    ```

* 类型预定义
    在协议中预先定义类型
    
    ```swift
    /// 类型预定义
    protocol FirstNameProtocol {
        associatedtype FirstNameType = Int
        func fullName() -> FirstNameType
    }
    
    struct NormalName: FirstNameProtocol {
        func fullName() -> Int {
            return 1
        }
    }
    ```
    
* 覆盖关联类型
    在具体遵循的类或结构体中覆盖预定义的关联类型
    
    ```swift
    /// 覆盖关联类型
    struct OverName: FirstNameProtocol {
        func fullName() -> String {
            return "yu"
        }
    }
    
    let overName = OverName()
    overName.fullName()
    ```
    
## 静态派发 & 动态派发
> “协议要求的方法是动态派发的，而仅定义在扩展中的方法是静态派发的。”

## 迭代器协议
> 迭代器每次产生一个序列的值，并且当遍历序列时对遍历状态进行管理。
在 `IteratorProtocol` 协议中唯一的一个方法是`next()`，这个方法需要在每次被调用时返回序列中的下一个值。
当序列被耗尽时，`next()` 应该返回 `nil`

```swift
protocol IteratorProtocol {
    associatedtype Element
    mutating func next() -> Element?
}
```

## 序列协议
> `Sequence`协议是集合类型结构中的基础.一个序列(`sequence`)代表的是一系列具有相同类型的值, 你可以对这些值进行迭代.

```swift
public protocol Sequence {
    associatedtype Element
    associatedtype Iterator: IteratorProtocol
    where Iterator.Element == Element
    // ...
}
```

## 集合协议
> `Collection` 协议是建立在 `Sequence` 协议上的。

```swift
protocol Collection: Sequence {
	associatedtype Element // inherited from Sequence
	associatedtype Index: Comparable
	associatedtype IndexDistance: SignedInteger = Int
	associatedtype Iterator: IteratorProtocol = IndexingIterator<Self>
	where Iterator.Element == Element
	associatedtype SubSequence: Sequence
	/* ... */
	associatedtype Indices: Sequence = DefaultIndices<Self>
	/* ... */
	var first: Element? { get }
	var indices: Indices { get }
	var isEmpty: Bool { get }
	var count: IndexDistance { get }
	func makeIterator() -> Iterator
	func prefix(through: Index) -> SubSequence
	func prefix(upTo: Index) -> SubSequence
	func suffix(from: Index) -> SubSequence
	func distance(from: Index, to: Index) -> IndexDistance
	func index(_: Index, offsetBy: IndexDistance) -> Index
	func index(_: Index, offsetBy: IndexDistance, limitedBy: Index) -> Index?
	subscript(position: Index) -> Element { get }
	subscript(bounds: Range<Index>) -> SubSequence { get }
}
```



待继续...

## 参考
[极客学院-协议](http://wiki.jikexueyuan.com/project/swift/chapter2/21_Protocols.html)

[Generic Protocols with Associated Type](https://www.bobthedeveloper.io/blog/generic-protocols-with-associated-type)

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/

