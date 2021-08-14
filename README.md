# iOS抽象工厂

![image-20210814144125016](https://tva1.sinaimg.cn/large/008i3skNgy1gtgbdmcekoj60i20a60tt02.jpg)

### 简介:

在正常使用中，客户端程序需要创建抽象工厂的具体实现，然后使用抽象工厂作为接口来创建这一主题的具体对象。客户端程序不需要知道（或关心）它从这些内部的工厂方法中获得对象的具体类型，因为客户端程序仅使用这些对象的通用接口。抽象工厂模式将一组对象的实现细节与他们的一般使用分离开来。

### 与工厂方法对比:

**抽象工厂模式**提供了一种方式，可以将一组具有同一主题的单独的[工厂](https://zh.wikipedia.org/wiki/工厂方法)封装起来。通常表现方式为工厂方法是一个工厂对应一个具体对象(产品)的实现。而抽象工厂一个工厂对应多个同一主题具体对象(产品)的创建

### 定义:

抽象工厂模式的实质是“提供接口，创建一系列相关或独立的对象，而不指定这些对象的具体类。

### UML:

![1280px-Abstract_factory_UML.svg](https://tva1.sinaimg.cn/large/008i3skNgy1gtgcnep4pjj60zk0njgno02.jpg)

图中角色:

+ 抽象工厂:具体工厂的超类,提供接口创建一系列不同的具体对象(产品)
+ 具体工厂:实现具体对象(产品)的对象,用来决定是产品的系列或主题
+ 抽象产品:具体产品的超类,暴露给客户端的具体对象(产品)类型
+ 具体产品:具体对象的实际类型,隐藏细节实现
+ 客户端:使用抽象工厂接口的代码

### 例子:

假设我们有两种产品接口 `Button` 和` Border` ，每一种产品都支持多种系列，比如 `Mac` 系列和 `Windows` 系列。这样每个系列的产品分别是` MacButton`, `WinButton`, `MacBorder`, `WinBorder` 。为了可以在运行时刻创建一个系列的产品族，我们可以为每个系列的产品族创建一个工厂 `MacFactory `和` WinFactory `。每个工厂都有两个方法 `CreateButton` 和 `CreateBorder` 并返回对应的产品，可以将这两个方法抽象成一个接口 `AbstractFactory` 。这样在运行时刻我们可以选择创建需要的产品系列

```swift
protocol Button {}//抽象Button类
struct MacButton:Button {}
struct WinButton:Button {}


protocol Border {}//抽象Border类
struct MacBorder:Border {}
struct WinBorder:Border {}


//它们的工厂
protocol AbstractFactory {
     func CreateButton()->Button
     func CreateBorder()->Border
}

struct MacFactory: AbstractFactory{//mac工厂
     func CreateButton() -> Button {
        return MacButton()
    }
    
     func CreateBorder() -> Border {
        return MacBorder()
    }
}

struct WinFactory: AbstractFactory{//win工厂
     func CreateButton() -> Button {
        return WinButton()
    }
    
     func CreateBorder() -> Border {
        return WinBorder()
    }
}

//客户端调用
var fac:AbstractFactory?
let style:Int = 1 //定义类型1.MacFactory 2.WinFactory
if style == 1 {
    fac = MacFactory()
} else {
    fac = WinFactory()
}
let macbutton = fac!.CreateButton() //macbutton
let macborad = fac!.CreateBorder() //macbutton
```

像变种的工厂方法模式写法的也很常见:

简单工厂写法:

```swift
protocol Button {}//抽象Button类
struct MacButton:Button {}
struct WinButton:Button {}


protocol Border {}//抽象Border类
struct MacBorder:Border {}
struct WinBorder:Border {}

enum Type {
      case Win
      case Mac
}

struct TypeFactory {
    static func createButton(type:Type)->Button{
        let button:Button
        switch type {
        case Type.Win:
            button = WinButton()
        case Type.Mac:
            button = MacButton()
        }
        return button
        
    }
    static func createBorder(type:Type)->Border{
        let board:Border
        switch type {
        case Type.Win:
            board = WinBorder()
        case Type.Mac:
            board = MacBorder()
        }
        return board
        
    }
    
 }
 let winBtn = TypeFactory.createButton(type: Type.Win)//winBtn的创建
 let MacBor = TypeFactory.createBorder(type: Type.Mac)//MacBor

```

像工厂"方法"而非工厂"类"写法:

```swift
//apple NSNumber
let boolNum = NSNumber(booleanLiteral: true)
let floatNum = NSNumber(floatLiteral: 0.2)

//实例化为本类的子类
class Button {
    static func creatMacBtn()->Button{
        return MacButton()
    }
    static func creatWinBtn()->Button{
        return WinButton()
    }
}
class MacButton:Button {}
class WinButton:Button {}

let macBtn = Button.creatMacBtn()//macBtn
let winBtn = Button.creatWinBtn()//winBtn
```

### 适用性:

+ 需要强调一系列相关的产品对象的设计以便进行联合使用时
+ 提供一个产品类库，而只想显示它们的接口而不是实现时

### 优点:

+ 具体产品从客户代码中被分离出来
+ 可以改变产品的系列

### 缺点:

+ 在产品族中扩展新的产品是很困难的，它需要修改抽象工厂的接口

### 参考:

+ [维基百科-抽象工厂](https://zh.wikipedia.org/wiki/%E6%8A%BD%E8%B1%A1%E5%B7%A5%E5%8E%82)

