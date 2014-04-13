---
layout: post
title: "Core Animation"
modified: 2014-04-07 22:57:17 +0800
tags: [ios,core animation]
image:
  feature: 
  credit: 
  creditlink: 
comments: true
share: true
---

##Timing Protocol 

{%highlight objective-c %}

CAMediaTimingFunction *timingFunction = [CAMediaTimingFunction functionWithControlPoints:0.9f :0.9f :0.25f :1.0f];
    
{%endhighlight%}

这里的参数是 cx1 : cy1 : cx2 : cy2， 代表2个point，加上函数默认的两个point实际上有四个坐标 (0,0), (cx1,cy1), (cx2, cy2), (1,1). 最后是根据这四个坐标画出一个Bezier曲线，可以理解为 y坐标代表动画的执行速度，x基本上是0 到 1是一个线性的变化；即是曲线越陡，执行越快。