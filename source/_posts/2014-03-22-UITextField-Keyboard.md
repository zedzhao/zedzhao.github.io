---
layout: post
title: 关于UITextField和Keyboard
description: ""
tags: [ios]
share: true
comments: true
---

- 想点击当前viewController中任意地方隐藏键盘可以用如下代码实现。

{% highlight ruby linenos %}
	
- (void)touchesBegan:(NSSet *)touches withEvent:(UIEvent *)event{
    _isKeyboardDisplay = NO;
    [self.view endEditing:YES];
}

{% endhighlight %}	


- 下面的代码处理keyboard遮挡住输入框的问题，并且通过`_isKeybordDisplay` 来解决两个输入框时，来回切换输入框焦点时键盘会收回再弹出的问题。

{% highlight objective-c linenos %}

- (void)touchesBegan:(NSSet *)touches withEvent:(UIEvent *)event{
    _isKeyboardDisplay = NO;
    [self.view endEditing:YES];
}

- (void)animateTextField: (UITextField*) textField up:(BOOL)up
{
  
    const int movementDistance = 100;
    const float movementDuration = .3f;
    int movement = (up ? -movementDistance : movementDistance);
    if (_isKeyboardDisplay) {
        return;
    }
	[UIView animateWithDuration:movementDuration animations:^{
        self.view.frame = CGRectOffset(self.view.frame, 0, movement);
    	}];
    if (up) {
        _isKeyboardDisplay = YES;
    }else{
        _isKeyboardDisplay =NO ;
    }

}
- (void)textFieldDidBeginEditing:(UITextField *)textField{
    [self animateTextField:textField up:YES];
}

- (void)textFieldDidEndEditing:(UITextField *)textField
{
    if (_isKeyboardDisplay) {
        return;
    }
	[self animateTextField:textField up:NO];
}


{% endhighlight %}

