---
time : 2018-05-10
---

#IOS setOnclick - 点击事件完美扩展

> 在Android中点击事件是以setOnclick的形式进行设置的，用起来十分方便，而在ios中是以addTarget方式进行的，每次设置点击事件都需要声明一个新的方法，在大部分情况下显得未免有些麻烦。而且通常来说我们使用的最多的是`TouchUpInside`方式的点击事件，所以为了方便使用，我对UIButton/UIView的点击事件进行了扩展。

使用
--
和之前一样，我们先来看看扩展之后如何使用

- oc版本
```objective-c
[_btn1 setOnclick:^{
    NSLog(@"click btn1");
}];
```
- swift版本
```swift
btn.setOnClick {
	print("click btn")
}
```

用起来真的是非常简单哈~ 

扩展过程
--
接下来我们就来看看，到底是如何扩展的呢？

- oc版本
    我们只需要为UIButton添加一个Category就可以使用了哦。

```objective-c
    #import <UIKit/UIKit.h>

    @interface UIButton(click)
    @property (nonatomic, strong) void (^clickBlock) (void);
    - (void) setOnclick : (void (^)(void))block;
    - (void) clickBtn : (UIButton*) sender;
    - (void) setTarget : action:(SEL)action;
    @end
    
```

```objective-c
    #import "UIButton+click.h"
    #import <objc/runtime.h>
    
    @implementation UIButton(click)
    
    static void *clickKey = &clickKey;
    - (void)setClickBlock:(void (^)(void))clickBlock{
        objc_setAssociatedObject(self, & clickKey, clickBlock, OBJC_ASSOCIATION_COPY);
    }
    
    - (void (^)(void))clickBlock{
        return objc_getAssociatedObject(self, &clickKey);
    }
    
    -(void)setOnclick:(void (^)(void))block{
        self.clickBlock = block;
        [self addTarget:self action:@selector(clickBtn:) forControlEvents:UIControlEventTouchUpInside];
    }
    
    - (void) clickBtn : (UIButton*) sender{
        self.clickBlock();
    }
    
    @end
```

如果我们不希望每次都需要导入`UIButton+click.h`,只需要将`UIButton+click.h`添加到pch文件中就可以了哦。

- swift版本
    相对来说swift版本就比较麻烦一点，竟然不能直接扩展闭包类型的属性，所以最后多创建了一个UIClick对象。

    
```swift
    class UIClick : Any{
    	var click : () -> Void = {return}
    }
    
    extension UIButton : Property{
    	var saveClick : UIClick{
    		get{
    			return get0()
    		}
    		set{
    			return set0(newValue)
    		}
    	}
    	
    	func setOnClick(click : @escaping () -> Void) {
    		self.saveClick = UIClick()
    		self.saveClick.click = click
    		self.addTarget(self, action: #selector(btnClick), for: .touchUpInside)
    	}
    	
    	@objc func btnClick(){
    		self.saveClick.click()
    	}
    }
```


UIView onClick
--
看了以上的扩展过程，相信大家对UIView onClick的扩展心中也有数了，过程基本是一样的,接下来只简单写一下不同的部分。
```swift
    func setOnClickView(click : @escaping () -> Void) {
        self.isUserInteractionEnabled = true
        self.saveClickView = UIClick()
        self.saveClickView.click = click
        let tap = UITapGestureRecognizer(target: self, action: #selector(btnClickView))
        tap.numberOfTapsRequired = 1
        self.addGestureRecognizer(tap)
    }
```
oc的就略过了，原理是一样的，代码也是非常简单。

总结
--
扩展并不复杂，但是确实还是带来了不少方便，希望这种扩展思路能够让你眼前一亮，以上内容有任何错误欢迎指正。
​    

作者  [sunflowereat]("https://github.com/sunflowerseat")



