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