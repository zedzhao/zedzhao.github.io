---
layout: post
title: "About UIGestureRecognizer"
modified: 2014-04-06 23:58:16 +0800
tags: [ios]
image:
  feature: 
  credit: 
  creditlink: 
comments: true
share: true
---

###Gesture 与 touch

事件传递顺序如下图
![image](/Users/zhaokun/Documents/jekyll/zedzhao.github.io/images/screenshot/GestureSequence.png)

如果当前view加上一个tapGesture，点击当前view时输出结果如下

>  touch Began
>  
>  tap
>  
>  touch Cancel



* delaysTouchesBegan

	简单理解为touch事件传递给`touchBegan`会被delay一段时间，即会优先响应gesture事件。
	
	
* delaysTouchesEnded

	同上，touch事件传递给`touchEnded`会被delay.
	
> With the property set to YES, the view gets touchesBegan:withEvent:, touchesBegan:withEvent:, touchesCancelled:withEvent:, and touchesCancelled:withEvent:. If this property is set to NO, the view gets the following sequence of messages: touchesBegan:withEvent:, touchesEnded:withEvent:, touchesBegan:withEvent:, and touchesCancelled:withEvent:, which means that in touchesBegan:withEvent:, the view can recognize a double tap.
	
	上面这段扯下来说， 如果一个双击的tap事件，这个属性设为NO的话，相当于touchBegan也可以识别这个双击手势。

