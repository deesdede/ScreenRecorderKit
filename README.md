# ScreenRecorderKit

[![CI Status](http://img.shields.io/travis/rickytan/RTRootNavigationController.svg?style=flat)](https://travis-ci.org/rickytan/RTRootNavigationController)
[![License](https://img.shields.io/cocoapods/l/RTRootNavigationController.svg?style=flat)](http://cocoapods.org/pods/RTRootNavigationController)
[![Platform](https://img.shields.io/cocoapods/p/RTRootNavigationController.svg?style=flat)](http://cocoapods.org/pods/RTRootNavigationController)

## 介绍

ScreenRecorderKit是一个基于ReplayKit封装的轻量级录屏框架。

本框架帮助开发者以一种更简单的方式处理App间的录屏冲突，App与系统之间的录屏冲突，以及其他异常的提示。

## 特点

* 轻松的处理App与App的录屏冲突、App与系统录屏的冲突、
* 录屏中的异常问题。
* 开始和结束录屏的方法均有成功和失败的回调。

## 使用

###1. 引用
在你需要调用录屏功能的地方`ScreenRecordManager.h`

设置 ScreenRecordDelegate 代理`[ScreenRecordManager shareManager];
    mgr.screenRecordDelegate = self;`

###2. 开始和结束方法

开始录制

```objective-c
     [[ScreenRecordManager shareManager] screenRecSuc:^{
            
            NSLog(@"录制启动成功");
            [weakself showToast:@"录制启动成功"];
            
        } failure:^(DUErrorHandle *error) {
            
            NSLog(@"%@", error.msg);
            [weakself showToast: error.msg];
            
        }];      
```

 
结束录制

```objective-c
    [[ScreenRecordManager shareManager] stopRecSuc:^{
            
            
            
        } failure:^(DUErrorHandle *error) {
        
            if (error.code == -2) { // -2是没有可以结束的录屏幕的进程
            
            }else{
                
                [weakself showToast: error.msg];
                
            }
            
        }];     
```

###3. 代理方法
`ScreenRecordManager`的录制状态发生了改变

```objective-c
-(void)recStateDidChange:(RecState)state withError:(NSError *)error{
    if (error) {
        
        [self showToast:[error localizedDescription]];
        
    }else{
        
        if (state == RecState_Rec) {
            
            self.recBtn.selected = YES;
            
        }else{
            
            [self showToast:@"录屏结束"];
            self.recBtn.selected = NO;
            
        }
        
    }
    
}
```

`ScreenRecordManager`保存录屏视频到相册成功的代理方法

```objective-c
-(void)savedPhotoImage:(UIImage *)image didFinishSavingWithError:(NSError *)error contextInfo:(void *)contextInfo{
    
    if(error){
        
        [self showToast:[error localizedDescription]];
        
    }else{
        
        [self showToast:@"保存相册成功"];
        
    }
    
}
```

`ScreenRecordManager`录屏视频的二进制文件获取，在这里你可以选择将二进制数转成.mp4文件或者将数据上传至服务器等等。

```objective-c
-(void)savedVideoData:(NSData *)data didFinishSavingWithError:(BOOL)isError{
    
    if (isError) {
        
        [self showToast:@"生成二进制文件错误"];
        
    }else{
        
        [self showToast:@"生成二进制文件成功"];
        
    }
    
}
```



## Requirements

* **iOS 9.0** and up
* **Xcode 7.0** and up

## Installation

ScreenRecorderKit is not available through [CocoaPods](http://cocoapods.org) currently. 

## Author

Huanghong, chinahuanghong@gmail.com

## Apps Integrated

* [夺镖体育](https://itunes.apple.com/cn/app/%E5%A4%BA%E9%95%96/id1294273600?mt=8)

## License

ScreenRecorderKit is available under the MIT license. See the LICENSE file for more info.
