## Predix Mobile iOS Example -- Device Application States

iOS has various application states to which your Predix Mobile webapp can subscribe in order to be more aware of its running environment. These include:

* Did Enter Background
* Will Enter Foreground
* Did Receive Memory Warning

There are many other application state notifications, but these are some of the more useful ones.

### Did Enter Background
The Did Enter Background state is announced with the notification "_UIApplicationDidEnterBackgroundNotification_". When an application receives this notification it indicates the iOS application is no longer the foreground application; it has been pushed to the background. At this point the application has a short time (a few seconds) to perform any state-preservation it may wish to do before the iOS operating system hibernates the application.

It is as this time a Predix Mobile webapp should preserve any state it may wish to maintain between running instances.

### Will Enter Foreground
The Will Enter Foreground state is announced with the notification "_UIApplicationWillEnterForegroundNotification_", and is essentially the opposite of the Did Enter Background state. This state indicates the application is resuming from being in the background. In general the application will continue as if the application was never backgrounded. 

This notification can be useful to ensure the running environment has not changed significantly while the application was in the background. For example, verification of authentication and online status, since other notifications may have been missed while the application was in the background.

### Did Receive Memory Warning
This state, announced with the notification "_UIApplicationDidReceiveMemoryWarningNotification_", alerts the application that it has physical memory pressure it must respond to. Failure to release memory resources when this warning is given can result in an application being denied additional memory when requested, or the application being shut down. When this notification is received the Predix Mobile webapp should immediately take steps to reduce its memory footprint.
## Before You Begin
To get started, follow this documentation:
* [Get Started with the Mobile Service and Mobile SDK] (https://www.predix.io/docs#rae4EfJ6) 
* [Running the Predix Mobile Sample App] (https://www.predix.io/docs#EGUzWwcC)
* [Creating a Mobile Hello World Webapp] (https://www.predix.io/docs#DrBWuHkl) 

### Demo webapp
In this Predix Mobile webapp example, we subscribe to the above three notifications and show how state can be stored in a local couchbase document when the application is backgrounding, and then retrieve that state when the application resumes to the foreground.

Separately, it also demonstrates listening for memory warnings, and releasing memory when this notification is received.

**Note about memory warnings and the iOS simulator**

Because it's running on your Mac, the iOS simulator does not have the same memory interaction as a real iOS device. Often the simulator can use far more memory than a device can, and then when it does reach a limit, the simulator or the App crashes rather than issue the _UIApplicationDidReceiveMemoryWarningNotification_ notification. When testing memory handling in the simulator, use the **Simulate Memory Warning** option in the Hardware menu rather than waiting for the warning to come from actual memory usage.

### Other application state notifications
Further details about iOS application state notifications can be found in Apple's documentation here: [Notifications](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIApplication_Class/#//apple_ref/doc/uid/TP40006728-CH3-DontLinkElementID_4).

Not all of these notifications are possible for a Predix Mobile webapp to receive. For example the  "_UIApplicationDidFinishLaunchingNotification"_ occurs when the iOS application first starts, but since a Predix Mobile webapp is not yet running at that time, the webapp cannot receive that notification.

