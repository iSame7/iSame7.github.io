---
layout: post
title: "The Checklist of my code review"
date: 2017-06-22 22:29:56 +0200
comments: true
categories:
---
{% img left /images/code-review-1.jpg screenshot #1 %}

# Definition of Code Review:
### According to Wikipedia :
_Code review is systematic examination (sometimes referred to as peer review) of computer source code. It is intended to find mistakes overlooked in the initial development phase, improving the overall quality of software._

Ok, I wanna do it! Wait, but …

# How to do it?
Reviews can be done in various forms such as pair programming, informal walkthroughs, and formal inspections. The most known is probably this one — show me your code (aka informal review)! Simply ask your peer to have a look at your code.
Formal code reviews can be performed by different tools e.g. Atlassian’s Crucible/Fisheye or a pull request on BitBucket/GitHub or whatever you use. Thanks to those tools you get a nice visualization of what changes were made to source code, you can comment on them, ask author some questions and they can explain their code in return. It’s like a conversation you would have in real life, but documented — what has been agreed should be performed before merging into develop. Wait, what merging?! We were just talking about a review …

# Git flow
When developing different parts of an application at work we use Git and Git Flow to merge all changes into a parent branch. Once a feature of an app is finished we create a pull request that contains changes to be added to a predecessor branch (usually develop). The best picture showing the flow comes, in my opinion, from [GitHub](https://guides.github.com/introduction/flow/?utm_source=swifting.io&utm_medium=web&utm_campaign=blog%20post).

{% img left /images/git-flow.jpg screenshot #1 %}

The flow simply goes like this:

* create a branch from e.g. develop

* apply your changes to the source code

* create a pull request to be merged into e.g. develop

* discuss changes with your peers, explain your point of view & apply suggested improvements

* your peers approves your changes

* merge your code into source branch

# What should you care about when doing a review?
You definitely should check code integrity — does the style match previous solutions, does it follow agreed conventions? Are features implemented correctly(for this point i would advice if the PR creator could add .GIF image that explain how the feature works that would be nice), does the old source code work correctly after changes?

# Why you should care about code review in your development process?
For sure because it ensures code integrity :) , catches what others’ eyes didn’t see. It allows to learn and share your knowledge and expertise, strengthens communication in the team and builds good relationships due to conversations about what you have in common — code and programming skills ;)!

_Personally i have seen that lately in my current project. the team had problems in communication but after  conversations we did in code review the communication became much more better._

Consider code review as an investment into the future. If you don’t catch bugs now you will definitely have to conquer them in the future.

# What if you didn’t perform code reviews
Imagine a company X. It delivers mobile app solutions for multiple clients. Their team is small and at some point they cannot meet clients’ demands. So they decide to outsource some of the work to an external company. They give project requirements to this company and meet again after 3 months to receive app source code. But the app is useless — it freezes at network calls, CoreData.ConcurrencyDebug flag crashes all the time. The project is delayed for a few months, team has to start from a scratch. They wish they had reviewed outsourced code on a daily basis…

# Do you want to perform code reviews?
This is usually the question that gets lack of enthusiasm from the audience :(. But really, are we too busy to improve??

{% img left /images/want-codereview.png screenshot #1 %}

This was an introduction to code review. Next up we’ll get deeper into the process. 😄 

So let’s get deeper into the code review process, present some tips & tricks when performing a review and also focus on some mistakes when reviewing a swift code. 

As iOS developer most of things i think of are somehow related to iOS platform, but i think there are generic things not related to the platform or specific programming language.

# General Tips:
It is said that a review goes best when conducted on less than 400 lines of code at a time. You should do a proper and slow review, however, don’t spend on it more than 90 minutes ⏰

you definitely will get tired after that time. It’s tiring to write and understand your own code and it’s even more tiring to understand someone’s.

After some years of experience in software development you know what common programming mistakes are. You also do realize what solutions to common problems are most efficient. It’s a good practice to write it down and to check code being reviewed against a checklist ✅ — it leads improved results. Checklists are especially important for reviewers because, if the author forgets a task, the reviewer is likely to miss it too.

In the git flow mentioned in above, code can get merged after receiving agreed number of approvals for a pull request. The number should depend on your team size, but it’s good to get at least 1️⃣ !😀

When you recommend fixes to the code, suggest their importance 💬. Maybe some of them could be postponed and extracted as separate tasks. Before approving a pull request, verify that defects are fixed 🔧🔨.

Foster in your company a Good Code Review Culture in which finding defects is viewed positively 😀. The point of software code review is to eliminate as many defects as possible, regardless of who “caused” the error. Few things feel better than getting praise from a peer. Reward developers for growth and effort. Offer as many positive comments as possible. I always try to put a line saying that a solution is good and clever (if it really is 😉).

You can also benefit from The Ego Effect 💎. The Ego Effect drives developers to write better code because they know that others will be looking at their code and their metrics. No one wants to be known as the guy who makes all those junior-level mistakes. The Ego Effect drives developers to review their own work carefully before passing it on to others.

And don’t bother with code formatting style …

… there are much better things on which to spend your time, than arguing ☔️ over the placement of white spaces.

## Looking for bugs and performance issues is the primary goal and is where the majority of your time should be spent.

# What i look for during a review?
# 1. Style
## Common red flags 🚩
# 1. DRY (Don’t repeat yourself)

There are lots of examples for DRY principle let’s take the most simple one:
the simple meaning of DRY is don’t write the same code repeatedly. Let’s take for example this code:

``` ruby
// let's assume we have a boolean and it's initialised with false
let isPositionCorrect = false
if isPositionCorrect {
    updateUI("Position Correct", isPositionCorrect)
} else {
    updateUI("Position InCorrect", isPositionCorrect)
}
```

If we look into this piece of code we see that the call updateUI function is duplicated twice. Now this is a bad code because from maintainability perspective if the method definition has changed then you need to change it twice for example now the method takes 2 parameters what if we need to change it to take 3 parameters we have to change it in 2 places.

# To fix that we have to follow the DRY principle:

``` ruby
let message = isPositionCorrect ? "Position Correct" : "Position InCorrect"

updateUI(message, isPositionCorrect)
```

So in this case if we need to change the function we will change only one place, no duplication any more.


# 2- Use early exit
The more loops you have the more effort required from our mind to make sense of it. So with early exit instead of having your code wrapped in one if loop statement, you can just add the condition for which the loop is not executed first and just return if that is the case.

Example, instead of that :

``` ruby
func function(items: [Int]) {
    if items.count > 0 {
        for item in items {
            // do something
        }
    }
}
```

you could write it like that:

``` ruby
func function(items: [Int]) {
    if items.isEmpty {
        return
    }
for item in items {
        // do something
    }
}
```

Even more Swifty style you could write it like that:

``` ruby
func function(items: [Int]) {
    guard !items.isEmpty else { return }
for item in items {
        // do something
    }
}
```

# 3- Returning booleans:

``` ruby
func isItemExist(x: Int, items: [Int]) -> Bool {
    if items.contains(x) {
        return true
    }
    else {
        return false
    }
}
```

If your function returns boolean, just take a closer look and see if you can write it in a bitter way, because most of the times, it can be written in a simpler way.

``` ruby
func isItemExist(x: Int, items: [Int]) -> Bool {
    return items.contains(x)
}
```

Also if you are dealing with booleans on objects:

``` ruby
func showImageViewIfEmptyItems(imageView: UIImageView, items: [Int]) {
    if items.isEmpty {
        imageView.isHidden = true
    }
    else {
        imageView.isHidden = false
    }
}
```

Could be written like that:

``` ruby
func showImageViewIfEmptyItems(imageView: UIImageView, items: [Int]) {
    imageView.isHidden = items.isEmpty
}
```

4- Remove unused code
_The Best Code is No Code At All_

Removing code should be one of your activities as a developer. Always delete unused code also code that is generated by IDE. In code review when you see empty method, unused variable, outdated comments, imports hanging around your code, don’t just leave it, it should be removed.

Sometimes i find stuff like this in codebase i review:


``` ruby
func foo(String: item) -> String {
  let baz = findKeyForItem(item)
  // we no longer do this for some good reason
  // if (baz == "foobar") {
  // return baz
  // } else {
  // return item.foobar()
  // }
  return baz
}
```

This function should looks like this:

``` ruby
func foo(String: item) -> String {
  return findKeyForItem(item)
}
```

Now if you need to go back to the old code just use git diff if you prefer command line, you can use your git client integrated with your IDE or your preferred source control tool .We can grape those changes and boom! we’re back.

# 5. Write “Shy” code 
As mentioned in The Pragmatic Programmer, your code should always be “shy code”. Write shy code that doesn’t interact with too many things. Write shy code that makes objects loosely coupled. Write everything with the smallest scope possible and only increase the scope if you really need to — for example, start with everything private, your properties as well as your methods.

This rule applies on the level of exposition of the properties your objects own. Let’s say you have a custom UI component which contains an image inside, which should also be set from outside your component. Instead of exposing the image as a read/write property, you could provide a method to update the corresponding property inside your class (in Swift we do that using computed properties as we can get and set this property). The image will only be exposed as read-only and you can also have more control on the values that are being set on your property.

For example:

``` ruby
class ReusableUIComponent {

    private var image: UIImage

    init(image: UIImage) {
        self.image = image
    }

    var componentImage: UIImage {        
        get {
            return image
        }

        set {
            image = newValue
        }
    }
}
```

# 6. Hard-coded values 

_Always think of making things configurable instead of Hard-coding values._

This one of the trivial activities of reviewing code. whenever you see Hard-coded values, first ask yourself if there is another way you could remove this static value (String, Float, Int,…) and make it configurable? then go a head and make it better. If there isn’t a way to get ride of it, store it in a static variable(normally this will be in a constants class/file you use across the project).

The benefit of doing that obviously is to avoid code duplication. without storing this static value in a variable, you would need to change the code in all places when your value change.

# 7. Language style guide & coding conventions
Almost every company should have it’s own language style guide and code convention e.g [Github’s swift style guide](https://github.com/github/swift-style-guide) . Always make sure that coding style and team standards are followed by everyone. Because doing things your way can be more fun, but your colleagues might not be used to your coding style and if it’s unusual, it will be harder for the next developer to work on what you have built. Your review should include consistent way of naming variables, code formatting, best practice for your language agreed on in your team.

# 2. Architecture / Design

* The code should follow the defined architecture, Whatever the architecture is MVC, MVP, MVVM or VIPER make sure that your peers is following it. Also look out for new patterns that could be useful for your project.

* Check if committed code  is reusable. Consider generic functions and classes.

* Check if committed code is designed correctly, So if your team is following TDD(Test Driven Development) you should always write tests before production code which is mean your code is designed perfectly. From my experience TDD is saving a lot of time for designing your code, but if TDD is not adding value in designing your code, then don’t do it and write your production code directly.

* Check if committed code is clean code. it should follow Object oriented analysis and design (OOAD) principles or SOLID principles.

# [SOLID Principles](https://robots.thoughtbot.com/back-to-basics-solid):

1. S -  Single Responsibility Principle (SRS): Do not place more than one responsibility into a single class or function, refactor into separate classes and functions (Plain Objects).

2. O - Open Closed Principle: While adding new functionality, existing code should not be modified. New functionality should be written in new classes and functions (extensions).

3. L - Liskov substitutability principle: The child class should not change the behavior (meaning) of the parent class. The child class can be used as a substitute for a base class.

4. I - Interface segregation: Do not create lengthy interfaces, instead split them into smaller interfaces based on the functionality. The interface should not contain any dependencies (parameters), which are not required for the expected functionality.

5. D - Dependency Inversion principle: High level modules should not depend on low level modules but should depend on abstraction (Protocols), Which make Dependency injection easy.

# 3. Testing

* Unit Test: Check if unit tests have been written for new features. Do they cover the failure conditions? Are they easy to read? How fragile are they? How big are the tests? Are they slow?

# Some Swift🕊 related Code Review

# 1. Bang or force unwrapping ❗
One of the things that i look for when reviewing Swift code is this exclamation mark ❗always look out for code that uses it.

``` ruby
var foo: Object!
print("\(foo.someString)")
```

as you can see here this print statement will cause a crash because we access foo variable which is not initialized. Always ask yourself isn’t there a better way of doing this? Maybe a guard, optional chaining.

Make sure that your peers (and you as well) use the bang/force unwrapping operator wisely in their code.


# 2. autoreleasepool
Do you remember autoreleasepool? if you don’t know what is that here is some explanation. Always i pay attention when reviewing body of loop to check if local  autoreleasepool could be used to reduce peak memory footprint.

For example in this piece of code we create UIImage 5 times

``` ruby
func useManyImages() {
    let filename = pathForResourceInBundle

    for _ in 0 ..< 5 {
        let image = UIImage(contentsOfFile: filename)
    }
}
```

To reduce peak memory footprint we should use autoreleasepool like this:

``` ruby
func useManyImages() {
    let filename = pathForResourceInBundle

    for _ in 0 ..< 5 {
        autoreleasepool {
            let image = UIImage(contentsOfFile: filename)
        }
    }
}
```

# 3. Mutable vs immutable property
If object’s properties depends on some values, never ever use var. Use let. Always value more immutable state over var. If your property really have to be mutable and another objects needs to access it like from unit tests, expose it to the public as immutable like that:

``` ruby
private(set) var foo: AnyObject
let bar: AnyObject
```

# 4. Remove/deregister observers
Whenever you are reviewing or writing code that registers for some sort of notification or subscribes as an observer for updates, make sure you check that they also unregister/unsubscribe. Please check that, because we all know how difficult to debug “Message sent to deallocated instance”.

# 5. Retain cycles
Avoid retain cycles when using delegates, make sure that delegates are weak. also when using blocks, make sure you capture self as weak.
An example:

``` ruby
class Foo {
	let bar = Bar()
	bar.closeAsynchronous{ [unowned self]
		self.barIsClosed()
	}
	func barIsClosed(){
		println("Bar")
	}
}
```

The code above has a retain cycle in it. The trailing closure closeAsynchronous{…} is called after some time without blocking the main queue. The [unowned self] is needed to brake the cycle. If bar is not a variable of Foo then we do not need [unowned self]

# 6. Delegates implemented in extension 
Make sure that code is readable and maintainable by ensuring that delegates are implemented in extensions. 
An Example:

``` ruby
class Foo {
}
extension Foo : Delegate {
    func delegateMethod (){
    }
}
```

7. Use most generic object references
Always make sure when you are using an object, save it to a variable that has the most generic type. 
See this example:

``` ruby
private var viewController: UIAlertController?

    init() {
        viewController = UIAlertController()
        navigationController?.pushViewController(viewController, animated: true)
    }
```

Here if you notice that pushViewController method only need ViewController not UIAlertController object. So as we don’t need any properties from UIAlertController, we can save it to ViewController like this:

``` ruby
private var viewController: UIViewController?

    init() {
        viewController = UIAlertController()
        navigationController?.pushViewController(viewController, animated: true)
    }
```

_Here the idea is not to give more information that is not needed. The less the objects know about each others, the less dependencies you have in your code._

# 8. Unit Tests
In unit tests use ! forced unwrap more because if it crashes on the unwrap this also means the test fails. As an example see testForcedUnwrap

``` ruby
class Foo() {
    func getOptional() -> Type? {
        return Type?()
    }
}
func testForcedUnwrap(){
    let foo = Foo()
    let optional = foo.getOptional()! //a crash here means the test failed

    XCTAssertNotNil(optional)
}
```

# 9. Code Documentation
When a class becomes too large, it is useful to add some kind of delimiter (pragma marks) for the public/private methods, the protocol implementations, public/private variables, anything that will help anyone to understand and work with the code and navigate easier through the class.

# Final Thought:
Code review is  a mindset, not a procedure.  Please keep an open mind during code reviews. I think this is something everyone struggles with. Try to not get defensive and don’t take it personal when someone says code you wrote could be better.

If the reviewer makes a suggestion, and I don’t have a clear answer as to why the suggestion should not be implemented, I’ll usually make the change. If the reviewer is asking a question about a line of code, it may mean that it would confuse others in the future. In addition, making the changes can help reveal larger architectural issues or bugs.

One of the important aspects for me, is that there is nothing too small to comment on. [As per the Broken window theory](https://en.wikipedia.org/wiki/Broken_windows_theory). This is also applies to software development.

And always — Review your own code first! Before committing a code look through all changes(diff) you have made!

Also you should use Lint tools e.g. [SwiftLint](https://github.com/realm/SwiftLint) that would make it easy for you to set automated code review guidelines. Simply you can automate some of this checklist and save time while doing code review.

These are basic stuff i consider while doing code reviews that i learned during the past years, while working with very talented and passionate people.

I’ll always keep my checklist open and will update it once new stuff comes up.

---

I would love to hear what is on your checklist for code review in your ?

---

I hope you enjoyed reading this and that it was able to provide a little guidance in your review process.

Please Don’t hesitate to get in contact, shoot me an email at [mabrouksameh@gmail.com](mailto:mabrouksameh@gmail.com)

---
