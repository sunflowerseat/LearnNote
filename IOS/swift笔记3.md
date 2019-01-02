#Swift笔记3

##泛型

- protocol 抽象泛型

  ```Swift
  protocol OfflineParamModel: class {
      associatedtype T
  }
  
  
  class LocalSampleCreate : OfflineParamModel{
      typealias T = ProjectSample
  }
  ```

- Class 定义泛型

  ```Swift
  class OfflineParamModel<T : Any> {
  }
  ```




##  cell选中状态背景色

1. cell.selectionStyle = UITableViewCellSelectionStyleNone;  

2. cell.selectedBackgroundView = [[[UIImageView alloc] initWithImage:[UIImage imageNamed:@"cellart.png"]] autorelease];   

3. 还有字体颜色   

4. cell.textLabel.highlightedTextColor = [UIColor xxxcolor];  [cell.textLabel setTextColor:color];//设置cell的字体的颜色  

### 设置tableViewCell间的分割线的颜色

 `  [theTableView setSeparatorColor:[UIColor xxxx ]]`

   链接地址

   5.pop返回table时，cell自动取消选中状态

   首先我有一个UITableViewController，其中每个UITableViewCell点击后都会push另一个ViewController，每次点击Cell的时候，Cell都会被选中，当从push的ViewController返回的时候选中的Cell便会自动取消选中。后来由于某些原因我把这个UITableViewController改成了UIViewController，之后就产生了一个问题：每次返回到TableView的时候，之前选中的Cell不能自动取消选中，经过查找得知：

   UITableViewController有一个clearsSelectionOnViewWillAppear的property，

   而当把UITableViewController修改成UIViewController后，这个属性自然就不存在了，因此我们必须手动添加取消选中的功能，方法很简单，在viewWillAppear方法中加入：

```objc
   [self.tableView deselectRowAtIndexPath:[self.tableView indexPathForSelectedRow] animated:YES];
```
## 输入框只允许输入数字

```swift
xxtf.keyboardType = .numberPad 
xxtf.delegate = self

extension XXX: UITextFieldDelegate {
    public func textField(_ textField: UITextField, shouldChangeCharactersIn range: NSRange, replacementString string: String) -> Bool {
        /// 如果是小数文本框则执行下面操作，如果是文字文本框 不需要做任何限制
        if textField.keyboardType == .numberPad {

            //限制长度
            if(textField.text!.count > 2 && string != ""){
                return false
            }
            //限制 仅能输入数字
            let scanner = Scanner(string: string)
            let numbers = NSCharacterSet(charactersIn: "0123456789")
            if !scanner.scanCharacters(from: numbers as CharacterSet, into: nil) && string.count != 0 {
                return false
            }
        }

        return true
    }
}
```

