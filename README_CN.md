文章翻译自[Raywenderlich/swift-style-guide](https://github.com/raywenderlich/swift-style-guide)  
目的只想练习一下英文阅读能力以及加深对Swift语言的理解，翻译使用的术语参考了:(https://www.cnswift.org/)，如果翻译存在错误，欢迎指出。谢谢。🙏  

 
## 正确性 
尽可能让你的代码在编译过程中没有警告。 下面的很多设计风格都遵循了这条规则，例如用 #selector 类型代替了字符串文字。

## 命名 
描述性和一致性的命名使软件更容易理解。参考[Swift命名风格](https://swift.org/documentation/api-design-guidelines/)  
关键点包括： 
 * 调用位置尽可能清晰；
 * 清晰比简洁更重要；
 * 使用驼峰式风格 camelCase ；(不要蛇式！！！snake_case)
 * 类型和协议使用大写字母，其他都用小写；
 * 保留所有必须的同时去掉所有多余的字；
 * 为含糊的部分补充上关键的词；
 * 尽可能保证使用流畅；
 * 使用`make`作为工厂方法的开端；
 * 方法命名能表达出细节
 	* 动作方法的动词遵循 -ed, -ing 格式； 
 	> verb methods follow the -ed, -ing rule for the non-mutating version 
 	* 名词方法遵循 formX 格式； 
 	> noun methods follow the formX rule for the mutating version 
 	* 布尔类应该读起来像断言； 
 	* 描述内容类协议读起来像名词； 
 	* 功能可用类协议应该以 -able 或 -ible 结尾； 
 * 不要使用让专家警惊诧或入门者困惑的术语；
 * 一般避免使用缩写；
 * 参照既有先例命名；
 * 尽量使用方法和属性替代函数；
 * 套管首字母缩略词和首字母统一向上或向下；
 > casing acronyms and initialisms uniformly up or down 
 * 为具有相同意义的方法提供相同的基础命名； 
 * 避免通过返回类型重载； 
 * 选择可提供给文档使用的参数名称； 
 * 尽量不要把函数的第一个参数名称包含在函数名字内，除非它是在委托中被提及； 
 * 标签闭包与元组参数；  
 * 考虑默认参数值；  

 ### 文法 
 当提及到方法命名的文法，语义不含糊是关键。所以，尽量采用最简单的文法命名一个方法。 
 1. 编写一个不含参数的方法。例如：下一步，你需要调用`addTarget`。 
 2. 编写一个带参数标签的方法。例如：下一步，你需要调用`addTarget(_:action:)`。 
 3. 编写一个带参数标签和类型的完整方法。例如：下一步，你需要调用`addTarget(_: Any?, action:Selector?)`。  

对于`UIGestureRecognizer`使用上面的例子，1是最明确和应优先选择的。  

**扩展**：你可以使用Xcode的jump bar追踪带参数标签的方法。如果你特别擅长同时按多个键，将鼠标移到方法名中并按下Shift-Control-Option-Command-C(所有4个修饰键)，这样Xcode将友好地将其拷贝到你的剪贴板中。 

![Methods in Xcode jump bar](screens/xcode-jump-bar.png)

### 类前缀 
Swift会在类型所属模块自动创建命名空间，所以你不需要加入像RW之类的类前缀。如果存在两个冲突的名字，你可以通过在名字前加入模块名去消除歧义。当然，这是不得不做的下策，尽可能不要这样做。  

```swift
import SomeModule

let myClass = MyModule.UsefulClass()
```

### 代理 
当自定义代理方法，代理源应该是第一个带省略实标签的参数。（UIKit中包含千千万万个这样的例子）  

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
使用编译器可推断的上下文编写更简短，更清晰的代码。（参考[类型推断](#type-inference)） 

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
泛型参数的命名必须是可描述性的上驼峰式风格。如果一个参数不需要带特定的关系或角色，可使用传统的单个大写字母表示。如`T`, `U`, `V`等。 

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

### 应该用哪一个？  
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


