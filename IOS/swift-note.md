- 新建工程删除Main.Storybord & LaunchScreen.storyboard
  直接cmd+del删除这两个文件
  单机工程名称删除Main Interface 和 Launch Screen File

在AppDelegate中加上：
window = UIWindow(frame: UIScreen.main.bounds)
        window.makeKeyAndVisible()
        window.rootViewController = XXVC()
- ViewController 初始化xib
  cmd+N 创建VC时直接勾选 create xib file
- 页面跳转
```swift
let app : AppDelegate = UIApplication.shared.delegate
app.window.rootViewController = xxVC()
```
```
self.navigationController.pushViewController(defectCreateVC, animated: true)
```
---

- SnapKit笔记
  参考博客：[Swift自动布局库SnapKit的使用详解](
  http://www.hangge.com/blog/cache/detail_1097.html)


- tableView相当于ListView
- collectionView相当于RecyclerView
- UserDefaults 相当于 SharedPreference，用法
  [Swift中安全优雅的使用UserDefaults](
  https://www.jianshu.com/p/3796886b4953)

- 便利构造器`convenience init()`必须调用类指定构造器`self.init(xx:xx)`

- kotlin中 ?: 运算符相当于 swift中的 ??

- 如何声明一个静态变量 直接使用static

- 如何使用for i ，for index in xx.indices

- protocol:
  optional 修饰的是可选实现的方法[protocol 前需要加@objc]<br>
  无关键字修饰的是必须实现的方法

- [swift如何实现抽象类](https://segmentfault.com/a/1190000013014498)

- swift泛型使用 : Test<T : Any>
- swift泛型传递|泛型方法：func createVertCollectionView <T:Any,V>(tableW : CGFloat,spacing : CGFloat = 0.0,delegate : CommonDelegate<T,V>) 

- AnyClass  T.self | XX.self 提示错误不管（编译器有时反应慢）

- swift可以直接返回多个参数|多参数
  eg：func test() -> (Int, String)
  var (i,s) = test()
---------------
- ios返回多个变量接收
```swift
var i = 0
var s = ""
(i,s) = test()
```

- 扩展已有类的属性 | extension扩展属性
```swift
private struct StateKeys{
    static var saveClick : UIClick = UIClick()
}

class UIClick : Any{
    var click : () -> Void = {return}
}

extension UIButton{
    		var saveClick : UIClick{
        	get {
            		return objc_getAssociatedObject(self, &StateKeys.saveClick) as! UIClick
        	}
        	set {
            		objc_setAssociatedObject(self, &StateKeys.saveClick, newValue, 		objc_AssociationPolicy.OBJC_ASSOCIATION_RETAIN_NONATOMIC)
       		 }
   	 	}
}
```

- 系统UIButton setOnclick|UIButton传多个参数
  结合上个笔记。
```swift
    func setOnClick(click : @escaping () -> Void) {
        self.saveClick = UIClick()
        self.saveClick.click = click
        self.addTarget(self, action: #selector(btnClick), for: .touchUpInside)
    }
    
    @objc func btnClick(){
        self.saveClick.click()
    }
```

- 函数中函数
```swift
func outsideFun(){
    fun test(){
    print("test... haha")
    }
}
```

- swift高阶函数
```swift
    func averageOfFunction(a:Float,b:Float,f:((Float) -> Float)) -> Float {
        return (f(a) + f(b)) / 2
    }
    
	func square(a:Float) -> Float {
    	return a * a
	}
     ---- 使用 1---
    averageOfFunction(a: 1, b: 2, f: square)
     ---- 使用 2---
    averageOfFunction(a: 1, b: 2) {
        f -> Float in
        return f * f
    }
```
- 高版本Swift开发引入低版本Swift库时如何处理<br>
  单击Pods 展开最左侧的窗口（如已展开请忽略）<br>
  找到报错的库，search<br> swift_version:修改到库自己的版本，一般改到3.2即可

- 修改Xcode工程最低支持的版本号|工程没有任何可用的模拟器解决方法<br>
  单击工程名称 左侧菜单（PROJECT & TARGETS)选择PROJECT ->Info->IOS Deployment Target<br>
  选择合适的版本号<br>

- Swift中使用高阶函数<br>
  1.初始化设置<br>
  2.临时设置<br>
```swift
    var callback : (Int) -> Void = { index in }
    func setCallBack(callback : @escaping (Int) -> Void){
        self.callback = callback;
    }
```

- 判断字符串相等 
```swift
if a == "425"{etc...}
```

- URL获取不带 file://的path  ，直接.path

- Xcode写内容到电脑上到文件中
```swift
 let path = "/Users/fancy/Documents/alfred/realm.txt"
 let pathUrl = URL(fileURLWithPath: path)
 try! url.write(to: pathUrl, atomically: true, encoding: .utf8)
- 把Realm数据库的路径写入到文件中：
 var url = Realm.Configuration.defaultConfiguration.fileURL!.path
 let path = "/Users/fancy/Documents/alfred/realm.txt"
 let pathUrl = URL(fileURLWithPath: path)
 try! url.write(to: pathUrl, atomically: true, encoding: .utf8)
```
 - 反射
 ```swift
 let a = all().sorted(byKeyPath: property).first!

let mirror = Mirror.init(reflecting: a)
for (name, value) in mirror.children {
	print("\(name) == \(value)")
	if(name == property){
		print(value)
		return value
	}
}
 ```
 - UIDataPicker/软键盘 etc. 点击空白处消失
 ```
let tap = UITapGestureRecognizer(target: self, action: #selector(loseFocus))
tap.cancelsTouchesInView = false
self.view.addGestureRecognizer(tap)
		
	@objc func loseFocus() {
		self.view.endEditing(true)
	}
 ```

 - image变Data：
```swift
let avaData = UIImageJPEGRepresentation(image!, 0.5)
```
- UIView变Image：
```swift
let size = testView.bounds.size
UIGraphicsBeginImageContextWithOptions(size, false, UIScreen.main.scale)
testView.layer.render(in: UIGraphicsGetCurrentContext()!)
var image = UIGraphicsGetImageFromCurrentImageContext()
UIGraphicsEndImageContext();
let imageData = UIImagePNGRepresentation(image!)
```
- 打开一个PDF：
```objc
MuDocumentController *document = [[MuDocumentController alloc] initWithFilename:_filename path:_filePath document: doc];
if (document) {
self.title = @"Library";
[self.navigationController pushViewController: document animated: YES];
[document release];
}
```
- 绘图不要使用Pan手势， 最好用touchMoved，etc.
- vc通信 <br>
    eg:vcA 打开 vcB<br>
    1.vcA在vcB关闭后获取vcB中数据 [可以通过闭包方式获得]()<br>
```swift
init(completion : @escaping (UIImage) -> Void) {
	self.completion = completion
	super.init(nibName: nil, bundle: nil)
}
```
    2.vcA在vcB未关闭时获取vcB中的数据 [持有引用，可以直接获取vcB的属性]()<br>
    3.vcB获取vcA中的数据 [init时传递]() [viewController强制转换后获取属性]()

- 配置桥接文件：<br>
  如果是OC工程，新建Swift文件时会默认创建一个 XXX-Bridging-Header.h的桥接文件。
- 彻底修改工程名称和图标：<br>
  1.直接点击工程名称，回车修改。<br>
  2.换掉原工程的ImageAssets<br>
  3.(Product -> Schema -> Edit Schema
  /
  停止运行图标 （口）工程名称 )
  -> manage Schema -> 找到工程名称回车修改<br>
  4.如果有桥接文件，桥接文件的名称应当修改成新工程的名称
  找到桥接文件 回车修改名称
  点击工程名称 - 找到对应工程配置 - Build Settings - Objective-C Bridging Header 修改成新的桥接文件名称。<br>
5. 如果存在OC引用Swift代码，引用处导入的 旧工程名称-Swift.h 应该改为 新工程名称-Swift.h<br>

- 保存文件到真机上，并且下次仍然能访问：(只有通过Search才能拿到，FileManager什么的 试过 暂时不行)
```swift
        var docUrl = NSSearchPathForDirectoriesInDomains(.documentDirectory, .userDomainMask, true)[0]
        let fileUrl = docUrl + "/test.pdf"
        print("fileUrl:\(fileUrl)")
        let data = NSData(contentsOfFile: fileUrl)
```
- 状态栏的显示和隐藏：<br>
  工程 -> Info -> Status bar is initially hidden ->YES/NO

- 设备横竖屏支持：<br>
  工程（Target）-> General -> Device Orientation 
  Tip:有iphone 和 ipad两个选择， 如果是开发横屏应用，注意都要设置。

- Alert:
```swift
let alert = UIAlertController(title: "Tip", message: "do you want to delete?", preferredStyle: .alert)
let confirm = UIAlertAction(title: "Confirm", style: .cancel, handler: nil)
alert.addAction(confirm)
self.present(alert, animated: true, completion: nil)
```
- 关闭ViewController <br>
    当你的控制器时push进去的时候，用pop的方法退出，当你的控制器时present进去的时候，用dismissViewControllerAnimated的方法 

- 根据path获取图片
```swift
var path = "xxxx.jpg"
var image = UIImage(contentsOfFile: path.getDocumentPath())
```

- tableView分割线 两边的间距如何调整
```swift
tableView.separatorInset = .zero
```

- ipad,iphone 系统升级之后 xcode需要添加新的包

    1.上网搜索并下载对应的包 如：11.3(15E217) 大约10M以下

    2.拷贝至 Finder > 应用程序 > XCode > 显示包内容 > "Contents / Developer /"