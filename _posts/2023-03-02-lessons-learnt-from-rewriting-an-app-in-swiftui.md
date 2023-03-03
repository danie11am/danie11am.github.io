## [SwiftUI] Lessons learnt from rewriting an app in SwiftUI

What are the pros and cons of writing in SwiftUI when compared to writing in UIKit? I recently finished rewriting [Jamophone app](https://apps.apple.com/app/id535422655) from the ground up using SwiftUI. This post highlights the joy and challenges of my experience.

### iOS Deployment Target version

This may be the biggest hurdle to using SwiftUI instead of from UIKit.

SwiftUI is only available for iOS 13 and above. According to https://iosref.com/ios-usage it should be true for 95.6% of iOS usages. However, using SwiftUI in iOS 13 could be buggy since that was the initial release to support SwiftUI. 

In my own project, I found attractive SwiftUI APIs that are only available starting in iOS 15. In particular, pull to refresh (`refreshable` modifier) and running an asynchronous task when a view is shown (`task` modifier). 

As at March 2023, iOS 15 and 16 cover almost 90% of iOS usages, which I consider good enough for my app. Besides, I don't have an iOS 13 or 14 device to test with. Therefore I set the deployment target version to iOS 15.


### Mindset shift from imperative to declarative

SwiftUI is declarative. If you don't have any experience in declarative/reactive programming such as ReactiveCocoa/ReactiveSwift, RxSwift, React.js (or something ancient, Haskell), changing from imperative programming in UIKit to SwiftUI can be somewhat challenging at least in the beginning. This is especially true when attempting to build more complex views and navigation that require data to be shared between views.

In practice the challenge is to understand and use the SwiftUI keywords properly, such as @State, @Binding, @ObservedObject, @StateObject, @Published. 

I found it helpful to not worrying about all the implications of each one too much, and just focus on finding the keyword that helps you build what you need. Don't over analyse. Get things done, and all will make more sense soon. 


### Speed of development

Imperative programming describes the steps to build the UI, while declarative programming describes the UI directly. Therefore it is by principle supposed to be faster to code with than using imperative programming. 

In reality that is what I found as well. Given that you have gone past the initial learning curve of SwiftUI, I would estimate that using SwiftUI is at least a few times faster than UIKit to code the same requirement.


### Using UIKit when the need arises

Sometimes SwiftUI simply lacks the API to achieve what you want. In such case `UIViewRepresentable` can be used to get the functionality from UIKit and supply it to the SwiftUI code.

Some argue that this shows how insufficent SwiftUI still is. In my experience, it is rare to require the use of UIViewRepresentable. It might have been more frequently needed for older iOS versions, but not for iOS 15, for me at least. Plus, being able to use UIKit when needed should offer us better peace of mind when facing complex requirement.


### Using SwiftUI for only part of the project

If you are hesitant to switch to SwiftUI, keep in mind that this does not have to be done in one go. You can start using SwiftUI in a new screen, or convert an existing screen, or even just one view. 


### Conclusion

SwiftUI is here to stay. I am grateful for this wonderful new technology where coding UI is so much simpler and more efficiently done.

Do you have other questions related to coding in SwiftUI? Let's talk in comments below.





