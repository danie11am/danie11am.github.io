## [tvOS] A bug in On-Demand Resources

I recently worked on On-Demand Resources for my [tvOS app](http://www.hungrysource.com/nature/index.html). The goal was to download a video file through ODR and play it. 

Found this bug where the downloaded ODR asset will magically disappear after you present or dismiss a view controller. 

Steps to reproduce this bug:

- Create a NSBundleResourceRequest like `NSBundleResourceRequest.init(tags: [“tag name”])`
- Call `beginAccessingResourcesWithCompletionHandler`
- Asset is ready for use, hooray!
- If `presentViewController` or `dismissViewControllerAnimated` is called, the downloaded asset will become unavailable, i.e. `NSBundle.mainBundle().pathForResource` returns nil.
 
The "workaround" I have is to avoid presenting/dismissing view controller as long as the ODR asset is still required.

Ticket #26892545 raised in [Apple Bug Reporter](https://bugreport.apple.com/) (a.k.a. Radar).
