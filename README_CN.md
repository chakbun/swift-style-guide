本文是翻译自Raywenderlich的[The Official raywenderlich.com Swift Style Guide.](https://github.com/raywenderlich/swift-style-guide)  
目的只想练习一下英文阅读能力以及加深对Swift语言的理解，翻译过程中使用的术语参考了 https://www.cnswift.org/ 。  
如果翻译存在错误，欢迎指出。谢谢。  

## 目录  
* [准确性](#准确性)
* [命名](#命名)
  * [文法](#文法)
  * [类前缀](#类前缀)
  * [代理](#代理)
  * [使用类型推断 ](#使用类型推断)
  * [泛型](#泛型)
  * [语言](#语言)
* [代码组织 ](#代码组织)
  * [协议实现](#协议实现)
  * [未使用的代码](#未使用的代码)
  * [少导入](#少导入)
* [间隔](#间隔)
* [注释](#注释)
* [类与结构体](#类与结构体)
  * [区别](#区别)
  * [定义的例子](#定义的例子)
  * [Self的使用 ](#Self的使用)
  * [计算属性](#计算属性)
  * [阻止重写](#阻止重写)
* [函数声明 ](#函数声明) 
* [函数调用](#函数调用)
* [闭包表达式](#闭包表达式)
* [类型](#类型)
  * [常量](#常量)
  * [静态方法与变量类型属性](#静态方法与变量类型属性)
  * [可选类型](#可选类型)
  * [延迟初始化](#延迟初始化)
  * [类型推断](#类型推断)
  * [语法糖](#语法糖)
* [函数与方法](#函数与方法)
* [内存管理](#内存管理)
  * [延伸对象生命周期](#延伸对象生命周期)
* [访问控制](#访问控制)
* [控制流](#控制流)
  * [三元运算符](#三元运算符)
* [黄金之路](#黄金之路)
  * [保护机制](#保护机制)
* [分号](#分号)
* [圆括号](#圆括号)
* [多行字符串](#多行字符串)
* [不要Emoji](#不要Emoji)
* [组织和包标识符](#组织和包标识符)
* [版权信息](#版权信息)
* [笑脸](#笑脸)
* [参考](#参考)

## 准确性 
尽可能让你的代码在编译时没有任何警告。下面的很多建议都是基于这条规则设计的，例如用 #selector 类型代替了字符串文字。

## 命名 
描述性和一致性的命名使软件更容易理解。参考[Swift命名风格](https://swift.org/documentation/api-design-guidelines/)  
关键点包括： 
 * 尽可能让调用清晰明确；
 * 清晰比简洁更重要；
 * 使用驼峰式风格`camelCase`；(不要使用`snake_case`)
 * 类型和协议使用大写字母，其他都用小写；
 * 保留所有必须的词，去掉所有多余的词；
 * 基于角色命名而不是类型；
 * 保留所有必须的词，去掉所有多余的词；
 * 在令人迷惑的部分补充必要说明；
 * 尽可能保证使用流畅顺滑；
 * 工厂方法使用`make`开始；
 * 方法命名要表达出细节
 	* 动作方法的动词遵循 -ed, -ing 格式区分可变类型；
 	* 可变类型的名词方法遵循 formX 格式； 
 	* 使布尔类读起来像断言； 
 	* 使描述内容类协议像名词，例如: `Collection`  
 	* 功能可用类协议应该以 -able 或 -ible 结尾；例如: `Equatable`, `ProgressReporting`  
 * 不要使用让专家惊诧或入门者困惑的术语；
 * 尽可能避免使用缩写；
 * 命名参照已有的先例；
 * 尽可能使用方法和属性替代函数；
 * 上下文的缩写保持一致；
 * 为具有相同意义的方法提供相同的基础命名； 
 * 避免通过返回类型重载； 
 * 选择可提供给文档使用的参数名称； 
 * 尽可能不要把函数的第一个参数名称包含在函数名字内，除非它是在委托中被提及； 
 * 标签闭包与元组参数；  
 * 考虑默认参数值；  

 ### 文法 
 方法命名的文法，关键要语义通俗不产生歧义。尽可能采用最简单的文法命名方法。 
 1. 编写一个不带参数的方法。例如：下一步，你需要调用`addTarget`。 
 2. 编写一个带参数标签的方法。例如：下一步，你需要调用`addTarget(_:action:)`。 
 3. 编写一个带参数标签和类型的完整方法。例如：下一步，你需要调用`addTarget(_: Any?, action:Selector?)`。  

对于`UIGestureRecognizer`使用上面的例子，1是最明确和首选的。  

**扩展**：你可以使用Xcode的jump bar跟踪带参数标签的方法。如果你擅长使用多个快捷键，将鼠标移到方法名中并按下Shift-Control-Option-Command-C(所有4个功能键)，这样Xcode会将方法列表拷贝到剪贴板中。 

![Methods in Xcode jump bar](screens/xcode-jump-bar.png)

### 类前缀 
Swift会在类型所属模块自动创建命名空间，所以你不需要加入像RW这样的类前缀。如果存在冲突的名字，可通过在名字前加入模块名解决。但建议尽可能不要这样做。  

```swift
import SomeModule

let myClass = MyModule.UsefulClass()
```

### 代理 
当自定义代理方法，第一个参数应该是代理源，而且参数是省略实标签的。（UIKit中包含千千万万个这样的例子）  

**建议**:  
```swift
func namePickerView(_ namePickerView: NamePickerView, didSelectName name: String)
func namePickerViewShouldReload(_ namePickerView: NamePickerView) -> Bool
``` 

**避免**:  
```swift
func didSelectName(namePicker: NamePickerViewController, name: String)
func namePickerShouldReload() -> Bool
```

### 使用类型推断  
使用编译器可推断的上下文编写更简短，更清晰的代码。（参考[类型推断](#类型推断)） 

**建议**:  
```swift
let selector = #selector(viewDidLoad)
view.backgroundColor = .red
let toView = context.view(forKey: .to)
let view = UIView(frame: .zero) 
``` 

**避免**:  
```swift
let selector = #selector(ViewController.viewDidLoad)
view.backgroundColor = UIColor.red
let toView = context.view(forKey: UITransitionContextViewKey.to)
let view = UIView(frame: CGRect.zero)
``` 

### 泛型 
泛型参数的命名必须是可描述性的上驼峰式风格。如果参数不带特定的关系或角色，使用传统的单个大写字母表示。如`T`, `U`, `V`等。 

**建议**:  
```swift
struct Stack<Element> { ... }
func write<Target: OutputStream>(to target: inout Target)
func swap<T>(_ a: inout T, _ b: inout T)
``` 

**避免**:  
```swift
struct Stack<T> { ... }
func write<target: OutputStream>(to target: inout target)
func swap<Thing>(_ a: inout Thing, _ b: inout Thing)
``` 

### 语言 
使用美式英文拼写配合苹果的API。 

**建议**:  
```swift
let color = "red"
``` 

**避免**:  
```swift
let colour = "red"
``` 

## 代码组织 
使用扩展使你的代码组织成逻辑功能代码块。每一个扩展通过`// MARK: -` 注释作为开始，使每个代码块看起来更有条理。 

### 协议实现   
特别是在模型遵循协议时，建议在独立的扩展中实现协议方法。这样使有关联的协议方法组成一起，还可以简化为类添加协议的实现方法的步骤。  

**建议**:  
```swift
class MyViewController: UIViewController {
  // class stuff here
}

// MARK: - UITableViewDataSource
extension MyViewController: UITableViewDataSource {
  // table view data source methods
}

// MARK: - UIScrollViewDelegate
extension MyViewController: UIScrollViewDelegate {
  // scroll view delegate methods
}
``` 

**避免**:  
```swift
class MyViewController: UIViewController, UITableViewDataSource, UIScrollViewDelegate {
  // all methods
}
``` 

自从编译器不允许用户在派生类中重新声明协议的实现方法，所以并不需要复制基类的扩展组。这在派生类是只重写少量方法的最终类中更是这样，对于什么时候保留扩展组就让编码者决定。

对于UIKit的视图控制器，可考虑将其生命周期，自定义访问器和IBAction分组到独立的扩展中。  

### 未使用的代码  
~~垃圾~~未使用的代码，包括Xcode模版预设的代码/提示/注释等都必须删掉。除非那些代码/注释是指导用户使用相关代码的。   
与简单实现调用父类的指导教程不相关的期望方法也应该删掉，包括任何空/没用的UIApplicationDelegate方法。  

**建议**:  
```swift
override func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
  return Database.contacts.count
}
``` 

**避免**:  
```swift
override func didReceiveMemoryWarning() {
  super.didReceiveMemoryWarning()
  // Dispose of any resources that can be recreated.
}

override func numberOfSections(in tableView: UITableView) -> Int {
  // #warning Incomplete implementation, return the number of sections
  return 1
}

override func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
  // #warning Incomplete implementation, return the number of rows
  return Database.contacts.count
}
``` 

### 少导入
只导入源文件所需的模块。例如，如果只需要使用`Foundation`的接口，就只导入`Foundation`而不要导入`UIKit`。同样地，假设你必须导入`UIKit`，就不要导入`Foundation`了。  

**建议**:  
```swift
import UIKit
var view: UIView
var deviceModels: [String]
``` 

**建议**:  
```swift
import Foundation
var deviceModels: [String]
``` 

**避免**:  
```swift
import UIKit
import Foundation
var view: UIView
var deviceModels: [String]
``` 

**避免**:  
```swift
import UIKit
var deviceModels: [String]
``` 

## 间隔  
使用2个空格代替制表符进行缩进，节省空间之余又有助于防止换行。请保证你的Xcode和工程项目中使用下面的配置： 

![Xcode indent settings](screens/indentation.png)

* 方法与其他（`if`/`else`/`switch`/`while` etc.）的花括号的打开应该与语句在同一行，括号的关闭在独立新一行。  
* 提示：你可以在选择一些代码后（或者 **command+A** 全选）按 **Control+I**(或菜单栏中**Editor ▸ Structure ▸ Re-Indent**) 重新缩进。Xcode的一些代码模版采用了4空格制表，通过这个方法可以很好解决这个问题。  

**建议**:  
```swift
if user.isHappy {
  // Do something
} else {
  // Do something else
}
``` 

**避免**:  
```swift
if user.isHappy
{
  // Do something
}
else {
  // Do something else
}
``` 

* 为了在视觉上更清晰和有组织，方法与方法之间使用一行空白分开。空格的作用是将方法按照功能分节，但如果存在太多的分节，你就要考虑将方法拆分成若干子方法了。  
* 方法体首尾不要有空行。  
* 冒号的左边不要有空格，右边带一空格。除非是三元运算符` ? : `，空字典`[:]`和`#selector`语法`addTarget(_:action:)`。  

**建议**:  
```swift
class TestDatabase: Database {
  var data: [String: CGFloat] = ["A": 1.2, "B": 3.2]
}
``` 

**避免**:  
```swift
if user.isHappy
class TestDatabase : Database {
  var data :[String:CGFloat] = ["A" : 1.2, "B":3.2]
}
``` 

* 单行长句应该控制在70个字符左右，但没有硬性规定。  
* 避免在行尾使用空格。 
* 文件以换行符结束。 

## 注释 
如果有必要，使用注释去解释特定代码块的作用。注释必须是保持最新的，否则就删除它。  
避免在代码内部使用注释，尽量让代码作为最好的注释。除非你不打算将这些注释放在文档中。  
避免使用C语言风格的注释(`/* ... */`)。建议使用双斜杠或三斜杠。  

## 类与结构体

### 区别    
切记，结构体是[值类型](https://developer.apple.com/library/mac/documentation/Swift/Conceptual/Swift_Programming_Language/ClassesAndStructures.html#//apple_ref/doc/uid/TP40014097-CH13-XID_144)。使用结构体处理不需要特定标识的事物。例如一个数组包含[a, b, c] 与 另外一个包含[a, b, c]的数组是相同的，他们完全是可互换的。对于使用的是第一个数组还是第二个数组没有任何影响，因为他们代表着相同的东西。这是考虑使用结构体的根本。  

类是[引用类型](https://developer.apple.com/library/mac/documentation/Swift/Conceptual/Swift_Programming_Language/ClassesAndStructures.html#//apple_ref/doc/uid/TP40014097-CH13-XID_145)。使用类需要考虑特定标识或存在特定生命周期的事物。例如你可以将一个人依靠类去建模，因为两个人的对象是两个不同的东西。虽然两个人都有姓名和生日，但这并不意味着他们是同一个人。一个人的生日可以是结构体，因为一个1950年3月3日的日期和另外一个1950年3月3日的日期是一样的。日期本身不需要特定的标识。  

有时，事物应该定义为结构体却遵循`AnyObject`的协议，或者历史原因曾经被定义为类(`NSDate`, `NSSet`)。所以，只好尽可能遵循这些准则吧。  


### 定义的例子  

下面是一个样式良好的类定义的编码例子： 

```swift
class Circle: Shape {
  var x: Int, y: Int
  var radius: Double
  var diameter: Double {
    get {
      return radius * 2
    }
    set {
      radius = newValue / 2
    }
  }

  init(x: Int, y: Int, radius: Double) {
    self.x = x
    self.y = y
    self.radius = radius
  }

  convenience init(x: Int, y: Int, diameter: Double) {
    self.init(x: x, y: y, radius: diameter / 2)
  }

  override func area() -> Double {
    return Double.pi * radius * radius
  }
}

extension Circle: CustomStringConvertible {
  var description: String {
    return "center = \(centerString) area = \(area())"
  }
  private var centerString: String {
    return "(\(x),\(y))"
  }
}
```

上面的例子展示了以下几点规则: 

* 为属性，变量，常量，参数声明以及其他语句的指定类型，在冒号前不带空格，后带一个空格。例如：`x: Int`, `Circle: Shape`。 
* 如果变量或结构体是共享类型或上下文，定义在同一行中。  
* 缩进getter和setter的定义和属性观察器。
* 不要显式声明默认的修饰符，例如变量的`internal`。  
* 在扩展中加入额外的功能。（例如 printing） 
* 对外隐藏不共享的实现细节。例如：扩展中用`private`限制`centerString`的访问权限。  

### Self的使用 
出于简洁，避免使用`self`因为Swift并不要求通过它才能访问对象的属性与它的方法。 
只有在编译器要求时才使用self（在`@escaping`闭包或者在参数中消除属性歧义的构造器中）。除此外，能不带self编译通过的麻烦去掉。 

### 计算属性 

出于简洁，如果计算属性只读，请把get语句去掉。 get语句仅在set语句声明后才强制要求。 

**建议**:  
```swift
var diameter: Double {
  return radius * 2
}
```

**避免**:  
```swift
var diameter: Double {
  get {
    return radius * 2
  }
}
```

### 阻止重写  

使用`final`标记类或成员会显得累赘，而且这并不是必须的。尽管如此，有时候使用`final`可以令你的意图更加明显，显然这是值得的。下面的例子中，`Box`有特定用途，并且不打算让其派生子类定制。标记`final`使其更加清楚了。  

```swift
// Turn any generic type into a reference type using this Box class.
final class Box<T> {
  let value: T
  init(_ value: T) {
    self.value = value
  }
}
```

## 函数声明 

保持函数声明（含花括号）在一行内。 

```swift
func reticulateSplines(spline: [Double]) -> Bool {
  // reticulate code goes here
}
```

对于具有长标签的函数，每一个参数独立一行并保持同样的缩进。  

```swift
func reticulateSplines(
  spline: [Double], 
  adjustmentFactor: Double,
  translateConstant: Int, comment: String
) -> Bool {
  // reticulate code goes here
}
```

使用 `()`代替`(Void)`表示空输入；使用`Void`代替`()`表示空输出。  

**建议**:  
```swift
func updateConstraints() -> Void {
  // magic happens here
}

typealias CompletionHandler = (result) -> Void
```

**避免**:  
```swift
func updateConstraints() -> () {
  // magic happens here
}

typealias CompletionHandler = (result) -> ()
```

## 函数调用  

在调用的地方镜像函数声明的风格，适合单行的调用应该这样写： 

```swift
let success = reticulateSplines(splines)
```

如果调用时候必须分行，那么每个参数独立一行并且保持相同的缩进效果。 

```swift
let success = reticulateSplines(
  spline: splines,
  adjustmentFactor: 1.3,
  translateConstant: 2,
  comment: "normalize the display")
```

## 闭包表达式  

仅当仅有一个闭包参数且是最后一个参数才使用尾随闭包。闭包参数需具有描述性的名字。  

**建议**:  
```swift
UIView.animate(withDuration: 1.0) {
  self.myView.alpha = 0
}

UIView.animate(withDuration: 1.0, animations: {
  self.myView.alpha = 0
}, completion: { finished in
  self.myView.removeFromSuperview()
})
```

**避免**:  
```swift
UIView.animate(withDuration: 1.0, animations: {
  self.myView.alpha = 0
})

UIView.animate(withDuration: 1.0, animations: {
  self.myView.alpha = 0
}) { f in
  self.myView.removeFromSuperview()
}
```

对于上下文清晰的单表达式闭包，使用隐式返回。

```swift
attendeeList.sort { a, b in
  a > b
}
```

采用尾随闭包的链式方法应是在上下文中清晰易读的。至于间距，换行和命名的风格都留给编码者决定。例如：  

```swift
let value = numbers.map { $0 * 2 }.filter { $0 % 3 == 0 }.index(of: 90)

let value = numbers
  .map {$0 * 2}
  .filter {$0 > 50}
  .map {$0 + 10}
```

## 类型  

尽可能使用Swift原生类型和表达式。Swift提供桥接Objective-C的方式，所以你仍可使用你所需的Objective-C类型集。  

**建议**:
```swift
let width = 120.0                                    // Double
let widthString = "\(width)"                         // String
```

**不建议**:
```swift
let width = 120.0                                    // Double
let widthString = (width as NSNumber).stringValue    // String
```

**避免**:
```swift
let width: NSNumber = 120.0                          // NSNumber
let widthString: NSString = width.stringValue        // NSString
```

在绘画编码中，可使用`CGFloat`避免过多频繁的类型转化，使代码更简洁。  

### 常量  

使用关键字`let`声明常量，而`var`声明变量。如果一个变量的值不会发生改变，应使用`let`代替`var`。  

**提示:** 技术好的编码者声明变量都使用`let`，除非编译器报错了才改用`var`。  

你可以声明类型属性代替声明常量实例。使用`static let`可简单声明类型属性。 因为这种方式更容易区别实例属性，所以相对于全局常量一般建议使用声明类型属性的方式。例如：  

**建议**:
```swift
enum Math {
  static let e = 2.718281828459045235360287
  static let root2 = 1.41421356237309504880168872
}

let hypotenuse = side * Math.root2

```
**注意:** 使用不区分大小写的枚举的好处是可以保证其不会意外被实例化并且被误作为命名空间工作；

**避免**:
```swift
let e = 2.718281828459045235360287  // 污染全局命名空间
let root2 = 1.41421356237309504880168872

let hypotenuse = side * root2 // root2是什么?
```

### 静态方法与变量类型属性  

静态方法与类型属性与全局函数和全局变量相似，应该谨慎使用。当功能范围限定为特定类型或需要与Objective-C互通时，静态方法与类型属性很有用。

### 可选类型 

使用`?`声明变量或函数返回值可选的,表示该值允许为`nil`。  
当你确定在使用前已初始化的实例变量，可通过`!`隐式展开可选类型，例如子视图将会在`viewDidLoad()`被初始化。建议在大多数其他场景优先使用可选类型和隐式展开的方式。 
访问可选类型的值时，当仅访问一次或链式访问多个可选类型时，可使用下面的`?`链式语法。  

```swift
textContainer?.textLabel?.setNeedsDisplay()
```

可选类型一次展开后执行多个操作的使用:  

```swift
if let textContainer = textContainer {
  // do many things with textContainer
}
```

避免使用像`optionalString`或`maybeView`的方式命名可选类型，因为在声明中已经很明确表示变量是可选类型。  
对于可选绑定，尽可能使用原名称，避免使用`unwrappedView`或`actualLabel`等名称。  

**建议**:
```swift
var subview: UIView?
var volume: Double?

// later on...
if let subview = subview, let volume = volume {
  // do something with unwrapped subview and volume
}

// another example
UIView.animate(withDuration: 2.0) { [weak self] in
  guard let self = self else { return }
  self.alpha = 1.0
}
```

**避免**:
```swift
var optionalSubview: UIView?
var volume: Double?

if let unwrappedSubview = optionalSubview {
  if let realVolume = volume {
    // do something with unwrappedSubview and realVolume
  }
}

// another example
UIView.animate(withDuration: 2.0) { [weak self] in
  guard let strongSelf = self else { return }
  strongSelf.alpha = 1.0
}
```

### 延迟初始化

在对象的生命周期更好地控制访问权限，可考虑延迟初始化。 特别是对于常常需要延迟加载试图的`UIViewController`作用更明显。你可以使用即时调用的闭包`{ }()`或调用一个私有工厂方法。例如：

```swift
lazy var locationManager = makeLocationManager()

private func makeLocationManager() -> CLLocationManager {
  let manager = CLLocationManager()
  manager.desiredAccuracy = kCLLocationAccuracyBest
  manager.delegate = self
  manager.requestAlwaysAuthorization()
  return manager
}
```

**注意:**
  - `[unowned self]` 这里不不需要，因为没有发生循环引用。  
  - Location manager会弹出请求用户允许，因此控制访问权限在这里有意义。  

### 类型推断  

多使用紧凑精简的编码，让编译器去推断各个实例常量、变量的实际类型。类型推断同样适用于小型非空数组与字典。必要时，可指明类似`CGFloat`, `Int16`等具体类型。  

**建议**:
```swift
let message = "Click the button"
let currentBounds = computeViewBounds()
var names = ["Mic", "Sam", "Christine"]
let maximumWidth: CGFloat = 106.5
```

**避免**:
```swift
let message: String = "Click the button"
let currentBounds: CGRect = computeViewBounds()
var names = [String]()
```

#### 空数组与字典的类型注解  

对于空的数组和字典，使用类型注解。(对已分配了大量，多行文字的数组和字典，使用类型注解)  

**建议**:
```swift
var names: [String] = []
var lookup: [String: Int] = [:]
```

**避免**:
```swift
var names = [String]()
var lookup = [String: Int]()
```

**注意**: 遵循这条规则意味着选择一个描述性的名字更加的重要了。    

### 语法糖

使用简短的声明类型语法糖。  


**建议**:
```swift
var deviceModels: [String]
var employees: [Int: String]
var faxNumber: Int?
```

**避免**:
```swift
var deviceModels: Array<String>
var employees: Dictionary<Int, String>
var faxNumber: Optional<Int>
```

## 函数与方法    

不依附在任何类或类型的自由函数（free functions）需要谨慎使用。尽可能使用方法代替函数，因为方法更利于代码的可读性和可发现性。  
函数更适用于没有关联任何特定类型或实例的场景。  

**建议**
```swift
let sorted = items.mergeSorted()  // easily discoverable
rocket.launch()  // acts on the model
```

**避免**
```swift
let sorted = mergeSort(items)  // hard to discover
launch(&rocket)
```

**函数场景**
```swift
let tuples = zip(a, b)  // feels natural as a free function (symmetry)
let value = max(x, y, z)  // another free function that feels natural
```

## 内存管理  

所有编码（包括测试代码，指导用的样本代码等）都必须避免产生循环引用。使用`weak`或`unowned`防止强循环。或者使用值类型 (`struct`, `enum`) 防止循环。  

### 延伸对象生命周期  

使用语句`[weak self]` 或 `guard let self = self else { return }` 延伸对象的生命周期。`[weak self]`比`[unowned self]`更安全，因为`self`生命周期更长。使用显式生命周期延伸，而不是可选类型链式结构。  

**建议**
```swift
resource.request().onComplete { [weak self] response in
  guard let self = self else {
    return
  }
  let model = self.updateModel(response)
  self.updateUI(model)
}
```

**避免**
```swift
// might crash if self is released before response returns
resource.request().onComplete { [unowned self] response in
  let model = self.updateModel(response)
  self.updateUI(model)
}
```

**避免**
```swift
// deallocate could happen between updating the model and updating UI
resource.request().onComplete { [weak self] response in
  let model = self?.updateModel(response)
  self?.updateUI(model)
}
```

## 访问控制  

官方教程中关于访问控制的写得很繁琐。说白了就是：合理地使用`private` 与 `fileprivate`，可增加代码清晰度，促进封装。除非编译器要求用`fileprivate`， 否则优先使用`private`。  

当你只有在需要完整的访问控制规范时，才显式使用`open`, `public`, 和 `internal`。  

除非是静态标识`static`与`@IBAction`, `@IBOutlet` 或 `@discardableResult`等属性标签，否则访问控制的关键字必须排在首位。  

**建议**:
```swift
private let message = "Great Scott!"

class TimeMachine {  
  private dynamic lazy var fluxCapacitor = FluxCapacitor()
}
```

**避免**:
```swift
fileprivate let message = "Great Scott!"

class TimeMachine {  
  lazy dynamic private var fluxCapacitor = FluxCapacitor()
}
```

## 控制流

建议`for-in`风格，避免`while-condition-increment`风格。  

**建议**:
```swift
for _ in 0..<3 {
  print("Hello three times")
}

for (index, person) in attendeeList.enumerated() {
  print("\(person) is at position #\(index)")
}

for index in stride(from: 0, to: items.count, by: 2) {
  print(index)
}

for index in (0...3).reversed() {
  print(index)
}
```

**避免**:
```swift
var i = 0
while i < 3 {
  print("Hello three times")
  i += 1
}

var i = 0
while i < attendeeList.count {
  let person = attendeeList[i]
  print("\(person) is at position #\(i)")
  i += 1
}
```

### 三元运算符  

三元运算符`?:`在可增加清晰度和代码整齐度的时候，单个条件判断使用。如果存在多个条件判断，通常使用`if`或重构为实例变量。三元运算符的最佳使用场景是在 决定采用哪一个值去赋值给变量的时候。 

**建议**:

```swift
let value = 5
result = value != 0 ? x : y

let isHorizontal = true
result = isHorizontal ? x : y
```

**避免**:

```swift
result = a > b ? x = c > d ? c : d : y
```

## 黄金之路  

当使用条件语句时，代码的左边距是“黄金”或“快乐”路径（ "golden" or "happy" path）。 这表示，不嵌套`if`语句前提下，多个return语句都可以接受。`guard`语句就是因为这样存在的。  

**建议**:
```swift
func computeFFT(context: Context?, inputData: InputData?) throws -> Frequencies {

  guard let context = context else {
    throw FFTError.noContext
  }
  guard let inputData = inputData else {
    throw FFTError.noInputData
  }

  // use context and input to compute the frequencies
  return frequencies
}
```

**避免**:
```swift
func computeFFT(context: Context?, inputData: InputData?) throws -> Frequencies {

  if let context = context {
    if let inputData = inputData {
      // use context and input to compute the frequencies

      return frequencies
    } else {
      throw FFTError.noInputData
    }
  } else {
    throw FFTError.noContext
  }
}
```

当多个可选类型使用`guard`或`if let`展开时，尽可能使用复合形式减少嵌套。使用复合形式，`guard`独立一行，然后每个条件保持相同缩进。`else`与条件相同缩进，其执行语句再缩进一级如下面例子所示:  

**建议**:
```swift
guard 
  let number1 = number1,
  let number2 = number2,
  let number3 = number3 
  else {
    fatalError("impossible")
}
// do something with numbers
```

**避免**:
```swift
if let number1 = number1 {
  if let number2 = number2 {
    if let number3 = number3 {
      // do something with numbers
    } else {
      fatalError("impossible")
    }
  } else {
    fatalError("impossible")
  }
} else {
  fatalError("impossible")
}
```

### 保护机制

想提前结束Guard语句有几种方式。一般简单的语句像`return`，`throw`，`break`，`continue` 和 `fatalError()`，避免大量代码块。如果多个退出点需要做代码的清理保存工作，使用`defer`代码块去避免重复的代码。  

## 分号

Swift 并不需要在语句结束的位置加入分号`;`。除非是你将多个语句放在一行。 
不要将多个语句放在一行用分号隔开的风格！！！ 

**建议**:
```swift
let swift = "not a scripting language"
```

**避免**:
```swift
let swift = "not a scripting language";
```

**注意**: Swift与JavaScript是截然不同的两种语言, JavaScript去掉分号[被认为是不安全的](http://stackoverflow.com/questions/444080/do-you-recommend-using-semicolons-after-every-statement-in-javascript)   

## 圆括号

条件语句的圆括号不是必须，一般应去掉！！！

**建议**:
```swift
if name == "Hello" {
  print("World")
}
```

**避免**:
```swift
if (name == "Hello") {
  print("World")
}
```

但一些复杂的表达式中，圆括号有时可保留使代码更清晰可读。  

**建议**:
```swift
let playerMark = (player == current ? "X" : "O")
```

## 多行字符串

当使用到一个很长的字符串，通常都会使用分行的方式处理。分行的开端与等号在同一行，但内容另起一行并缩进一级。  

**建议**:

```swift
let message = """
  You cannot charge the flux \
  capacitor with a 9V battery.
  You must use a super-charger \
  which costs 10 credits. You currently \
  have \(credits) credits available.
  """
```

**避免**:

```swift
let message = """You cannot charge the flux \
  capacitor with a 9V battery.
  You must use a super-charger \
  which costs 10 credits. You currently \
  have \(credits) credits available.
  """
```

**避免**:

```swift
let message = "You cannot charge the flux " +
  "capacitor with a 9V battery.\n" +
  "You must use a super-charger " +
  "which costs 10 credits. You currently " +
  "have \(credits) credits available."
```

## 不要emoji  

不要在你的项目中使用emoji。对于编码的读者来说，这是一个不必要的摩擦。虽然emoji很可爱，但它们对学习不会有任何帮助之余甚至阻碍编码的流畅度。  

## 组织和包标识符  

涉及Xcode项目的组织应为`Ray Wenderlich`，教程的项目名为`TutorialName`吗，所以包标识符应为`com.raywenderlich.TutorialName`。  

![Xcode Project settings](screens/project_settings.png)

## 版权信息  

下面的版权信息应该包含在每一个源文件的头部。  

```swift
/// Copyright (c) 2019 Razeware LLC
/// 
/// Permission is hereby granted, free of charge, to any person obtaining a copy
/// of this software and associated documentation files (the "Software"), to deal
/// in the Software without restriction, including without limitation the rights
/// to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
/// copies of the Software, and to permit persons to whom the Software is
/// furnished to do so, subject to the following conditions:
/// 
/// The above copyright notice and this permission notice shall be included in
/// all copies or substantial portions of the Software.
/// 
/// Notwithstanding the foregoing, you may not use, copy, modify, merge, publish,
/// distribute, sublicense, create a derivative work, and/or sell copies of the
/// Software in any work that is designed, intended, or marketed for pedagogical or
/// instructional purposes related to programming, coding, application development,
/// or information technology.  Permission for such use, copying, modification,
/// merger, publication, distribution, sublicensing, creation of derivative works,
/// or sale is expressly withheld.
/// 
/// THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
/// IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
/// FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
/// AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
/// LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
/// OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
/// THE SOFTWARE.
```

## 笑脸  

笑脸是[raywenderlich.com](https://www.raywenderlich.com/)网站一个重要的风格。拥有一个正确的微笑是非常重要的，这标志着编码主题的巨大幸福感与兴奋感。使用闭方括号是因为ASCII颜文字所能表达出最大的微笑。闭圆括号像表达出一个半心意的微笑，所以不建议。

**建议**:
```
:]
```

**避免**:
```
:)
```

## 参考

* [The Swift API Design Guidelines](https://swift.org/documentation/api-design-guidelines/)
* [The Swift Programming Language](https://developer.apple.com/library/prerelease/ios/documentation/swift/conceptual/swift_programming_language/index.html)
* [Using Swift with Cocoa and Objective-C](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/BuildingCocoaApps/index.html)
* [Swift Standard Library Reference](https://developer.apple.com/library/prerelease/ios/documentation/General/Reference/SwiftStandardLibraryReference/index.html)


