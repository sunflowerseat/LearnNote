---
title : oc notes
Time  : 2018-05-10
author: sunflowerseat
---

# Xcode操作记录

> 仅为个人做一些记录，便于回顾查询。
>
> 内容包含objective-c 语法、封装、快捷键等知识。

## Xcode添加新的包

`/Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/DeviceSupport`

在该位置添加新的包即可。Tip：Xcode显示包内容



## 如何修改权限提示

- 点击项目 -> Target -> Info -> Privacy - Camera Usage Description ….

## 如何打IPA包

- 先修改版本号！！！
- Edit schema -> Archive 
- Product -> Archive
- export -> Ad Hoc

## 如何上传App到AppStore

- Archive -> upload to appstore -> next next...
- 登录https://itunesconnect.apple.com/login，等待15分钟左右，在活动里面找到所有构建版本。
- 如果已经通过，可以添加出口合规证明。
- AppStore 板块➕一个 `版本或平台`，填写好相关信息
- 在构建版本里面选最新提交上的版本(构建版本如果没有可选项，应该是没有提交新的版本 或者 新的版本正在处理当中)

  ![image-20180704162741133](/var/folders/jk/ghg5rltn3_q5h5l03350dvkh0000gp/T/abnerworks.Typora/image-20180704162741133.png)




## 如何分析错误日志

### 准备工作

- 已打包的IPA文件
- CrashTest工程

### 开始

- 将IPA文件后缀名改为zip，然后解压，找到CtrlDoc.app，复制到`/Users/fancy/Documents/crash/`文件夹下

- 修改下图2处的内容

  ![image-20180704162252348](/var/folders/jk/ghg5rltn3_q5h5l03350dvkh0000gp/T/abnerworks.Typora/image-20180704162252348.png)