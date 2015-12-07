---
layout: post
title: "Symbolicating iOS Crash Reports"
date: 2015-08-30 03:25:05 +0200
comments: true
categories: 
---
It's often difficult to reproduce crashes reported from users, During your app beta testing, and even when you have apps on the App Store.
Yesterday i faced an issue that my app works well in Debug mode on Xcode but it's crash when installing it from TestFlight and trying the same scenario on it. Unfrtuanalty i was integratting InstaBug "it's a crash reporting tool" but the trial period ended and no more crahses reported to me over email. So i have to do it manually. Luckily, iOS devices keep logs of each crash and can tell you the exact line of code that causes the problem. Let's take a look at a crash log for my app.

{% img left images/CrashReport.png #2 %}

From the image above only the hex symbols of the objects and the memory addresses are there, So we need to symbolicate them.

First, you need to have the .app and .dSYM for the build that generated the crash log. 

These are the following steps:

Step1: Create a folder in desktop, and give it a name "CrashReport" and put three files ("MyApp.app", "MyApp.app.dSYM", MyApp.crash) in it.

Step2: Open finder and got to Applications, find Xcode application, right click on it and click "Show Package Contents" Then follow this path,
For Xcode 6 and above the path is Applications/Xcode.app/Contents/SharedFrameworks/DTDeviceKitBase.framework/Versions/A/Resources
Where you find "symbolicatecrash" file , copy this and paste it to "CrashReport" folder.

Step3:  launch the terminal, run these 3 Command

cd /Users/UserName/Desktop/CrashLog and press Enter button

export DEVELOPER_DIR="/Applications/Xcode.app/Contents/Developer" and press Enter

./symbolicatecrash -A -v MYApp_2013-07-18.crash MyApp.app.dSYM and press Enter Now its Done..
(NOTE: versions around 6.4 or later do not have the -A option -- just leave it out)

Now you should see a lovely, symbolicated crash report in the terminal:

{% img left images/SymbolicatedCrashReport.png #2 %}

Happy Coding :)

