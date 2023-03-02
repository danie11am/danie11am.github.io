## [SwiftUI] Lessons learnt from rewriting an app in SwiftUI

I recently finished rewriting my app Jamophone in SwiftUI. This post highlights the joy and challenges of my experience.


### iOS Deployment Target version

In March 2023, iOS 16

This is probably the biggest hurdle for existing projects to switch from UIKit to SwiftUI. 


SwiftUI supports iOS version as far back as iOS 13, however there were probably bugs for such early version. More importantly, a lot of useful SwiftUI API are only available on iOS 15 and iOS 16. 

In my own project, I found setting deployment target to 15 the best. 
iOS 15 and 16 combined have over 90% userbase which is good enough for my app.


### Mindset shift from imperative to declarative


### Speed of development

Coding in SwiftUI is really fast

It has been out for a while now

It is here to stay

SwiftUI is a few times faster than using UIKit.


### Using UIKit when SwiftUI 


### Use SwiftUI



### Conclusion

SwiftUI is here to stay. I am grateful for this wonderful new technology where coding UI is easier and more efficient than ever before.

Are you planning on migrating an existing project from UIKit to SwiftUI?  Or starting a new project and wondering if you should choose SwiftUI over UIKit? Do you have concerns not mentioned above? Please let me know.

