# 新建一个Flutter项目

https://www.jianshu.com/p/91f1b82957a4

https://www.jianshu.com/p/a482eda71161

- 新建一个普通的Android项目假设路径是 `~/Android/FlutterAndroid`

- 在`~/Android`目录下执行`flutter create -t module vp_flutter`

- 然后在FlutterAndroid项目中 File - New - New module - Import Flutter Module

- 在app/build.gradle中添加以下代码

  ```groovy
  android{
      defaultConfig{
          ndk {
              abiFilters "armeabi-v7a", "x86"
         }
      }
      buildTypes {
            debug {
                ndk {
                  abiFilters "armeabi-v7a", "x86"
                }
            }
            release {
                ndk {
                   abiFilters "armeabi-v7a"
                }   
           }
      }
  }
  ```

  然后同步一下~~

- 

- 然后添加一个FlutterApplication 代码如下：

  ```
  public class FlutterApplication extends Application {
      public FlutterApplication() {
      }
  
      @CallSuper
      public void onCreate() {
          super.onCreate();
          FlutterMain.startInitialization(this);
      }
  }
  ```

  

- 在manifest中加入该类

- 修改Activity代码

  ```Java
  public class MainActivity extends FlutterActivity {
  
      @Override
      protected void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          GeneratedPluginRegistrant.registerWith(this);
      }
  }
  ```

- 直接运行，或者打开flutter module项目 然后运行。后者比较好 可以实现热更新

## 在ios项目中集成 flutter_module

- http://tryenough.com/flutter03

- 新建一个项目 并`init pod`

- 在podFile中增加两行

  ```
  #新添加的代码
  flutter_application_path = '../vp_flutter'
  eval(File.read(File.join(flutter_application_path, '.ios', 'Flutter', 'podhelper.rb')), binding)
  ```

- 然后 `pod install`

- 关闭当前项目 打开后缀为workspace的项目

- 点击项目名称 - Target - 项目名称 - Build Phase - +添加脚本 - New Run Script Phase

- 粘贴如下代码

  ```
  "$FLUTTER_ROOT/packages/flutter_tools/bin/xcode_backend.sh" build
  "$FLUTTER_ROOT/packages/flutter_tools/bin/xcode_backend.sh" embed
  ```

- Build Setting search bitcode 设置成 false

- cmd + b 编译 理论上可以编译通过

- 修改AppDelegate

  ```swift
  import UIKit
  import Flutter
  import FlutterPluginRegistrant // Only if you have Flutter Plugins.
  
  @UIApplicationMain
  class AppDelegate: FlutterAppDelegate {
      
  
      // Only if you have Flutter plugins.
      override func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
          GeneratedPluginRegistrant.register(with: self);
          return super.application(application, didFinishLaunchingWithOptions: launchOptions);
      }
  }
  ```

- Capbilities - Background Modes - ON - 并勾选Background fetch & Remote notifications

- 运行 理论上可以运行至模拟器 但可能会是黑屏。

- 打开vp_flutter 运行至ios设备或模拟器均可。