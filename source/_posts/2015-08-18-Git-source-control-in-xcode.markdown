---
layout: post
title: "Git Source Control in Xcode"
date: 2015-08-18 18:51:56 +0200
comments: true
categories: 
---

It's obvious that adding a Git repository to a project is totally effortless using Xcode. However, what happens if you don't include a Git repo during a project setup, or if you want to add one at a later time? Well, the good news is that you can add a repo for your project at any time, but without using Xcode.

Befor demonstrating anything, you must download the Command line tools through Xcode, as we are going to work in terminal and we need some extra tools available. to install the command line tools, goto the Xcode > Preferences... menu on Xcode, and then select the Downloads tab. In the upper side of the window, under the Components section, click on the button with a down arrow existing at the right side of the Commnad line Tools options. Once the download is over, a checkmark will appear instead of the download button.

Now, create Xcode project for the sake of this example. make sure to deselect the Create git repository options. We don't want Xcode to prepare a repository for us in this case. Name the project and save it in your Desktop, so you can follow along with the commands i will show.

Once everything is ready, open a Terminal window(if you have already opend any, make sure to close it first, so any changes made by installing the command line tools to be applied). For starters, navigate your self to the new project's directory:

{% codeblock lang:swift %}
cd /users/YOUR-USERNAME/Desktop/ProjectName
{% endcodeblock %}

{% codeblock lang:swift %}
git init
{% endcodeblock %}

This will initialize an empty repo, and if you either go to Finder or use the ls terminal command, you'll see that the .git sub-directory has been created. Cool. Keep going:

{% codeblock lang:swift %}
git add.
{% endcodeblock %}

With this one, all contents of the current directory(the dot [.] symbol) will be added to the repo.
Finally commit all(make the changes permanent):

{% codeblock lang:swift %}
git commit -m 'initial commit'
{% endcodeblock %}

A list of all files commited to the local git repo will appear to the terminal window. the next screen illustrates my terminal:

{% img left images/git-screenshot.png #2 %}



