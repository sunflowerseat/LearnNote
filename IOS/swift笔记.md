---
title : oc notes
Time  : 2018-05-10
author: sunflowerseat

---

# Swift 学习记录

## 代码块

- Swift-async `async`



## java 代码转swift 

primaryKey -> primary

Description -> descriptions

File -> Data



## 处理键盘遮挡

```Swift
//简单处理
extension DefectReviewVC : UITextFieldDelegate{
	func textFieldDidBeginEditing(_ textField: UITextField) {
		UIView.animate(withDuration: 0.4) {
			
		}
	}
	
	func textFieldDidEndEditing(_ textField: UITextField) {
		UIView.animate(withDuration: 0.4) {
			self.view.frame.origin.y = 0
		}
	}
}

//Scroll 和 根据TF高度处理
extension ItpActivityView : UITextFieldDelegate{
	func textFieldDidBeginEditing(_ textField: UITextField) {
		let relate = textField.convert(textField.bounds.origin, from: app.window)
		UIView.animate(withDuration: 0.4) {
			self.scroll.frame.origin.y = relate.y + 300
			self.content.frame.origin.y = relate.y + 300
		}
	}
	
	func textFieldDidEndEditing(_ textField: UITextField) {
		UIView.animate(withDuration: 0.4) {
			self.scroll.frame.origin.y = 0
			self.content.frame.origin.y = 0
		}
	}
}
```

## 打印Response

`debugPrint(NSString(data: rep.data!, encoding: String.Encoding.utf8.rawValue)) `

## 为VC新建一个带参数的init方法

`带参数init`

```swif
convenience init(callback : @escaping (String) -> Void) {
	self.init(nibName: nil, bundle: nil)
	self.callback = callback
}

override init(nibName nibNameOrNil: String?, bundle nibBundleOrNil: Bundle?) {
	super.init(nibName: nibNameOrNil, bundle: nibBundleOrNil)
}
```



## 监听返回按钮

监听系统返回按钮

```Swift
import UIKit

class MyNavigationController: UINavigationController ,UINavigationBarDelegate{
    func navigationBar(_ navigationBar: UINavigationBar, shouldPop item: UINavigationItem) -> Bool {
        var shouldPop = true
        if let viewController = topViewController as? NavigationControllerBackButtonDelegate {
            shouldPop = viewController.shouldPopOnBackButtonPress()
        }

        if(shouldPop){
            self.popViewController(animated: true)
        }

        return shouldPop
    }
}

protocol NavigationControllerBackButtonDelegate {
    func shouldPopOnBackButtonPress() -> Bool
}
```



`ViewController`继承`NavigationControllerBackButtonDelegate`

```Swift

	func shouldPopOnBackButtonPress() -> Bool {
		let alert = MyAlertController(title: "A", message: "AA") { success in
			if(success){
				app.popVC()
			}
		 }
		alert.showIn(vc: self)
		return false
	}
```

