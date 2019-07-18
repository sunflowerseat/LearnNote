## 新建Swift项目

新建工程删除Main.Storybord & LaunchScreen.storyboard
直接cmd+del删除这两个文件
单机工程名称删除Main Interface 和 Launch Screen File

在AppDelegate中加上：

``` swift
window = UIWindow(frame: UIScreen.main.bounds)

window.makeKeyAndVisible()

window.rootViewController = XXVC()

```

新建IOS

```objective-c

    self.window = [[UIWindow alloc]initWithFrame:UIScreen.mainScreen.bounds];
    [self.window makeKeyAndVisible];
    self.window.rootViewController = [[ViewController alloc]init];
```

