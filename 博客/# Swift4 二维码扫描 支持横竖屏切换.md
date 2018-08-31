# Swift4 二维码扫描 支持横竖屏切换

> 网上二维码扫描的轮子实在是太多了，为啥还要自己写呢？实在是因为没有找到合适的，找了十几二十个轮子， swift 、oc的都找了，全都不支持横竖屏切换，所以只能自己造了。
>
> 这是一款使用Swift4编写的二维码扫描器，支持二维码/条形码的扫描，支持横竖屏切换，支持连续扫描，可识别框内，使用超简单，可扩展性强，很适合需要高度自定义小伙小姑凉们。

因为不太喜欢storyBoard、xib那一套，所以界面是纯手写的，所以很方便移植哈~

## 使用

[代码地址：https://github.com/sunflowerseat/SwiftQRCode](https://github.com/sunflowerseat/SwiftQRCode)

先贴一下使用方法，实在太简单，就一个ViewController，根本懒得弄什么pod，直接把SwiftQRCodeVC拷贝过来用就ok了， 记得要给权限 `Privacy - Camera Usage Description`

备注：声音文件需要右键点击项目名称，add Files to "项目名称"，acc文件是无效的

扫描完成后，在`metadataOutput`方法中写回调 ，默认是连续扫描的

### 自定义

里面有些属性方便自定义

| 属性名称              | 属性含义                 |
| --------------------- | ------------------------ |
| scanAnimationDuration | 扫描时长                 |
| needSound             | 扫描结束是否需要播放声音 |
| scanWidth             | 扫描框宽度               |
| scanHeight            | 扫描框高度               |
| isRecoScanSize        | 是否仅识别框内           |
| scanBoxImagePath      | 扫描框图片               |
| scanLineImagePath     | 扫描线图片               |
| soundFilePath         | 声音文件                 |



## 代码

```swift
//
//  SwiftQRCodeVC.swift
//
//  Created by fancy on 18/9/1.
//  Copyright © 2018年 fancy. All rights reserved.
//

import UIKit
import AVFoundation

private let scanAnimationDuration = 3.0//扫描时长
private let needSound = true //扫描结束是否需要播放声音
private let scanWidth : CGFloat = 300 //扫描框宽度
private let scanHeight : CGFloat = 300 //扫描框高度
private let isRecoScanSize = true //是否仅识别框内
private let scanBoxImagePath = "QRCode_ScanBox" //扫描框图片
private let scanLineImagePath = "QRCode_ScanLine" //扫描线图片
private let soundFilePath = "noticeMusic.caf" //声音文件

class SwiftQRCodeVC: UIViewController{
    
    var scanPane: UIImageView!///扫描框
    var scanPreviewLayer : AVCaptureVideoPreviewLayer! //预览图层
    var output : AVCaptureMetadataOutput!
    var scanSession :  AVCaptureSession?
    
    lazy var scanLine : UIImageView = {
            let scanLine = UIImageView()
            scanLine.frame = CGRect(x: 0, y: 0, width: scanWidth, height: 3)
            scanLine.image = UIImage(named: scanLineImagePath)
            return scanLine
            
    }()
    
    override func viewDidLoad(){
        super.viewDidLoad()
        
        //初始化界面
        self.initView()
        
        //初始化ScanSession
        setupScanSession()
    }
    
    override func viewWillAppear(_ animated: Bool){
        super.viewWillAppear(animated)
        startScan()
    }
    
    //初始化界面
    func initView()  {
        scanPane = UIImageView()
        scanPane.frame = CGRect(x: 300, y: 100, width: 400, height: 400)
        scanPane.image = UIImage(named: scanBoxImagePath)
        self.view.addSubview(scanPane)
        
        //增加约束
        addConstraint()
        
        scanPane.addSubview(scanLine)
    }
    
    func addConstraint() {
        scanPane.translatesAutoresizingMaskIntoConstraints = false
        //创建约束
        let widthConstraint = NSLayoutConstraint(item: scanPane, attribute: .width, relatedBy: .equal, toItem: nil, attribute: .notAnAttribute, multiplier: 0, constant: scanWidth)
        let heightConstraint = NSLayoutConstraint(item: scanPane, attribute: .height, relatedBy: .equal, toItem: nil, attribute: .notAnAttribute, multiplier: 0, constant: scanHeight)
        let centerX = NSLayoutConstraint(item: scanPane, attribute: .centerX, relatedBy: .equal, toItem: view, attribute: .centerX, multiplier: 1.0, constant: 0)
        let centerY = NSLayoutConstraint(item: scanPane, attribute: .centerY, relatedBy: .equal, toItem: view, attribute: .centerY, multiplier: 1.0, constant: 0)
        //添加多个约束
        view.addConstraints([widthConstraint,heightConstraint,centerX,centerY])
    }
    
    
    //初始化scanSession
    func setupScanSession(){
        
        do{
            //设置捕捉设备
            let device = AVCaptureDevice.default(for: AVMediaType.video)!
            //设置设备输入输出
            let input = try AVCaptureDeviceInput(device: device)
            
            let output = AVCaptureMetadataOutput()
            output.setMetadataObjectsDelegate(self, queue: DispatchQueue.main)
            self.output = output
            
            //设置会话
            let  scanSession = AVCaptureSession()
            scanSession.canSetSessionPreset(.high)
            
            if scanSession.canAddInput(input){
                scanSession.addInput(input)
            }
            
            if scanSession.canAddOutput(output){
                scanSession.addOutput(output)
            }
            
            //设置扫描类型(二维码和条形码)
            output.metadataObjectTypes = [
                .qr,
                .code39,
                .code128,
                .code39Mod43,
                .ean13,
                .ean8,
                .code93
            ]
            //预览图层
            let scanPreviewLayer = AVCaptureVideoPreviewLayer(session:scanSession)
            scanPreviewLayer.videoGravity = AVLayerVideoGravity.resizeAspectFill
            scanPreviewLayer.frame = view.layer.bounds
            self.scanPreviewLayer = scanPreviewLayer
            
            setLayerOrientationByDeviceOritation()
            
            //保存会话
            self.scanSession = scanSession
            
        }catch{
            //摄像头不可用
            self.confirm(title: "温馨提示", message: "摄像头不可用", controller: self)
            return
        }
        
    }
    
    func setLayerOrientationByDeviceOritation() {
        if(scanPreviewLayer == nil){
            return
        }
        scanPreviewLayer.frame = view.layer.bounds
        view.layer.insertSublayer(scanPreviewLayer, at: 0)
        let screenOrientation = UIDevice.current.orientation
        if(screenOrientation == .portrait){
            scanPreviewLayer.connection?.videoOrientation = .portrait
        }else if(screenOrientation == .landscapeLeft){
            scanPreviewLayer.connection?.videoOrientation = .landscapeRight
        }else if(screenOrientation == .landscapeRight){
            scanPreviewLayer.connection?.videoOrientation = .landscapeLeft
        }else if(screenOrientation == .portraitUpsideDown){
            scanPreviewLayer.connection?.videoOrientation = .portraitUpsideDown
        }else{
            scanPreviewLayer.connection?.videoOrientation = .landscapeRight
        }
        
        //设置扫描区域
        NotificationCenter.default.addObserver(forName: NSNotification.Name.AVCaptureInputPortFormatDescriptionDidChange, object: nil, queue: nil, using: { (noti) in
            if(isRecoScanSize){
                self.output.rectOfInterest = self.scanPreviewLayer.metadataOutputRectConverted(fromLayerRect: self.scanPane.frame)
            }else{
                self.output.rectOfInterest = CGRect(x: 0, y: 0, width: 1, height: 1)
            }
        })
    }

    //设备旋转后重新布局
    override func viewDidLayoutSubviews() {
        super.viewDidLayoutSubviews()
        setLayerOrientationByDeviceOritation()
    }
    
    //开始扫描
    fileprivate func startScan(){
        
        scanLine.layer.add(scanAnimation(), forKey: "scan")
        guard let scanSession = scanSession else { return }
        if !scanSession.isRunning
        {
            scanSession.startRunning()
        }
    }
    
    //扫描动画
    private func scanAnimation() -> CABasicAnimation{
        
        let startPoint = CGPoint(x: scanLine .center.x  , y: 1)
        let endPoint = CGPoint(x: scanLine.center.x, y: scanHeight - 2)
        
        let translation = CABasicAnimation(keyPath: "position")
        translation.timingFunction = CAMediaTimingFunction(name: kCAMediaTimingFunctionEaseInEaseOut)
        translation.fromValue = NSValue(cgPoint: startPoint)
        translation.toValue = NSValue(cgPoint: endPoint)
        translation.duration = scanAnimationDuration
        translation.repeatCount = MAXFLOAT
        translation.autoreverses = true
        
        return translation
    }
    
    //MARK: -
    //MARK: Dealloc
    deinit{
        ///移除通知
        NotificationCenter.default.removeObserver(self)
    }
    
}


//MARK: -
//MARK: AVCaptureMetadataOutputObjects Delegate
extension SwiftQRCodeVC : AVCaptureMetadataOutputObjectsDelegate
{
    
    //扫描捕捉完成
    func metadataOutput(_ output: AVCaptureMetadataOutput, didOutput metadataObjects: [AVMetadataObject], from connection: AVCaptureConnection) {
        
        //停止扫描
        self.scanLine.layer.removeAllAnimations()
        self.scanSession!.stopRunning()
        
        //播放声音
        if(needSound){
            self.playAlertSound()
        }
        
        //扫描完成
        if metadataObjects.count > 0 {
            if let resultObj = metadataObjects.first as? AVMetadataMachineReadableCodeObject{
                self.confirm(title: "扫描结果", message: resultObj.stringValue, controller: self,handler: { (_) in
                    //继续扫描
                    self.startScan()
                })
            }
        }
    }
    
    //弹出确认框
    func confirm(title:String?,message:String?,controller:UIViewController,handler: ( (UIAlertAction) -> Swift.Void)? = nil){
        
        let alertVC = UIAlertController(title: title, message: message, preferredStyle: .alert)
        
        let entureAction = UIAlertAction(title: "确定", style: .destructive, handler: handler)
        alertVC.addAction(entureAction)
        controller.present(alertVC, animated: true, completion: nil)
        
    }
    
    //播放声音
    func playAlertSound(){
        guard let soundPath = Bundle.main.path(forResource: soundFilePath, ofType: nil)  else { return }
        guard let soundUrl = NSURL(string: soundPath) else { return }
        var soundID:SystemSoundID = 0
        AudioServicesCreateSystemSoundID(soundUrl, &soundID)
        AudioServicesPlaySystemSound(soundID)
    }
}
```




## 总结

界面是有点粗制滥造哈~ 不过没关系啊，在initView里面稍微改改就可以变成你想要的了。

对代码有什么疑问，可以随时来问 ，秋秋邮箱：970201861@qq.com