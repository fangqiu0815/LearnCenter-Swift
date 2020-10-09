# LearnCenter-Swift
LearnCenter-Swift
以下内容均为平时学习Swift时积累的笔记

1. swift中使用标记（oc中的#pragma mark）
```
// MARK: -  你的标记
...

// TODO: -  你的待办
...

// FIXME: -  待修复的内容
```

2. for...in遍历数组同时拿到下标和对应的元素
```
for (index, element) in arr.enumerated() {
    print("index: \(index), element: \(element)")
}
```
oc写法是 
```
[arr enumerateObjectsUsingBlock:^(id  _Nonnull obj, NSUInteger idx, BOOL * _Nonnull stop) {
        
}];
```
3. Int和String互相转换
```
/// Int to String
let i = 10
let str = String(i)
print("String is : \(str)")

/// String to Int
let num = Int(str)
/// 因为转换过来是可选的，所以需要先尝试解包
if let num = num {
    print("Int is : \(num)")
}
```

4.  查看本地swift版本
swift版本是和Xcode版本一一对应的，一般来说打开终端运行，云心命令就能看到：
```
xcrun swift -version
```
我的Xcode版本是11.1，运行这个命令行打印的是：

```
Apple Swift version 5.1 (swiftlang-1100.0.270.13 clang-1100.0.33.7)
Target: x86_64-apple-darwin19.0.0
```

有种情况是你的电脑上安装了多个Xcode（比如你安装了某个Beta版本），这时候可以使用以下命令修改路径：

```
sudo xcode-select -s /Applications/Xcode_Beta.app
```

5.  项目中的swift版本

项目中指定使用swfit某个版本
```
选择target -> build settings -> 搜索swift_version -> 选择指定版本
```

指定使用swfit某个版本
代码中判断swift版本

```
#if swift(>=5.2)
print("Swift版本 >= 5.2")

#elseif swift(>=5.1)
print("Swift版本 >= 5.1")

#elseif swift(>=5.0)
print("Swift版本 >= 5.0")

#elseif swift(>=4.2)
print("Swift版本 >= 4.2")

#elseif swift(>=4.1)
print("Swift版本 >= 4.1")

#elseif swift(>=4.0)
print("Swift版本 >= 4.0")

#elseif swift(>=3.2)
print("Swift版本 >= 3.2")

#elseif swift(>=3.0)
print("Swift版本 >= 3.0")

#elseif swift(>=2.2)
print("Swift版本 >= 2.2")

#elseif swift(>=2.1)
print("Swift版本 >= 2.1")

#elseif swift(>=2.0)
print("Swift版本 >= 2.0")

#elseif swift(>=1.2)
print("Swift版本 >= 1.2")

#elseif swift(>=1.1)
print("Swift版本 >= 1.1")

#elseif swift(>=1.0)
print("Swift版本 >= 1.0")

#endif
```

6.  获取当前设备型号（包括iPhone、iPad、iPod、Apple TV、HomePod和模拟器）

方法一，在ViewController中重写touchBegin方法

```
public extension UIDevice {
    static let modelName: String = {
        var systemInfo = utsname()
        uname(&systemInfo)
        let machineMirror = Mirror(reflecting: systemInfo.machine)
        let identifier = machineMirror.children.reduce("") { identifier, element in
            guard let value = element.value as? Int8, value != 0 else { return identifier }
            return identifier + String(UnicodeScalar(UInt8(value)))
        }
        func mapToDevice(identifier: String) -> String {
            #if os(iOS)
            switch identifier {
            case "iPod5,1":                                 return "iPod touch (5th generation)"
            case "iPod7,1":                                 return "iPod touch (6th generation)"
            case "iPod9,1":                                 return "iPod touch (7th generation)"
            case "iPhone3,1", "iPhone3,2", "iPhone3,3":     return "iPhone 4"
            case "iPhone4,1":                               return "iPhone 4s"
            case "iPhone5,1", "iPhone5,2":                  return "iPhone 5"
            case "iPhone5,3", "iPhone5,4":                  return "iPhone 5c"
            case "iPhone6,1", "iPhone6,2":                  return "iPhone 5s"
            case "iPhone7,2":                               return "iPhone 6"
            case "iPhone7,1":                               return "iPhone 6 Plus"
            case "iPhone8,1":                               return "iPhone 6s"
            case "iPhone8,2":                               return "iPhone 6s Plus"
            case "iPhone9,1", "iPhone9,3":                  return "iPhone 7"
            case "iPhone9,2", "iPhone9,4":                  return "iPhone 7 Plus"
            case "iPhone8,4":                               return "iPhone SE"
            case "iPhone10,1", "iPhone10,4":                return "iPhone 8"
            case "iPhone10,2", "iPhone10,5":                return "iPhone 8 Plus"
            case "iPhone10,3", "iPhone10,6":                return "iPhone X"
            case "iPhone11,2":                              return "iPhone XS"
            case "iPhone11,4", "iPhone11,6":                return "iPhone XS Max"
            case "iPhone11,8":                              return "iPhone XR"
            case "iPhone12,1":                              return "iPhone 11"
            case "iPhone12,3":                              return "iPhone 11 Pro"
            case "iPhone12,5":                              return "iPhone 11 Pro Max"
            case "iPad2,1", "iPad2,2", "iPad2,3", "iPad2,4":return "iPad 2"
            case "iPad3,1", "iPad3,2", "iPad3,3":           return "iPad (3rd generation)"
            case "iPad3,4", "iPad3,5", "iPad3,6":           return "iPad (4th generation)"
            case "iPad6,11", "iPad6,12":                    return "iPad (5th generation)"
            case "iPad7,5", "iPad7,6":                      return "iPad (6th generation)"
            case "iPad7,11", "iPad7,12":                    return "iPad (7th generation)"
            case "iPad4,1", "iPad4,2", "iPad4,3":           return "iPad Air"
            case "iPad5,3", "iPad5,4":                      return "iPad Air 2"
            case "iPad11,4", "iPad11,5":                    return "iPad Air (3rd generation)"
            case "iPad2,5", "iPad2,6", "iPad2,7":           return "iPad mini"
            case "iPad4,4", "iPad4,5", "iPad4,6":           return "iPad mini 2"
            case "iPad4,7", "iPad4,8", "iPad4,9":           return "iPad mini 3"
            case "iPad5,1", "iPad5,2":                      return "iPad mini 4"
            case "iPad11,1", "iPad11,2":                    return "iPad mini (5th generation)"
            case "iPad6,3", "iPad6,4":                      return "iPad Pro (9.7-inch)"
            case "iPad6,7", "iPad6,8":                      return "iPad Pro (12.9-inch)"
            case "iPad7,1", "iPad7,2":                      return "iPad Pro (12.9-inch) (2nd generation)"
            case "iPad7,3", "iPad7,4":                      return "iPad Pro (10.5-inch)"
            case "iPad8,1", "iPad8,2", "iPad8,3", "iPad8,4":return "iPad Pro (11-inch)"
            case "iPad8,5", "iPad8,6", "iPad8,7", "iPad8,8":return "iPad Pro (12.9-inch) (3rd generation)"
            case "AppleTV5,3":                              return "Apple TV"
            case "AppleTV6,2":                              return "Apple TV 4K"
            case "AudioAccessory1,1":                       return "HomePod"
            case "i386", "x86_64":                          return "Simulator \(mapToDevice(identifier: ProcessInfo().environment["SIMULATOR_MODEL_IDENTIFIER"] ?? "iOS"))"
            default:                                        return identifier
            }
            #elseif os(tvOS)
            switch identifier {
            case "AppleTV5,3": return "Apple TV 4"
            case "AppleTV6,2": return "Apple TV 4K"
            case "i386", "x86_64": return "Simulator \(mapToDevice(identifier: ProcessInfo().environment["SIMULATOR_MODEL_IDENTIFIER"] ?? "tvOS"))"
            default: return identifier
            }
            #endif
        }
        return mapToDevice(identifier: identifier)
    }()
}

调用的时候：

print("当前设备为：\(UIDevice.modelName)")
```

7.  Swift-----访问控制（private、fileprivate、internal、public、open）
　　在swift中，访问修饰符有五种，分别是：private、fileprivate、internal、public、open。其中fileprivate和open是swift 3 新添加的。由于之前的访问控制符是基于文件的，不是基于类的。这样会有问题，故swift 3 增加了两个修饰符，对原来的private、public进行了细分。
　　
```
　从高到低的排序如下：

　open > public > interal > fileprivate > private

（1）private：修饰的属性或者方法只能在当前类中访问。

（2）fileprivate：修饰的属性或者方法只能在当前文件中访问。如果一个文件中含有多个类，也是可以访问的。

（3）internal：默认访问级别，可写可不写。

　　修饰的属性和方法在源代码的整个模块都可以访问。

　　如果是框架或库代码，则在整个框架都可以访问，框架以外是不可以访问的。

　　如果是App代码，则在整个App内部都是可以访问的。

（4）public：可以被任何代码访问。但其他模块不可以被override和继承，而在模块内是可以被override和继承的。

（5）open：可以被任何模块的代码访问，包括override和继承。
```

8.   class 和 struct 的区别

class 为类, struct 为结构体, 类是引用类型, 结构体为值类型, 结构体不可以继承

9.  static 和 class 有什么区别

static 定义的方法不可以被子类继承, class 则可以
```
class AnotherClass {
    static func staticMethod(){}
    class func classMethod(){}
}
class ChildOfAnotherClass: AnotherClass {
    override class func classMethod(){}
    //override static func staticMethod(){}// error
}
```
10.  枚举定义

与OC不一样，Swift的枚举扩展性很强了，OC只能玩Int，swift 支持

整型(Integer)
浮点数(Float Point)
字符串(String)
布尔类型(Boolean)
```
enum Movement {
    case letf
    case right
    case top
    case bottom
}
enum Area: String {
    case Dong = "dong"
    case Nan = "nan"
    case Xi = "xi"
    case Bei = "bei"
}

//嵌套枚举
enum Area {
    enum DongGuan {
        case NanCheng
        case DongCheng
    }
    
    enum GuangZhou {
        case TianHe
        case CheBei
    }
}

//枚举关联值
enum Trade {
    case Buy(stock:String,amount:Int)
    case Sell(stock:String,amount:Int)
}

let trade = Trade.Buy(stock: "003100", amount: 100)

switch trade {
case .Buy(let stock,let amount):
    
    print("stock:\(stock),amount:\(amount)")
    
case .Sell(let stock,let amount):
    print("stock:\(stock),amount:\(amount)")
default:
    ()
}
```

11.  swift中set 、get 方法

swift 中的set 和 get 要复杂一点。 在swift 中主要分存储型属性 和 计算型属性 这两种 ， 一般 我们只是给计算属性添加 get\set 重写
```
var command:Int {
    get {
        //return command; 会导致死循环
        //return self.command; 会导致死循环
        //return _command; 会导致死循环
        //且不能像OC那样 return _command;
        return 1
    }
    set {
        //新值 newValue
        //value 为一个外部属性变量
        value = newValue
    }
}

//为了解决储型属性的set、get 方法问题
var _command:int?
var command ：Int {
    get {
       return _command 
    }
    set {
       _command = newValue 
    }
}
```

12. swift 中 willset 和didset 方法

属性初始化的时候，willSet 和didSet 不会调用，只有在初始化上下文之外，属性发生改变的时候调用；
给属性添加观察者属性的时候，必须声明属性类型，否则编译会报错；
```
var command ：Int {
    willSet {
       print("newValue is \(newValue)")
    }
    didSet {
      print("newValue is \(newValue)")
      print("oldValue is \(oldValue)")
    }
}
```
13. swift block循环引用解决

在其参数前面使用[weak self]或者[unowned self]
```
let emtyOb = Observable<String>.empty()
_ = emtyOb.subscribe(onNext: { [weak self] (number) in
    
    print("订阅:",number)
    self.label.text = number
})
```

14.  ?? 的用法

可选值的默认值, 当可选值为nil 的时候, 会返回后面的值. 如
```
let someValue = optional1 ?? 0
```

15.  lazy 的用法

swift 中的懒加载，只有被调用到的时候，才初始化和赋值
```
class LazyClass {
    lazy var someLazyValue: Int = {
        print("lazy init value")
        return 1
    }()
    var someNormalValue: Int = {
        print("normal init value")
        return 2
    }()
}
let lazyInstance = LazyClass()
print(lazyInstance.someNormalValue)
print(lazyInstance.someLazyValue)
```

16.  map、filter、reduce 的作用

map 用于映射, 可以将一个列表转换为另一个列表
```
[1, 2, 3].map{"\($0)"}// 数字数组转换为字符串数组
//["1", "2", "3"]
```
filter 用于过滤, 可以筛选出想要的元素
```
[1, 2, 3].filter{$0 % 2 == 0} // 筛选偶数
//[2]
```
reduce 合并
```
[1, 2, 3].reduce(""){$0 + "\($1)"}// 转换为字符串并拼接
//"123"
```

17. map 与 flatmap 的区别

flatmap 有两个实现函数实现

    public func flatMap<ElementOfResult>(_ transform: (Element) throws -> ElementOfResult?) rethrows -> [ElementOfResult] 
这个方法, 中间的函数返回值为一个可选值, 而 flatmap 会丢掉那些返回值为 nil 的值
例如
```
//例如
["1", "@", "2", "3", "a"].flatMap{Int($0)}
// [1, 2, 3]
["1", "@", "2", "3", "a"].map{Int($0) ?? -1}
//[Optional(1), nil, Optional(2), Optional(3), nil]
另一个实现
public func flatMap<SegmentOfResult>(_ transform: (Element) throws -> SegmentOfResult) rethrows -> [SegmentOfResult.Iterator.Element] where SegmentOfResult : Sequence
中间的函数, 返回值为一个数组, 而这个 flapmap 返回的对象则是一个与自己元素类型相同的数组

func someFunc(_ array:[Int]) -> [Int] {
    return array
}
[[1], [2, 3], [4, 5, 6]].map(someFunc)
//[[1], [2, 3], [4, 5, 6]]
[[1], [2, 3], [4, 5, 6]].flatMap(someFunc)
//[1, 2, 3, 4, 5, 6]
其实这个实现, 相当于是在使用 map 之后, 再将各个数组拼起来一样的

[[1], [2, 3], [4, 5, 6]].map(someFunc).reduce([Int]()) {$0 + $1}
// [1, 2, 3, 4, 5, 6]
```

18.  如何获取当前代码的函数名和行号

file 用于获取当前文件文件名

line 用于获取当前行号

column 用于获取当前列编号

function 用于获取当前函数名

```
var file: String = #file
var line: String = #line
var column: String = #column
var function: String = #function
method: String = #function,
print("\(file)--\(line)--\(columne)--\(function))
5.private，fileprivate，internal，public 和 open 五种访问控制的权限

//1.当private 或fileprivate 修饰属性的时候
privte 修饰的属性只能在本类的作用域且在当前文件内能访问
fileprivate 修饰的属性只能在当前文件内访问到，不管是否在本类作用域

//2. fileprivate 修饰方法的时候
fileprivate 修饰的方法，类的外部无法调用
fileprivate 修饰方法，子类定义的同一个文件中，可以访问
fileprivate 修饰方法，子类定义不在同一个文件，不可以访问


//3.public 和 open 的区别
这两个都用于在模块中声明需要对外界暴露的函数, 区别在于, public 修饰的类, 在模块外无法继承, 而 open 则可以任意继承, 公开度来说, public < open

```

19.  guard 的使用场景

swift 的语法糖之一

//当（条件） 为false 的时候进入{}
guard 条件
{
        
}


20. if let 语法糖使用场景

swift 中因为有optional，所以需要经常判空，举例说明if let 解决了什么问题
```
//不使用if let
func doSometing(str: String?){
    ...
}
let value :String ! = str
if value != ni {
     //如果value 不为 nil进入大括号执行
}

//使用了if let 简洁了很多
func doSometing(str: String?){
     ...
}
if let value = str {
    //如果value 不为 nil进入大括号执行
}
```

21. defer的使用场景

defer 语句块中的代码, 会在当前作用域结束前调用, 常用场景如异常退出后, 关闭数据库连接
```
func someQuery() -> ([Result], [Result]){
    //假如打开数据库失败        
    let db = DBOpen("xxx")
    defer {
        db.close()
    }
    //或者 查询失败导致异常退出 调用defer，执行里面的代码
    guard results1 = db.query("query1") else {
        return nil
    }
    guard results2 = db.query("query2") else {
        return nil
    }
    return (results1, results2)
}
```
22. String 与 NSString 的关系与区别
```
/// NSString 与 String 之间可以随意转换

let someString = "123"
let someNSString = NSString(string: "n123")
let strintToNSString = someString as NSString
let nsstringToString = someNSString as String
```
23. associatedtype 的作用

简单来说就是 protocol 使用的泛型
例如定义一个列表协议
```
protocol ListProtcol {
    associatedtype Element
    func push(_ element:Element)
    func pop(_ element:Element) -> Element?
}
```
实现协议的时候, 可以使用 typealias 指定为特定的类型, 也可以自动推断, 如
```
class IntList: ListProtcol {
    typealias Element = Int // 使用 typealias 指定为 Int
    var list = [Element]()
    func push(_ element: Element) {
        self.list.append(element)
    }
    func pop(_ element: Element) -> Element? {
        return self.list.popLast()
    }
}
class DoubleList: ListProtcol {
    var list = [Double]()
    func push(_ element: Double) {// 自动推断
        self.list.append(element)
    }
    func pop(_ element: Double) -> Double? {
        return self.list.popLast()
    }
}
```
24. 关于泛型的使用
```
class AnyList<T>: ListProtcol {
    var list = [T]()
    func push(_ element: T) {
        self.list.append(element)
    }
    func pop(_ element: T) -> T? {
        return self.list.popLast()
    }
}
```
25. 可以使用 where 字句限定 Element 类型, 如:
```
extension ListProtcol where Element == Int {
    func isInt() ->Bool {
        return true
    }
}
```

26.  dynamic 的用法

由于 swift 是一个静态语言, 所以没有 Objective-C 中的消息发送这些动态机制, dynamic 的作用就是让 swift 代码也能有 Objective-C 中的动态机制, 常用的地方就是 KVO 了, 如果要监控一个属性, 则必须要标记为 dynamic, 可以参考文章http://www.jianshu.com/p/ae26100b9edf

什么时候使用 @objc
@objc 用途是为了在 Objective-C 和 Swift 混编的时候, 能够正常调用 Swift 代码. 可以用于修饰类, 协议, 方法, 属性.
常用的地方是在定义 delegate 协议中, 会将协议中的部分方法声明为可选方法, 需要用到@objc
```
@objc protocol OptionalProtocol {
    @objc optional func optionalFunc()
    func normalFunc()
}
class OptionProtocolClass: OptionalProtocol {
    func normalFunc() {
    }
}
let someOptionalDelegate: OptionalProtocol = OptionProtocolClass()
someOptionalDelegate.optionalFunc?()
```
27. max(max())与min(min()) — 获取最大值与最小值
```
// 只有整型有
let a = Int8.max // 127
let b = Int8.min // -128
// 获取数组中的最大与最小值，支持整型，浮点型
let intArray = [1, 2, 3]
intArray.max() // 3
intArray.min() // 1

let doubleArray = [1.1, 2.2, 3.3]
doubleArray.max() // 3.3
doubleArray.min() // 1.1
```

isMultiple — 倍数判断（Swift 5）
```
let number = 4
// 检查一个整数是否为另一个整数的倍数
if number.isMultiple(of: 2) {
    print("Even")
} else {
    print("Odd")
}
```
random — 随机数（Swift 4.2）
```
// 随机数生成
let ranInt = Int.random(in: 0..<5)
let ranFloat = Float.random(in: 0..<5)
let ranDouble = Double.random(in: 0..<5)
let ranBOOL = Bool.random()

var names = ["ZhangSan", "LiSi", "WangWu"]
// 对数组元素进行重新随机排序，重新返回一个数组
let shuffled = names.shuffled()
```
randomElement — 随机元素（Swift 4.2）
```
var array: [String] = ["Animal", "Baby", "Apple", "Google", "Aunt"]
// 随机取得数组中的一个元素
let element = array.randomElement()
print(element!)
```
toggle — 布尔切换（Swift 4.2）
```
var isSwift = true
// toggle函数没有返回值
isSwift.toggle()
print(isSwift)  // 打印false
```
UUID — 唯一识别码
```
// Swift获取UUID很简单
let uuid = UUID().uuidString
print(uuid) // 类似 F1559B67-C89B-47E9-9C31-5D9366588552
```

