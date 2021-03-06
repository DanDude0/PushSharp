PushSharp v3.0 - The Push Awakens!
==================================

PushSharp is a server-side library for sending Push Notifications to iOS/OSX (APNS), Android/Chrome (GCM), Windows/Windows Phone, Amazon (ADM) and Blackberry devices!

PushSharp v3.0 is a complete rewrite of the original library, aimed at taking advantage of things like async/await, HttpClient, and generally a better infrastructure using lessons learned from the old code.

[![Join the chat at https://gitter.im/Redth/PushSharp](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/Redth/PushSharp?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

![AppVeyor CI Status](https://ci.appveyor.com/api/projects/status/github/Redth/PushSharp?branch=3.0-dev&svg=true)

 - Read more on my blog http://redth.codes/pushsharp-3-0-the-push-awakens/
 - Try out the latest -PreRelease [NuGet Packages](https://www.nuget.org/packages/PushSharp/3.0.0-beta29)
 - Join the [Gitter.im channel](https://gitter.im/Redth/PushSharp) with questions/feedback

---

## Sample Usage

The API in v3.x series is quite different from 2.x.  The goal is to simplify things and focus on the core functionality of the library, leaving things like constructing valid payloads up to the developer.

Here is an example of how you would send an APNS notification:
```csharp
// Configuration
var config = new ApnsConfiguration ("push-cert.pfx", "push-cert-pwd");

// Create a new broker
var broker = new ApnsServiceBroker (config);
    
// Wire up events
broker.OnNotificationFailed += (notification, exception) => {
	Console.WriteLine ("Notification Failed!");
};
broker.OnNotificationSucceeded += (notification) => {
	Console.WriteLine ("Notification Sent!");
};

// Start the broker
broker.Start ();

// Queue a notification to send
broker.QueueNotification (new ApnsNotification {
		DeviceToken = "device-token-from-device",
		Payload = JObject.Parse ("{\"aps\":{\"badge\":7}}")
	});
   
// Stop the broker, wait for it to finish   
broker.Stop ();
```

## Interested in helping to test?

If you have an app with a significant number of installs/users, and are interested in helping test this library at scale, please contact me!  

 

## Roadmap

 - **APNS - Apple Push Notification Service** 
   - Working!
   - Uses latest binary format from Apple's docs
   - Untested on very large scale (Looking for help here!)
   - Waiting for Apple to release HTTP/2 transport specs to add support
 - **GCM - Google Cloud Messaging** 
   - HTTP transport working!
   - XMPP transport still under development
   - Untested for Chrome (should work in theory)
 - **ADM - Amazon Device Messaging**
   - Working!
 - **Windows**
   - Working (Basic toasts tested)
   - Drops support for WPS (old Windows Phone push service)
   - XML Payload is up to the developer to construct
 - **Firefox**
   - The SimplePush format should work, but is untested
   - There's some push to make W3C standardized push notifications, waiting to see how this plays out
 - **Blackberry**
   - Untested - code ported from previous PushSharp releases
 - **Other**
   - More NUnit tests to be written, with a test GCM Server, and eventually Test servers for other platforms
   - New Xamarin Client samples (how to setup each platform as a push client) will be built and live in a separate repository to be less confusing
   


License
-------
Copyright 2012-2015 Jonathan Dick

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
