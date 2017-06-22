---
layout: post
title: "Reasons Swift should be your iOS language"
date: 2016-04-09 16:17:05 +0200
comments: true
categories: 
---

#Intro:

Introduced with iOS 8 in June 2014 at WWDC, Apple’s developer conference, Swift is Apple’s brand new language for creating apps. Now in its second iteration, and made Open Source in December 2015, Swift is not just designed to be a simpler language, helping apps run faster and be more secure, but could also be one of the primary coding languages across other platforms in the future.

As a language, Swift enables developers to build their apps alongside interactive Playgrounds, helping them to see the results of the lines of code they write side-by-side. With computer programming skills being seen as more important than ever, part of the appeal of Swift is that it’s a comparatively easy language to learn and code in. This was by design.

As a language, work on Swift began in the summer of 2010 by an Apple programmer called Chris Lattner. Chris spent a year and a half working on the language, with the eventual collaboration of many other programmers at Apple.

This post outline the reasons why yiu should choose swift as iOS language.


# 1. It's faster

Where Swift currently falls behind Objective-C is its compiler. If we look at writing a simple JSON parser in Swift and the equivalent parser in Obj-C, the following would likely be true:

###• The Swift parser would contain fewer lines of code than the Obj- C parser
###• The Obj-C parse would compile considerably quicker than the Swift parser
###• Once compiled, the Swift parser would execute quicker than the Obj-C parser

Currently, the Swift compiler within Xcode takes longer. This isn’t linked to the number of lines of code, but the use of functional programming paradigms, which are gaining popularity in Swift. Once compiled however, Swift apps will execute faster than Objective-C, thanks to the compiler optimisations that can be made with Swift.

Swift also uses roughly half the amount of files to work with, compared to Objective-C. With Objective-C, you need a header file (.h) and an implementation file (.m), with Swift however, you just need a single .swift file. This makes it less confusing for developers that aren’t accustomed to working with header and implementation files.

# 2. It's safer

As Apple says on its [developer site1](https://developer.apple.com/swift/) :
‘Swift eliminates entire classes of unsafe code. Variables are always initialized before use, arrays and integers are checked for overflow, and memory is managed automatically. Syntax is tuned to make it easy to define your intent — for example, simple three- character keywords define a variable ( var ) or constant ( let ).’
This, in addition to removing null pointers, one of the common programming errors and routes of bugs, means apps are less likely to crash, helping to improve the user experience.
“Swift uses variables to store and refer to values by an identifying
name. Swift also makes extensive use of variables whose values
cannot be changed. These are known as constants, and are much
more powerful than constants in C. Constants are used
throughout Swift to make code safer and clearer in intent when
￼you work with values that do not need to change.” Excerpt From: 2
Apple Inc. [“The Swift Programming Language (Swift 2.1).” iBooks](https://itunes.apple.com/gb/book/swift-programming-language/id881256329?mt=11).

# 3. It's easier to learn

In a discussion with [Daring Fireball](http://daringfireball.net/thetalkshow/139/federighi-gruber-transcript)  host and Apple commentator, John Gruber and Craig Federighi, Apple’s senior vice president of software engineering, Craig said:
‘Swift is, we think, the primary programming language that developers should be taught programming in, actually. I mean, if you’re going to learn computer science, Swift is a fantastic learning language.’

Apple has been open with the fact that Swift was designed to be easier for people to learn, opening up coding to a wider group of skill sets.
For someone with programming experience, even the complex coding within Swift is relatively easy to learn. For most people though, learning Swift is just a matter of learning its features and syntax, then practicing them. Thankfully, the official Swift documentation is easy to follow, making it easy to understand the language and how to test drive the code.

# 4. Playgrounds let you see the impact of your code in real-time

When developing apps using Apple’s Xcode and Swift, there’s the option of using Playgrounds, a secondary window that enables developers to test the code they’ve written and display results in real time.
This is helpful for newbies learning the language and seasoned developers testing an advanced algorithm for their current project.

# 5.  It is better at memory management

Building great apps is much harder than people may think. Creating an app is one thing, but creating one that doesn’t eat battery or use up memory, requires more expertise. Memory management issues can be harder to pin point as there won’t always be obvious bugs. Instead, memory management issues tend to manifest themselves through obscure bugs like repeat notifications or random crashes.
First introduced in 2010 with Xcode 4.2, with iOS 4.0, Automatic Reference Counting (ARC) is able to track and manage an app’s memory usage. ARC has been a core part of memory management for iOS and OS X projects, since iOS 5.0. It is one of the features that has been carried over from Objective-C to Swift. In most cases, this means that memory management “just works” in Swift.
There is still some work to be done here, however. Currently, the Clang static analyser, which flags up potential memory leaks, dead stores, unused variables, etc. isn’t compatible with Swift. As a tool which is built into Xcode, Clang is a useful tool for spotting potential memory issues at compile time. Whilst “just works” with Swift is often true, there are times when additional optimisations are required, which is precisely where Clang would help.

# 6. Now that Swift is Open Source

Being Open Source opens up Swift to the potential that it can be used across a number of different platforms and for backend infrastructure. Open Sourcing Swift means that Apple will be able to get feedback from the community to make improvements on a regular, ongoing basis.
By going Open Source independent developers, development teams and academics, across the world, can all contribute to the success of the language. Various frameworks, libraries and web servers/web components can be developed by this community, all at no cost to Apple. These projects can then be used by other developers to create an unlimited number of products, making Swift more important outside of the Apple developer community.

# 7. It’s the top language on GitHub

A week after officially going Open Source in December 2015, Swift jumped to the top of the most popular languages on GitHub. GitHub is the go-to source for open source language development, so the fact that Swift quickly jumped to the top, and has stayed there since, is a good indication of its popularity.
As Swift matures over the next few years, we can expect to see it being used by an ever increasing array of platforms. Whilst this is positive, it doesn’t mean that large companies will change their current language to Swift. Swift is the newest kid on the block, so it will take time to get adoption by larger companies outside of the development of new iOS apps.

# 8. It’s interoperable with Objective-C

Objective-C and C were the languages that native iOS apps were largely written in, prior to the introduction of Swift. Swift is interoperable with both, meaning developers can build on their existing apps, using Swift. This means that developers don’t have to do a full re-write. One of the most important aspects of interoperability is that it enables developers to work with Objective-C APIs when writing Swift code. This means that APIs don’t need to be rewritten to work with Swift and that apps will continue working.
As it is interoperable, developers are able to slowly replace older code in apps, with Swift code, without breaking the app (though this is possible, if the coding isn’t done properly). Migration provides an opportunity to revisit an existing Objective-C app and improve its architecture, logic and performance by replacing pieces of it in Swift.

# 9. Swift will be server side language

Though Swift is currently just for iOS and OS X, there is a huge appetite within the developer community to use it for servers and backend development.
According to Federighi4, “IBM, for instance, jumped all over Swift for building their mobile apps, and almost immediately they were coming back to us with, “We really want to use this on the server. How can we get this on the server?”
Of course, it’s not just IBM, as Federighi explained to John Gruber, Apple’s iCloud team is already keen to use it for iCloud, “And our own iCloud team has been completely champing at the bit to be able to apply it in many, many of the things they do. So, I think it’s going to be one of the first break-out uses of Swift. And of course these days there are so many mobile applications that are part mobile app, part server code. And in a lot of cases, at the very least you want to share your knowledge, but very often you want to share parts of code, parts of your model layer, some of your utility libraries, and having Swift enabling you to do that is going to be huge for a lot of our community.”

Whether Swift’s use on the server takes off or not depends on what the development community does with the language. Already, developers are busy making cool projects, such as:
###• [Swifter](https://github.com/glock45/swifter): a small HTTP server engine, written in Swift
###• [SwiftCGI](https://github.com/ianthetechie/SwiftCGI): a modular Microframework that encourages a functional approach to developing web apps in Swift.
###• [Perfect](http://perfect.org/) : an enterprise-grade web server and web toolkit for the Swift programming language.

We can also expect to see other current Objective-C projects being converted to Swift, such as CocoaHTTPServer and HTTPKit in the near future.

￼