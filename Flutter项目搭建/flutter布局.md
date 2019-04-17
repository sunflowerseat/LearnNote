参考https://www.jianshu.com/u/312aad1f1c8b

#Flutter布局

## MaterialApp

### Scaffold

Scaffold 有下面几个主要属性：

- appBar：显示在界面顶部的一个 AppBar，也就是 Android 中的 ActionBar 、Toolbar
- body：当前界面所显示的主要内容 Widget
- floatingActionButton：纸墨设计中所定义的 FAB，界面的主要功能按钮
- persistentFooterButtons：固定在下方显示的按钮，比如对话框下方的确定、取消按钮
- drawer：侧边栏控件
- backgroundColor： 内容的背景颜色，默认使用的是 ThemeData.scaffoldBackgroundColor 的值
- bottomNavigationBar： 显示在页面底部的导航栏
- resizeToAvoidBottomPadding：类似于 Android 中的 android:windowSoftInputMode=”adjustResize”，控制界面内容 body 是否重新布局来避免底部被覆盖了，比如当键盘显示的时候，重新布局避免被键盘盖住内容。默认值为 true

Read more: <http://blog.chengyunfeng.com/?p=1042#ixzz5hpHvnd19>

Tip:可以设置AppBar 如果要全屏 appBar:null即可

### Container

官方给出的简介，是一个结合了绘制（painting）、定位（positioning）以及尺寸（sizing）widget的widget。

未设置宽高，无子控件时，充满父布局，有子控件 是子控件的宽高

尺寸：

```Dart
constraints: new BoxConstraints.expand(
  height:340,
  width: 300,
)
```

背景色，背景图片，圆角，前景图片等

```Dart
color: Colors.orange,
decoration: BoxDecoration(
              color: Colors.orange,
              image: DecorationImage(image: AssetImage("images/test.jpg")),
              borderRadius: BorderRadius.all(Radius.circular(8)),
            ),
foregroundDecoration: BoxDecoration(
              color: Colors.red
            ),
```

Padding,margin,alignment

```dart
padding: EdgeInsets.all(8.0),
            margin: EdgeInsets.all(10),
            alignment: Alignment.center,
```

### TextField

TextField只能在Material元素包裹内使用，如果最外层是MaterialApp相关元素则无需包裹。

### Padding

设置Padding

### Align

控件的对齐方式

```Dart
Align(
                alignment: Alignment.center,
                widthFactor: 2.0,
                heightFactor: 2.0,
                child: new Text("Align"),
              )
```

### SizedBox

设置控件的宽高

```Dart
SizedBox(
                    width: 50,
                    height: 50,
                    child: Text("Align",textAlign: TextAlign.center,),
                  )
```



### Card

一个有圆角 和阴影的控件

```Dart
Card(
                  child: Text('Hello World'),
                  elevation: 10,

                )
```



### FittedBox

子元素填充方式 以及位置

```Dart
new FittedBox(
                fit: BoxFit.fitWidth,
                alignment: Alignment.bottomCenter,
                child: new Container(
                  color: Colors.red,
                  child: new Text("FittedBox"),
                ),
              )
```



### AspectRatio

设置控件宽高比

```Dart
new AspectRatio(
                aspectRatio: 1.5,
                child: new Container(
                  color: Colors.red,
                ),
              )
```

### FractionallySizedBox

宽高因子 在需要设置子布局占父布局 百分之多少时 可用

### Offstage

决定控件是否可见 ,offstage:true 不可见 false可见

```Dart
Offstage(
            offstage: true,
            child: Container(
              color: Colors.blue,
              child: Text("AA"),
            ),
          )
```

我们一定要清楚一件事情，Offstage并不是通过插入或者删除自己在widget tree中的节点，来达到显示以及隐藏的效果，而是通过设置自身尺寸、不响应hitTest以及不绘制，来达到展示与隐藏的效果

### OverflowBox

允许child超出parent的范围显示

```Dart
Container(
      color: Colors.green,
      width: 200.0,
      height: 200.0,
      padding: const EdgeInsets.all(5.0),
      child: OverflowBox(
        alignment: Alignment.topLeft,
        maxWidth: 300.0,
        maxHeight: 500.0,
        child: Container(
          color: Color(0x33FF00FF),
          width: 400.0,
          height: 400.0,
        ),
      ),
    );
```

### Transform

平移 缩放 旋转

```Dart
Center(
      child: Transform(
//        origin: Offset(50, 50),
        transform: Matrix4.rotationZ(3.14 * 0.25),
        child: Container(
          color: Colors.blue,
          width: 100.0,
          height: 100.0,
        ),
      ),
    );
```



### Expanded+Row+Column

Flex布局 弹性 mainAxisSize 主轴长度。 MainAxisAlignment主轴分布

### Stack层叠布局

相当于绝对布局 帧布局 可以设置子布局的alignment

```Dart
Stack(
        alignment: Alignment.bottomCenter,
        children: [
          CircleAvatar(
            backgroundImage: AssetImage('images/pic.jpg'),
            radius: 100.0,
          ),
          Container(
            width: 20,
            height: 20,
            color: Colors.black,
          ),
        ],
      )
```

### ListView

可以横向 可以纵向

```Dart
ListView.builder(
  scrollDirection: Axis.horizontal,
  itemCount: 10,
  itemExtent: 200,//每个item在主轴上的size
  itemBuilder: (context, index) {
    return ListTile(
      title: Text("$index"),
    );
  },
)
```

### Flow

流布局 在某些时候比较有用

```Dart
class TestFlowDelegate extends FlowDelegate {
  EdgeInsets margin = EdgeInsets.zero;

  TestFlowDelegate({this.margin});
  @override
  void paintChildren(FlowPaintingContext context) {
    var x = margin.left;
    var y = margin.top;
    for (int i = 0; i < context.childCount; i++) {
      var w = context.getChildSize(i).width + x + margin.right;
      if (w < context.size.width) {
        context.paintChild(i,
            transform: new Matrix4.translationValues(
                x, y, 0.0));
        x = w + margin.left;
      } else {
        x = margin.left;
        y += context.getChildSize(i).height + margin.top + margin.bottom;
        context.paintChild(i,
            transform: new Matrix4.translationValues(
                x, y, 0.0));
        x += context.getChildSize(i).width + margin.left + margin.right;
      }
    }
  }

  @override
  bool shouldRepaint(FlowDelegate oldDelegate) {
    return oldDelegate != this;
  }
}
```

### Wrap

Wrap控件非常有用，一行放不下的时候 会自动多行

### Chip

很棒的一个小控件 可以做button 自动圆角 还可以设置图标或者其他任何Widget。。。 以及删除按钮。

### CircleAvatar

一个圆形控件 非常棒的控件  可以设置背景图片 已经其他内容 。很好！

```dart
CircleAvatar(
        child: Text("AA"),
        backgroundImage: AssetImage(""),
      )
```



