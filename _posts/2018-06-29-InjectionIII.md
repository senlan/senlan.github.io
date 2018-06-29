---
layout: post
title: "Xode界面调试软件InjectionIII"
date:       2018-06-29
author:     "Senlan"
header-img: "img/post_bg.jpg"
---

优化Xcode开发时的速度，可以在不用编译的情况下看到代码的效果。先提供一下几个地址：
[Injection链接](http://johnholdsworth.com/injection.html)
[Mac App Store下载地址](https://itunes.apple.com/us/app/injectioniii/id1380446739?ls=1&mt=12)
[Injection：iOS热重载背后的黑魔法](http://www.cocoachina.com/ios/20180613/23780.html)
[参考文章:Injection III, the App  ](http://johnholdsworth.com/injection.html)
1、在AppDelegate的didFinishLaunchingWithOptions方法里添加一下代码
```
if DEBUG
Bundle(path: "/Applications/InjectionIII.app/Contents/Resources/iOSInjection.bundle")?.load()
//for tvOS:
Bundle(path: "/Applications/InjectionIII.app/Contents/Resources/tvOSInjection.bundle")?.load()
//Or for macOS:
Bundle(path: "/Applications/InjectionIII.app/Contents/Resources/macOSInjection.bundle")?.load()
endif
```
2、在需要调试的controller上添加一下方法
```
@objc func injected() {
print("I've been injected: \(self)")
self.reloadInjected()
}
```
**注意事项：
在swift3.0 后，Xcode编译做了优化，使用了Whole-Module Optimization（全模块优化，以下简称 WMO），需要禁用WMO，才能运行成功**
设置步骤：Targets-->Build Setting-->Compilation Mode-->Single File
![image.png](https://upload-images.jianshu.io/upload_images/1463975-c9559ec18262c840.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

【那WMO是什么呢？
补充：Whole-Module Optimization（全模块优化，以下简称 WMO），即在编译项目时，将同属于一个 Module（可以理解为一个 Target、一个 Package）的所有源代码都串起来，进行整体的一个分析与优化，区别于 Single-File Optimization（单文件优化，以下简称 SFO），WMO 可以更好的统筹全局，去 inline 函数调用、排除死函数（即写了却从不调用的函数）等等，大幅优化最终目标文件，使性能提升 2~5 倍。参考：[评测Swift 3.0项目编译优化选项对编译速度的影响](https://news.cnblogs.com/n/555739/)
】


