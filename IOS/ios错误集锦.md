- close接口测试失败原因：
    defectId 填错了， 3780 填成了 3708
- Source type 1 not available 模拟器没有摄像头
- 平板运行UIAlertView报错，需定义一个显示位置
  [参考网页](http://blog.csdn.net/tsyccnh/article/details/52737367)
- selector内容没执行：没有开启交互
- ImageView添加手势没有执行，没有开启交互
- imageView , 子类扩展时， setNeedDisplay，是不会走draw方法的。
- UIButton字体颜色设置失败：`
   clearBtn.titleLabel?.textColor = Color.btn_blue`
   这样设置是不行的，应该用setTitleColor