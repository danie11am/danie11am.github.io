## How to resolve iOS code signing issues

[Reviewed and updated in 2023]

iOS app code signing issues can be painful. So many things can go wrong! 

This post attempts to explain the concepts and motivations behind the code signing mechanism, which hopefully equips us to deal with any variants of the code signing issues.

### The big picture

Why is code signing needed? It is all about security.

> Apple offers app security through such things as mandatory code signing ...
> - [https://support.apple.com/en-au/guide/security/sec7c917bf14/web](Apple Platform Security)

The concept sounds simple: Before an app can be run, the system checks that it is "signed", meaning it is verified to be a trusted piece of software. 

In practice, at build and run time, Xcode or the OS gathers information such as identities of the app, its developer, and the device, and then make a decision on whether or not this is an acceptable combination for building/running. If it fails, the error is expressed as some kind of code signing issues. Here is [a list from Apple](https://developer.apple.com/library/archive/technotes/tn2407/_index.html).

Basically Apple asks these questions at various points such as build, install, release and run time,

- Who is the developer? 
- What app is this? 
- What device is the app going to run on?
- Are the answers expected, i.e. matching the ones in the provisioning profile?

Here is an analogy that may help: Consider the management of a big car park/parking lot. The car park management (Apple) wants to enforce which car (app) can be parked by which driver (developer) in which parking lot (devices).

Well, I don't think any parking management would get down to that level of details - but you get the idea.

Let's take a closer look.


### Provisioning profile

Provisioning profile is where the system (Xcode/iOS/other Apple OS) gathers the information it needs, before deciding if building/installing/running an app is allowed:

- Who is the developer?
- What app is this provisioning profile for? I.e. What is the App ID?
- If the app is still under development, which devices are allowed to run it?

Provisioning profile needs to be created in Apple dev center first, then downloaded and opened in Xcode to become an available option in Xcode build settings. When an app is built, a valid provisioning profile must be specified, otherwise the build process will fail.

When you create a provisioning profile, you provide the answers for the questions above, by selecting App ID, certificates and devices. During build/run time, info in Xcode build settings are verified and need to match the info in the profile.

Using the parking analogy earlier, imagine registering a parking request - Here is my driver licence, this is my car plate number, and I want to park in this spot. Provisioning profile is like a piece of document recording such information. When you request to park the car, management will check it and verify all the info, before the request is granted.

What can go wrong with provisioning profile?

- It expires 12 months after the creation date.
- Provisioning profile needs to be downloaded and opened, before it can be used in Xcode build settings. For example, if a new provisioning profile is required and has been created by your team member, the new profile may have already been selected in source code (since your team member has committed the change), but you need to download and open the profile first before the build can be run.
- A provisioning profile can become [invalid](https://developer.apple.com/library/archive/qa/qa1878/_index.html) when there are changes to its associated certificate or App ID. A new profile is probably required.
- In some cases, expired/invalid profiles can cause problems even when valid profiles exist and selected.
 Check `~/Library/MobileDevice/Provisioning Profiles` to see if there are expired/invalid profiles.

### Certificates

Certificates in the provisioning profile identify the developer. This allows Apple to trace back the identity of a real person or organisation that published an app. This is why the certificate is also known as the code signing identity.

Using the parking analogy again, the certificate is like a driver licence. Driver licence identifies the driver. Certificate identifies the developer. 

- A certificate is created/requested by you, the developer, either through Xcode or through Apple developer portal. Generated certificates can be found in the Keychain app. 
- When a provisioning profile is created, one or more certificates are selected. This defines which developers are allowed to build using this provisioning profile. With the parking analogy, this is like specifying who can drive the car into the parking lot.
- During build time, Xcode checks that the current Code Signing Identity specified in build settings is among the certificates recorded the provisioning profile. 
- With this certificate, Xcode is able to "code sign" the output app bundle during the build process, therefore forever associating the output binary with the person who developed this software. This is why the certificate is also called "Signing Certificates" and "Code Signing Certificates".

There are many types of certificates, just like there can be different types of driver licences which determine the associated types of vehicles. The important distinction is development vs distribution certificates. Only distribution certificates allow publishing to the app store, and you can only get them if you join the $99 USD/year Apple Developer Program. 

One more thing about certificate. When it is generated, there will actually be two parts: The certificate itself, and an associated private key. 
	- The private key is a file stored in the computer where you generated your certificate.
	- If you use a new computer for development, and you want to reuse the old certificate, you will need  to export private key from old computer and import to the new one. Alternatively, you can create a new certificate from the new computer.
    
What can go wrong with certificates?

- It expires 12 months after the creation date.
- Missing/invalid certificate - perhaps a new machine is being used.


### App ID

When you create a provisioning profile, you have to choose an App ID. Before an App ID can be selected, it has to be created first in Apple Developer portal -> "Certificates, Identifiers & Profiles" -> Identifiers.

When you create a new App ID, you will be asked to select an App ID Prefix (e.g. `1A2BC345D6`) which is generally your Team ID ([an Apple note on this topic](https://developer.apple.com/library/archive/technotes/tn2311/_index.html)) and also enter a Bundle ID (e.g. `com.companyname.appname`). Together, they form the full App ID which generally looks like `1A2BC345D6.com.companyname.appname`.

The App ID terminology can be a bit confusing. 

- In many places, the Bundle ID itself without the prefix is shown as the App ID. E.g. the App ID field when you go to Apple portal and open a provisioning profile.
- On the other hand, the full App ID is supposed to be a combination of both parts, e.g. `1A2BC345D6.com.companyname.appname`. E.g. the App ID field when you go to Apple portal and try to _edit_ a provisioning profile.

When the provisioning profile is used during build/run time, Xcode/the system double checks that the Bundle Identifier in build settings matches the App ID in the profile. Using the parking analogy again, the App ID is like a car plate number. It should be provided when you register a parking request, and it should be checked when you actually park the car.

In Xcode, Bundle Identifier of the app is specified in Project -> Target -> Info -> Custom iOS Target Properties -> Bundle identifier. It usually has a value of `$(PRODUCT_BUNDLE_IDENTIFIER)` which means the actual value in in Build Settings -> Packaging -> Product Bundle Identifier.

What can go wrong?

- Sometimes multiple App IDs are used for the same app, with different suffices such as `.debug`, `.test`, `.prod` so that different version can co-exist on the same device. It is then easier to have a mismatch between the Bundle ID in the particulr build scheme and App ID in the provisioning profile.


### Devices

When a provisioning profile _for development_ is created, you need to select devices. This is where you select the actual iPhone where you are going to run your app while it is in development. Before the device can be selected, you need to add it using the Devices section first. 

Remember our analogy again, parking management is not only controlling who can drive which car, but the parking spot is also pre-allocated. Apple is controlling which device your app can run on while it is still in development.

The reason is that apps still in development have not been reviewed by Apple yet. If it is a buggy or malicious app, the only harm it can do is to the devices registered by the developer.

What can go wrong?

- Your test device may not have been added to the provisioning profile yet.


### Conclusion

Code signing issues can be confusing. Keep the big picture described above in mind, consider if there have been recent changes (e.g. expiration) in any of the three aspects, should help you narrow down the root cause more easily. Good luck!

If you have more questions about code signing, feel free to ask in the comments section below!

