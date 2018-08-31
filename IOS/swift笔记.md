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
extension DefectReviewVC : UITextFieldDelegate{
	func textFieldDidBeginEditing(_ textField: UITextField) {
		UIView.animate(withDuration: 0.4) {
			if(textField == self.commentTextField){
				self.view.frame.origin.y = -390
			}else{
				self.view.frame.origin.y = -150
			}
		}
	}
	
	func textFieldDidEndEditing(_ textField: UITextField) {
		UIView.animate(withDuration: 0.4) {
			self.view.frame.origin.y = 0
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

