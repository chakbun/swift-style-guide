文章翻译自[Raywenderlich/swift-style-guide](https://github.com/raywenderlich/swift-style-guide)  
目的只想练习一下英文阅读能力以及加深对Swift语言的理解，如果翻译错误欢迎指出，谢谢。🙏  

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





