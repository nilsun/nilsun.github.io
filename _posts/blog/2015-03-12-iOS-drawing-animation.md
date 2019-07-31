---
layout: post
title: iOS绘图和动画基础
description: 从UIView开始，关于绘图：UIView的主要职责：展示视图，响应事件（点击，手势等）。对UIView显示内容的设置，使用UIKit的高层接口，通常我们并不了解UIView是怎样显示内容的。
category: blog
published: ture
---

## 从UIView开始，关于绘图

UIView的主要职责：展示视图，响应事件（点击，手势等）。对UIView显示内容的设置，使用UIKit的高层接口，通常我们并不了解UIView是怎样显示内容的。

### 1、UIView DrawInRect

利用UIKit或CoreGraphic相关API绘制图图形，图形经硬件渲染到屏幕。DrawInRect耗费CPU时间，在主线程上执行，建议不要在该方法进行很耗时的绘图逻辑。但也有方法使绘图的过程异步进行。

### 2、设置图片，图片

直接调用接口设置图片，图片文件经过解码，生成位图数据，由硬件渲染到屏幕。实际上图片是设置到UIView的CALayer上，CALayer，CALayer是UIView绘图，动画的基础。

### 3、应该选择哪种模式

尽量使用系统的基于View的绘图，苹果已经做了很大程度的优化。


## UIView的如何实现动画

### 1、DrawInRect
	
自己控制重绘的时间，和内容，呈现动画。缺点是效率低下，逻辑复杂，做简单动画都很费力。

### 2、利用系统的CoreAnimation动画框架

![list app](/images/tech/iOSDrawingAnimation/core_animation.png)


## 动画原理及CALayer动画实现

### 1、Layer图层绘图模型

![list app](/images/tech/iOSDrawingAnimation/layer_drawing.png)

### 2、Layer图层变化

CATransform3D构建变换矩阵

![list app](/images/tech/iOSDrawingAnimation/transform_matrix.png)

### 3、Layer动画实现

代码层面：在Layer上添加CAAnimation，CAAnimation制定动画的参数。
系统实现层：Model Layer， Presentation Layer， Render Layer


## CALayer动画

![list app](/images/tech/iOSDrawingAnimation/animation_class.png)

### 1、隐式动画

	- (IBAction)implictAnimation:(id)sender
	{
	    //MacOS
	    //_imageView.layer.opacity = _imageView.layer.opacity == 0 ? 1 : 0;

	    //iOS
	    [UIView animateWithDuration:1.0 animations:^{
	        _imageView.layer.opacity = _imageView.layer.opacity == 0 ? 1 : 0;
	    }];
	}

### 2、显式动画

	- (IBAction)explictAnimation:(id)sender
	{
	    float opacity = _imageView.layer.opacity;
	    CABasicAnimation* fadeAnim = [CABasicAnimation animationWithKeyPath:@"opacity"];
	    fadeAnim.fromValue = opacity == 1 ? @1.0 : @0.0;
	    fadeAnim.toValue = opacity == 1 ? @0.0 : @1.0;

	    fadeAnim.duration = 1.0;
	    [_imageView.layer addAnimation:fadeAnim forKey:@"opacity"];
	    
	    //change model layer
	    _imageView.layer.opacity = !opacity;
	}

CABasicAnimation animation key path:

![list app](/images/tech/iOSDrawingAnimation/animation_keypath.png)

### 3、组合动画

	- (IBAction)groupAnimation:(id)sender
	{
	    CABasicAnimation *fadeAnim=[CABasicAnimation animationWithKeyPath:@"opacity"];
	    fadeAnim.fromValue=[NSNumber numberWithDouble:1.0];
	    fadeAnim.toValue=[NSNumber numberWithDouble:0.2];
	    
	    CABasicAnimation *rotateAnim=[CABasicAnimation animationWithKeyPath:@"transform.rotation.y"];
	    rotateAnim.fromValue=[NSNumber numberWithDouble:0.0];
	    rotateAnim.toValue=[NSNumber numberWithDouble:M_PI];
	    
	    CAAnimationGroup *group = [CAAnimationGroup animation];
	    group.duration = 2;
	    group.repeatCount = 1;
	    group.autoreverses = YES;
	    group.timingFunction = [CAMediaTimingFunction functionWithName:kCAMediaTimingFunctionEaseInEaseOut];
	    group.animations = [NSArray arrayWithObjects:fadeAnim, rotateAnim, nil];
	    
	    [_imageView.layer addAnimation:group forKey:@"allMyAnimations"];
	}

### 4、关键帧动画

	- (IBAction)keyFrameAnimation:(id)sender
	{
	    CAKeyframeAnimation* colorAnim = [CAKeyframeAnimation animationWithKeyPath:@"borderColor"];
	    NSArray* colorValues = [NSArray arrayWithObjects:(id)[UIColor greenColor].CGColor,
	                            (id)[UIColor redColor].CGColor, (id)[UIColor blueColor].CGColor,  nil];
	    colorAnim.values = colorValues;
	    colorAnim.duration = 0.4;
	    colorAnim.calculationMode = kCAAnimationPaced;
	    
	    CAKeyframeAnimation *moveAnim = [CAKeyframeAnimation animation];
	    moveAnim.keyPath = @"position.x";
	    moveAnim.values = @[ @0, @20, @-20, @20, @0 ];
	    moveAnim.keyTimes = @[ @0, @(1 / 6.0), @(3 / 6.0), @(5 / 6.0), @1 ];
	    moveAnim.duration = 0.4;
	    moveAnim.additive = YES;
	    
	    CAAnimationGroup *group = [CAAnimationGroup animation];
	    group.duration = 2;
	    group.repeatCount = 1;
	    group.autoreverses = YES;
	    group.animations = [NSArray arrayWithObjects:colorAnim, moveAnim, nil];
	    [_imageView.layer addAnimation:group forKey:@"allMyAnimations"];
	}

### 5、Transition

	- (IBAction)transitionAnimation:(id)sender
	{
	    CATransition* transition = [CATransition animation];
	    transition.startProgress = 0;
	    transition.endProgress = 1.0;
	    transition.type = kCATransitionPush;
	    transition.subtype = kCATransitionFromRight;
	    transition.duration = 1.0;
	    
	    [_imageView.layer addAnimation:transition forKey:@"transition"];
	    [_imageView1.layer addAnimation:transition forKey:@"transition"];
	}

## 列表优化

MacOS上开启View的Layer支持；性能还需要优化可以自己重写Layer-based Table；iOS上尽量用系统控件，或者直接在Cell上添加Layer。

[基本动画的代码][1]

[1]: https://github.com/nilsun/CoreAnimationTutorial