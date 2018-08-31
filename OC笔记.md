---
title : oc notes
Time  : 2018-05-10
author: sunflowerseat
---

# Objective-c 学习记录

> 仅为个人做一些记录，便于回顾查询。
>
> 内容包含objective-c 语法、封装、快捷键等知识。


## 语法


- 多参数方法

```objective-c
//.h 声明
- (void) sayHello : (NSString*) str : (NSString*) str2;
//.m 实现
- (void)sayHello:(NSString *)str :(NSString *)str2{
}
```

## 代码块

###default

- @interface-category  ` category` 
- @implementation-category ` category`
- @try `try`
- if / ifelse
- Init / initialize / initWithCoder / initWithFrame [作用域在类中]

### custom

- strong / weak /



## Xcode 设置

- 重置xcode

  终端输入`defaults delete com.apple.dt.Xcode` 

  [网页链接](https://blog.csdn.net/qq_27633421/article/details/51202367)

- 显示编译时间

  终端输入`defaults write com.apple.dt.Xcode ShowBuildOperationDuration YES`

- 修改xcode主题

  Xcode -> preference -> 