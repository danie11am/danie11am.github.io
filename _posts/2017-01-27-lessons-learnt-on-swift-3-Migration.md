## [Swift 3] Lessons learnt from Swift 3 migration

If you work on iOS projects with Swift, now (Edit: 2017-01) is the time to migrate them to Swift 3 if that is not done yet. It is better to do this now rather than being forced to do so when Xcode 8.3 comes out. 

The following are lessons learnt from migrating a project with around 50K LOC.


### CocoaPods

You need to check each pod and update its version to the the one that supports Swift 3. If it doesn’t support Swift 3 yet, you can include the source code directly and migrate it yourself (but you might as well help the author with a PR), or wait till someone does it, or remove the pod. 

One problem I encountered was that a pod seems to get the old v2.3  code even though the latest pod version has been specified, thus causing compile errors. Cleaning and re-doing `pod install` did not help. 

It turns out that our main project had a specific value for the “Legacy Swift” build settings, and this blocked the same settings in the Pods project. Once that was removed, `pod install` gave the right code. The pod was CocoaLumberjacks, by the way, but I suspect it could happen to other pods as well.


### Xcode migration tool

The Xcode migration tool would fix most syntactic changes automatically. Remaining compile errors can also sometimes be auto-fixed by Xcode: click on the red dot on the line number, a pop-up would appear to show the error details, and if a “Fix-it …..” option is available, see if it makes sense and use it if appropriate. 

However, be cautious when using it because the suggestion is not always appropriate. For example, an auto-fix/the migration tool in one case has used forced-casting to change a parameter type to `Any`, and the function signature expects `AnyObject`. The problem? It crashes on runtime without any compile time error or warning. The solution is to change the `AnyObject` in signature to `Any`, and no force casting is required.


### IBActions

Function signature expected by Storyboard in Swift 3 is different from 2.3. The migration tool may or may not fix them automatically, so keep an eye out on them:

``` 
// Swift 2.3 - This works in v2.3 but in v3 it will crash on runtime.
@IBAction func tappedOnSomething(sender: AnyObject) {

// Swift 3 - To fix the runtime crash it needs the `_` before first argument.
@IBAction func tappedOnSomething(_ sender: AnyObject) {

// Swift 3 - Same for unwind segue, you also need `_` otherwise it would fail silently on runtime.
@IBAction func unwindFromSomewhere(_ segue: UIStoryboardSegue) {
``` 


### Merge conflicts

Once you started the migration work on a feature branch, your team members usually would still need to continue development on the original branch with the older Swift version. That inevitably results in merge conflicts. To minimise merge conflict, choose a time window to start Swift migration when there are minimal coding activity. Avoid major feature work/re-factoring in parralel while Swift migration is happening.

Quick reference for resolving merge conflict:

```
<<<<< HEAD
// "Mine" - your local code before the merge event.
===== 
// “Theirs" - code in the remote branch.
<<<<< develop
```

The two sections may look very similar and therefore it is easy to lose changes. Consider every line and what the remote changes are, then apply it to local if needed. 


### NSThing -> Thing

All NSThing types have been converted to just Thing. `NSDate` -> `Date`, `NSString` -> `String`, etc. A lot of times it would just work once you change the types in declaration or signature of functions. However, a lot other times it would not work, and you may need to cast back to the NS type to reach your property/function. 

For example, we previously had a `NSError` helper function in Swift 2.3, and the rewrite in Swift 3 would be:

```
// Swift 2.3 - This is how it looks like originally.
if error.isInternetIssue {

// Swift 3 - Now this, because `isInternetIssue` was defined in NSError+Something.h.
if (error as? NSError)?.isInternetIssue ?? false {

// Swift 3 - Alternative way - but not preferred/applicable often because you actually want to use Error more often than NSError.
if let error = error as? NSError {
        if error.isInternetIssue {
```



