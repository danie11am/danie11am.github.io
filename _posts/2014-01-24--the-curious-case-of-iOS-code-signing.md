## The curious case of iOS code-signing 

This post was originally posted on 2014-01-24.

iOS app code signing issues can be painful. There are simply too many things that can go wrong!

This post does not cover any specific code signing issues. It attempts to explain the concepts and motivations behind Apple's code signing mechanism, which hopefully equips us to deal with any variants of code-signing issues in the future.

### The big picture

Why is code signing needed? It's all about security.

> Apple offers app security through such things as mandatory code signing 
- https://support.apple.com/en-au/guide/security/sec7c917bf14/web

So how does code signing improve security? When an app is to be installed or run on an Apple device, Apple wants to know the identities of the app, its developer, and the device. If the build/run process does not have all the answers, or if any answer is not satisfactory, the app will not install or run and the error is expressed as some kind of code signing issues.

Here is an analogy: Consider a big car park/parking lot with many cars and car spaces. The car park management (Apple) wants to enforce which driver (developer) is parking which car (app) at which parking lots (devices). 

Well, I don't think any parking management would do that - but you get the idea.

Let's take a closer look.


### Identity of the developer

Who is the person or the organisation behind an app? Apple gets this info from "certificates". 

In order for Apple to answer this question, Apple requires every rightful developer to obtain a "certificate" and use it as a form of identification. This is a way for Apple to hold developers accountable as Apple can invalidate your certificate any time.

Keeping that in mind, try to understand the following facts:

- The certificate is requested by you, the developer, either through Xcode or through Apple's dev center portal.
- You will only get the certificate if you join Apple's US$99 per year developer program.
- The certificate is used to code-sign your app, which is basically "the process by which your compiled iOS application is sealed and identified as yours". This is why the certificate is also called "Signing Certificates" and "Code Signing Certificates".
- There are two types of certificates that you can request, depending on your needs: 
	- "iPhone Developer" certificate lets you run your app through Xcode on usb-connected iOS devices
	- "iPhone Distribution" certificate lets you submit your app to app store or  to distribute remotely via "ad-hoc" distribution
- While generating a certificate, two parts will be generated: The certificate itself, and an associated Private key. 
	- The Private key is a file  stored in the computer where you generated your certificate. 
	- If you use a new computer for development, you wlil need to export Private Key from old computer and import to the new one, before you can use the certificate for code-signing.
- When you have the certificate in your computer, you can see it in the Keychain app as shown below.

Re-cap: **You need a certificate. Only Apple can give it to you.**


### What is this app?

Has this app been registered by this developer?

Now you have your certificate to prove to Apple that you are a legitimate Apple developer. Are you allowed to publish an app right away? No. Because Apple wants to know and enforce exactly what apps you can publish.

To look at the bright side, if a bad guy somehow stole your source codes, he wouldn't be able to distribute/publish it under the same app name.

This is why you need to associate your app with a Bundle ID that must be unique across the app store and register the ID with Apple. Otherwise, you will get an error during the code-signing process.

The concept of the ID comprises a few more confusing facts:

- A Bundle ID usually looks like "com.mycompany.myapp".
- A Bundle ID can be "explicit" or "wildcard". An "explicit" Bundle ID looks like "com.mycompany.myapp", while a "wildcard" one looks like "*"
- A Bundle ID is also known as the "Bundle Identifier". 
	- In Xcode, you can set it in Project -> Target -> Info settings.
	- In Apple's dev center portal, you need to register the same Bundle ID.
- A Bundle ID is only the 2nd part of the a full App ID. A full App ID looks like "8A2KB123A9.com.mycompany.myapp"
	- The first part of a full App ID (e.g. "8A2KB123A9") is called, well, the App ID Prefix. It used to be called "Bundle Seed ID" and "Team ID" as well.
- To make it more confusing, Bundle ID is also known as the App ID occassionaly. For example if you click on App IDs in Apple Developer portal, you see the Bundle ID in the "ID" column.

Re-cap: **You need to give your app a unique ID and register with Apple.**


### Is this for development or production? If it's development, what device is being used? 

If the app is in development (so it has not been under the scrutiny of the app review process), has the device been explicity granted permission to run this app?


Even though you are a legitimate Apple developer, and you are deploying an app that you have told Apple about, it may still be disallowed! 

This third hurdle is that Apple wants to know and enforce exactly which devices you are trying to deploy to during  development and testing phases of your app. That's right, every Apple device has a unique ID, and you have to tell Apple that *"I'm Joe, I developed this app and I'm deploying it to my iPhone ABC123456"* before your app can run.

Why would Apple control this? Apple wants to control the availability of your app before Apple has reviewed it. You can't distribute malicious app anywhere. You can't deploy your apps privately to everyone to skip the sharing of revenue with Apple. 

Once your app has gone through the app approval process and is available on app store to download, there is no device limit.

Re-cap: **During development and testing (i.e. non-app store deployment), you have to tell Apple which devices need to run your app.**


### Is Apple happy with the answers?

How do you give Apple these info? And how does Apple evaluate them? 

There are two steps. 

First, you need to go to ["Certificates, Identifiers & Profiles" in iOS Dev Center](https://developer.apple.com/account) and create entries in these sections:

- **Certificates** - Create your developer identity and download to your computer. Xcode might have already done this for you.
- **Identifiers** - Create a unique ID for each app you are creating.
- **Devices** - Tell Apple which devices may be used during development and testing phases.

Second, since it is possible to for you to have many certificates, many app IDs and many devices, you need to create a Provisioning profile to specify the exact combination for a particular app deployment configuration: who you are, which app is being developed and which devices you plan to test it on.

[The Provisioning Profile](/blog/content/images/2014/Feb/code_signing_2.png)

Still confused? Remember our parking analogy?

> Here is an analogy: Consider a big parking complex. There are many people driving many cars to park in many parking lots. At any time, the parking management wants to know and enforce which driver (developer) is driving which car (app) to which parking lots (devices).

After you have created the Provisioning profile, you need to download it in Xcode. Apple will then check it at various points during deployment and app submission to ensure that it is happy with your answers to the 3 questions above.


### Conclusion

Understand that the build/run process requires verification of three parties (app, developer, and device) through the use of provisioning profile. Hopefully this helps When facing code signing issues, consider which of these could be causing the issue. 

Any follow-up questions? Please comment below.



