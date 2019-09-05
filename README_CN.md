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
 * 使用命名先例；
 * 尽量使用方法和属性替代函数；
 * 均匀地在上下文使用统一缩写前缀；
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
对于`UIGestureRecognizer`使用上面的例子，1是最明确和优先选择的的。  
**扩展**：你可以使用Xcode的jump bar追踪带参数标签的方法。如果你特别擅长同时按多个键，将鼠标移到方法名中并按下Shift-Control-Option-Command-C(所有4个修饰键)，这样Xcode将友好地将其拷贝到你的剪贴板中。 

