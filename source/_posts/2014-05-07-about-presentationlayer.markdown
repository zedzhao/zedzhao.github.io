---
layout: post
title: "About PresentationLayer"
date: 2014-05-07 22:11:54 +0800
comments: true
categories: Animation
---


###PresentationLayer到底是什么~

官方文档中是这么写的:

	The layer object returned by this method provides a close approximation of the layer that is currently being displayed onscreen. While an animation is in progress, you can retrieve this object and use it to get the current values for those animations.
	
翻译过来是说，这个`presentationLayer`返回的是一个非常接近当前显示在屏幕上`layer`。比如当前layer正在执行一个动画，那么可以通过这个属性来获取layer当前的一些状态。

写点代码来搞清楚这个到底是做什么的~~

新建一个layer 
{% highlight objective-c lineos%}

	CALayer *movingLayer = [CALayer layer];
    [movingLayer setBounds:CGRectMake(0, 0, layerSize, layerSize)];
    [movingLayer setBackgroundColor:[UIColor orangeColor].CGColor];
    [movingLayer setPosition:CGPointMake(layerCenterInset, layerCenterInset)];
    [self.view.layer addSublayer:movingLayer];
    [self setMovingLayer:movingLayer];
    
{% endhighlight %}

添加一个动画
{% highlight objective-c lineos%}


	CAKeyframeAnimation *moveLayerAnimation = [CAKeyframeAnimation animationWithKeyPath:@"position"];

    [moveLayerAnimation setValues:@[[NSValue valueWithCGPoint:CGPointMake(100, 100)], [NSValue valueWithCGPoint:CGPointMake(200, 250)]]];
     
    moveLayerAnimation.repeatCount = HUGE_VALF;
    moveLayerAnimation.duration = 2.0;
    [moveLayerAnimation setTimingFunction:[CAMediaTimingFunction functionWithName:kCAMediaTimingFunctionLinear]];
    moveLayerAnimation.calculationMode = kCAAnimationCubic;
    [self.movingLayer addAnimation:moveLayerAnimation forKey:@"move"];

{% endhighlight %}

然后我们加一个Gesture在当前view
{% highlight objective-c lineos%}


	- (IBAction)pressedLayer:(UIGestureRecognizer*)gestureRecognizer{
    CGPoint touchPoint =[gestureRecognizer locationInView:self.view];
    
    if ([self.movingLayer.presentationLayer hitTest:touchPoint]) {
        [self blinkLayerWithColor:[UIColor yellowColor]];
    }else if ([self.movingLayer hitTest:touchPoint]){
        [self blinkLayerWithColor:[UIColor redColor]];
    }
	}

{% endhighlight %}

OK，运行一下代码，可以看到，当我们点击显示在屏幕上的layer的时候，会变黄色， 点击layer的原始位置会变红色。

由此得出结论， presentationLayer实际上就是先在在屏幕上的layer，而在任何的动画效果中，原始的layer的状态其实是不会改变的。即是说，动画只是动画而已，不会改变CALayer任何的属性，如果想要让layer保持动画结束时的状态，需要自己手动修改。

- Demo [PresentationLayer](https://github.com/zedzhao/CoreAnimationLessons/tree/master/PresentationLayer)
- 参照Blog:[Hit testing animating  layers](http://ronnqvi.st/hit-testing-animating-layers/)
