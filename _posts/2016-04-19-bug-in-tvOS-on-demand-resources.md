## Bug in tvOS On-Demand Resources

Here is a bug in tvOS (possibly in iOS too) that can affect you if you work on On-Demand Resources on tvOS app.

Recently I’ve been working on On-Demand Resources for my [tvOS app](http://www.hungrysource.com/nature/index.html). Nothing fancy, just want to get a video file through ODR and play it. 


Found this bug where the downloaded ODR asset will magically disappear after you present or dismiss a view controller. 

It goes like this:

- Create a NSBundleResourceRequest like `NSBundleResourceRequest.init(tags: [“tag name”])`
- Call `beginAccessingResourcesWithCompletionHandler`
- Asset is ready for use, hooray!
- If `presentViewController` or `dismissViewControllerAnimated` is called, the downloaded asset will become unavailable, i.e. `NSBundle.mainBundle().pathForResource` returns nil.
 
Work-around? Avoid presenting/dismissing view controller as long as you still need the ODR asset.

Ticket #26892545 raised in [Apple Bug Reporter](https://bugreport.apple.com/) (a.k.a. Radar).