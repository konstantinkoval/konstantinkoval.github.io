---
layout: post
title: "UIAAlert tap Handler"
categories: iOS, UIAutomation
published: true
---

I've been using UIAutomation to produce screenshots with [snapshot](https://github.com/KrauseFx/snapshot) tool. It works great except one problem:  
**I can't tap on the correct button in** `UIAlert`


The Instruments Automation tool produces this scrip code

```Objc
var target = UIATarget.localTarget();
target.frontMostApp().alert().buttons()["OK"].tap();
```
When you try running it, it will not tap "OK" button, but instead it will always tap default cancel button.   

After struggling for a while I found that this behaviour is actually documented on Apple website. It says that in order to be able to control taping on UIAlert buttons you need to add this code. 

```objc
UIATarget.onAlert = function onAlert(alert) {
  return true;
}
```

When you return `true` you take the full responsibility for correct actions handling of UIAlert and after that you can tap any button of `UIAlert`     

> ### Handling Alerts
>   
> When UI Automation encounters an alert during the execution of your script, it calls your alert handler, passing a reference to the UIAAlert object representing the alert. Your script should handle the alert appropriately and return a value of true, upon which normal script execution continues...
 
Read more at Apple's UI Automation [JavaScript Documentation](https://developer.apple.com/library/prerelease/ios/documentation/DeveloperTools/Reference/UIAutomationRef/index.html#//apple_ref/doc/uid/TP40009771), "Handling Alerts" sections

