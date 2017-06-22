---
layout: post
title: "watchOS 2: Animations"
date: 2015-08-03 00:43:06 +0200
comments: true
categories: 
---

The new operating system for Apple Watch, watchOS 2, was introduced a couple of weeks ago at WWDC 2015. It brings a lot of improvements, mostly for developers looking to create an Apple Watch app. These are the things that I find to be most important for developers:

- WatchKit apps are now running natively on the watch. This brings the much needed improvement in speed, resulting in a better user experience.
- The new Watch Connectivity framework enables all sorts of communication and data sharing between the parent iOS app and the watchOS app.
- watchOS 2 apps have access to hardware data, such as data from the motion sensor, audio recording, and they can even access heart rate data.
- watchOS 2 also introduced animations. On watchOS 1, the only option to perform an animation was to generate a series of images and then iterate through them. watchOS 2 brings true animations to the Apple Watch. You can animate the user interface by changing layout properties inside an animation block. That’s where this tutorial comes in.

## 1. Why Care About Animations?

Before we get to the nuts and bolts, I’d like to spend a minute talking about the purpose of animations on Apple Watch apps.

The obvious reason is that they make the user interface more enjoyable if used appropriately. And when it comes to Apple Watch, that is a big if. Since most app interactions only last for a few seconds, you really don’t want to go overboard with animations.

The second, and I believe more important reason, is that they allow for custom navigation hierarchies inside Apple Watch apps. Let's suppose you need to present a screen that the user can only leave by taking a specific action. Normally, Apple Watch apps always have a cancel button in the top left corner when a modal interface controller is presented. With animations and clever layout manipulation, you could create your own "present view controller" routine that shows your app's content full-screen, dismissing it by that specific action. That is one of the things you’ll learn in this tutorial.

## Basics 

//Describe the animation in watchOS 1 

The principle of animations on watchOS 2 is simple, you set one or more of the animatable properties inside an animation block. The following example illustrates how this works.

{% codeblock lang:swift %}
[self animateWithDuration:0.5 animations:^{
    [self.circleGroup setHorizontalAlignment:WKInterfaceObjectHorizontalAlignmentRight];
}];
{% endcodeblock %}


This method causes the circleGroup to be aligned to the right, with an animation with a duration of 0.5 seconds. As you can see, we are calling animateWithDuration:animations: on self, which is an instance of WKInterfaceController. This is different from iOS where the animation methods are class methods on UIView.

The below list shows which properties are animatable:

.opacity

.alignment

.width and height

.background color

.color and tint color

Bear in mind that it’s still not possible on watchOS 2 to create user interface elements at runtime. But since you can hide them or set their alpha to 0 in the storyboard, this shouldn’t be that big of a problem.

That’s it. Armed with your knowledge about the WatchKit layout system, you are now ready to start working with native animations on watchOS. Let’s get started by creating a sample app so I can show you a couple of examples of how this all fits together.