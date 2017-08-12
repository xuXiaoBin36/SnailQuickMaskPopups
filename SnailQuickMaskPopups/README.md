# SnailQuickMaskPopups 
![enter image description here](https://img.shields.io/badge/pod-v1.0.0-brightgreen.svg)
![enter image description here](https://img.shields.io/badge/platform-iOS%207.0%2B-ff69b5152950834.svg) 
<a href="https://github.com/snail-z/OverlayController-Swift/blob/master/LICENSE"><img src="https://img.shields.io/badge/license-MIT-green.svg?style=flat"></a>
  
 快速创建蒙版并支持弹出自定义的view，可以设置蒙版样式、过渡效果、还支持手势拖动、弹性动画等等。简单快捷，方便使用!
 参考下面的example ☟
  
#### _[Swift version is here.](https://github.com/snail-z/OverlayController-Swift) - [OverlayController-Swift](https://github.com/snail-z/OverlayController-Swift)_

## Installation

SnailQuickMaskPopups is available through [CocoaPods](http://cocoapods.org). To install it, simply add the following line to your Podfile:  
  
          pod 'SnailQuickMaskPopups', '~> 1.0.1'
      
      
## Example 
![image](https://github.com/snail-z/SnailQuickMaskPopups/blob/master/sample/alert%20style.gif)
![image](https://github.com/snail-z/SnailQuickMaskPopups/blob/master/sample/WeChat%20style.gif)
![image](https://github.com/snail-z/SnailQuickMaskPopups/blob/master/sample/Qzone%20style.gif)  
![image](https://github.com/snail-z/SnailQuickMaskPopups/blob/master/sample/Shared%20style.gif)
![image](https://github.com/snail-z/SnailQuickMaskPopups/blob/master/sample/Sidebar%20style.gif)
![image](https://github.com/snail-z/SnailQuickMaskPopups/blob/master/sample/Full%20style.gif)


## Import
 ``` objc
    #import "SnailQuickMaskPopups.h"
 ```  

## Update 
* 更新过渡动画
```objc
typedef NS_ENUM(NSInteger, TransitionStyle) {
    // 从中心点变大
    TransitionStyleFromCenter
};
``` 
* 为视图弹出时添加弹性动画，通过修改属性dampingRatio回弹阻尼比的值来设置，使用了系统方法usingSpringWithDamping动画
```objc
// - usingSpringWithDamping的范围为0.0f到1.0f，数值越小「弹簧」的振动效果越明显
// - initialSpringVelocity表示初始的速度，数值越大一开始移动越快
[UIView animateWithDuration:duration
                          delay:0.0
         usingSpringWithDamping:0.6
          initialSpringVelocity:0.0
                        options:UIViewAnimationOptionCurveLinear
                     animations:^{
                     } completion:^(BOOL finished) {
                     }];  
```  
* 修改接口api，简化方法和枚举值并增加详细注释，具体如下   
```objc
// 蒙版样式
typedef NS_ENUM(NSUInteger, MaskStyle) {
    // 黑色半透明效果
    MaskStyleBlackTranslucent = 0,
    // 黑色半透明模糊效果
    MaskStyleBlackBlur,
    // 白色半透明模糊效果
    MaskStyleWhiteBlur,
    // 白色不透明的效果
    MaskStyleWhite,
    // 全透明的效果
    MaskStyleClear
};

// 呈现样式
typedef NS_ENUM(NSUInteger, PresentationStyle) {
    // 居中显示，类似UIAlertView
    PresentationStyleCentered = 0,
    // 显示在底部，类似UIActionSheet
    PresentationStyleBottom,
    // 显示在顶部
    PresentationStyleTop,
    // 显示在左边，类似侧滑栏
    PresentationStyleLeft,
    // 显示在右面
    PresentationStyleRight
};

// 过渡样式
/// ! 若PresentationStyle不是'居中样式(Centered)' 该设置是无效的
typedef NS_ENUM(NSInteger, TransitionStyle) {
    // 淡入淡出
    TransitionStyleCrossDissolve = 0,
    // 轻微缩放效果
    TransitionStyleZoom,
    // 从顶部滑出
    TransitionStyleFromTop,
    // 从底部滑出
    TransitionStyleFromBottom,
    // 从左部滑出
    TransitionStyleFromLeft,
    // 从右部滑出
    TransitionStyleFromRight
};

```
```objc
@property (nonatomic, assign) PresentationStyle presentationStyle;  // 呈现样式，默认值是PresentationStyleCentered
@property (nonatomic, assign) TransitionStyle transitionStyle;      // 过渡样式，默认值是TransitionStyleCrossDissolve
@property (nonatomic, assign) CGFloat maskAlpha;                    // 蒙版透明度，默认值0.5
@property (nonatomic, assign) CGFloat dampingRatio;                 // 视图呈现时是否设置回弹动画效果，默认值1.0 (当'springDampingRatio'值为1.0时没有动画回弹效果；当该值小于1.0时，则开启回弹动画效果)
@property (nonatomic, assign) NSTimeInterval animateDuration;       // 动画持续时间，默认值0.25
@property (nonatomic, assign) BOOL isAllowMaskTouch;                // 蒙版是否可以响应事件，默认值YES
@property (nonatomic, assign) BOOL isAllowPopupsDrag;               // 是否允许弹出视图响应拖动事件，默认值NO
@property (nonatomic, assign) BOOL isDismissedOppositeDirection;    // 是否反方向消失，默认值NO
```
```objc
/**
 popupsWithMaskStyle: aView:
 
 - parameter maskStyle: 设置蒙版样式
 - parameter aView:     设置要弹出的视图
 */
+ (instancetype)popupsWithMaskStyle:(MaskStyle)maskStyle
                              aView:(UIView *)aView;

/**
 presentInView: animated: completion
 
 - parameter superview:  将蒙版添加在superview上，若superview为nil，则显示在window上
 - parameter animated:   显示时是否需要动画，默认YES
 - parameter completion: 视图显示完成的回调
 */
- (void)presentInView:(nullable UIView *)superview
             animated:(BOOL)animated
           completion:(void (^ __nullable)(SnailQuickMaskPopups *popups))completion;

/**
 presentAnimated: completion
 ! 视图显示在window上
 
 - parameter animated:   显示时是否需要动画
 - parameter completion: 视图显示完成的回调
 */
- (void)presentAnimated:(BOOL)animated
             completion:(void (^ __nullable)(SnailQuickMaskPopups *popups))completion;

/**
 dismissAnimated: completion
 
 - parameter animated:   隐藏时是否需要动画
 - parameter completion: 视图已经消失的回调
 */
- (void)dismissAnimated:(BOOL)animated
             completion:(void (^ __nullable)(SnailQuickMaskPopups *popups))completion;
```
```objc
@protocol SnailQuickMaskPopupsDelegate

@optional
/**
 SnailQuickMaskPopupsDelegate
 
 - snailQuickMaskPopupsWillPresent: 视图将要呈现
 - snailQuickMaskPopupsWillDismiss: 视图将要消失
 ! 这两个是'present'和'dismiss'方法中completion对应的代理方法，completion优先处理
 - snailQuickMaskPopupsDidPresent: 视图已经呈现
 - snailQuickMaskPopupsDidDismiss: 视图已经消失
 */
- (void)snailQuickMaskPopupsWillPresent:(SnailQuickMaskPopups *)popups;
- (void)snailQuickMaskPopupsWillDismiss:(SnailQuickMaskPopups *)popups;
- (void)snailQuickMaskPopupsDidPresent:(SnailQuickMaskPopups *)popups;
- (void)snailQuickMaskPopupsDidDismiss:(SnailQuickMaskPopups *)popups;

```
  
## Usage
 *  实例化SnailQuickMaskPopups传入自定义的view并设置遮罩样式，然后将视图弹出
``` objc
    _popups = [SnailQuickMaskPopups popupsWithMaskStyle:MaskStyleBlackBlur aView:v];
    _popups.presentationStyle = PresentationStyleCentered;
    _popups.transitionStyle = TransitionStyleFromTop;
    _popups.isDismissedOppositeDirection = YES;
    _popups.isAllowMaskTouch = NO;
    _popups.dampingRatio = 0.5;
    _popups.delegate = self;
    [_popups presentWithAnimated:YES completion:NULL];
 ```
* 实现代理方法
```objc
- (void)snailQuickMaskPopupsWillPresent:(SnailQuickMaskPopups *)popups {
    // do something
}

- (void)snailQuickMaskPopupsWillDismiss:(SnailQuickMaskPopups *)popups {
    // do something
}

- (void)snailQuickMaskPopupsDidPresent:(SnailQuickMaskPopups *)popups {
    // do something
}

- (void)snailQuickMaskPopupsDidDismiss:(SnailQuickMaskPopups *)popups {
    // do something
}
```  

## License

SnailQuickMaskPopups is distributed under the MIT license.
