---
layout: post
title: "How i built Parallax effect Inspired by Yahoo Weather"
date: 2015-07-14 22:07:19 +0200
comments: true
categories: 
---

One of the first projects Yahoo’s Marissa Mayer oversaw as CEO was the Yahoo Weather app. The app was so well received that it even ended up receiving a coveted Apple Design Award in 2013.


{% img left images/image_iphone6_gold_portrait.png screenshot #1 %}


## Background:

I'm a big fan of Yahoo weather app. I intented to replicate and generalize the approach they used for anyone going to use. 

## Implementation:

Glassy View is a subclass of UIView. It contains two UIScrollView, background and foreground. backgroundScrollView consists of two UIImageView, backgroundImageView and blurredImageView. The alpha of the blurred one changes as the background scrolls. And the background scrolls as the foreground scrolls(at a different rate). The foregroundScrollView consists of maskLayers and the foregroundContainerView(which is whatever you want it to be).


## How to use:

1. Create an instance of GlassyScrollView init(frame: CGRect, backgroundImage:UIImage, blurredImage:UIImage, viewDistanceFromBottom:CGFloat, foregroundView:UIView) and add it as a subview to whereever you need it to be.
- backgroundImage is essential, pick whatever fancy image that makes all the difference.
- blurredBackgroundImage is not neccessary but you can provide your own custom one.
- viewDistanceFromBottom is how much your foregroundView is visible from the bottom (like Yahoo temperature)
- foregroundView this is the view that will contain all the info.

## Parallax effect implementation:

To achieve the parallax horizontal effect just call func scrollHorizontalRatio(ratio:CGFloat) that control each backgroundScrollView's contentOffset while scrolling.

The following 2 screenshots describe the behavior:

{% img left images/image_iphone6_gold_portrait-2.png screenshot #2 %}
{% img left images/image_iphone6_gold_portrait-3.png screenshot #3 %}



Just control the contentOffset of backgroundScrollView like this: 

{% codeblock lang:swift %}

func scrollViewDidScroll(scrollView: UIScrollView) {
    let ratio:CGFloat = scrollView.contentOffset.x/scrollView.frame.size.width
    page=ratio

    if ratio > -1 && ratio < 1 {

        glassScrollView1.scrollHorizontalRatio(-ratio)
    }
    if ratio > 0 && ratio < 2 {

        glassScrollView2.scrollHorizontalRatio(-ratio + 1)
    }
    if ratio > 1 && ratio < 3 {

        glassScrollView3.scrollHorizontalRatio(-ratio + 2)
    }
}
{% endcodeblock %}

{% codeblock lang:swift %}
func scrollHorizontalRatio(ratio:CGFloat){
    // when the view scroll horizontally, this works the parallax magic
    backgroundScrollView.contentOffset = CGPointMake(maxBackgroundMovementHorizontal+ratio*maxBackgroundMovementHorizontal,     backgroundScrollView.contentOffset.y)
}
{% endcodeblock %}

## Scrolling all foregroundScrollViews Vertically to the same point implementation:

To simulate the same behavior achieved by Yahoo's Weather app that scroll all foregroundScrollViews Vertically to the same point, just control the contentOffset of foregroundScrollView and change offsetY accordingly.

So let's assume that you scrolle one of the foregroundScrollViews vertically so then all foregroundScrollViews(others foregroundScrollViews 2 foregroundScrollViews in our case) should be scrolled vertically to the same point(offsetY) accordingly.

The following screenshot describe the behavior:

{% img left images/image_iphone6_gold_portrait-1.png screenshot #4 %}

Start by calling scrollVerticallyToOffset(offsetY:CGFloat) for each glassScrollView in scrollViewWillBeginDragging:

{% codeblock lang:swift %}
func scrollViewWillBeginDragging(scrollView: UIScrollView) {

    let glass:GlassyScrollView = currentGlass()

    //this is just a demonstration without optimization
    glassScrollView1.scrollVerticallyToOffset(glass.foregroundScrollView.contentOffset.y)
    glassScrollView2.scrollVerticallyToOffset(glass.foregroundScrollView.contentOffset.y)
    glassScrollView3.scrollVerticallyToOffset(glass.foregroundScrollView.contentOffset.y)
}
{% endcodeblock %}

Then get the current Glassy scrollview:
{% codeblock lang:swift %}
func currentGlassyScrollView() ->GlassyScrollView{

    var glass:GlassyScrollView=glassScrollView1

    switch page{
    case 0:
        glass = glassScrollView1
    case 1:
        glass = glassScrollView2
    case 2:
        glass = glassScrollView3
    default:
        break
            
    }
    return glass
}
{% endcodeblock %}

And then call scrollVerticallyToOffset(offsetY:CGFloat) to change the contentOffset of foregroundScrollView accordingly:

{% codeblock lang:swift %}
func scrollVerticallyToOffset(offsetY:CGFloat){

    foregroundScrollView.contentOffset=CGPointMake(foregroundScrollView.contentOffset.x, offsetY)
}
{% endcodeblock %}


Check out Glassy control on [GitHub](https://github.com/iSame7/Glassy).


