# devtools-and-swift-lounge QAs
### Lounge Contributors
#### [pol-piella](https://github.com/pol-piella)
#### [emin@github](https://github.com/roblack) / [emin@twitter](https://twitter.com/emin_ui)
#### [shirblc](https://github.com/shirblc)
#### [tikimcfee](https://github.com/tikimcfee)
---

--- 
> ####  Good morning. How can I automatically launch multiple simulators at once by the run keyboard shortcut? (e.g., iPhone 8, iPhone X, iPad, 14.5/16)


|U03HL5ECHUL|:
Xcode doesn't currently support selecting multiple run destinations for the Run action to use. It would be great if you could file a feedback request asking for that feature and provide some details on how/why you'd use it.

|U03HHPY6YQ2|:
Hi Ryan (great name!)

As a follow-up to Scott, the `simctl` command line tool might be an option for you. It can perform many of the same tasks as Xcode and you can operate it via script

|U03HL5ECHUL|:
Another possible alternative that might work for you is to use test plans with schemes to automatically run through a set of tests across multiple devices.

|U03HWD593CH|:
Hello Ryan,
If you are looking to speed up running Unit or UI tests on simulators you could enable an option on your testing targets to “Execute in Parallel”. This will boot many copied simulators at once and speed up test execution.

|U03JSPMPTMW|:
<@U03HL5ECHUL> &amp; <@U03HWD593CH> these are both awesome options! Consume them all of the time across all sorts of language and hardware combinations.

|U03J4DLGHUL|:
<@U03HHPY6YQ2> <@U03HL5ECHUL> I'll check your suggestions out. One use case is, before submitting a PR with UI work, I like "see to believe" one last time, preferably on sims already setup with various accessibility settings or staging environment user accounts. It would be great to mix in with my live test devices, one of which is always on a verbose VoiceOver setting.

|U03HL5ECHUL|:
I'd encourage you to look at test plans with a combination of unit tests and UI tests to automate some of that confidence building. Where you are using Swift UI you can also use previews to get better visual coverage of changes made to the UI.

|U03J1V52JQK|:
Snapshot tests (not from Apple, but there are libraries) are also a great tool here. They let you record snapshots of views and then diff the images if they change.

--- 
> ####  I'm working on an Apple Watch app that uses compass heading for navigation. However, the debugging on an actual watch is slow. Is there a way to simulate compass data using the watchOS Simulator?


|U03HHPY6YQ2|:
I believe the compass requires physical sensors on device, and therefore it is not available in Simulator

|U03HHPY6YQ2|:
As always, file feedback using Feedback Assistant if this is important to you!

|U03HL5EFWP6|:
Indeed, we do not currently support simulating Compass but we would be interested in hearing about your use case in detail

|U03JRR4R3CY|:
Thanks, I filed a feedback for this last year and was hoping to see it this year. I had already checked the simulator but did not find the feature.
The feedback number is FB9161848

The use case is an app for theme parks, these locations are poorly mapped if mapped at all. So I wanted to offer a simple navigation using an arrow indicating the direction. And a similar case for a public water fountain finder for Apple Watch

|U03HHPY6YQ2|:
Thanks for your feedback

--- 
> ####  Hi, are there plans to include network link conditioning into Xcode/simulators so that we can simulate bad network connections on apps running in simulators without impacting the entire computer's internet connection (or requiring the use of software like Charles or Proxyman)?


|U03HL5EFWP6|:
Each Simulator is a separate userspace but shares the kernel and networking stack with macOS so there is not presently a way to do this.

|U03HL5EFWP6|:
But we are aware of the desire for this!

--- 
> ####  Good day! How can I change simulators location and language programmatically?  My primary use case is Unit Tests need to check multiple locations and languages and am hoping for a better option than shell scripts that change plist files...


|U03J7H4CG6L|:
In your test plan file, there's a "Simulated Location" setting for configuring the system location, as well as an "Application Language" setting.

|U03J7H4CG6L|:
If you need to change the system location/application language outside of a testing context, could you file a feedback assistant requesting this?

|U03JE7PBQPP|:
TestPlans allow for a change to the App Language and Region, but not the System Lang/Region

|U03JE7PBQPP|:
And for testing things like various UTF formatting (Currency definitely comes to mind), it really helps to be able to change the System levels

|U03HL5EFWP6|:
Currently Simulator does not surface a UI to change this directly. Instead of manipulating plists you can use `simctl` to spawn the `defaults` utility inside a given Simulator:
`xcrun simctl spawn &lt;device&gt; defaults write -globalDomain AppleLanguages -array en-US fr-FR`

|U03HL5EFWP6|:
The same goes for the `AppleLocale`

--- 
> ####  Is there any means of using the iOS Simulator for ARKit or AVFoundation, wherein the Mac's camera (or Continuity camera in macOS Ventura) could be used for testing AR or camera experiences?


|U03HHPY6YQ2|:
Hi Brandon. I don't think this is supported at present. Please file a feedback request if this is important to you

|U03J20E7UBV|:
Thank you!  :slightly_smiling_face:

|U03HL5EFWP6|:
It is especially helpful when you can provide motivating use cases: What does this enable or how does it improve your dev workflow?

|U03J20E7UBV|:
Thanks, <@U03HL5EFWP6>!  I will file a feedback request.  Mostly, this is a request to be able to quickly test ARKit-related code without `#if` statements, though this might only be practical for ARFaceTrackingConfigurations rather than ARWorldTracking.  I'll write up a use case and file a Feedback Request.  Thank you!

|U03JH1ULBCH|:
Hi <@U03HL5EFWP6> Even if the simulator didn't provide live camera, that we are unable to ~test~ run any Camera code in the simulator has been a big pain point over the years. If it even just let us run the camera APIs and delivered a static/placeholder image would be a huge help

--- 
> ####  Hi, are the simulators being updated to support Stage Manager? What about Stage Manager support in external displays?


|U03HL5EFWP6|:
This is a known bug for Seed 1 that didn't make it into the release notes

|U03HWD3Q7LH|:
Support for enabling Stage Manager will be coming in a future beta.  Check out our future release notes for information.

--- 
> ####  How can I build and run in the command line using the "My Mac (Designed for iPad)" destination? I've tried `xcrun simctl list devices`, but it does not have this destination.


|U03HWD3Q7LH|:
`xcrun simctl list devices` shows simulator devices.
To get a list of supported run destinations to pass to `xcodebuild` from the command line, check `xcodebuild -showdestinations` .

|U03J58WAJ2J|:
Ok, I can specify the macOS destination but it seems to build the usual iPad `.app`. How do I install and run it on the mac? From my investigations, Xcode seems to create a wrapper app inside the `.XCInstall` folder.

|U03J58WAJ2J|:
Do you think I should request this in a Feedback? Anyway, thanks for your time.

|U03HWD3Q7LH|:
I believe you should be able to just pass it to the `open` command line tool or double click it.

|U03J58WAJ2J|:
I can’t :disappointed:
```
⇒  open <http://mmyapp.app|mmyapp.app>
The application cannot be opened because it has an incorrect executable format.
```
And the application icon have that :no_entry_sign:

|U03HWD3Q7LH|:
Ok, well it's getting built right (as an iPad app).  But we should be able to launch it.  I need to dig into this a bit more with experts in this area.  Can you please file a report with Feedback Assistant and report the number here?

|U03J58WAJ2J|:
Here it is: FB10138671

--- 
> ####  Simulators and previews take up a ton of space on my Mac. I noticed in particular Previews keeps growing and growing to the point where the previews folder in the developer folder takes up more that 100gigs of space. Is there a way to limit the amount of space previews and simulators take up, and if so what is the best and safest way to clear that space?


|U03HL5EFWP6|:
There were some known issues with Xcode 13 that should be fixed in Xcode 14. Please file a feedback request with the disk space usage output from a program like GrandPerspective if you can. We would like to know what files are taking up the space and if you still see the issue in Xcode 14.

|U03HL5EFWP6|:
In the mean time you can use `xcrun simctl erase` to erase simulators you don't need anymore.

|U03K7KBHSV6|:
Thank you!

|U03HL5EFWP6|:
Previews, Interface Builder, and Playgrounds use separate device sets which you can specify. So for example to list all Previews simulators:
`xcrun simctl --set previews list`
Then do the same with the erase/delete subcommands.

|U03JSBCTCQ1|:
Thanks for this question, just removed almost 100gb of previews files today :slightly_smiling_face:

--- 
> ####  This is not a question, but just so your team sees it, thank you for the amazing work you do.  Xcode and Simulators are the tools so many of us use to build fun apps, or even build businesses that support our lives.  We couldn't do this without your hard work.


|U03HHPY6YQ2|:
Thank you Brandon!

|U03HHPY6YQ2|:
Speaking just for myself, helping developers reach their potential is why I do this, and I love seeing what people create using our tools!

|U03HL5EFWP6|:
Thank you, positive feedback like this is always appreciated!

|U03J20E7UBV|:
Thank you all for the replies!  It's great to have that incredible support and it makes doing this work fun and empowering every day.  Keep up your great work!

|U03H36U6JHM|:
Thank you Brandon!

|U03J4CWFAN8|:
Argh, wrong thread, sorry

--- 
> ####  What's the best place to turn to for help in getting our apps to run in Xcode 14 (errors like ld: symbol(s) not found for architecture x86_64 are commonplace).  It'd be great to start testing Xcode 14 to have the chance to find questions to ask before WWDC ends!


|U03HL5EFWP6|:
Is this specifically about such errors when building for Simulator? If this is a general project build issue please come back for the Xcode Open Hours Lab.

|U03J20E7UBV|:
Good question, <@U03HL5EFWP6>.  I didn't even think to try on-device vs. Simulator.  I'll come back for Open Hours.  Thank you!

|U03HL5EFWP6|:
If this is Simulator specific then can you tell us if you are on an Apple Silicon Mac or not? Missing symbols errors are often a result of a link time dependency being missing or using a binary dependency that doesn't have all the architectures required

|U03J20E7UBV|:
This is indeed on an Apple Silicon machine (running Xcode 14 in Rosetta).  Regretfully, the app I work on has quite a few dependencies outside of my control, requiring Rosetta, but the aforementioned symbol error is seemingly the last barrier standing in my way to getting the app to run on Xcode 14.

|U03HL5ECHUL|:
There is also the Xcode Build Performance lab on Thursday (9am-1pm) where they are happy to field questions about getting your project to build properly.

|U03HL553PNG|:
We have an Xcode Open Hours Q&amp;A here in Slack tomorrow at 3pm to 5pm Pacific Time.  Or you can make a 1:1 appointment at <https://developer.apple.com/wwdc22/labs-and-lounges/dashboard/upcoming?q=Xcode>

|U03J20E7UBV|:
Thanks <@U03HL5EFWP6> <@U03HL5ECHUL> <@U03HL553PNG>!  I will make an appointment if I'm not able to resolve it!

|U03HWD3Q7LH|:
Please be sure to come prepared with a build log for folks to look through.  It might even be quicker to post your question (and log) in the developer forums.  If you do so, please provide a link to the dev forums in this thread to connect the conversations.

|U03JLB0L3L1|:
Just want to confirm the issue and add some additional information and observations. We have a project that depends on XCFramework distributed through SPM, that contains frameworks for ios-arm64 and ios-arm64-simulator (basically iPhone and Simulator on M1 Mac. Everything works fine on Xcode 13 but on Xcode 13 the framework does not link properly. Judging from the error message: “warning build: Ignoring file “/path to binary/“, building for iOS Simulator-x86_64 but attempting to link with file built for iOS Simulator-arm64” it appears that for some reason Xcode tries to build for x86 Simulator architecture, which is obviously not correct considering that Xcode runs *without* Rosetta and on a M1 Mac. My biggest fear is that this behavior somehow slipped into new version as a way of ‘fixing’ a major problem with old-style fat-frameworks that contain a slice for x86 simulator and thus de-facto break Simulator on M1 machines, and people do all sorts of terrible things with their project to hot-fix the problem.

P.S. For folks interested in details there is great blog post by _Bogo Giertler_ titled “_Hacking native ARM64 binaries to run on the iOS Simulator_”.

|U03JLB0L3L1|:
Small follow-up: our project depends on framework that only distributed for arm64 (which is not uncommon)  so on x86 machines we don’t have simulator, but we managed to get everything working on m1's by duplicating and modifying an arm64 slice to iphonesimulator and creating xcframework with it, which greatly improved our dev experience. I truly hope what we see on Xcode 14 is bug.

|U03J4CWFAN8|:
<@U03J20E7UBV> I have successfully converted one of our projects to build successfully for the arm64 simulator, but every library, every case is different. In our case, I have two cases:
1. Recompile the library for the arm64 simulator, and then link different libraries for device and simulator (if you want to go the extra mile, build an xcframework)
2. Find a new (in our case) beta version of the dependency we used
3. What <@U03JLB0L3L1> pointed

|U03JLB0L3L1|:
Also forgot to mention that undefined symbols problem mention in the beginning results from framework not linking and xcode treating this as a warning and not an error, so the build pipeline continues without necessary symbols. I submitted the overall bug report with the following reference: FB10068243

|U03J20E7UBV|:
Thanks, <@U03J4CWFAN8>!  In my case, our frameworks and dependencies are building fine on Apple Silicon in Xcode 13.4.1 (running in Rosetta), but I'm getting an error doing the same in Xcode 14, and was hoping to get going with Xcode 14 during WWDC!

--- 
> ####  Will it ever be possible to simulate accessibility gestures (such as the VoiceOver “Z” gesture and the rotor) in the simulator, especially in calls from UI Tests?


|U03HHPY6YQ2|:
We can't discuss what might or might not be coming in future releases. If this is important to you, please file feedback through Feedback Assistant 

|U03HHPY6YQ2|:
It would also be helpful to know your use case 

|U03JBJTFHEZ|:
Anything that can’t be put into either, or both of, unit or UI tests, can’t be counted on to keep working as we continue to develop our code. Everything we rely on our code to do should be testable. “What gets measured matters” and automated tests are a key tool of “measuring” our code, and maintaining good engineering practice.

Right now, accessibility interactions are subject to significant risk of regressions because we have to resort to manual testing in our apps.

It feels like, while Apple has developed some incredible, and sometimes life-changing, accessibility features, they’ve only gone half way by leaving them out of automated testing.

|U03J7H4CG6L|:
We definitely agree with you on the importance of being able to test the accessibility features of your app! There's no UI testing API today for automating the VoiceOver gestures you listed, but this would be a great enhancement. If you capture that request into a feedback assistant, it would be really appreciated. You can also check out one of the "Xcode Cloud and testing open hours" labs to discuss this use case.

--- 
> ####  Hello everyone, for Xcode project having large codebase, every time if its a clean build or branch change, Xcode start preparing the indexing which causes whole mac system makes so much slower until indexing task completes. Any workaround or tips for it?


|U03HL5ENC1J|:
Hi there, indexing is an integral part of Xcode so that other features like Source Editor will work optimally. If indexing is causing your system to be slow, please file a feedback using Feedback Assistant. We will follow up to ask for logs so that we can triage the problem correctly.

|U03HZ47UMRT|:
How about breaking your code into Swift Packages?

|U03HZ3E3GTF|:
I have the same issue.
There are two threads about this issue on the forum:
<https://developer.apple.com/forums/thread/694058>
<https://developer.apple.com/forums/thread/699112>
This issue is still present in Xcode 14 :-(

--- 
> ####  Hey guys! In order to inject a dynamic library to an application we use `xcrun simctl launch` with `SIMCTL_CHILD_DYLD_INSERT_LIBRARIES` argument, which works only for iOS Simulators of course. What is the alternative for iOS real (hardware) devices?


|U03HL5EFWP6|:
This is not something supported for on-device debugging for iOS, watchOS, or tvOS. The system only allows a specific approved list of dylibs to be injected (the ones used by Xcode for things like View Debugger).

|U03HL5EFWP6|:
You must build and link any dylibs you want to use with your app.

|U03J1TVG79R|:
Thanks <@U03HL5EFWP6>!
Is there a reason behind why you don’t enable this for on-device debugging? Even if the app is code signed by you?

|U03HWD3Q7LH|:
Is there a use case you have that isn't solved by directly linking in your dylibs?

|U03J1TVG79R|:
Yes, for example I have some library that I’d like to insert only from CI agents for debugging (logging and monitoring) purposes

|U03HL5EFWP6|:
So is the issue that you want your CI to build once, make the artifacts available everywhere, but only load your logging library when running tests in CI?

|U03J1TVG79R|:
Exactly

|U03HL5EFWP6|:
So there is not an easily supported way to do this but in theory if you weak link the dylib and include it in your build you could take the built app, remove the dylib from its bundle, then resign it to update the list of resources in the bundle. Trying that will be very fragile and I don't recommend it because you will be fighting `xcodebuild` the entire way.

|U03HL5EFWP6|:
Instead I recommend you include your code and use some kind of switch to enable or disable it at startup.

|U03J1TVG79R|:
&gt; Instead I recommend you include your code and use some kind of switch to enable or disable it at startup.
It feels very risky to have this code part of the app, I want this to be as far as possible from production :smile:

|U03J1TVG79R|:
&gt; So there is not an easily supported way to do this but in theory if you weak link the dylib and include it in your build you could take the built app, remove the dylib from its bundle, then resign it to update the list of resources in the bundle. Trying that will be very fragile and I don’t recommend it because you will be fighting `xcodebuild` the entire way.
Interesting, what kind of fighting? Is there anything specific you can think of?

|U03HL5EFWP6|:
Normally you would build the app for distribution if we are talking about a device build. You'd need to unwrap that, make the change, resign the app bundle manually with all the correct flags and the correct signing identity, then get it packaged back up for distribution.

|U03HWD3Q7LH|:
Also, in order to weak link it, the dylib (or tbd) actually needs to be there.  So you'd need to build it even though you don't use it, which is not ideal.  As you start to unwind all of that with various build settings, I expect you'd converge on adding it to OTHER_LD_FLAGS[config=Debug] (or similar) and adding something to GCC_PREPROCESSOR_DEFINITIONS[config=Debug] to only use and link it in your debug configuration.

|U03J1TVG79R|:
&gt; So you’d need to build it even though you don’t use it, which is not ideal
indeed :confused:

|U03J1TVG79R|:
OK, thanks for the answers guys

--- 
> ####  Will it ever be possible to get the Apple Watch battery level via 3rd party apps? I do not see any documentation for this.


|U03HL5EFWP6|:
I'd recommend bringing this to the watchOS Developer Lab. They'd be able to give better guidance than we can

|U03HMD6FP9V|:
Will do.

|U03HL553PNG|:
You can <https://developer.apple.com/wwdc22/labs-and-lounges/dashboard/upcoming?q=watch|sign up for a 1:1 appointment> in one of the watchOS labs too.

--- 
> ####  Can I try Focus filters in the simulator? Thanks!


|U03HL5EFWP6|:
Which aspect of Focus Filters are you interested in?

|U03HMEM4TM5|:
I can’t seem to find a way to test focus filters implemented in a third-party app in the simulator

|U03HMEM4TM5|:
(i.e. there’s no “Focus” item in Settings, no control center in the simulator)

|U03HL5EFWP6|:
OK so you are using App Intents to set a focus filter but don't have a way to test the behavior?

|U03HL5EFWP6|:
Can you please file this as a feedback item?

|U03HMEM4TM5|:
correct! will do, thanks

--- 
> ####  When running UI tests, we sporadically get an error like this inside of XCTest (when trying to do element queries), and it seems like it persists until restarting the simulator. Any tips for how to debug this kind of thing or what might cause it?  `*** -[__NSArrayM insertObject:atIndex:]: object cannot be nil (NSInvalidArgumentException)`


|U03HL5EFWP6|:
Is this a crash happening in your application code, your test code, or an Apple framework?

|U03HL5ENC1J|:
To help you with your triage, you can create an `Exception` Breakpoint in the Breakpoint Navigator, run your test and examine your backtrace.

|U03HL5ENC1J|:
Note that the exception will be raised in `Foundation` framework but the important stack frames are the ones that set up the `NSArray`

|U03JRNYGBNC|:
It seems to be in test frameworks, but I'll do the exception breakpoint and get more info next time I see it. It's kind of tricky because it seems to show up randomly and then when you want to try to debug it, it won't show itself :slightly_smiling_face:

|U03J7H4CG6L|:
What iOS version are you hitting this on? There was a bug impacting UI element queries that was fixed in the iOS 15.6 beta, which you may be hitting.

|U03JRNYGBNC|:
I saw it today with 15.5, but I think it's been going on for the last few versions...can't run the Xcode 14 beta yet due to IT policies :(

|U03HL5ENC1J|:
You can leave your Exception breakpoint on until you see the problem. In terms of performance impact, it is minimal. You can choose to delete the breakpoint after that.

|U03J7H4CG6L|:
If you manage to capture the backtrace before the end of today's digital lounge, you can post it here, or you could check out one of the "Xcode Cloud and testing open hours" labs and discuss the issue with one of the engineers there.

|U03JRNYGBNC|:
sounds good! i've requested an appt about some other UI testing stuff too, so if I get a trace before then I'll share it as well

|U03J7H4CG6L|:
Also, correction to what I said earlier: the bug I was referencing was fixed in the Xcode 14 beta, not in the iOS 15.6 beta. So the device OS version doesn't matter in this case, you just need to build &amp; run your tests with the Xcode 14 beta.

|U03JEMN82JV|:
I ran into this issue or a similar one with the same exception in XCTest on Xcode 13. I submitted Feedback FB9905764 with a sample project.
I’ll try it out with Xcode 14 beta too.

--- 
> ####  For purely logical unit tests (i.e. a Unit Test bundle - not UI Unit Tests), what difference is there between running the tests on the simulator versus running them in catalyst?  If I'm not planning on distributing my app via catalyst, would it make running unit tests using them less helpful?  I like running them in catalyst, because it seems to be easier to automate - but I don't know if there are downsides to running them like that.


|U03HL5EFWP6|:
This depends on what your code does and what your testing goals are.

|U03HL5EFWP6|:
Simulator and Catalyst have different behavior in various ways (for example Simulator treats all filesystem access as case-sensitive even on macOS where most volumes are not which helps catch errors). The Catalyst environment is also tied to the version of macOS you are running unlike Simulator. Simulator is also a separate environment with its own Photo Library, Contacts, and so forth so it doesn't affect your Mac.

|U03HL5EFWP6|:
Those things may or may not matter to you.

|U03HL5EFWP6|:
If your unit tests are just exercising data structures or doing straightforward CRUD operations on a database then you can certainly run them under Catalyst.

|U03JEAPSKE2|:
Thank you - I'm actually trying to just test out basic functionality of a shared cross-platform library that we are starting to port to iOS.  I think that the Catalyst environment is probably sufficient for what we would want in this initial phase.

However, we may at some point benefit from being able to run unit tests on a simulator as well - are there many differences between the simulator and a physical device as it pertains to unit testing?  Are there ways to automate the execution of unit tests on the simulator as well? (Sorry for the basic questions - this is the first we've dealt with development for iOS...yeah - we're late to the game! :slightly_smiling_face:)

|U03HL5EFWP6|:
Not as it pertains to unit testing unless you have specific hardware needs like BT. And yes you can automate on simulators. Many developers test on both and that is what I would also recommend.

|U03HL5EFWP6|:
If you want to build your own automation outside of what Xcode provides you can check out `xcrun simctl` as well.

--- 
> ####  We’ve recently launched widgets at my company. It was really hard to develop with the sim as the widget extension would fail to attach to the simulator after the second or third run. The simulator would crash and not be reusable until restarting the Mac. Any tips of how to avoid that?


|U03HHPYC53L|:
We improved Widget debugging in Xcode 13.3 and iOS 15.4. If you are still having issue with those versions please file a Feedback, especially when you experience any crashes! And please try the Xcode 14 beta and let us know how that works for you.

|U03HMDF31P1|:
i will make sure to file a feedback too. This is what we see once the simulator breaks

|U03H36PM1BR|:
Thanks Dominic! Please definitely do file a Feedback for that if you're still seeing this after updating to the versions Christopher mentioned. And please attach the output from `xcrun simctl diagnose` to that Feedback so we can have more information to look at up front and hopefully cut down on the back and forth. :slightly_smiling_face:

|U03HMDF31P1|:
gotta update my mac to get access to the beta xcode but im able to reproduce it with Xcode 13.4. Feedback has been filed: FB10074851 i can check the new xcode tomorrow

|U03HMDF31P1|:
Thank you for your help!!

|U03H36PM1BR|:
Sure thing!

--- 
> ####  At home when I recently updated my router from an old AirPort Extreme to a mesh Orbi system, wireless debugging has stopped working and I can never get it to work. Is there any known issues with mesh networks and wireless debugging?


|U03HES3E8DT|:
As a first troubleshooting step, I would recommend checking whether your wireless network is filtering Bonjour/mDNS traffic. Devices which have been configured for wireless development advertise their presence via bonjour and Xcode depends on this traffic to discover and connect to these devices.

|U03HES3E8DT|:
You can also check whether other devices on your network which you normally expect to be discoverable via bonjour are discoverable on your new WiFi network. That can help isolate whether the issues are specific to your development workflows or all workflows which depend on Bonjour.

|U03J4CR8WAG|:
AirPrint does work, but not sure if that uses Bonjour….thanks for the thoughts, I’ll take a look

|U03HES3E8DT|:
If neither of those steps work, please send us a feedback report and include a sysdiagnose from the mac running Xcode and the device on which you would like to develop.

|U03JEJRJS2J|:
<@U03J4CR8WAG> (I’m not an Apple Employee, but thought I would interject)…I found the free app “Flame Services Browser” useful for browsing bonjour.  I’ve used it on iOS, and looks like it’s available for macOS.  Thought that may help.

|U03JRR4R3CY|:
Also dropping in (not an Apple employee), we had this issue at my previous company as well we used an Asus router and figured out that “Airtime Fairness” was causing debugging not to work.

Not sure how this fixed it, but it might have been a broken implementation. So that is a step you could try as well.

|U03J223TNSE|:
<@U03JRR4R3CY> I`m an Asus router user and have a huge pain with wireless debugging. Did you turned “Airtime Fairness” on or off?

|U03JRR4R3CY|:
We turned it off

--- 
> ####  Is it possible to run Xcode 13.4.1 under Ventura?  If so, how do you install it?


|U03HES3E8DT|:
Hi Edward, macOS Ventura requires Xcode 14.

|U03HZ47UMRT|:
I was coming to that conclusion.  Any advice on installing Monterey on a volume?

|U03JE2L47J8|:
<https://support.apple.com/en-us/HT208891>

|U03HZ47UMRT|:
I set up a volume to install Ventura, stepped away for a little bit, and found the update was installing over Monterey :disappointed:. It's working great - good job, but need access to Xcode 13.

|U03HL5EFWP6|:
When you start the macOS installer you should be able to select a different volume to have it install to a separate one. Or you can use the `createinstallmedia` utility inside the installer bundle to make an installer disk, then restart holding Option and select that disk as the boot disk.

|U03JE2L47J8|:
Make sure to run the installer in a way you can select the volume. I guess that volume can now be for Monterey :man-shrugging: <https://support.apple.com/en-us/HT201372>

|U03HZ47UMRT|:
I thought it would ask for a destination, but it didn't.    How do I actually get Monterey?  Downloads links to the App Store, which opens Settings, which says I'm up to date (Ventura) :disappointed:

|U03JE2L47J8|:
Good question. That might be messed up from new Settings app. Usually a direct link works.

|U03HZ47UMRT|:
Yeah... not working.  Get in App Store pops up the Settings app, with no way to download.

|U03JE2L47J8|:
Try running this command in Terminal `softwareupdate --fetch-full-installer --full-installer-version 12.4` It might take a moment to start downloading. Seems to be working on my test Mac.

|U03HZ47UMRT|:
That's doing something.. but it says "Installing: 3.0%", etc.  Where is it going?

|U03JE2L47J8|:
Applications folder. It isn't really installing. Just downloading.

|U03JE2L47J8|:
You wont see it in there until it's done.

|U03HZ47UMRT|:
Right... so into some tmp space until done.

|U03JE2L47J8|:
Yeah

|U03HZ47UMRT|:
Awesome.  So should be able to run it from there, choose my "Mammoth" (yeah, I was guessing) volume, then get Xcode 13.4.1, etc.

|U03JE2L47J8|:
Rather than running it, make sure to create a USB installer. Details in the two links I shared.

|U03HZ47UMRT|:
OK... I saw all that last night.  Would be nice to just "run" it, but I understand.

|U03HZ47UMRT|:
THANKS !!

|U03JE2L47J8|:
To select another volume you need to use it to create a USB installer. No problem. Good luck!

|U03HZ47UMRT|:
FYI, success.  Thanks again.

--- 
> ####  When testing my app over different simulators it creates multiple instances of simulator which over time cause simulator to become unresponsive. The only fix I have found for this is to quit and relaunch the simulator, which in turn causes me to have to rebuild for each instance. Is there an easier way to clean my cache without me having to start over?


|U03J7H4CG6L|:
It's possible to run your app on the simulator without rebuilding it, via the `Product &gt; Perform Action &gt; Run without Building` menu item.

|U03J7H4CG6L|:
Using that action (and the similar action for testing, `Product &gt; Perform Action &gt; Test without Building`) can avoid unnecessary rebuilds in between launches of Xcode or the simulator.

|U03JP09AXLL|:
Ok, I will give that a try! Thanks so much!

--- 
> ####  Can I use SPM to build executable binaries for non apple platforms, like example Windows or Platform.custom(“nintendo”)?  


|U03HHPMJCDR|:
Swift core team member "compnerd" (Saleem) has been working on windows support for SwiftPM. Something like "nintendo" would require that someone else from the community make a similar effort.

|U03HB5XJY14|:
You can find out about platforms Swift supports for development and deployment here: <https://www.swift.org/platform-support/>

|U03JSFUKL2U|:
Ok, what the point to use Platform.custom()? And can simple developer create custom product type, like ApplePlatforms SDK?

|U03HB5XJY14|:
Great question! `Platform.custom(_:)` is intended for use when someone is adding support for a new platform to SwiftPM. During the development process, and before that official support is added, you can use the `.custom` API

|U03JSFUKL2U|:
Thats clear for me. Thanks a lot)

--- 
> ####  What is the reason for Swift Regex only being available in iOS, is it because it's a part of the standard library not the swift language it self? If so would it be possible to ship the standard library with our app to gain backwards compatibility like we did before we had ABI stability?


|U03HES7M40M|:
The swift regex API is available on all Apple platforms. See the documentation here:
<https://developer.apple.com/documentation/regexbuilder>

|U03HB5XE9K8|:
It’s because it’s part of the standard library. The minimum deployment target is iOS 16 and macOS 13. You can deploy to macOS if you target the beta macOS.

|U03HHPMJCDR|:
You cannot ship the standard library with your app.

|U03K110VCN5|:
But being able to would be extremely good, that way new features could be supported on older iOS versions. For something like regex I will have to wait at least two years befor I can use it…

|U03HHPMJCDR|:
There are some technical problems with having types defined in multiple modules (the standard library shipped with your app and the standard library that the SDK frameworks your app interacts with are linked against), so simply including the standard library with your app cannot work.

If having specific features be back-deployable to older OSes is important for you, that's a great request to provide via feedback assistant.

|U03J9D6803X|:
Is there a good resource or estimates available for how many people still use iPhone 6s, iPhone 7, and original SE?  I've seen articles written in 2022 still recommending that people buy iPhone 7 who can't afford newer phones, and I still have multiple family members who use iPhone 7.  It seems like a particularly large burden this year to either drop or continue support for iPhone 7.

--- 
> ####  When to use class and actor?


|U03HWDCSCHX|:
Good Swift style generally revolves around the use of structs instead of classes. However, if you must use a class to model something, generally you should ask if you need to protect some shared resource or state. This comes up very frequently when using async-await and Swift's other concurrency features. If you find yourself using async-await it's good to start with an actor to protect the state of the type from concurrent modifications. So when do you use classes? Generally when you need a type that has a built-in notion of identity, or when interacting with other languages like Objective-C that require subclassing.

|U03HWDCSCHX|:
For more information please see "Protect mutable state with Swift actors" from WWDC 2021 <https://developer.apple.com/videos/play/wwdc2021/10133/>

|U03JRQ81NEL|:
Thanks, like in a SwiftUI project’s view model where one method fetches data from remote and update the some properties. In this class with @MainActor is used if that property read/observed by any UI part?

|U03HHPMGVEF|:
Yes, if the class has an instance property that is read/observed by the UI, you will need a `@MainActor class`.

|U03JRQ81NEL|:
And to communicate between actors and other data types, the property should be of sendable type?

|U03HWDCSCHX|:
Yes, in general you should ensure that types are `Sendable` when crossing concurrency boundaries, including async calls. The Swift compiler has warnings and errors in place to catch a wide variety of errors that can potentially occur when writing this kind of code.

|U03HHPMGVEF|:
Yes, only `Sendable` types can be safely passed amongst different actors and tasks. The session "Eliminate data races using Swift Concurrency" (<https://developer.apple.com/videos/play/wwdc2022/110351/>) goes into detail on `Sendable` types and how they interact with tasks and actors.

--- 
> ####  SwiftUI and Swift Standard Library sometimes rely on emitting public protocols with underscored/hidden requirements that end up in module interface but – crucially – not in code completion or documentation. I would find this extremely useful as a framework vendor. Is there a way to achieve this using a special flag or attribute?


|U03HB5WK03G|:
All names beginning with an underscore are hidden by default from code completion and documentation. The Swift Standard Library and some of the packages we maintain rely on this feature to hide entry points that need to be public for technical reasons but aren’t part of the module’s public API. Feel free to use the same convention in your own modules!

|U03JRN827HN|:
Thank you for taking the time to answer my question, Karoy! That’s great news.

|U03HHPMJCDR|:
Note that you can also hide inits and subscripts by underscoring their argument labels.

|U03JRN827HN|:
Ha, nice! I would’ve never discovered this. Thanks!

--- 
> ####  Other than being open source, is Apple really interested in advancing Swift on the server? I'd love to replace some Spring Boot webservices with Swift that use Kafka/Cassandra...unfortunately the community and support just doesn't yet seem mature enough. Is this just a dream of mine or is this a priority for Apple?


|U03HES7M40M|:
Hey Sean, thanks for your question. Apple is committed to advancing Swift on Server. This is why we have three employees participate in the Swift on Server workgroup. Further we mentor in the google summer of code, where one project is a kafka library. (<https://summerofcode.withgoogle.com/programs/2022/projects/5nq3GuxI>).

Regarding your wish for a cassandra client, I have noted this and will bring this feedback to the SSWG.

|U03HHPMGVEF|:
Swift on Server Workgroup (SSWG): <https://www.swift.org/sswg/>

|U03J4CR8WAG|:
Thanks! Gives me hope of ditching Java soon…I really don’t want to start using Go

|U03HL5K6L04|:
I’d love to hear more about any specific requirements (besides the one you mention) that you feel are missing

|U03HL5K6L04|:
(not that I’m claiming that we have everything already :wink:)

|U03J4CR8WAG|:
To be honest, I haven’t taken a very hard look at it in about 2 years, but keeping me in SpringBoot is probably just the plethora of support for nearly anything that may come along requirements-wise. Off the top of my head would be Kafka, Cassandra, WebSockets, and support for securing APIs using a plethora of ways (keycloak, ping, etc - -spring security plugins seems to be available for everything).

Maybe it is time for me to go back and take another look at using it

|U03HL5K6L04|:
Thanks <@U03J4CR8WAG>

|U03J4CR8WAG|:
Thank you, <@U03HL5K6L04>

--- 
> ####  What is the plan for XPC support in Distributed Actors? Will there be support or will that be left to the community?


|U03HESHGASH|:
We’re always interested in improving our APIs, but can’t comment on unreleased products or future plans. If you have specific enhancements you’d find useful, please file feedbacks requesting them

|U03HHPMGVEF|:
Distributed Actors is designed to work across multiple transport mechanisms and with different ways of encoding/decoding data, so it should be possible to build an XPC transport that integrates fully with the language feature. The session "Meet distributed actors in Swift" (<https://developer.apple.com/videos/play/wwdc2022/110356/>) discusses several transport mechanisms.

|U03JE2L47J8|:
Thanks. This session is top of my list. I'm looking for ways to clean up my XPC communication code between processes.

|U03J7B27ADD|:
is this Distributed framework in the docs a repackaged version of <https://github.com/apple/swift-distributed-actors>

|U03HHPMGVEF|:
No, they are different things. The `Distributed` module provides the basic protocols and types needed to interact with the `distributed actor` feature of the language, e.g., the `DistributedActor` protocol, `DistributedActorSystem` protocol, etc. The swift-distributed-actors package you reference is one implementation of a distributed actor system (what I referred to as a "transport" above) for clusters. Other packages could provide alternative distributed actor system implementations for different environments.

|U03J7B27ADD|:
so if i wanted to implement a server-side distributed counterpart to a mac app, say, i’d want to use something like the above, as long as it conforms to the DistributedActors protocols?

|U03JE2L47J8|:
Can the Distributed module be baked in to an app if targeting macOS 12?

|U03HHPMGVEF|:
Yes, you could implement a new type that conforms to `DistributedActorSystem`, which communicates over HTTP or sockets or whatever with your server, and then your `distributed actor` types could communicate over that. I expect other packages with implementations of `DistributedActorSystem` will crop up in time.

|U03HHPMGVEF|:
No, the `Distributed` module cannot be baked into an app for targeting macOS 12. It's only available in macOS 13 / iOS 16-era OS's

|U03JE2L47J8|:
Thanks. Is this a macOS/iOS only feature at this point or will it be available to Linux too?

|U03HHPMGVEF|:
It's also available on Linux!

|U03HHPMGVEF|:
Any recent Swift 5.7 Linux toolchain from <https://www.swift.org/download/> provides support for distributed actors

--- 
> ####  When converting a blocking method to async like `Foundation.Process`’s `waitUntilExit()` will block the current thread, so do we embed that logic to a continuation block, or just making function async is enough?


|U03HWDCSCHX|:
You are correct that `waitUntilExit()` will block the current thread, and is therefore not appropriate. You can use Process.terminationHandler as an appropriate place to resume the continuation <https://developer.apple.com/documentation/foundation/process/1408746-terminationhandler>

|U03JRQ81NEL|:
Awesome, that sounds perfect :star-struck:

|U03HWDD6RED|:
An extra note: you may want to set the termination handler from a thread that has a run loop running, which is hard to guarantee using a task. The easiest way is to set up your task on the main actor before launching it.

|U03HWDD6RED|:
You also can use continuations to convert your termination handler use to async/await; check out withCheckedContinuation(…) for that.

|U03HWDD6RED|:
<https://developer.apple.com/documentation/swift/withcheckedcontinuation(function:_:)|https://developer.apple.com/documentation/swift/withcheckedcontinuation(function:_:)>

|U03JRQ81NEL|:
Thanks. I think I need to request for lab appointment for more details :sweat_smile::crossed_fingers::skin-tone-2:.

--- 
> ####  There is a lot of different architectural patterns out there used to separate UI from businesslogic like MVVM, MVC, VIPER and so on..  Apple still seems to be favouring MVC as the preferred style. Is that correct or are you more considering your self as agnostics in that matter?


|U03HB5XE9K8|:
This would be better answered by a UI toolkit team, but note that the standard library designers want to support every reasonable use case, including all useful app architectures.

|U03JBCWHJF8|:
:popcorn: - SwiftUI feels like it gels much more with an MVVM approach where your view is directly driven by the data massaged and bound directly in some ViewModel :thinking_face: but driven by a model.  I'm actually really curious about this too.

|U03HB5XE9K8|:
I would suggest asking this in the SwiftUI lounge; there would be better answers from that team.

--- 
> ####  Saw Foundation team merged the Foundation Swift Overlay into Foundation.framework.  Does it mean entire Swift runtime will be loaded and linked as part of Foundation (for a pure Obj-C project) for iOS 16 unconditionally?  Trying to understand what extra cost there will be during app launch (for a zero-swift project but import Foundation).


|U03HL5K6L04|:
Hi <@U03HZ3L66QM>! Great question, thanks for asking. We’ve done a lot of measurements on this with the apps that ship with the system and made a ton of small improvements all over the place to make sure that any additional dirty memory cost was ‘paid for’

|U03HL5K6L04|:
If you see any regressions or other issues, please do reach out so we can investigate. Probably via a bug report if it’s after this lab is done

|U03HL5K6L04|:
One thing to note here is that part of what we did is swap around some dependencies that previously were brought in when you `import Foundation`. That will bring in a lot fewer other libraries now

|U03HZ3L66QM|:
Thanks for answering this the measurement works!
Not sure if I understand it correctly. Do you mean that
“though now whole Swift runtime is linked even for a pure objc project, there will be less dependencies. So the overall impact is neutral” ?

|U03HL5K6L04|:
It turns out that “the whole swift runtime” is actually not that big of a dependency, basically

|U03HL5K6L04|:
It's a small runtime library plus the standard library

|U03HL5K6L04|:
We add in a few more like the overlays for dispatch and XPC; but on Apple’s platforms these are all cached and the impact to launch should be really small, if it's even measurable

|U03HZ3L66QM|:
<@U03HL5K6L04> Really appreciate the detailed answers and all the amazing works you’ve done!

--- 
> ####  There are so many different means to approach concurrency, or even asynchronous development in iOS today. Does GCD and operations still have their place in a project today, or could that all be trumped by focussing on Combine and/or async/await?


|U03HL5K6L04|:
Great question, thanks <@U03JKD29SCV>. You’re 100% correct that there are a lot of ways to add concurrency to your project. It’s been a long-standing challenge to make concurrency safe and easy to understand. I hope you’ll forgive my bias, but given the channel we’re in I’ll say this — async/await is the best way to use concurrency in your app. If you can use it, use that first. You’ve probably seen that we’re actively improving it over time as well, with more support for `Sendable` this year, additional algorithms for Async Sequence, and more.

|U03HL5K6L04|:
What I love most about async/await is the deep integration with the language that it provides. That allows the highest level of safety because the compiler and language can help point out places where there may have otherwise been a concurrency bug. For example, using actors to isolate state

|U03HL5K6L04|:
Now, do other things still have a place today? I think the answer is yes if you’re deploying to platforms that don’t support async/await (although with back deployment and the rapid uptake of new OSes, this should hopefully be a short lived problem). Another reason is just supporting legacy code that you don’t want to refactor right away. We hope that there are enough pieces available that you can start to adopt it incrementally where possible as well

|U03HL5K6L04|:
Hope that helps!

|U03JKD29SCV|:
Thanks a ton <@U03HL5K6L04>, it helps yeah. I’ve been diving quite deep into the new async algorithms, hence the question. I’ve personally always been a fan of operations. Not just for concurrency or asynchronous tasks, but also just to plainly schedule a flow to happen in a certain order, that might be a different conversation though :sweat_smile: . I’m excited to (hopefully soon) start incorporating these swift async algorithms.

--- 
> ####  Has it been considered to have a type like instancetype that could be used as a return type for a method? Use case: methods that return self (or other instances of the same class), that subclasses could override, and the type checker would know it was an instance of the subclass. Currently working around it with generics, but that requires explicitly typing the receiving variable…


|U03H36U6JHM|:
Hi Amy! I hope your WWDC is going well so far!

Generally speaking, the equivalent to `instancetype` in Swift is `Self`. For example:

```
class Animal {
  class func adopt() -&gt; Self {
    ...
  }
}

class Dog: Animal {
  override class func adopt() -&gt; Self {
    ...
  }
}
```
Are you running into a scenario where `Self` isn't the right tool or isn't building? If I'm misunderstanding the issue, please let me know! :slightly_smiling_face:

|U03J1R49EVB|:
Can this be used on an instance method?

|U03H36U6JHM|:
Yes! I couldn't think of a specific example to share, but it should work with instance methods as well. You just need to drop the `class` specifier on the function(s).

|U03J1R49EVB|:
The specifics: I'm porting Lexical (originally JavaScript text editor library). We have a clone-on-write pattern for our node classes. You call node.getWriteable() and it should return either the same node (if it's already writeable) or a mutable copy. 

I could have sworn I investigated Self as a type, but maybe I overlooked it! I will check it out. Thanks. 

|U03H36U6JHM|:
Happy to help!

|U03J1R49EVB|:
(If this does work, why don't the multiple foundation mutablecopy methods use it?)

|U03HESHGASH|:
Those are imported from ObjC, rather than written natively in Swift

|U03J1R49EVB|:
Ah gotcha!

|U03H36U6JHM|:
As David says. `mutableCopy()` is an interface into Objective-C's `NSMutableCopying` protocol rather than being a Swift feature. One constraint here is that `mutableCopy()` does _not_ necessarily return an instance of `Self`—it could, for instance, return an instance of a type elsewhere in the class hierarchy. Compare `copy()` which often _cannot_ return `Self`:

```
let a: NSMutableString = ...
let b = a.copy() // type of `b` is `NSString`, not `NSMutableString`.
```


|U03HHPMGVEF|:
`-copy` and `-mutableCopy` in Objective-C don't use `instancetype`, either, because they don't fulfill the contract: `-copy` on an `NSMutableString` returns some subclass of `NSString` that won't necessarily by an `NSMutableString`. And `-mutableCopy` will return some `NSMutableString` or subclass thereof that might not be related to the `NSString` (or subclass) it was called from.

|U03H36U6JHM|:
There's no generic way to express the relationship between the types of `a` and `a.copy()` in Objective-C, so `id` is used, and that's imported into Swift as `Any`.

--- 
> ####  with xcode 14,  ``` @available(iOS 15.0, *) private(set) lazy var myVar: &lt;Some type only available on iOS15.0+ ``` gives `Stored properties cannot be marked potentially unavailable with '@available'` error. what is the new workaround we should use to have a stored property of a type that is only available on some versions?


|U03HWDCSCHX|:
The solution is generally to hide the true underlying type of the variable. One potential solution is:
```
final class Foo {
  private lazy var _myBackingVar: Any { ... }
  @available(iOS 15.0, *)
  var myConditionallyAvailableVar: RealType { _myBackingVar as! RealType }
}
```

|U03HB5WK03G|:
Another option that is sometimes useful is to define separate types for the cases with/without the partially available stored property, and to define a protocol over them that covers common members and can hold extension methods

--- 
> ####  I've been using RxSwift and Combine, and am starting to really like Swift Async Algorithms. Would you say that's the way forward for async sequences, superceding even Combine?


|U03JJ0NS3NZ|:
I would like to extend the question about back pressure support :slightly_smiling_face:

|U03HHPMGVEF|:
I think the commentary in <https://wwdc22.slack.com/archives/C03H0JN431U/p1654628520096709?thread_ts=1654628350.752979&amp;cid=C03H0JN431U> applies here as well: async/await and async sequences integrate really well into the language, such that we can give stronger guarantees, and it makes control flow easier to reason about. The Async Algorithms package provides a wealth of algorithms to work with async sequences.

|U03HHPMGVEF|:
For back pressure specifically, check out the Channel type within the Async Algorithms package: <https://github.com/apple/swift-async-algorithms/blob/main/Sources/AsyncAlgorithms/AsyncAlgorithms.docc/Guides/Channel.md>

|U03HHPMGVEF|:
And where you do hit friction in using async sequences, or are missing functionality, please do tell us.

|U03J2C8ER9Q|:
That makes sense to me. Sorry I missed that thread above. We haven’t converted anything in production to async sequences yet (we started with RxSwift because iOS 12 back in the day, but we have quite a few Combine usages)

|U03HL5K6L04|:
Beyond the Slack channels this week, we have a place on the Swift forums to discuss Async Algorithms

|U03JJ0NS3NZ|:
Oh, I wasn't aware of the async-algorithms. Nice, I will try it.

|U03HL5K6L04|:
We have a talk about it today too! Meet Async Algorithms. And a live watch party here at 2pm PDT (50 min from now)

--- 
> ####  Just wondering if asking for Swift Regex to be back-ported to older OSes is practical? Seems like it 'should' be simpler than back-porting async/await was.


|U03HB5WK03G|:
The `Regex` type is built on core Standard Library features for Unicode processing that were not available in previous releases. Please file a feedback if you’d like the new regex functionality be made available on earlier OS releases!

|U03J2C8ER9Q|:
Is that part of the redesign to a smaller embeddable library, you mentioned removing a dependency to an external unicode library in “What’s new in Swift”

|U03JUCK9B16|:
Will do, Sadly I suspect I’ll have to support iOS 14 for a few more years due to my market.

|U03HHPMJCSX|:
Yes, some of the work needed to remove that dependency allows us to implement core regex features that are not available on older platforms.

--- 
> ####  First I want to thank everyone who worked on the new regex APIs. It is so Apple that you went back to first principles and asked how programming with regexes could be improved, and the result is so above and beyond what I expected!  My question: I usually forget about `lazy` when writing collection code. I know in principle what it means, but I haven't gotten in the habit of using it when I should. Are there simple rules of thumb to memorize, or is it more complicated than that? How does a StdLib expert think about this topic?


|U03HESHGASH|:
Whether to use `lazy` is pretty situational, but here’s what I look for:
• am I going to access the resulting collection more than once? -&gt; probably not lazy
• am I *not* going to access the entire collection? -&gt; consider lazy
• am I chaining multiple collection operations? -&gt; consider lazy
• do my collection operation closures have side effects? -&gt; *lazy can be dangerous here*
Also note that you can combine things! For example, you could do `Array(someCollection.lazy.filter {…}.map {…})` to avoid some intermediate allocations but ultimately end up with the same result as if it was eager

|U03HMD22287|:
Thank you, this is helpful!

--- 
> ####  Is this trending of placing new developments into packages separate from the standard library, like async sequences and collections, going to be more common going forward? I was wondering if the aim is to allow for more frequent updates like what’s been going in the android ecosystem with the jetpack suite of libraries


|U03HL553PNG|:
BTW this question is from the previous Swift Standard Library lounge but we still wanted to answer it. :slightly_smiling_face:

|U03HHPMJCSX|:
Hi! We love our packages and would be more than excited to make more if it made sense as a package. Some things make more sense in the standard library and others as standalone packages. We’re also using packages to get feedback on things we’d like to eventually add to the standard library.

--- 
> ####  is accessing the projected value publisher of a `@Published` variable thread safe? and does that publisher emit the current value as the first event?


|U03H36UD5QX|:
so to clarify the behavior of `@Published` - accessing the value at any time is thread safe, however the transition from the state of unsubscribed to subscribed is technically not thread safe; ideally those transitions should be bound to the main actor

|U03HL553PNG|:
This question was also left over from the previous Swift StdLib lounge. :slightly_smiling_face:

--- 
> ####  Is there a way to use new iPadOS 16 features in Swift Playgrounds? I love to use it to learn new APIs and early prototyping.


|U03HL551ATW|:
At this time, the shipping versions of Swift Playgrounds contain only the iOS 15 and macOS 12 SDKs.

|U03HV47RD8X|:
Is there a way to run Swift Playgrounds on iPadOS 16 at all?

|U03JE2L47J8|:
<@U03HV47RD8X> I think it works fine with the older SDKs. It seems to work fine on my test iPad running iPadOS 16.

|U03J4EJ0CVA|:
Confirmed for me. iPadOS 16

|U03HV47RD8X|:
Hmmmmmmm I wonder what I did wrong? I haven’t been able to see anything and everything crashes (and these are previous projects that run under iPadOS 15 fine)

--- 
> ####  Do the steps described in the video work on iOS 15? Thank you for an excellent presentation.


|U03HL551ATW|:
They do indeed!

--- 
> ####  I would really like to find a help document for the code in Swift Playgrounds. Now is this only done through the documentation search, there is no shortcut to do this, similar to Xcode's right click menu.


|U03JF5LHNTS|:
If you tap on something in the source editor, there should be a Help option which brings up Quick Help, which will let you see documentation for that symbol and which will let you view that symbol in the full documentation window.

|U03J225HUN6|:
sorry, I didn’t see any Help option.

|U03JF5LHNTS|:
Ah, you'll need to right-click on something from an Apple SDK -- try right-clicking on `View` there instead of on your symbol named `ContentView`

|U03J225HUN6|:
This is a screenshot from my mac, and it really isn’t either.Whether it is View or struct

|U03JF5LHNTS|:
Oh interesting! I would've expected a "Help" option when you right-clicked on `View`. Would you mind filing a feedback at <http://feedbackassistant.apple.com|feedbackassistant.apple.com>?

|U03J225HUN6|:
ok, I will create a feedback. I’ve been using Xcode for a long time and am very familiar with it, but the kids really need this kind of help information.

|U03J225HUN6|:
I submitted a feedback. I hope this will help to improve the problem

|U03JF5LHNTS|:
If you still have the FB number handy, mind sharing it?

|U03J225HUN6|:
The number is FB10080275

|U03JF5LHNTS|:
Thank you!

--- 
> ####  Does Swift playgrounds integrate with Xcode cloud?


|U03HL551ATW|:
At this time, Swift Playgrounds does not have any integration with Xcode Cloud.

--- 
> ####  How was the debugging working in the preview? Can you tell us a little about that? Looks super helpful.


|U03HL551ATW|:
In Swift Playgrounds, the App Preview is always running, so every change you make reflects immediately and automatically! You can also use `print` statements, which show as message bubbles in the console.

|U03JBMMB10A|:
Cool! Thank you.

--- 
> ####  Suuuuuper minor but is there any way to tighten up the line spacing on iPad so more code is visible on screen? I always feel like there’s not quite enough visible


|U03HHPNR942|:
Yes, you are able to change the font size of text in the editor on both the iPad and the Mac.

|U03JE2L47J8|:
Try the "..." button.

|U03HHPNR942|:
You can use the View Menu on macOS and the ••• option on iPad.

--- 
> ####  What’s the right way to add Info.plist settings to an app? For stuff like export restrictions? 


|U03JF5LHNTS|:
If you edit the Package.swift inside of the project, you can add the `additionalInfoPlistContentFilePath` parameter to the app initializer. This is the path within the project to a file which contains additional Info.plist key-value pairs, and they'll be added to the app you build.

|U03HV47RD8X|:
Can you say more about that? I can’t find the Package.swift in playgrounds

|U03JF5LHNTS|:
The Package.swift file isn't visible inside of Swift Playgrounds, but if you have a Mac you can right-click it and choose "Show Package Contents" and then edit the Package.swift file from there

--- 
> ####  How do we run Swift playgrounds on iPad OS 16? I can’t seem to get it to run previews or apps even with developer mode on 


|U03HL551ATW|:
Please file a Feedback Assistant report! In Swift Playgrounds in the ••• menu, there's a Send Feedback option that will gather some additional diagnostics from Previews.

|U03JE2L47J8|:
Is developer mode required for Playgrounds? I thought that was just for Xcode.

|U03JF5LHNTS|:
Developer Mode is not required for Swift Playgrounds.

|U03J4EJ0CVA|:
Is the more menu ••• available on iPad Swift Plagrounds? I don't see it. 

|U03JE2L47J8|:
Actually, it looks like if you turn Developer mode on Playgrounds crashes. I force closed it and relaunched and it started working again. I'm not sure how stable it is with dev mode on.

|U03HV47RD8X|:
:face_palm: Ok. That’s what I need to do. Thank you so much

|U03HL551ATW|:
Whoa! That's a great discovery <@U03JE2L47J8>; <@U03HV47RD8X> I wonder if that solution will work for you?

|U03J4EJ0CVA|:
Never mind. I see the Send Feedback from within the Swift Playground app itself. 

|U03HV47RD8X|:
Followup: Yes! Turning off developer mode has fixed things up for me and I can run Swift Playgrounds. Thank you, all

--- 
> ####  Any new features in Swift playgrounds beyond version 4.1… e.g. more capabilities like Core Data and CloudKit integration?


|U03JF5LHNTS|:
Swift Playgrounds 4.1 is the current version of Swift Playgrounds. Please file feedback reports at <http://feedbackassistant.apple.com|feedbackassistant.apple.com> for any features you'd like to see in Swift Playgrounds!

|U03J4EJ0CVA|:
Feedback Request submitted. Thanks. 

|U03JF5LHNTS|:
Thank you! If you still have it handy, can you share the FB number that it gave you?

|U03J4EJ0CVA|:
Standby…

|U03J4EJ0CVA|:
FB10079951

|U03J4EJ0CVA|:
Great job on presenting. I appreciate it. Same to Collette. 

|U03JF5LHNTS|:
Thank you!

--- 
> ####  Is there a way to use asset catalogs? Or a right away to include @2x and @3x assets?


|U03HERY1BMK|:
Yes! Importing your first image to the app creates an asset catalog for ya. The app imports it as a universal asset. You can add the @2x &amp; @3x assets to the project when opened in Xcode.

|U03HV47RD8X|:
Y’all are champs. Thank you for answering my million questions!

--- 
> ####  I want to use CoreData or Realm in Swift Playgrounds App project, any good suggestions? My student very like Swift Playgrounds 4.1.Because it has very fast preview and very little resource requirements on Mac.


|U03JF5LHNTS|:
You can use the CoreData framework by importing it, but Swift Playgrounds does not support managed object model files at this time. Please file a feedback at <http://feedbackassistant.apple.com|feedbackassistant.apple.com> to request this functionality.

|U03J225HUN6|:
thank you

--- 
> ####  Can Swift Playgrounds be opened inside an Xcode project? Is that the best first step in moving code developed in a playground to Xcode?


|U03HL551ATW|:
The new app project format used by Swift Playgrounds can also be opened by Xcode directly! Does that answer your question?

|U03J22ZLFUJ|:
Yes, thank you!

|U03J4EJ0CVA|:
I've done this but it remains a playground in Xcode. Is there any way that it will automatically create a Xcode project from the swift playgrounds? 

|U03HL551ATW|:
There is no feature to do that at this time. You can submit your feedback at <http://feedbackassistant.apple.com|feedbackassistant.apple.com> to request it!

|U03J4EJ0CVA|:
Thanks :pray: 

|U03J22ZLFUJ|:
One clarification  - if I open a playground inside an Xcode project, does Xcode  modify the file so that it no longer works properly as a Playground document? and assuming I don't mess up the document myself, obviously. :slightly_smiling_face:

|U03HL551ATW|:
In general, Xcode won't make any changes to your app project that make it incompatible with Swift Playgrounds! There are some things that you could do that are only supported in Xcode, but you'll get a warning if you do any of those

|U03J4EJ0CVA|:
Feedback Request submitted. Thanks. 

|U03J4EJ0CVA|:
FB10080022

--- 
> ####  Can swift playground handle unsupported file formats? I wanna use similar with Resource.copy(""). In this case I can manage files like CoreData Model, metal shaders files and anything I want!


|U03JF5LHNTS|:
Yes! If you manually edit the Package.swift file, you can set up explicit `Resource.copy` rules for resources which Swift Playgrounds doesn't know how to process.

|U03JSFUKL2U|:
Great! But warning note on top said: “Don’t change it file manually, because Package.swift is generated automatically”.  Should I scare this note or Swift Playground smart enough?

|U03JF5LHNTS|:
You should be careful when editing the file, and making changes in the App Settings sheet may cause your edits to be lost, but _generally_ Swift Playgrounds should preserve them.

|U03JSFUKL2U|:
Wow! Thats sound great!

|U03JSFUKL2U|:
Thanks a lot, Connor!

--- 
> ####  Not a question -- just a message of thanks to the entire team for beautiful, elegant work. I am definitely going to be playing in Swift Playgrounds this summer.


|U03HL551ATW|:
Thank you so much for your kind words! We've got the whole team sitting here reading along. :relaxed: Can't wait to see what you build!

--- 
> ####  What is each of your favorite type of tea? :)


|U03JF5LHNTS|:
Either a nice English Breakfast or a good oolong!

|U03HERY1BMK|:
mint :^)

|U03HL551ATW|:
That's a tough question! I'm partial to darjeeling a lot of the time

|U03HHPNR942|:
Ginger or Green tea. :smile:

|U03HHP2237V|:
Sencha/English Breakfast!

|U03HERY4SN9|:
Matcha, Sencha, Butterfly Pea

|U03HWD56KG9|:
Chamomile :slightly_smiling_face:

|U03JUCK9B16|:
Tetley Decaf

|U03JRP87THN|:
Hard to pick

|U03JRP87THN|:
 apple tea?:grin:

|U03JSFUKL2U|:
Ivan Chai

|U03J22ZLFUJ|:
Harney and Sons Earl Grey

|U03HERY4SN9|:
I think we are going to need a bigger wheel! Too many great options :smile:

|U03HB5C4DHC|:
Jasmine Pearls!

|U03HB5C4DHC|:
Oolong is a _*very*_ close second…

|U03J7BXV8KA|:
Hibiscus and pu'er

--- 
> ####  swift playground book have any upgrade?


|U03HL551ATW|:
There are no updates to the playground book format at this time

--- 
> ####  Why AsyncStream can't use multiple subscribers? Swift crashed with unstopped task error. I wanna see same result as in Combine Publisher.


|U03H36UD5QX|:
That got changed in swift 5.7: originally the problem was that the case that multiple consumers was undefined behavior, but we refined that to now allow for multiple consumers. But note: each consumer will consume the iteration independently so they will not share the values.

|U03JSFUKL2U|:
Sound great!
I will wait Swift 5.7.

P.S. Your hair color is great!

|U03H36UD5QX|:
thanks!

|U03H36UD5QX|:
Note: if you are using an AsyncStream pre 5.7 you have nothing to worry about if the stream is used just by one task

--- 
> ####  Are these new algorithms compatible with earlier versions of iOS?


|U03H36UD5QX|:
A good amount of them can be used before the most recent releases; the ones that use time need OS level support to do it

|U03H36UD5QX|:
so for example `zip` can be used as far back as Swift concurrency can

--- 
> ####  What would be Combine's place with Swift concurrency now released? How to choose between the two? Will Combine be mainly used behind the scenes for SwiftUI only?


|U03HL5K6L04|:
Thanks for the question <@U03J4BG14R2> — we actually talked about this a bit earlier today over here, which I think answers it: <https://wwdc22.slack.com/archives/C03H0JN431U/p1654628520096709?thread_ts=1654628350.752979&amp;cid=C03H0JN431U>

|U03HL5K6L04|:
To add a bit more about Combine specifically, we think that the AsyncSequence-based API in Async Algorithms, deeply integrating into the language support, is going to be the best way to use these algorithms

|U03HL5K6L04|:
I talked a bit about it here too: <https://www.swift.org/blog/swift-async-algorithms/>

|U03HL5K6L04|:
Another big bonus: Async Algorithms is open source and cross-platform

|U03HVC03PC6|:
But does Async Algorithms provide an equivalent to @Published/CurrentValueSubject, i.e. a way to do state observation?

|U03H36UD5QX|:
There isnt a property wrapper for changes in AsyncAlgorithms but there is a thing similar to subjects

|U03HL5K6L04|:
Specifically for SwiftUI, those remain the way to notify views of state that needs to change

|U03H36UD5QX|:
the thing similar to subjects is `AsyncChannel` . That is a way to send values between tasks. But as Tony points out for the SwiftUI side of things `ObservableObject` and `@Published` are good ways to still communicate events.

--- 
> ####  Does the new Swift Package give us anything akin to a Parallel For-Loop?


|U03H36UD5QX|:
This package does not have a way to do that because swift concurrency already has a pretty good tool to do it already: `withTaskGroup` - that allows parallel iteration and manages the lifetime of that iteration accordingly

|U03KD24FB40|:
That's good to know thanks!

|U03HWDD6RED|:
The docs for it are at <https://developer.apple.com/documentation/swift/withtaskgroup(of:returning:body:)> if you’re interested.

|U03HWDD6RED|:
Bonus: this is part of the base async/await implementation, so it works with iOS 15 and Big Sur :slightly_smiling_face:

|U03HWDD6RED|:
all the way back to 10.15 and iOS 13, actually, thanks to back-deployment.

|U03J22A0C4S|:
You can look at <https://github.com/JohnSundell/CollectionConcurrencyKit> if you want examples of parallel collection operations.

|U03J22A0C4S|:
Also note that `withTaskGroup` may not perform well for large collections in the Swift 5.5 runtime IIRC.

--- 
> ####  What are ways to define async work that returns an AsyncSequence, which is lazy. So it only runs later when explicitly requested or started?


|U03H36UD5QX|:
AsyncSequences are by nature very similar to lazy algorithms because they process things via their next function of their iterators

|U03H36UD5QX|:
so that first call to next is that request for things to be done; and that function is mutating so that it can know if it has started or not

--- 
> ####  Which algorithm in the package are you most proud of?


|U03H36UD5QX|:
Merge was really fun to write

|U03H36UD5QX|:
AsyncBuffer and the AsyncBufferSequence was probably the one that really was the most tricky and perhaps the most impressive in my mind.

|U03HL5K6L04|:
I like the test cases!

|U03KD24FB40|:
Cool stuff, thanks!

|U03HL5K6L04|:
<https://github.com/apple/swift-async-algorithms/blob/main/Sources/AsyncAlgorithms/AsyncMerge2Sequence.swift|Merge>

|U03HL5K6L04|:
<https://github.com/apple/swift-async-algorithms/blob/main/Sources/AsyncAlgorithms/AsyncBufferSequence.swift|AsyncBufferSequence>

|U03H36UD5QX|:
and the tests <https://github.com/apple/swift-async-algorithms/blob/main/Tests/AsyncAlgorithmsTests/TestMerge.swift#L314|look like this>

|U03HL5K6L04|:
yup, with docs <https://github.com/apple/swift-async-algorithms/blob/main/Sources/AsyncSequenceValidation/AsyncSequenceValidation.docc/Validation.md|here>

--- 
> ####  I’m struggling to grasp what is go on behind the scenes during an await for an AsyncSequence like in your examples from today’s session with Zip and Merge. Could you show an example similar to those on how to build an AsyncSequence that can take advantage of these new algorithms?


|U03H36UD5QX|:
So AsyncSequence is nothing more than just a fancy way of having multiple await calls to a single function; `next`. Those awaits are suspension points for the concurrency runtime to say; hey suspend here and go potentially do work somewhere else and resume me when you have something

|U03JXD1AQRE|:
Is there an example somewhere I could reference? In the docs or somewhere else?

|U03HWDD6RED|:
There’s some discussion up at <https://developer.apple.com/documentation/swift/asyncsequence>

|U03H36UD5QX|:
we have a ton of documentation on each algorithm; <https://github.com/apple/swift-async-algorithms/tree/main/Sources/AsyncAlgorithms/AsyncAlgorithms.docc/Guides>

|U03JXD1AQRE|:
Like an example of setting up the next for the primaryAccount.messages

|U03H36UD5QX|:
ah so the bases in that example are `AsyncStream`

|U03H36UD5QX|:
<https://developer.apple.com/documentation/swift/asyncstream/>

|U03H36UD5QX|:
but they could be anything

|U03HWDD6RED|:
Building an async sequence from scratch may or may not be trivial — starting is not very hard, as all you need to do is to build a type that conforms to `AsyncSequence`, but coordinating with asynchronous processes can be a little difficult. `AsyncStream` is a very easy way to take a traditional, callback-based or delegate-based asynchronous process and make repeated results available as a ready-made async sequence that does that work for you.

|U03HWDD6RED|:
This package exists so that the more complicated cases — the more complicated implementations of `next()` — are handled for you, and you need only combine them :slightly_smiling_face:

|U03HWDD6RED|:
Thankfully, if you’re curious, it’s open source — just look at the GitHub link above :slightly_smiling_face:

|U03JXD1AQRE|:
Oh I see so with *AsyncStream* it's basically a collection of continuations. 

|U03HWDD6RED|:
Well, just the one, but it can be invoked repeatedly.

|U03JXD1AQRE|:
That quake example reminds me of subscribing to a combine Future. 

|U03JXD1AQRE|:
So basically to make an `AsyncSequence` you create a `struct` that wraps a `struct` that conforms to the `AsyncIteratorProtocol` that provides the `mutating func next() async` function that performs some asynchronous action and returns the expected `Element`?

|U03HWDD6RED|:
Correct :slightly_smiling_face:

|U03HWDD6RED|:
I’ll be moving on from this lounge to give space to the new topic — but you got the gist of it.

|U03JXD1AQRE|:
Thank you

--- 
> ####  Hello Everybody! Why XCode 14 is deprecating the use of bitcode? Is it will be enabled automatically?


|U03HL553PNG|:
Yes, that is correct.  We announced bitcode deprecation in the &lt;https://developer.apple.com/go/?id=xcode-14-sdk-rn
|Xcode 14 Release Notes&gt;.  We can't comment on those decisions but do you have any particular concerns around this deprecation or are you seeing any issues that we can help you work through?

|U03HL553PNG|:
If your project contains bitcode, you will get a warning message asking you to remove it.  IPAs that contain bitcode will have the bitcode stripped before being submitted to the App Store.

|U03HL553PNG|:
<@U03HVE965FY> asked a similar question so tagging them so they see this.

|U03HVE965FY|:
No concerns from my side, part of my curiosity also ties into some of those issues that were present in older version of Xcode with back-deployed swift concurrency.

|U03J5DFGJNT|:
No concerns either, I knew that bitcode optimizes the size of our application and deliver only the necessary elements to a specific device, but if it will be considered by default I don't have any worries

|U03JELM0ZNV|:
:grinning: I got burned by that swift concurrency bug, utterly unintentionally because a Realm update linked it in. My app was broken for iOS13 &amp; 14 for 2 months until I found out!

&gt; IPAs that contain bitcode will have the bitcode stripped before being submitted to the App Store.
Are we _strongly encouraged_ to remove bitcode settings rather than relying on this?

|U03HL553PNG|:
<@U03J5DFGJNT> - Bitcode is actually not needed for "app thinning" which is what you're talking about.  We will continue to produce different versions of your app with only the necessary architectures and resources on the Store side.

|U03HVE965FY|:
to <@U03JELM0ZNV>'s point there, with supporting Swift concurrency I would probably want to tell our customers to not enable bitcode regardless. I imagine the bitcode stripping would occur regardless of Xcode version since it's probably happening store side?

|U03HL553PNG|:
<@U03JELM0ZNV> at some point in the future, we will remove the option for you so yes, might as well just turn it off now.  If there's some reason why you need to have it turned on, we'd love to know more.

|U03HL553PNG|:
Bitcode stripping happens on the client side with Xcode 14.  If you are submitting your app with an older version of Xcode, you can continue to submit bitcode.  But at some point, those SDKs will no longer be supported and you will need to update your Xcode.

|U03HVE965FY|:
Thank you for that clarification

--- 
> ####  Hello. I am trying to build with both ASAN and UBSAN turned on, but when I do that I can't launch my app. Turning on only one or the other works fine. Any ideas?


|U03HL55GTQU|:
Hi Michael, you should be able to use Address Sanitizer and Undefined Behavior Sanitizer at the same time.  Can you provide more information which error you are seeing?
• Is it a compile-time error, or
• Failure to launch app, or
• Runtime crash?
In any case, are you able to file a feedback ticket with a standalone reproducer of your issue?

|U03JNEZB0TU|:
I get this at startup of my app:

|U03JNEZB0TU|:
*dyld[43500]: dyld cache ‘/System/Library/dyld/dyld_shared_cache_arm64e’ not loaded: syscall to map cache into shared region failed*
*dyld[43500]: Library not loaded: /System/Library/Frameworks/Accelerate.framework/Versions/A/Accelerate*
 *Referenced from: /Users/hecht/Library/Developer/Xcode/DerivedData/JMP-dgmxmeayjbgthlckfcezeplzucmk/Build/Products/Debug/JMP.app/Contents/MacOS/JMP*
 *Reason: tried: ‘/Users/hecht/Library/Developer/Xcode/DerivedData/JMP-dgmxmeayjbgthlckfcezeplzucmk/Build/Products/Debug/Accelerate.framework/Versions/A/Accelerate’ (no such file), ‘/System/Library/Frameworks/Accelerate.framework/Versions/A/Accelerate’ (no such file), ‘/Library/Frameworks/Accelerate.framework/Versions/A/Accelerate’ (no such file)*
*(lldb)*

|U03JNEZB0TU|:
Reported as FB9981185

|U03JNEZB0TU|:
I cannot create a standalone reproducer. I was hoping to show my project that fails here.

|U03HL55GTQU|:
Oh, I see that this feedback ticket already has some history, my understanding is that:
• You have a large, existing project that shows the issue
• Newly-created projects (same Xcode, same OS version, on same machine) don't show this
• Trying to extract a smaller reproducer to share with us is not feasible
• Another apple engineer has investigated this and concluded that binary size shouldn't be the issue here
• Comparing build settings between the existing and new projects hasn't been fruitful so far

|U03JNEZB0TU|:
Correct. Any way I can have someone look at my actual project which is failing during WWDC?

|U03HL55GTQU|:
Unfortunately, we don't have a way to connect with you in a way that allows you to show us your setup via screen sharing in this Q&amp;A session.  There are additional lab sessions with 1:1 calls in which you can share your screen.

|U03HL55GTQU|:
Let me confirm how you can register for those.

|U03JNEZB0TU|:
My request for lab time was denied and I was told to come here.

|U03JNEZB0TU|:
That was for today’s Xcode open hours lab. Is there a better lab I should try, and specific wording I need to use to get my request accepted?

|U03HL55GTQU|:
We have the "1:1 lab" version of this Q&amp;A on Thursday, 09:00-13:00.

|U03JNEZB0TU|:
OK, I will try to sign up for that one.

|U03HL55GTQU|:
Yes, please do.  Let me also try to come up with some additional suggestions, especially because we haven't been able to be all to helpful w.r.t. to your problem.

|U03HL55GTQU|:
The above errors you posted are dyld (dynamic loader) errors.  So if we go back to the idea of comparing the 2 projects: large existing, with error vs. new, from template, that works.

|U03HL55GTQU|:
Any differences in build/configuration settings that pertain to the "linker", "dyld", "loader", "rpath", or "deployment target" would be worth investigating.

|U03JNEZB0TU|:
OK, I will try to gather that information in the hopes that I can obtain a lab slot.

|U03HL55GTQU|:
Ideally, we could somehow find a way to reduce the larger project until it works (and then investigate the step that triggers the change), or go from the other way: make the empty project more and more complicated to resemble your project until it fails.

|U03JNEZB0TU|:
Unfortunately, there is no good way to remove individual elements from the project, because of interdependencies.

|U03HL55GTQU|:
Can we go from the other way; complicating the empty project until it resembles the existing one?

|U03HL55GTQU|:
This is a macOS app, correct?

|U03JNEZB0TU|:
Perhaps, but not easily. My executable is 2.28 GB with both ASAN and UBSAN turned on, so it would take some doing to generate that much code. Yes, macOS.

|U03JNEZB0TU|:
```
LD_RUNPATH_SEARCH_PATHS = $(inherited) @executable_path/../Frameworks
```

|U03JNEZB0TU|:
Not seeing anything else that is non-default

|U03JNEZB0TU|:
I was thinking screen share is the path of least resistance.

|U03HL55GTQU|:
(I am not a dyld/linker person, but it seems `LD_RUNPATH_SEARCH_PATHS` might be an old setting.)

|U03HL55GTQU|:
In the above error message it says that we fail to find/load Accelerate.framework.

|U03HL55GTQU|:
Maybe setting `DYLD_FRAMEWORK_PATH` can help here.

|U03JNEZB0TU|:
Yes. But it is looking in the right place for it. Accelerate.framework lives in the dyld_shared_cache as of Monterey. And also, it works with only ASAN or only UBSAN or with neither.

|U03HL55GTQU|:
That's a good point and lends more credence to the "binary too big" theory.

|U03HL55GTQU|:
So then our working theory is that this fails, because the instrumented code is too big.  :thinking_face:

```
dyld cache '/System/Library/dyld/dyld_shared_cache_arm64e' not loaded: syscall to map cache into shared region failed
```

|U03JNEZB0TU|:
That was my initial guess, but as you saw in the FB, it was ruled out. Maybe it shouldn’t have been?

|U03HL55GTQU|:
I know we have suggested this before: any chance we can reduce the size of the project, to verify/give us another data points there.

|U03JNEZB0TU|:
To be clear, we need to reduce the size of the executable which dyld is mapping, right? Just tossing out resources, for example, wouldn’t really help.

|U03HL55GTQU|:
That's my understanding, yes.

|U03JNEZB0TU|:
I could try building it as Release configuration instead of Debug configuration. That might slim it down enough to make a difference.

|U03JNEZB0TU|:
Building now, but it will take a while. I’ll update here when I’m able to try it.

|U03HL55GTQU|:
Sounds good.  One more angle of attack would be to observe what dyld is doing and compare between "ASan only" and "ASan &amp; UBSan", e.g., by using the `DYLD_PRINT_LIBRARIES=1` environment variable.

|U03HL55GTQU|:
See the dyld man pages: `man dyld`

|U03JNEZB0TU|:
OK, I can collect that too. Each variant requires a rebuild which might take more time than is available in this session.

|U03JNEZB0TU|:
Release build with ASAN + UBSAN launches. Executable size is 813.5 MB

|U03HL55GTQU|:
I connected with the dyld team and they confirmed that binary size can indeed be a problem and most likely _*is*_ the culprit here!  Unfortunately, we don't have a way to "fix" this in the short term.

The suggested workarounds are:
• If possible, you could move some of your code into a *.dylib to split up the large executable.
• (Keep what you are doing right now:) Use ASan/UBSan separately
• Use Release for testing with sanitizer/CI

|U03HL55GTQU|:
I've associated your feedback ticket to our internal ticket that tracks the underlying issue.  You should be notified via the Feedback website if this issue is resolved.

|U03JNEZB0TU|:
OK, thanks for checking into that! It makes me feel like the world makes sense again. :slightly_smiling_face: We currently do separate Debug CI builds for ASAN and UBSAN, and run them against our test suite. It is just for those individual developers who like to run with both options turned on all the time, that I am investigating. I have signed up for the Thursday lab, so I will follow up there in case someone from Apple wants to confirm with the actual project.

|U03HL55GTQU|:
Also to keep in mind: in case your binary grows, the "ASan only" version might run into the same issue in the future.

|U03JNEZB0TU|:
Maybe I’ll be retired by then! :grinning:

|U03JNEZB0TU|:
Thanks so much for your help. Have a great week!

|U03HL55GTQU|:
Thank you Michael, your intuition was correct from the start! :male-detective:

--- 
> ####  Hello, does using optimization  level (e.g. [-Oz]) generate any negative impact on the performance of the app?


|U03HHP2ESQK|:
Different optimization levels have different tradeoffs they attempt to make, but the compiler can’t know precisely the impact that a given optimization decision will have. In general -Oz will trade runtime performance for codesize when the two are in opposition, but still generates faster code than -O0 or -O1 (in rare cases even faster than -O2 or -O3). The default (-Os) is usually best, but the optimization level can affect both size and performance. Benchmarking with your app is the best way to know for sure. You can use Instruments to analyze the performance of your app and compare multiple optimization levels.

|U03J5DFGJNT|:
Do you recommend to set (-Os) in Apple Clang and (-Osize) in Swift Compiler to make a difference in the size of application?

|U03HERYFK6H|:
On Apple platforms -Os is the default optimization level.

|U03J5DFGJNT|:
Thank you!

|U03JMF1GFV0|:
And using Asset Catalog Compiler - Options (space) what is the impact in the assets quality? Thank you.

|U03HERYFK6H|:
This question might be better suited for the Interface Builder Lab (Jun 10, 9am).

|U03JMF1GFV0|:
Thank you,

--- 
> ####  Hey all, congrats on shipping and thanks for all the great code and features! I'm working to compile clang PCMs in a remote build system and consume them in Xcode tooling. Do you have any pro tops for how to compile PCMS for the Apple SDKs in Xcode? Currently seems like the dependency graph needs to be generated for these SDKs. Any suggestions here would be appreciated


|U03HHP2ESQK|:
You can explicitly compile Clang modules by using the internal -cc1 command line and various flags. You would need to discover the module build graph to know which modules are needed when building each module and for the final translation unit. Explicitly building modules this way is not supported by Xcode or the various platform SDKs and will likely not work in all cases.

|U03J7LBB9DM|:
Hey <@U03HHP2ESQK> - cool, thanks for sharing! Awesome, yep I'm looking into the tooling side of it as well. It looks like it will be a fine dance to make this work end to end.  We're attempting to try and make all paths in the PCMs relative. Do you suggest this or use chroot?

|U03HHP2ESQK|:
Making the paths the same everywhere is going to work much better, there are lots of paths that show up in modules.

|U03J7LBB9DM|:
Ok cool, thanks for the tip! I'll give it a try

--- 
> ####  Is there a way to disable/workaround the need for public qualifiers on classes/funcs in the Source folder? Often I pull a swift class out of an existing project to experiment against it, and end up having to change this.


|U03JF5LHNTS|:
It is not possible to disable <https://docs.swift.org/swift-book/LanguageGuide/AccessControl.html|access control> for separate modules, including the module created for sources in the Sources folder of a playground.

Swift Playgrounds supports creating app projects, which you can do by creating a new "app" instead of a new "playground" using the button at the bottom-left of the document browser. In an app project, by default all sources are part of a single module, so you don't need to use `public` to make things visible. App projects are also great for experimentation thanks to the live SwiftUI previews!

--- 
> ####  Hi y’all! First of all, thanks so much for your work on Swift Playgrounds, especially the latest ability to make apps!  Are there recommendations for being able to sync user data across different devices from within an app? Like a Swift Playgrounds app creating a list of objects that would automatically go to Mac, iPad, and iPhone


|U03H32HJQEB|:
Some services (like CloudKit) are not supported in app projects at this time. But you can use `UIDocumentPickerViewController` to ask the user to pick a folder in iCloud drive. Save a bookmark of the folder's file `URL` and you will have sandbox permissions to read and write to that folder whenever you want. Then it will be synced over iCloud Drive.

|U03J2004PGT|:
Oooo, really cool idea

--- 
> ####  Hello! Question about how to build “guided instructional content”. Where is that done: Swift Playgrounds, or Xcode? Thanks!


|U03H36QJWG7|:
You can build guided content in either Swift Playgrounds or Xcode! If you do want to edit your guided content on Playgrounds, make sure to turn on Authoring Debug Mode in the app's settings. To see how our team writes content, check out <https://developer.apple.com/videos/play/wwdc2022/110349/>

|U03J21EKNSE|:
Thank you for your reply! I managed to watch half of your talk, then raced to the lounge. In the older Playgrounds you could get in trouble when trying to use Markup’s invisible writing (the moment you closed the line it would vanish!). Has that been taken care of?

|U03H36QJWG7|:
App projects no longer use the hidden code  block syntax that was available in playground books. All code in a source file is visible to learners. The only thing that is hidden is the code comments used to mark walkthrough and experiments

|U03J21EKNSE|:
Thanks! And thanks for mentioning “code comments”: could not remember the correct terms! Do you just type the code comments in Playgrounds? I could never wrap my head around using Markup (without losing the content!). 

|U03H36QJWG7|:
You can totally type them in on playgrounds! Just make sure you've entered the id before closing the comment `*/` for walkthroughs and `)` for experiments. If you do forget to add the ID before closing the comment, you can open the project in xcode and make that change there

|U03J21EKNSE|:
Apologies: “entered the id”?

|U03J21EKNSE|:
Do you mean something like /*#-code-walkthrough(filename.swift)*/ ?

|U03J21EKNSE|:
(It “ate” the asterisks before and after the “/“… I guess that’s Markup rendering? 

|U03H36QJWG7|:
Sorry if I didn't explain that well. The id that goes in the comment should match the id of a page in your walkthrough task. You can see an example of that in our session video. When writing the comment in playgrounds write `/*#-code-walkthrough(id)`  part first before writing the `*/` part just to make sure the comment doesn't disappear before you're done editing it

|U03H36QJWG7|:
It also sounds like you might be running into issues with the comments being partially hidden at the wrong times. If that is the case please file a feedback request so my team and I can look into it

|U03J21EKNSE|:
Thank you! I will try and see where things go. Have a good afternoon!

--- 
> ####  In previous versions of Playgrounds, touching the “Advanced” menu would take you to a menu to access “View Auxiliary Source Files”. From there you had access to the *.playgroundbook file, and the Chapters/UserModules. Is there anything similar in these educational content files? Thanks.


|U03HWD4ACAD|:
Unlike Playground books, app projects show all their files in the source directory in the sidebar. In the case of an app project with guided learning content, the Guide directory can be made available in the sidebar as well by enabling the Authoring Debug switch in Playgrounds settings.

|U03J21EKNSE|:
Thank you for replying, and explaining the difference. Keeping track of the “original” and “edited” versions could get confusing! Even more so when we had to do some of the work in Xcode (Playgrounds could not see/work with lists).

--- 
> ####  Side note: if possible, please say hi to Jonathan P. He was very helpful in previous years when I asked questions about Swift Playgrounds 3!


|U03H32HJQEB|:
Thank you! That means a lot. :heart: Glad to hear that this tool is working for you!

|U03J21EKNSE|:
Jonathan! Thanks for saying hi; I know you are busy. I hope to be able to “sink my teeth” in this new version of educational material. I am looking forward to creating modules for physics students.

|U03H32HJQEB|:
Ooooo, that sounds awesome!

|U03J21EKNSE|:
I always hated the old paper strip/electric discharge experiments to calculate speed and acceleration. It would be good to build something to show how classical mechanics work. But I have to learn to crawl before running! Have a good afternoon.

--- 
> ####  The Swift Playground for Mac does not totally work for SwiftUI. Even if I just create a basic template or open a tutorial like "Keep Going with Apps," it will show errors that shouldn't exist for the playground, like:  Could not build Objective-C module 'SwiftUl'  NSEvent Could not build module 'ApplicationServices' module Umbrella for module 'ApplicationServices.ATSUl' already covers this directory Inferred submodules require a module with an umbrella SwiftUl Could not build module 'UlKit' NSToolbar+UlKitAdditions Could not build module 'AppKit'  Please fix it since these playgrounds play perfectly on my iPad :sob:


|U03HL551ATW|:
Thanks for reporting! We have heard some rare reports of this happening to a small number of users. If you uninstall Swift Playgrounds and then reinstall from the Mac App Store, everything should be working great!

--- 
> ####  Do you have any tips or recommendations for improving the reliability &amp; performance of LLDB? Anything in particular to watch out for? In my project, it sometimes takes up to 30-40 seconds (on M1 Max!) between pausing on a breakpoint and LLDB becoming responsive.


|U03JPFQNX5K|:
This, imagine still having an Intel Mac! :sweat_smile:

|U03HES2QKGV|:
We've worked to improve the performance of lldb in Xcode 14 when debugging Swift. Please try Xcode 14 and file a feedback if the performance is an issue. From there we can get profiling data and figure out the root cause.

There's also the opportunity to 1:1 on this. I'd suggest &lt;https://developer.apple.com/wwdc22/labs-and-lounges/dashboard/upcoming?q=C%2B%2B
|making an appointment for a lab&gt;.  We have a C, C++, Objective-C compiler, analyzer, sanitizer, debugger, and linker lab tomorrow which would be great.

|U03JRN827HN|:
Thank you! I’ll make sure to request an appointment.

|U03HHPTMS74|:
Great, we look forward to talking with you!  When you make the appointment, be sure to specify whether your app is Swift or C/C++/ObjC.  The right person to help you out will depend on what kind of code you are debugging.

|U03JRN827HN|:
It’s a C/C++/Obj-C/Obj-C++/Swift framework. :smile:

|U03HHPTMS74|:
Note, there's also a WWDC session (it's up on the Sessions page, but will be posted tomorrow) called "Debug Swift debugging with LLDB" which covers some of the issues with Swift debugging in complex projects (which I'm guessing you might have...)  You might also find that helpful.

|U03JRN827HN|:
Already bookmarked! Thanks, I’ll make sure to watch it as well.

--- 
> ####  I have some questions regarding virtualization. Not sure where to ask.


|U03HB5H7E7Q|:
Can you please share the specific question you have? It’ll help us determine where does it suit the best.

|U03J8GR73HT|:
thanks Jan!
1. Regarding the entitlement `com.apple.vm.networking` at <https://developer.apple.com/documentation/bundleresources/entitlements/com_apple_vm_networking>
&gt; This entitlement is restricted to developers of virtualization software. To request this entitlement, contact your Apple representative.
How do I contact my _Apple representative_?

|U03HL553PNG|:
You can reach out through <http://developer.apple.com/contact/>

|U03J8GR73HT|:
I did that and received a reply email:
&gt; I understand you are wanting to request an entitlement. You should have the option to request this entitlement on the webpage.


|U03HL553PNG|:
Did they give you a link to the webpage? :slightly_smiling_face:   I'm guessing that this specific entitlement is not listed there... is that right?

|U03HL553PNG|:
Do you have a ticket number by any chance?

|U03J8GR73HT|:
the case number is 101717476463

|U03HL553PNG|:
Thanks!  I'll see what I can figure out for you and circle back to this thread when I have an answer for you.

|U03J8GR73HT|:
I just went to _Development and Technical_ then _Certificates, Identifiers, and Provisioning Profile_ then filled out the text box with the request.

|U03J8GR73HT|:
Thanks <@U03HL553PNG> I really appreciate it.

|U03HL553PNG|:
Alrighty!  I got some info for you.  It seems like we had an error in some of our documentation so we're working to correct that.  In the mean time, the actual right team to get the request is <https://developer.apple.com/contact/technical/> .  Please mention that we spoke in the Slack Digital Lounges and also include your previous case number as well.  We'll keep an eye out for your request and make sure it goes to the right folks.  Sorry for that trouble!!

|U03J8GR73HT|:
I accidentally submitted two tickets :disappointed: because I forgot to include the case number and the Slack mentioned.

|U03J8GR73HT|:
801648279 - with the case number and Slack mention
801648226 - the one I forgot to

|U03J8GR73HT|:
Thanks <@U03HL553PNG>

|U03HL553PNG|:
thanks!

|U03J8GR73HT|:
I do have another question if I may about the API.

|U03J8GR73HT|:
It seems we only have access to <https://developer.apple.com/documentation/virtualization/vzmacosrestoreimage/3835970-mostfeaturefulsupportedconfigura|t>he `fetchLatestSupported` is there a way to access previous versions of macOS and their respective restore images/`ipsw` files.

--- 
> ####  I have (or rather a dependency has) an Obj-C type which has a core definition and many categories spread amongst many files. Xcode's new build visualizer shows this type can take many seconds to compile (at least when integrated through SPM). Is there any way to speed it up? (<https://github.com/google/promises/tree/master/Sources/FBLPromises)|https://github.com/google/promises/tree/master/Sources/FBLPromises)>


|U03HWCZ76SD|:
You mention SPM, so I want to clarify: are you talking about clang build times or Swift build times? Assuming that you are talking about clang build times, enabling modules for your project may improve build times (and it looks like there is already a clang module map for this library).

|U03J22A0C4S|:
This is an Obj-C framework integrated through SPM, so it presumably uses SPM's build system to call clang during builds.

|U03J22A0C4S|:
According to the build log, that single library takes 5 seconds to build.

|U03J22A0C4S|:
On an M1 Ultra.

|U03J22A0C4S|:
Unfortunately the logs don't offer any additional detail as to why it's taking that long, but I wonder if the category usage might be part of it.

|U03HWCZ76SD|:
And this is just a dependency that you are using? Is the dependency getting re-built every time? I would expect that it would only be built once and then Swift could use the cached/already-built clang module.

|U03J22A0C4S|:
Right, and that works. However, clean builds are used when switching branches and on CI, so I'm optimizing for those cases. For something so simple this seems a disproportionate amount of time.

|U03J22A0C4S|:
Obj-C's supposed to be fast! :laughing:

|U03HWCZ76SD|:
I just downloaded the SPM package and tried building it locally, it took less than half a second to do a clean build. So, let's see what's going wrong here. Is this a regression or has it always been this slow?

|U03J22A0C4S|:
Hard to tell, previous versions of Xcode didn't have the visualizer. And Xcode 14 seems to have broken my ability to open this project in 13, so just a minute.

|U03HWCZ76SD|:
No worries. Alternatively, would you be willing to run one of these clang invocations from the command line to see how long it takes?

|U03HWCZ76SD|:
Another option: you can submit a question for tomorrow's 1:1 labs. Then we can spend half an hour debugging this over a WebEx call, which might be easier.

|U03J22A0C4S|:
I requested an open lab for today but was denied since it was a build performance issue. It was supposed to transfer me to another Xcode lab but I haven't heard anything. I'll make a request for the build performance lab tomorrow.

|U03HB5GP894|:
From my observation, SPM is using modules by default already. With the modules on, it should perform relatively good even the declarations of the categories are over lots of headers. But do note module build might sequentialize the module generation in the beginning of a clean build, so it might not  scale as well with the core count (since you are on a M1 Ultra)

|U03J22A0C4S|:
I'll gather info again once SPM is done checking out my modules.

|U03HB5GP894|:
Agree with Zoe, visiting a lab or submit a feedback report with detailed build visualization and data will help speed up the conversation.

|U03J22A0C4S|:
I'm also tracking overall build weirdness on this project where clean builds can take from 21s to 70s on my Ultra.

|U03J22A0C4S|:
For instance, here's one where FBLPromise had parts take over 20s:

|U03J22A0C4S|:
Of course, that seems batched with other commands that also take that long.

|U03J22A0C4S|:
Sorry, can't create a snippet in here.

|U03J22A0C4S|:
```
CompileC /Users/jshier/Library/Developer/Xcode/DerivedData/CoinCraft-gwuknzufleeuswameyuiqpvmnuvj/Build/Intermediates.noindex/Promises.build/Debug-iphonesimulator/FBLPromises.build/Objects-normal/arm64/FBLPromise+Async.o /Users/jshier/Library/Developer/Xcode/DerivedData/CoinCraft-gwuknzufleeuswameyuiqpvmnuvj/SourcePackages/checkouts/promises/Sources/FBLPromises/FBLPromise+Async.m normal arm64 objective-c com.apple.compilers.llvm.clang.1_0.compiler (in target 'FBLPromises' from project 'Promises')
    cd /Users/jshier/Library/Developer/Xcode/DerivedData/CoinCraft-gwuknzufleeuswameyuiqpvmnuvj/SourcePackages/checkouts/promises
    /Applications/Xcode-beta.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/clang -x objective-c -target arm64-apple-ios9.0-simulator -fmessage-length\=0 -fdiagnostics-show-note-include-stack -fmacro-backtrace-limit\=0 -fobjc-arc -fmodules -gmodules -fmodules-cache-path\=/Users/jshier/Library/Developer/Xcode/DerivedData/ModuleCache.noindex -fmodules-prune-interval\=86400 -fmodules-prune-after\=345600 -fbuild-session-file\=/Users/jshier/Library/Developer/Xcode/DerivedData/ModuleCache.noindex/Session.modulevalidation -fmodules-validate-once-per-build-session -Wnon-modular-include-in-framework-module -Werror\=non-modular-include-in-framework-module -fmodule-name\=FBLPromises -Wno-trigraphs -fpascal-strings -O0 -Wno-missing-field-initializers -Wno-missing-prototypes -Wno-return-type -Wno-implicit-atomic-properties -Wno-objc-interface-ivars -Wno-arc-repeated-use-of-weak -Wno-missing-braces -Wparentheses -Wswitch -Wno-unused-function -Wno-unused-label -Wno-unused-parameter -Wno-unused-variable -Wunused-value -Wno-empty-body -Wno-uninitialized -Wno-unknown-pragmas -Wno-shadow -Wno-four-char-constants -Wno-conversion -Wno-constant-conversion -Wno-int-conversion -Wno-bool-conversion -Wno-enum-conversion -Wno-float-conversion -Wno-non-literal-null-conversion -Wno-objc-literal-conversion -Wshorten-64-to-32 -Wpointer-sign -Wno-newline-eof -Wno-selector -Wno-strict-selector-match -Wno-undeclared-selector -Wno-deprecated-implementations -DSWIFT_PACKAGE -DDEBUG\=1 -DOBJC_OLD_DISPATCH_PROTOTYPES\=1 -isysroot /Applications/Xcode-beta.app/Contents/Developer/Platforms/iPhoneSimulator.platform/Developer/SDKs/iPhoneSimulator16.0.sdk -fstrict-aliasing -Wprotocol -Wdeprecated-declarations -g -Wno-sign-conversion -Wno-infinite-recursion -Wno-comma -Wno-block-capture-autoreleasing -Wno-strict-prototypes -Wno-semicolon-before-method-body -fobjc-abi-version\=2 -fobjc-legacy-dispatch -index-store-path /Users/jshier/Library/Developer/Xcode/DerivedData/CoinCraft-gwuknzufleeuswameyuiqpvmnuvj/Index/DataStore -I/Users/jshier/Library/Developer/Xcode/DerivedData/CoinCraft-gwuknzufleeuswameyuiqpvmnuvj/Build/Products/Debug-iphonesimulator/include -I/Users/jshier/Library/Developer/Xcode/DerivedData/CoinCraft-gwuknzufleeuswameyuiqpvmnuvj/SourcePackages/checkouts/promises/Sources/FBLPromises/include -I/Users/jshier/Library/Developer/Xcode/DerivedData/CoinCraft-gwuknzufleeuswameyuiqpvmnuvj/Build/Intermediates.noindex/Promises.build/Debug-iphonesimulator/FBLPromises.build/DerivedSources-normal/arm64 -I/Users/jshier/Library/Developer/Xcode/DerivedData/CoinCraft-gwuknzufleeuswameyuiqpvmnuvj/Build/Intermediates.noindex/Promises.build/Debug-iphonesimulator/FBLPromises.build/DerivedSources/arm64 -I/Users/jshier/Library/Developer/Xcode/DerivedData/CoinCraft-gwuknzufleeuswameyuiqpvmnuvj/Build/Intermediates.noindex/Promises.build/Debug-iphonesimulator/FBLPromises.build/DerivedSources -F/Users/jshier/Library/Developer/Xcode/DerivedData/CoinCraft-gwuknzufleeuswameyuiqpvmnuvj/Build/Products/Debug-iphonesimulator -F/Applications/Xcode-beta.app/Contents/Developer/Platforms/iPhoneSimulator.platform/Developer/Library/Frameworks -iframework /Applications/Xcode-beta.app/Contents/Developer/Platforms/iPhoneSimulator.platform/Developer/SDKs/iPhoneSimulator16.0.sdk/Developer/Library/Frameworks -DXcode -MMD -MT dependencies -MF /Users/jshier/Library/Developer/Xcode/DerivedData/CoinCraft-gwuknzufleeuswameyuiqpvmnuvj/Build/Intermediates.noindex/Promises.build/Debug-iphonesimulator/FBLPromises.build/Objects-normal/arm64/FBLPromise+Async.d --serialize-diagnostics /Users/jshier/Library/Developer/Xcode/DerivedData/CoinCraft-gwuknzufleeuswameyuiqpvmnuvj/Build/Intermediates.noindex/Promises.build/Debug-iphonesimulator/FBLPromises.build/Objects-normal/arm64/FBLPromise+Async.dia -c /Users/jshier/Library/Developer/Xcode/DerivedData/CoinCraft-gwuknzufleeuswameyuiqpvmnuvj/SourcePackages/checkouts/promises/Sources/FBLPromises/FBLPromise+Async.m -o /Users/jshier/Library/Developer/Xcode/DerivedData/CoinCraft-gwuknzufleeuswameyuiqpvmnuvj/Build/Intermediates.noindex/Promises.build/Debug-iphonesimulator/FBLPromises.build/Objects-normal/arm64/FBLPromise+Async.o -index-unit-output-path /Promises.build/Debug-iphonesimulator/FBLPromises.build/Objects-normal/arm64/FBLPromise+Async.o
```

|U03J22A0C4S|:
Other files from the module took just .3s

|U03J22A0C4S|:
If there are any additional flags I can add for additional build detail, I can investigate in preparation for the lab.

|U03HB5GP894|:
Few things I will suggest to try. Run and time the compile command from terminal (for example, with `time` ) and see if that differs from the visualizer. Do that also after deleting Xcode DerivedData to make sure modules are rebuilt.

|U03HB5GP894|:
Another useful flag to peek into time spent inside clang is to use `-ftime-trace` flag or use the timer profiler inside instruments for individual compile command

|U03J22A0C4S|:
Unfortunately clearing derived data also blows away the local SPM cache, so timing the regular build is thrown off by package resolution.

|U03HB5GP894|:
You can just clear the path pointed by `-fmodules-cache-path`

|U03J22A0C4S|:
Building with `xcodebuild` directly shows the same performance differential between subsequent `clean` builds. I don't know what I need to delete to get back to my fast times.

|U03HB5GP894|:
How about running each individual `clang` invocation from command-line? Does it match the number you see in timeline?

|U03J22A0C4S|:
Is there a way to extract those commands so they can run by themselves? They seem to depend on a lot of stuff Xcode sets up.

|U03J22A0C4S|:
Well, I can't tell what's going on. What seems to happen is that, since all of these files are interdependent, they all get batched and timed together, even though the total build time for the batch is longer than it should be. So most of the files are shown as taking the time of the whole batch to build. Sometimes (seemingly after most cleans) this batch is broken up so some of these dependent files aren't built soon enough, so the overall build time for some files is huge. It doesn't just happen to this library, it's just most visible. But I can't tell why some files are charged for their batch while others aren't, or what is causing my build time regressions on clean builds. Having to constantly reset my packages isn't helping either.

|U03HB5GP894|:
You can easily re-run clang commands after the build is finished (and after cleaning up module cache).

|U03HB5GP894|:
At this point, I will recommend visit our lab tomorrow if you still can't figure out.

|U03J22A0C4S|:
Right. Rerunning them right out of Xcode depends on various cache directories and I'm not sure which to remove to pretend it's a clean build. In any case, I'll try to get a lab time.

|U03JPFQNX5K|:
Not sure how viable would that be for your use case, but I'd also try integrating the dependency (as maybe all external ones) as a binary package – we've been on that path for some time already &amp; having an option to skip building Facebook, Firebase &amp; other SDKs on different steps (mainly CI where time is really precious) is really a great helper.

At this point, it's not easy to manage local Swift packages referencing to binary artifacts (worked fine since Xcode 12.5 &amp; 13.x) a bit hacky way, yet it's impossible at this point in Xcode 14, so the only option is fetching it from remote – that means hosting it on your GitHub/GitLab organisation, just a repo with `Package.swift` file referencing the binary stored in Releases/Packages/wherever you choose. :thumbsup:

If it's possible, I'd like to talk about the local binary package issue with some dev, hopefully I'll catch a proper session here. :smile:

--- 
> ####  What if the build timeline shows unusually long compile times for a source file when building an app target containing the subtarget, but building the subtarget alone is appropriately fast?


|U03HL5A4YA0|:
Here are a couple of suggestions for things to look for
• Could the increased time when built with the rest of the app be related to higher overall system resource contention during the build? You could try to isolate this by copying the command-line used to build the file from the Build Log in the Report Navigator, and executing just that command in Terminal with the `time` utility.
• It's possible that slightly different command-line arguments could have been used when building the file.  You could look in the Build Log in the Report Navigator for the two different builds and find the file that took a variable amount of time. Are the commands identical, or are there differences in the command-line arguments?


|U03HZ4PT2ER|:
Can resource contention really block a 200 line file for ~22s?

|U03HL5A4YA0|:
It's possible, but if that is significantly slower than building the target in isolation and the overall machine still seems responsive during this time you could start with the other suggestion to compare compiler arguments.

|U03HZ4PT2ER|:
For supertarget = ~22s, alone = ~0.1s

|U03HZ4PT2ER|:
Comparing arguments now

|U03HZ4PT2ER|:
Looks like the supertarget adds another `-I`, and `-ivfsoverlay` with a yaml file

|U03J4DJHVL3|:
related question, I tested out the xcode beta and couldn’t find build timeline… how do you access that?

|U03HZ4PT2ER|:
Right click on a line in the build log

|U03HL5A4YA0|:
&gt; Looks like the supertarget adds another `-I`, and `-ivfsoverlay` with a yaml file
Interesting. Are you about to run those two different commands from Terminal? Does that reproduce the performance issue?

|U03HZ4PT2ER|:
Looks like it's waiting for all the other source files to finish?

|U03HZ4PT2ER|:
I assume this virtual filesystem overlay is doing something

|U03HZ4PT2ER|:
By the way, this is a qlgenerator in a macOS app

|U03HZ4PT2ER|:
The two commands don't take the same amount of time in isolation using `time(1)`. Supertarget = 1.758s, alone = 0.136s

|U03HZ4PT2ER|:
Solved it

|U03HL5A4YA0|:
What turned out to be the issue?

|U03HZ4PT2ER|:
Still a bug in the build system I think, but the subtargets are old and came with PCHs from the template

|U03HZ4PT2ER|:
We use some in some targets but these were empty

|U03HL5A4YA0|:
To confirm: disabling Precompiled headers was the fix?

|U03HZ4PT2ER|:
I noticed the build log had the PCH mentioned two or three times in each subtarget, even though precompile was turned on

|U03HZ4PT2ER|:
since we're not using these, I just removed them from the build settings

|U03HZ4PT2ER|:
Still a strange behavior, empty PCHs shouldn't cause this, esp if they're being precompiled

|U03HL5A4YA0|:
Thanks for following up!

It would be good to capture this issue in a Feedback report, where we can investigate in more depth what is going on.  Here are some details that would be helpful to include in the Feedback
• The Build Logs (showing the exact command-line arguments) and the build timelines for the two builds that differ
• Your findings RE: precompiled headers
• If you can capture a `spindump` while the slow compilation is happening (E.g. Activity Monitor &gt; Select Process &gt; Spindump)
• If possible, attaching the project that reproduced the issue (I understand this is not always possible).

|U03HZ4PT2ER|:
<@U03HL5A4YA0> Should this be happening?

|U03HL5A4YA0|:
Not sure what you mean - if you disabled "Precompile Prefix Header" for the target, then no.

|U03HZ4PT2ER|:
I'm referring to the multiple precompiles in a single pass for a single target

|U03HL5A4YA0|:
I believe that happens when the same prefix header is used from files that require different compiler arguments that would affect how the PCH is built.  For example, if you create a `.m` file and `.cpp` file in the same target, they require separate builds of the precompiled header

|U03HZ4PT2ER|:
I see, so if we have ARC and MRR, those are separate

|U03HZ4PT2ER|:
I wonder what the third is

|U03HZ4PT2ER|:
thanks again

|U03HL5A4YA0|:
Correct, `-fobjc-arc` will affect how the PCH is built, so it needs to build it twice

--- 
> ####  We Are working which is quite old. It has combination of objective c , c ++, Swift and even Swift ui now. It  had good compile time on Xcode 12. But performance has really degraded with Xcode 13. I have actually stopped indexing of project. I have using m1 MacBook Pro with 16 G-Block memory. It there something u can suggest which will help us in reducing our compile time.  We also use git for version control.


|U03HWCZES3T|:
Xcode 14 includes a new build time visualizer for build logs that focuses on parallelism to help identify build performance issues. To view it go to the build log of your project, then select ‘Xcode menu -&gt; Editor -&gt; Assistant’; this should give you detailed information about where the time is spent during a build.

It is also useful to try your project with Xcode 14 to see if the build performance has improved on your project, since Xcode 14 also includes build improvements.

|U03JFEZ9BJR|:
I have downloaded beta version of Xcode 14. I will check that out for sure.thanks 

|U03JUCK9B16|:
Just tried that here in my own project - Nothing shows up (except for “No Assistant results”)

|U03J22A0C4S|:
Click it again, it sometimes doesn't load it immediately.

|U03HWCZES3T|:
also note clicking on an individual entry in the build log makes the visualizer to jump there, but if the entry is not included in the visualizer then you see “no results”. Try clicking on the top “Prepare build” entry

|U03JUCK9B16|:
Nope :disappointed: . Intel MB Pro BTW - Its not Apple Silicon only is it?

|U03JUCK9B16|:
Ahh - OK it started working once I clicked an individual entry

|U03JUCK9B16|:
It looks really useful. Any way to zoom out, even if individual items become unreadable?

|U03J22A0C4S|:
Hold option and scroll.

|U03J22A0C4S|:
It works like the Instruments graphs.

|U03J2842ALS|:
Some general strategies, having gone through the same on a large project:
• Reduce Objective-C importing -Swift.h header
• Create Objective-C shims for Swift classes, so if you have Swift code used in a lot of Objective-C you only import the Objective-C shim rather than the Swift header
• Reduce Objective-C in your bridging header

|U03JUCK9B16|:
Option scroll:+1:I was thinking of shrinking vertically too.

|U03JFEZ9BJR|:
Real eye opener to see the visualiser. Thanks <@U03HWCZES3T> 

|U03HWCZES3T|:
Glad to help! For related build performance issues with Xcode 14 I highly recommend filing feedback reports that include build visualizations, so we can follow-up with you with the specifics of your project.

|U03JFEZ9BJR|:
Any labs u can recommend?

|U03HWCZES3T|:
there’s “Xcode build performance lab” on Thursday

--- 
> ####  Are this year's linking improvements related to Swift-only codebases or should we see observe reduced linking time in mixed codebases (including objc++/c++) as well?


|U03HHP6TTKM|:
The build time performance improvements in linking this year are mostly language independent, so you should see improvement in mixed codebases. The exact improvement will depend on the structure of your code. In particular, the way you use static libraries may greatly impact how much of an improvement you see.

The runtime dynamic linker improvements this year focus on Swift, but if you build with a new enough deployment target to take advantage of page in linking you should see some improvements for all types of code.

For more info please check out this years talk: <https://developer.apple.com/wwdc22/110362>

--- 
> ####  Are there any improvements to PCH generation/use? It kind of looks like the clang pch don’t quite instantiate templates when generating them, so adding templates code to pch tends to pessimise build times. The performance profile is quite different compared to $COMPILER.  Is the build time visualiser based on -ftime-report, or is it “just” the build system timings?


|U03HL5A25FW|:
There are no changes in PCH generation/use. Do you have specific improvements in mind? Also you can always provide your suggestions through a feedback report.
As for the templates, you are correct, they are not instantiated until they are used. And if they are not used in PCH, it doesn't improve the instantiation time. Have you tried explicit template instantiation in PCH? Does it improve the build time?
Build time report doesn't take into account -ftime-report. You can still add the flag yourself and check the generated timing reports.

|U03HTE8C8G7|:
Haven’t done too deep exploration on the instantiation issue, analysing build times is a thankless job as it takes a long time to get any results, and separating signal from the noise is hard.

Which reminds me, when using `-ftime-report`, comparing two results from a bigger build job on a M1 is useless, as sometimes the timing can be from a performance core, and sometimes effeciency. Is there a way to peg a process, like `xcodebuild` and all its children processes to a certain type of a core?

|U03HL5A25FW|:
I don't know off top of my head about any mechanism for pegging a process to a certain type of a core. Let me ask around.

|U03HL5A25FW|:
And I totally agree that comparing results for the same file from different cores is frustrating. So close but not actionable.

|U03HL5A25FW|:
Looks like you can try `taskpolicy` tool. Specifically force a process into the background to achieve consistent results. Other than that there are no guaranteed ways to force usage of a specific core type.

|U03HTE8C8G7|:
Ah, sounds a bit slow, but I suppose better than useless timing diffs :slightly_smiling_face:
So I guess clamping to background QoS never chooses performance cores then

|U03HTE8C8G7|:
Thank you! :slightly_smiling_face:

|U03HTE8C8G7|:
Bah, someone pointed out to me `-fpch-instantiate-templates`, which would then instantiate the templates in pch, time to experiment!

--- 
> ####  We have a large project written in C++ and Objective-C++ (and Kotlin and Typescript for Android and Web) and the artifacts we produce are XCFrameworks. We use CMake to generate the project (it’s the tool our C++ devs are using). We would like to embed some Swift inside to provide a more modern API to our Swift customers. With CMake we were not able to do that. Do you recommend any other tool for generating the Xcode project? Or do you have tips for CMake? Do you guys at Apple have an xcodeproj file in your fit repo? :smile: 


|U03HHQ7T15Y|:
The easiest answer here is that `cmake` very recently added support for Swift if you’re willing to live on a nightly `cmake` build

|U03HHQ7T15Y|:
Check out <https://forums.swift.org/t/announcing-swift-support-in-cmake/24792>

|U03HHQ7T15Y|:
We (and Saleem in open source) are very interested in making that a seamless experience, so if that isn’t working well for you, please engage there

|U03JA49DP9R|:
Thank you. I'll check that out!

--- 
> ####  What is the state of `std::filesystem` on Apple platforms? Do you have any recommendations for integrating with system-level file coordination mechanism AND doing so from c++?


|U03J7GZG8V6|:
`&lt;filesystem&gt;` has been available since macOS 10.15 (and other Apple OSes released in alignment). It is supported out-of-the-box with the C++ Standard Library shipped with Xcode: you should only make sure to compile as C++17 (or above) to get access to the library.

If you are deploying to an earlier target than macOS 10.15 (and aligned OSes), the compiler will complain at compile-time if you try to use something that would not be available on your deployment target.

|U03J7GZG8V6|:
Could you clarify what you mean by system-level file coordination mechanisms? I am not sure I understand exactly what is being referred to.

|U03JM92AMJ5|:
I meant `NSFileCoordinator` specifically. To illustrate the problem: part of the app might use `NSFileCoordinator` to move the some file and other one (in c++, possibly much “lower level”) might use `&lt;filesystem&gt;` to attempt to read it at the same time. Is there any way for this c/c++ part to participate with system file coordination?

|U03J7GZG8V6|:
Unfortunately, the `&lt;filesystem&gt;` library does not provide any utilities to coordinate reading and writing of files between various "users" of the file, so you'd have to implement that yourself on top of the `&lt;filesystem&gt;` API.

|U03J7GZG8V6|:
The root of the issue here is that the C++ Standard does not have provisions for `&lt;filesystem&gt;` to track changes to files, it's somewhat lower level than that.

|U03JM92AMJ5|:
Makes sense! I wonder if there are any other public system headers with C API for achieving such integration without the need to use Foundation, but that’s another story :slightly_smiling_face: Thanks for the answer <@U03J7GZG8V6>! :slightly_smiling_face:

|U03HESGTE77|:
NSFileCoordinator is a strictly Foundation-level concept, so at this time there isn’t any lower level API that interacts with the subsystem.

|U03JM92AMJ5|:
Good to know! I always thought `NSFileCoordinator` in `Foundation` only provides a wrapper to some file system level functionality. But now I have no idea how `NSFileCoordinator` achieves coordination across processes :smile:

--- 
> ####  Nice work with the build timeline! :tada:  I see it shows file level details, is it possible to zoom out or change the view to show a summary timeline by target instead? Thanks :pray: 


|U03HWCZES3T|:
If by ‘zoom-out’ you mean zoom-out from the UI view, you can hold option and scroll. Also I would highly recommend filing feedback reports with suggestions about how to improve the timeline to better fit your needs!

|U03J4D51VC4|:
Meant zoom out a detail level from file to target (but I see now that is an overloaded term!) 

Will file a feedback with the suggestion, thanks!

--- 
> ####  How do I activate the Build Timeline view? I can't seem to find it anywhere on Xcode 14. I see an option for "Recent Build Timeline" but it doesn't do anything.


|U03HWCZES3T|:
Go to the build log of your project, then select an entry from the build log and do ‘right click -&gt; Show in timeline’, or select ‘Xcode menu -&gt; Editor -&gt; Assistant’.

|U03K908Q6DN|:
I also couldn't find it without instructions. It's hard to discover. Great feature though :+1:

|U03HWCZES3T|:
One of the best features of Xcode 14! :smile:

|U03HZ2VBE21|:
Great! I found it. Now why do some (simple) Objective-C .m files take 7-seconds to compile! Any idea what to look for?

|U03HWCZES3T|:
are you able to reproduce taking this long by copying the command-line compiler invocation for one of the .m files from the build log and running it in terminal?

|U03HZ2VBE21|:
What's the command-link command for this?

|U03HWCZES3T|:
I mean from the build log, if you select “All messages”, then find the entry for “Compile &lt;file&gt;.m” that you are interested in, then you expand the entry, you can see the full compiler invocation for that file.

|U03HZ2VBE21|:
Hmm, if I"m doing this correctly, I go to my source folder and type in a really long command that begins something like `/Applications/Xcode-beta.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/clang -x objective-c -target arm64-apple-ios13.1-simulator -fmessage-length\=0 -fdiagnostics-show-note-include-stack -fmacro-backtrace-limit\=0 -std\=c99 -fobjc-arc -fmodules -gmodules -fmodules-cache-path\=/Users/zulfishah/Library/Developer/Xcode/DerivedData/ModuleCache.noindex -fmodules-prune-interval\=86400 -fmodules-prune-after\=345600 -fbuild-session-file\=/Users/zulfishah/Library/Developer/Xcode/DerivedData/ModuleCache.noindex/Session.modulevalidation -fmodules-validate-once-per-build-session -fmodule-name\=CJournal -Wno-trigraphs -fpascal-strings -O0 -fno-common -Wno-missing-field-initializers -Wno-missing-prototypes -Wunreachable-code -Wno-implicit-atomic-properties -Wno-objc-interface-ivars -Wno-arc-repeated-use-of-weak -Wduplicate-method-match -Wno-missing-braces -Wparentheses -Wswitch -Wunused-function -Wno-unused-label -Wno-unused-parameter -Wunused-variable -Wno-unused-value -Wempty-body -Wuninitialized -Wno-unknown-pragmas -Wno-shadow -Wno-four-char-constants -Wno-conversion -Wconstant-conversion -Wint-conversion -Wbool-conversion -Wenum-conversion -Wno-float-conversion -Wno-non-literal-null-conversion -Wno-objc-literal-conversion -Wshorten-64-to-32 -Wpointer-sign -Wno-newline-eof -Wno-selector -Wno-strict-selector-match -Wundeclared-selector -Wno-deprecated-implementations -DDEBUG -DCONFIGURATION_Debug -DOBJC_OLD_DISPATCH_PROTOTYPES\=0 -isysroot /Applications/Xcode-beta.app/Contents/Developer/Platforms/iPhoneSimulator.platform/Developer/SDKs/iPhoneSimulator16.0.sdk -fstrict-aliasing -Wprotocol -Wdeprecated-declarations -g -Wno-sign-conversion -Winfinite-recursion -Wno-comma -Wno-block-capture-autoreleasing -Wno-strict-prototypes -Wno-semicolon-before-method-body -fobjc-abi-version\=2 -fobjc-legacy-dispat`

|U03HZ2VBE21|:
it goes on and on ... but eventually if I try it, it returns almost immediately

|U03HWCZES3T|:
there’s an “Xcode build performance lab” tomorrow, I would highly recommend to join so an engineer can dig more into this

|U03HZ2VBE21|:
Ok cool, thanks!

--- 
> ####  We have a bug that occurs sometimes and it can be narrowed down to the fact that this code fails sometimes when running on the iOS 15 simulator:  long double r = 1.0; long double x = 100.0; assert(!std::isnan(r * x));  Can somebody help with this?


|U03HHQ7T15Y|:
I think we’ll need a test case for this (a buildable project); we can’t see any reason it would happen with what you’ve written here

|U03HHQ7T15Y|:
With different values, the fact that `long double` is sometimes a different type on simulator platforms than it is on OS platforms might play a factor

|U03HHQ7T15Y|:
But it should never matter for multiplying 1.0 by 100.0

|U03HHQ7T15Y|:
Please file a feedback report

|U03K7HG7GQH|:
Done, thanks

|U03HHQ7T15Y|:
Can we get the feedback number?

--- 
> ####  Can the "any" extential types be deployed to earlier OS versions, or does it require the 5.7 runtime? Thanks!


|U03HB5WSFRC|:
`any P` for any protocol type will work on older runtimes. With associated types it's a bit more complicated. You can't generally use `any P&lt;Type&gt;` as a first-class type without the 5.7 runtime, but you can use it in stored properties of other types, allowing you to write a wrapper struct that can be back deployed

|U03HB5WSFRC|:
```
// this will be back deployable
struct AnyP&lt;T&gt; {
  var value: any P&lt;T&gt;
}
```

|U03JTPM78E7|:
Is there a concrete code example of what cannot be done without 5.7 and how this back deployable struct would work for older deployment targets?

|U03HB5WSFRC|:
You won't be able to expose the property directly as public API, but it will behave as expected in your internal implementation—as long as you're using a new enough Xcode, you can call methods on it etc. internally. So you can use `any P&lt;T&gt;` as a way of more easily implementing a type-erasing wrapper that you might've used subclassing or closures for previously.

|U03HHQ7T15Y|:
Basically, you won’t be able to use those types as a “type argument”: if you’re calling a generic function or using a generic type over `&lt;T&gt;`, you can’t let `T` be `any Collection&lt;Int&gt;` (or any type that uses it as part of its identity, like `(any Collection&lt;Int&gt;)?` or `(Int, Float, any Collection&lt;Int&gt;)`)

|U03HHQ7T15Y|:
But the types of properties aren’t part of the *identity* of a `struct` or `class`, and `any` types have fixed static layouts, so it turns out that you can just wrap them inside other types and the runtime will get by just fine

|U03JTPM78E7|:
Is the Swift 5.7 runtime iOS (16) bound or is it built in with the app?

|U03HB5WSFRC|:
The runtime is part of the OS

|U03JTPM78E7|:
I see on <https://www.swift.org/blog/abi-stability-and-apple/>

&gt;  Apps deploying back to earlier OS releases will have a copy of the Swift runtime embedded inside them. Those copies of the runtime will be ignored — essentially inert — when running on OS releases that ship with the Swift runtime.
That seems to contradict. So if I build on Swift 5.7, I’ll bundle the 5.7 runtime, then if the OS has that runtime, the dylib in my bundle will be stripped and I’ll link the system one, and if it doesn’t I’ll just link the one I’ve bundled? Or you’re saying I don’t have access to the 5.7 runtime if I don’t target min iOS X.Y?

|U03JTPM78E7|:
What I’d love to know is if my team can use `any Collection&lt;any P&gt;` in public API when targeting iOS 13. We already know we can use `some Collection` in the iOS 13 runtime

--- 
> ####  I have two, possibly obscure questions: 1) is the --disassembler-options=no-aliases not supported for objdump? and 2) Is there comprehensive documentation for the as assembler anywhere?


|U03HB5GP894|:
You can find the documentation for `objdump` here: <https://llvm.org/docs/CommandGuide/llvm-objdump.html>
`--disassembler-options` should work like following but it doesn't work for you, please file a feedback report to us:
```
objdump --macho --disassemble-all --disassembler-options=no-aliases arm64.o
```

|U03HB5GP894|:
For `as` , it is just an alias to `clang`. You can just build your assembly file with `clang` instead.

|U03J4CWFAN8|:
True. What I was referring to was more the syntax and keywords. Like, gcc as will accept ```
`ADD X2, X1, X0, SXTB```
clang needs
```ADD X2, X1, W0, SXTB
```


|U03J4CWFAN8|:
or
```
MUL V6.4H, V0.4H, V3.4H[0]```
vs
```MUL.4H V6, V0, V3[0]
```


|U03HB5GP894|:
`as` exists as an compatibility tool to binutils. You can find documentation by running `man as` on commandline

|U03J4CWFAN8|:
As to objdump, thank you, I was doing it wrong!

|U03J7GZQ8G0|:
To me, that looks like GCC is incorrect in accepting `ADD X2, X1, X0, SXTB`

|U03J4CWFAN8|:
<@U03J7GZQ8G0> That may very well be the case! I would still be very happy to find the authoretive guide to the correct syntax and keywords. So far, I have figured that out on my own :wink:

|U03J7GZQ8G0|:
You can find the official assembler spec in the ARM architecture reference manual, for aarch64 in particular.

|U03J7GZQ8G0|:
You can also see some assembler documentation from ARM's assembler docs: <https://developer.arm.com/documentation/dui0801/g/A64-General-Instructions/ADD--extended-register-> the table at the bottom I believe shows that the extended reg should be `Wn`

|U03J4CWFAN8|:
<@U03J7GZQ8G0> And, I’m not saying you’d be wrong with that, any deviation that is in gcc is likely wrong?

|U03J7GZQ8G0|:
Perhaps incorrect/wrong is too strong of a term, especially for a project like GCC which we aren't associated with. They may well choose to do this on purpose as a convenience for developers.

|U03J4CWFAN8|:
For `MUL`  though I find:
`MUL Vd.T, Vn.T, Vm.Ts[index]`
whereas clang wants:
`MUL.4H V6, V0, V3[0]`

|U03J4CWFAN8|:
Here, the docs look exactly like what gcc is accepting: `MUL V6.4H, V0.4H, V3.4H[0]`

|U03J7GZQ8G0|:
This is likely due to the assembler expecting Apple's NEON assembler syntax (due to historical...reasons).

|U03J7GZQ8G0|:
I'll check if there's a way to change that...

|U03J7GZQ8G0|:
Oh it looks like it's not related to the Apple neon syntax actually. If you remove the `4` in `v3.4h[0]` it works

|U03J7GZQ8G0|:
<https://developer.arm.com/documentation/dui0801/g/A64-SIMD-Vector-Instructions/MUL--vector--by-element->

|U03J7GZQ8G0|:
That page says that the final type specifier is either `H` or `S`, not sure why GCC accepts `4H`. Maybe it's a bug, maybe it's a feature. :man-shrugging:

|U03J7GZQ8G0|:
But it also seems that `as` should accept the official ARM assembler syntax too, no need to do anything special.

|U03J4CWFAN8|:
Thank you so much, and thank you for all the tools! I will try all of this out, and I am publishing this so more people can benefit from this knowledge!

|U03J7GZQ8G0|:
Glad to help. I'm going to sign off now. If you have any issues you can also file a feedback report too.

|U03J4CWFAN8|:
Thank you for your time!

--- 
> ####  Our system has a well defined data model with data structures and algorithms that are intuitive to understand and reason about for our business logic. I'm a bit dismayed at the prospect of being forced to carve these structures into other units (actors) to satisfy language requirements while undertaking the migration of our algorithms and processes to utilize Swift concurrency. This seems to impact readability, concise modeling, and ultimately maintainability. I'd like to maintain an open mind about this, though, since I've used async/await syntax so effectively in single-threaded languages. And so I'm wondering if you have any best practices or guidelines to share in adding this new dimension to our data modeling. Should actors be long-lived objects? Are monolithic singletons as actors discouraged or should we model after MainActor? Should we be as granular as possible?


|U03HB5WGZU6|:
Thanks for asking Paul, I'm sure this is on the mind of many devs as the new isolation features add a new "axis" to your design process.

The goal of Swift concurrency is to give you the tools to control how your data is accessed from different concurrent contexts. You’ve probably already made your code safe against concurrency, and you shouldn’t need to completely rewrite that around actors. Instead, you just need to tell Swift how you’re making things safe, and then the compiler will help enforce that you’re getting it right.

As a guide for granularity: you can carve out things which should be executed concurrently into independent actors; Things which definitely should NOT be executed concurrently, should live on the same actor. This way you'll navigate your way to the amount of actors that make sense for your use-case.

The <https://developer.apple.com/videos/play/wwdc2022/110351/|Eliminate data races using Swift Concurrency> session from today is a good resource to get into this mindset.

--- 
> ####  Hello,  You wrote that `AsyncChannel and AsyncThrowingChannel was heavily inspired from [Combine] Subject` But is it possible to have multiple subscription to a single AsyncChannel ? It seems not. Why not?


|U03H36UD5QX|:
so i kinda touched on this a bit before

|U03H36UD5QX|:
<https://wwdc22.slack.com/archives/C03H0JN431U/p1654636897761789?thread_ts=1654636838.973459&amp;cid=C03H0JN431U>

|U03H36UD5QX|:
so they will support multiple consuming, but those consumers wont share the values as they are sent

|U03HVC03PC6|:
Sorry I didn't see this previous answer :pray:
But the question is still (somewhat :slightly_smiling_face: ) relevant : if the consumers don't receive the same values the behavior is different from Subject. Can you explain the advantage of this choice?

|U03H36UD5QX|:
part of that is that the iteration is stateful; having them share the values would put a big burden on folks writing their own AsyncSequences - it would end up getting into some gnarly bits like using locks etc

|U03H36UD5QX|:
now there is evolution of the swift-async-algorithms package and that is an ongoing discussion of what the solution for that will be

|U03H36UD5QX|:
because sharing is something folks want and need (rightfully so)

--- 
> ####  Is there any way to guarantee that a Task will run not on the main thread?


|U03HHPM6QLB|:
A task only runs on the main thread under the following circumstances:
  * When it is running a main actor isolated function
   * If the task has been created from the context of a main actor isolated function, then it will inherit the main actor and run on the main thread

If your task has none of these properties, it should not run on the main thread.

|U03J21F2PQS|:
Thank you!

|U03JWQLN8RJ|:
Since a view controller is typically main actor isolated, you would need to use Task.detached(), right?

|U03HHQ7T15Y|:
Tasks are designed to move fluidly between actors, running wherever the code needs it to run

|U03HHQ7T15Y|:
Sync functions that aren’t actor-isolated will run wherever their caller was running

|U03HHQ7T15Y|:
Async functions, however, will actively switch off actors if they aren’t actor-isolated, so you can always tell where they’re running statically

|U03HHQ7T15Y|:
So if you call an async function that isn’t part of an actor and doesn’t inherit actor isolation from its context, it will not run on an actor

|U03HHQ7T15Y|:
If you’re in a view controller, yeah, that’s likely main actor isolated, so `Task { /**this code**/ }`  will also be isolated.  But if that code calls something async that’s not isolated, then you can be confident that that code won’t be running on the main actor, and you don’t specifically need to detach to do that

|U03HHPMGVEF|:
If you haven't seen it yet, we cover this in more detail in the session "Eliminate data races with Swift Concurrency": <https://developer.apple.com/videos/play/wwdc2022/110351/>

|U03J21F2PQS|:
If I use `let v = await doSomething()` it execute in main thread. But if I use `async let v = doSomething()` it execute in secondary thread. And yes, I am in a view controller. Is it a normal behavior or I do something wrong?

|U03HHPMGVEF|:
That is the intended behavior. The initializer for `async let` runs in a child task that is concurrent with the code that declared `v`

|U03H36U6JHM|:
If `doSomething()` is main-actor-isolated and the current function is also, what's the effect?

|U03HHPMGVEF|:
The child task will start and then immediately suspect until the main actor is available.

|U03H36U6JHM|:
And if the caller awaits the value `v`, it should suspend the caller and start executing `doSomething()`?

|U03HHPMGVEF|:
Yes, the caller has to `await` to get the value of `v`, at which point the child task can execute `doSomething()`.

|U03H36U6JHM|:
Cool, thanks!

|U03J21F2PQS|:
Thank you!

--- 
> ####  Are Actors intended to be long lived objects? Do they have a lifecycle?


|U03H36YQ5GX|:
You can define and use the actor type like other types in the language. An actor instance is inexpensive to create and use for coordination in a concurrent program. You can define an `init` and `deinit` for an actor's lifecycle management.

--- 
> ####  Given all of the recent changes and additions to swift concurrency through async/await, async sequence and now async algorithms, and given that combine has not changed in obvious ways, should those of us that have leveraged combine as the basis of our concurrency models within our apps be looking at migrating these towards the async await styles or are these likely to remain true siblings within Swift ecosystem?


|U03HB5WGZU6|:
In general both systems have a place of their own; The new async APIs, thanks to their help from the compiler, are able to provide better ergonomics and safety, so they've got some clear advantages here. This does not mean that there is no place for Combine though -- Tony had a great thread explaining the team's thoughts on this yesterday, see here: <https://wwdc22.slack.com/archives/C03H0JN431U/p1654628520096709?thread_ts=1654628350.752979&amp;cid=C03H0JN431U>

|U03HMCTSM2B|:
Given that SwiftUI is still seemingly leveraging publishers through @Published is there a recommendation on a best way to link with async sequences in place of using combine that we should explore/experiment with?

|U03HMCTSM2B|:
Or is it still best to leverage publisher streams when dealing with @published

|U03H36UD5QX|:
`@Published` is still a pretty good way to interact with SwiftUI - but there are other options; e.g. the `View.task` method can modify `@State`

|U03HMCTSM2B|:
Thanks, we’ll take some time to look into those options.

|U03HMCTSM2B|:
Does View.task allow for modification of @StateObject as well?

|U03H36UD5QX|:
I dont think that modifying `@StateObject` in non `@MainActor` contexts is safe

|U03H36UD5QX|:
`@State` however from my understanding is actually one of the few things that is safe from any queue (but perhaps someone more attuned to the inner workings of SwiftUI might have a more definitive answer)

--- 
> ####  Is there a decent guide how to mix Core Data and structured concurrency? I find some 3rd party guides but nothing that would be written by Apple


|U03HB5XGWCW|:
We don't currently have documentation on using Core Data in concurrent Swift code. Please file a bug in Feedback — you can post the feedback number here to help us route your bug to the documentation team more quickly.

|U03J2019QTV|:
It is nice that Core Data added NSManagedObjectContext.perform support for concurrency, so that you are able to do `await context.perform { }`. I’m using this a lot in my current app with structured concurrency and it’s working pretty well.

|U03J4D1FEP6|:
I would love to see a future where Core Data complexities around only accessing NSManagedObjects in their NSManagedObjectContext are somehow wrapped into Actors and we just don’t have to think about it much any longer…

|U03JL18FUHH|:
Robert, But you have to make sure that none of the NSManagedObjects are used outside of that `perform` block (I think they don’t conform to `Sendable`), correct? What if you need to use that elsewhere in your code? Do you simply convert those to structs for that purpose or is there some trick enabling use of managed objects?

|U03J2019QTV|:
<@U03JL18FUHH> Correct, you need to be very careful about where you use the `NSManagedObjects`. I have been using structs to pass the data around my app — which is a new approach for me. I’m actually quite liking how that's working.

--- 
> ####  About Minimal Strict Concurrency Checking, do we need to add Sendable conformance to struct and enum even though they are implicitly Sendable if we want to check Sendability?


|U03HHPMGVEF|:
The compiler will infer the `Sendable`conformance for these types unless there is an explicitly-non-`Sendable` type in their instance data. If you want to be sure that the types are `Sendable`, add the explicit conformance and the compiler will produce warnings for types used in instance data that aren't known to be `Sendable`.

|U03JJUHHF7W|:
Thank you!

|U03JJUHHF7W|:
Sorry, I have one more question. If structs and enums are implicitly Sendable,  does Xcode emit warning to this redundant explicit conformance when we change Checking mode?

|U03HHPMGVEF|:
No, it will not. If you place an explicit `Sendable` conformance on your struct or enum, that will suppress the implicit `Sendable` conformance

|U03JJUHHF7W|:
I see. Thank you very much!

--- 
> ####  In Xcode 14, some of the UIKit structs like `UIEdgeInsets` have their `Sendable` conformances marked explicitly as unavailable. That's a bit surprising, given that they are trivial data types. Why is that, and what's the purpose of conforming to `Sendable` and marking this as unavailable?


|U03H36Z4LQP|:
It looks like an accident that UIEdgeInsets was marked explicitly non-`Sendable` in the header. Please file a report in Feedback Assistant and we’ll look into it!

|U03JRN827HN|:
Thanks! So writing `@available(*, unavailable) extension X: Sendable` means that a type is _explicitly_ not thread safe?

|U03H36Z4LQP|:
Yes, exactly. The compiler treats an explicitly unavailable `Sendable` conformance as a stronger sign that the type is not thread-safe, so it may treat them slightly differently from ones that are simply missing—for instance, it may show warnings it would normally suppress, or phrase them differently, or suggest different Fix-Its.

|U03JRN827HN|:
Got it, thanks!

|U03JRN827HN|:
The feedback number is FB10112675.

--- 
> ####  I’ve recently started a new project using Core Data. Should I endeavor to avoid DispatchQueues and NSManagedObjectContext.perform and instead model all concurrency via async/await and Tasks? Is there a downside to mixing the two approaches?


|U03HHQ7T15Y|:
We understand that this interaction isn’t great right now

|U03HHQ7T15Y|:
You can bridge between these worlds yourself by writing `async` functions that call `perform` and use continuations to wait until the operation is complete

|U03J2019QTV|:
Thank you. I’ve been doing some of that and will expand the approach.

|U03HHQ7T15Y|:
Sharing what some of those functions look like in Feedback would be really valuable to us

|U03J2019QTV|:
Thanks, <@U03HHQ7T15Y> . I will do that.

|U03HHQ7T15Y|:
Thank you!

--- 
> ####  In trying to test code that used an AsyncSequence recently, I discovered that for NotificationCenter.Notifications the underlying subscription isn't established before the sequence value is returned to the caller, which results in test failures due to race conditions.  My expectation would be that a sequence value should not be returned for an AsyncSequence until it is ready to respond to the events in question—but the AsyncSequence protocol does not specific this per se. Do you think that is a reasonable expectation?


|U03H36UD5QX|:
I think this might be a bug; could you please file a feedback on that? The notifications sequence does register for the notifications and under normal scenarios does ensure they are pending before returning to the client.

|U03J4D1FEP6|:
I definitely am planning on filing a Feedback, with a sample project. I was waiting to test this on iOS 16/Xcode 14 in case the behavior changed first.

|U03H36UD5QX|:
There is obviously a small window between the call and the actual registration that things might slip into in obscure cases

|U03H36UD5QX|:
worth filing a bug either way :wink:

|U03J4D1FEP6|:
It led to quite some discussion (arguments?) here: <https://forums.swift.org/t/reliably-testing-code-that-adopts-swift-concurrency/57304/71>

|U03H36UD5QX|:
yea ive been following that - quite an interesting topic in general

|U03J4D1FEP6|:
… where my view was async/await code is generally fairly easily testable, but the OP and others appeared to be of the belief that the majority of it is not.

|U03H36UD5QX|:
i am pretty sure asynchronous code is testable, but sometimes things can get a bit tricky so i can see where it could be daunting

|U03J4D1FEP6|:
Well dang, it looks like the conclusion in that thread and my pointing at NotificationCentre.Notifications was wrong. Having extracted the test into one that removes the View and ViewModel initialization of the original example, this test passes 100,000 times without error:

|U03J4D1FEP6|:
`*import* XCTest`

`*class* AsyncSequenceTests: XCTestCase {`

  `*func* test_notificationCentreNotifications_doesNotDisplayInitializationRaceCondition_whenRun100000times() *async*` `*throws* {`
    // Actions and expectation
    `*let* expectation = *self*.expectation(description: "Notification received.")`
    `*let* fulfillExpectation = { expectation.fulfill() }`

    // Initiate the NotificationCentre.Notifications AsyncSequence
    `*let* asyncSequenceTask = Task {`
      `*let* screenshotSequence = *await* NotificationCenter.default.notifications(`
        named: UIApplication.userDidTakeScreenshotNotification
      )
      *`for`* `*await* _ *in* screenshotSequence {`
        fulfillExpectation()
      }
    }

    // Post the notification created when a screenshot occurs.
    _ = `*await* MainActor.run {`
      Task.detached(priority:.userInitiated, operation: {
        `*await* <http://NotificationCenter.default.post|NotificationCenter.default.post>(name: UIApplication.userDidTakeScreenshotNotification, object: *nil*)`
      })
    }

    // Test expectations
    *`await`* `*self*.waitForExpectations(timeout: 10)`
    asyncSequenceTask.cancel()
  }

}

|U03H36UD5QX|:
that is a bit much to really get into on slack - id have to really pull it apart; make sure to file a feedback with any repro case like that

--- 
> ####  Hi! Thank you for taking the time to answer our questions. I was wondering what is a recommended approach for serializing a series of async functions with Swift concurrency, so that they run one at a time, and run in order so one must complete before the next? My current attempt involves an `AsyncStream` within an actor, but it seems complex and I'm unsure it is correct:  ``` final class Repository {     func _request(_ requestMethod: RequestMethod) async throws - RequestResult {       // Execute an async request against a remote or local source (perhaps both)...     } }  actor AsyncUserRepositoryWithStream {     private let repository: AsyncUserRepository      struct StreamElement {         let continuation: CheckedContinuation&lt;AsyncUserRepository.RequestResult, Error         let action: () async throws - AsyncUserRepository.RequestResult     }      var task: Task&lt;Void, Error!     var stream: AsyncStream&lt;StreamElement!     var streamContinuation: AsyncStream&lt;StreamElement.Continuation!      init(urlSession: URLSession, baseUrl: URL, persistentContainer: NSPersistentContainer) {         self.repository = .init(urlSession: urlSession, baseUrl: baseUrl, persistentContainer: persistentContainer)          var streamContinuation: AsyncStream&lt;StreamElement.Continuation!          stream = AsyncStream {             streamContinuation = $0         }          self.streamContinuation = streamContinuation     }      func request(_ requestMethod: RequestMethod) async throws - AsyncUserRepository.RequestResult {         if task == nil {             task = Task {                 for await element in stream {                     do {                         let result = try await element.action()                         element.continuation.resume(returning: result)                     } catch {                         element.continuation.resume(throwing: error)                     }                 }             }         }          let result = try await withCheckedThrowingContinuation { (continuation: CheckedContinuation&lt;AsyncUserRepository.RequestResult, Error) in             streamContinuation.yield(                 .init(                     continuation: continuation,                     action: {                         try await self.repository._request(requestMethod)                     }                 )             )         }          return result     } } ```  Do you have a recommended approach?


|U03HHPM6QLB|:
Please do file a Feedback request with some information about your motivating use case for requiring FIFO behaviour. In the meantime, a  potential approach to get the desired behaviour with existing constructs, would be to make `Repository` an actor which has a queue of work items. It then creates a single Task in its initializer to drain that queue of work items - thereby giving you FIFO ordering as well as mutual exclusion.

|U03HHQ7T15Y|:
You might also consider just using a non-actor class that protects the queue with its own internal lock; that way, you’ll be able to enqueue things on it synchronously

|U03JSG77YDA|:
Thank you both! This is indeed something where I have to rethink things. The basic use case is serializing access (reads/writes) to a remote server resource that is also backed by Core Data. There can be a number of inflight requests that I want to make sure are in order, e.g. fetch a user resource from a server, store in Core Data, then later write an update to the user resource to Core Data, and then write that update to the server. These can be repeated many times, alongside fetches

I have previously approached this with a `DispatchSemaphore(value: 1)`

|U03HHQ7T15Y|:
It’s definitely something we’re thinking about.  Understanding what the code looks like that wants to use this would be helpful — in particular, whether it’s likely to want to wait, whether you rely on FIFO ordering or just want to make sure that events don’t interleave, etc.

|U03JSG77YDA|:
Thanks, I have a larger code sample with unit tests I’ll be sure to submit!

--- 
> ####  How can the Reader/Writer problem solved efficiently with Swift concurrency? Write operations should have exclusive access (serialized), readers can perform in parallel. Actors serialize everything, which is not optimal from a performance perspective.


|U03HHPM6QLB|:
In general, we have found that reader writer locks sound like a good idea in theory, but in practice they degrade to bad behaviour including lack of priority inversion avoidance support, starvation issues etc. Most use cases we have found are able to perform just as well while using an efficient lock implementation - like OSAllocatedUnfairLock or actors. If you have a motivating use case that needs RW locks, we’d like to hear about it, so please do file a Feedback request.

|U03H36U6JHM|:
For more information on `OSAllocatedUnfairLock`, which is new in macOS Ventura and iOS 16, see <https://developer.apple.com/documentation/os/osallocatedunfairlock>

|U03HHQ7T15Y|:
I would add that if something is both heavily contended and heavily biased towards readers, the best solution is usually to jump all the way to a lockless algorithm like read-copy-update

|U03HHQ7T15Y|:
Even in the short term, if you can’t make make the leap to be truly lockless, holding a lock briefly while you copy or overwrite the value should have similar overhead to a reader/writer lock, and you’ll be forced to architect clients in a way that’s consistent with a lockless approach if you eventually adopt one

|U03HHQ7T15Y|:
Or you can simply use a lock normally but design your data so that reads spend very little time in the critical section, e.g. just copying a small COW data structure or object reference

|U03HHQ7T15Y|:
The key insight here is that acquiring a reader/writer lock is usually not cheaper than acquiring a mutex, so the only efficiency win is parallelism within reads, which you can mostly duplicate by simply reducing the amount of time you spend in the critical section during those reads

|U03HB5WSFRC|:
if you want to integrate data structures with manual synchronization into an actor, such as lockless data structures or data structures with manually-tuned locking like what <@U03HHQ7T15Y> described, you could define them in a `Sendable` class and expose them in actors as `nonisolated` properties, which will allow code to access them from the actor without going through the actor's own queue

|U03HB5WSFRC|:
(or even traditional `pthread_rwlock`s, if you've established your existing code benefits from using them)

|U03J20K7526|:
Thanks a lot for taking the time to answer my question and all the suggestions! I will look into custom locking behavior within an actor.

The way we currently solve it is to setup `NSOperation` dependencies when an operation is enqueued (within specific groups of operations that relate to a certain part of the model). This guarantees that operations are executed in the desired order.
That's a very easy to understand solution to the problem and not as error-prone as locking because the details are abstracted away behind the dependency management of the queue.

It would be really great to have more control over the execution order of tasks in certain cases without having to fallback to locking via mutexes etc.

|U03HB5WSFRC|:
Thanks for the reply! It would be great to get a feedback report from you about what you're trying to accomplish, if you have a moment to write up more. We're looking into adding more facilities to control execution order and having examples of what people are doing in practice will help inform our design. In the meantime, it may help to think about managing execution order within a single task, rather than trying to coordinate execution order across different tasks—for instance, you could have an actor that holds onto a queue of work items that other tasks can send work items to, and a single task that drains the queue in order.

--- 
> ####  If `Task {` is called from a MainActor, does that mean the Task will always run on the main thread?


|U03HHQ7T15Y|:
It’ll start running on the main thread, but tasks flow between actors as needed.  In particular, if the task calls an `async` function which isn’t actor-isolated, it’ll switch off of the main actor while it’s in that function, and it’ll only switch back when it calls or returns to code that’s actor-isolated again

|U03HHQ7T15Y|:
There’s more about this in today’s session called “Eliminate data races with Swift Concurrency”: <https://developer.apple.com/videos/play/wwdc2022/110351/>

|U03JXD1AQRE|:
So an `async` function inside a `MainActor` makes an `await` call. That await will switch off of the main actor?

|U03HHQ7T15Y|:
If it’s calling an `async` function that isn’t `MainActor`-isolated, yes

--- 
> ####  I have a question about structured concurrency cancelation. In particular best practice for sharing responsibilities when canceling.  Given the code snipet: ``` ... task = Task {     let longRunningWorker = LongRunningWorker(inputURL: fileURL)     fileURL = try await longRunningWorker!.doAsyncWork()      let image = try await ImageService.i.generateImage()     let modifiedImage = try await doSomeMoreAsyncWork(image)     let andMore = try await andSomeMore(modifiedImage) } ... ```  At some point I might call `task.cancel()`. How that should be handled?  After reading all of the tutorials I see two paths and I'm not sure which one is better: 1) Add `try Task.checkCancellation()` after each line in the code snippet above 2) Those async methods should handle cancellation in-them which would result in some boilerplate code in each of them.


|U03HB5WGZU6|:
You're right to observe that task cancellation is cooperative in Swift; So you'll need to write the checking "somewhere" to interrupt the work.

The question is equally about semantics and code organization at that point -- do you control those other APIs? Do they perhaps already throw on cancellation? If they can't be made to respect it, you can add the checks yourself after/before calls to them.

In general it depends on the frameworks you're using and if they handle it or you have to yourself -- refer to their documentation to find out.

|U03HB5WGZU6|:
It also might not be worth checking before "every" operation; You should check before starting some "heavy" work, or in order to avoid holding onto precious resources for longer than necessary etc. On the other hand, it's some cheap call, letting it run might be just fine.

|U03JL18FUHH|:
Thank you for your answers. Assuming all those APIs are in-house, the approach where those methods handle their cancellation is preferred, correct?

|U03HB5WGZU6|:
most likely, but keep in mind what semantics you want from it all :slightly_smiling_face:

|U03JL18FUHH|:
Thank you

--- 
> ####  If MainActor func is called from MainActor context is there a way to execute it synchronously without scheduling for the next loop?


|U03HESHGASH|:
nice question

|U03J7HD4E64|:
If we know that we are on the main actor, the call is made synchronously.

|U03HHQ7T15Y|:
That’s generally true for calls between actors.  If they’re dynamically between the same actor, there’s no scheduling overhead

|U03JM1PJE9G|:
but, if the func is called inside of the main thread, but not in MainActor context, then it’s not called immediately?

|U03HHQ7T15Y|:
By “call”, do you mean something like `Task { call(); }`?

|U03JM1PJE9G|:
yes

|U03HHQ7T15Y|:
Okay.  The code inside `Task {}` does not run synchronously, and no, there’s currently no way to run part of it synchronously.  If you need something to happen synchronously, you need to pull it out of the `Task {}`

|U03JM1PJE9G|:
okay. Thank you for the answer

--- 
> ####  Hi, with the new Async Algorithms for AsyncSequence, how would you use `.debounce` for example, on a SwiftUI TextField text change? (i.e how would you convert a text change to  a an AsyncSequence) I know how to do it with Combine (or RxSwift) but can't grab my head around how to do this with AsyncSequence


|U03H36UD5QX|:
So the way to get the values is to emit them into something like an `AsyncStream` ; when a value needs to be emitted it can be sent via the `AsyncStream&lt;String&gt;.Continuation`'s `yield` method.

|U03JK6RTGSJ|:
Thanks, I’ll try that!
I don’t think that there are any examples of it yet, so if you have a link to a usage example of it, it would be lovely!
Thanks again!

|U03HWDD6RED|:
there’s a short example of AsyncStream use in its documentation: <https://developer.apple.com/documentation/swift/asyncstream>

|U03HWDD6RED|:
you would capture the `continuation` and call its `yield` method when the text changes.

|U03JK6RTGSJ|:
Got it, so I guess the continuation needs to be captured outside of the SwiftUI `View` otherwise it’ll reset it upon state change, right?

|U03H36UD5QX|:
yea, that is a supported (and relatively common) way of using it

|U03JK6RTGSJ|:
Something like that?:
```
TextField("Your Location", text: $location)
            .onChange(of: location) {
                viewModel.capturedTextfieldCntinuation.yield($0)
            }```
```class ViewModel {
  var capturedTextfieldCntinuation: ...

  var textFieldStream: {
     AsyncStream { continuation in
          capturedTextfieldCntinuation = continuation
      }
  }

  textFieldStream
     .debounce()
     ...
}
```

|U03JK6RTGSJ|:
Or is there maybe another way to turn a `@Published`  which is bound to the textfield to an AsyncStream?

|U03H36UD5QX|:
`.values` is a property on all `Publisher` types that is an `AsyncSequence`

|U03H36UD5QX|:
so yea, that would work too

|U03JK6RTGSJ|:
Got it, does SwiftUI has an AsyncSequence alternative to `@Published` which is used for bindings or Combine still gonna stick on the SwiftUI binding side?

|U03H36UD5QX|:
`@Published` remains a good way to interact with SwiftUI

|U03JK6RTGSJ|:
Got it, thank you so much for the follow up questions!

--- 
> ####  Do you have any recommendations for debugging race conditions when the repo uses a mix of completion handlers, combine pipelines, and async?


|U03HL5AMFCL|:
For debugging data races and other memory safety issues in an App, sanitizers are a good general purpose tool and a great starting place (TSAN, ASAN, UBSan). If you're interested in specifically analyzing Swift Tasks/Actors, the new Swift Concurrency Template in Instruments 14 (<https://developer.apple.com/videos/play/wwdc2022/110350/|Visualize and optimize Swift Concurrency>) can provide a lot of additional insight and may help debugging these types of race conditions.

--- 
> ####  How do we opt out of clang-tidy checks in the analyzer? (i.e. bugprone-move-forwarding-reference)


|U03KDARQ0QY|:
Apologies, didn't realize the previous Q&amp;A was over :)

|U03HHPM3VQT|:
Hey Lukas. Unfortunately, we don't have the right people here right to answer this questions. You can make a <https://developer.apple.com/wwdc22/labs-and-lounges/dashboard/QY7ZN5Z723/dashboard|1:1 appointment >for the  C, C++, Objective-C compiler, analyzer, sanitizer, debugger, and linker lab on Friday to get your answer.

From <https://developer.apple.com/wwdc22/topics/developer-tools/#current>:
&gt; C, C++, Objective-C compiler, analyzer, sanitizer, debugger, and linker lab
&gt; Friday @ 1:00 - 4:00 p.m.

|U03H36EUL95|:
Hi, I just found your question! There are build settings to control individual static analysis checks. They live in Project build settings in "Static Analysis - Issues - ..." sections. In particular, bugprone-move-forwarding-reference is controlled by the setting "Moves of Universal References" in section "Static Analysis - Issues - C++" (the setting name in the config files is `CLANG_TIDY_BUGPRONE_MOVE_FORWARDING_REFERENCE`). We're also curious whether you've encountered any specific problems with the check! We can further discuss them here or in the Webex lab linked above or you can always tell us about them through Feedback Assistant.

|U03KDARQ0QY|:
Thank you <@U03H36EUL95> for getting back to me on this! There are no real problems with the check, we just have a large codebase and want to light up static analysis overall but don't want to wait for every check to be green. We want to do it incrementally.

--- 
> ####  I am using NSProgress to handle task cancellation because it is used by some low level code.  Is it sendable?  What do I need to do to use NSProgress with tasks and Swift concurrency?


|U03HB5WGZU6|:
So while NSProgress is Sendable, you have to be careful with how you use it.

The "implicit" APIs it has ( "Adding a Progress Operation Implicitly <https://developer.apple.com/documentation/foundation/nsprogress> ) relies on thread local data, so you can't use them safely with async/await.

You could either pass the progress object around explicitly, and update it that way, or manually make sure to use a concrete thread or queue as you use it.

|U03KC4LFL64|:
Ok, thanks.  I think that explains some issues I was having.  Providing my own sync lock around its access.  I am trying to coordinate Asynchronous Fetch for searching a large dataset with the UI search field.  I need to add more projections.

|U03KC4LFL64|:
Protections*

|U03HB5WGZU6|:
maybe you can ensure all the places you update the progress from are on main actor or something similar?

|U03HB5WGZU6|:
actually sorry that still would not be safe; if you'd have more progress objects; and suspensions happen between; you could end up with the wrong on in a thread local

|U03HB5WGZU6|:
long story short: thread local based apis won't work in combination with async/await

|U03KC4LFL64|:
Yeah, it took a lot to get it "reliable” in the current release which immediately broke in iOS 16.

--- 
> ####  What's `functional programming` alternative to `for await value in values`? i.e what's the `foreach()` for AsyncSequence


|U03HESHGASH|:
Discussion about this in the community is ongoing, you can follow along here on the Swift Forums if you’re interested: <https://forums.swift.org/t/do-we-want-foreach/56929>

|U03JK6RTGSJ|:
Thanks, i’ll check it out

--- 
> ####  I'm seeing a concurrency warning in my `UIViewController.deinit` in which I'm calling `NotificationCenter.default.removeObserver(observer)`. This is the documented &amp; recommended technique in UIKit. The warning says: "cannot access property 'observer' with a non-sendable type '(any NSObjectProtocol')?." How I can keep using this old-school UIKit pattern while also satisfying strict concurrency checks? I've seen some recent discussions around concurrency + (de)initializers but I don't fully understand them.


|U03HL5AMFCL|:
Waiting until `deinit` to removeObserver() is a bit late. Even if a view controller object is primarily used on the main thread, the `deinit` can occur on a background thread if it's _ever_ referenced by code such as a dispatch_async or Swift concurrency.

This is what the compiler's complaining about — `observer` is `@MainActor`-isolated and `deinit` can happen anywhere. Please file a feedback report for updating documentation/recommendations to better cover this case, but in the mean time it's probably worthwhile moving the `removeObserver()` call earlier in the view controller's lifecycle (e.g. viewDidDisappear or another time before the deinit when it makes sense to remove the observation)

|U03JRN827HN|:
Thank you for the detailed explanation, Daniel! I understand it now.

|U03JRN827HN|:
I’ll make sure to file a radar about updating documentation.

|U03JRN827HN|:
Opened FB10114021!

|U03J4J6MMK8|:
I thought that NotificationCenter observers are automatically removed when the observer is deallocated?

|U03JRN827HN|:
Only targer/selector ones.

--- 
> ####  I’ve noticed that when creating new files inside SPM packages source directories, there’s no prompt for filename, and it instantly creates a `File.swift`.  Also (in SPM file creation), I cannot choose custom templates, and file headers aren’t generated with copyright and such. I’ve noticed this hasn’t been fixed in xcode 14 beta, and it’s a real pain, especially when creating new SwiftUI files because you have to rename so much stuff there. Any plans on fixing it or a workaround?


|U03J7H5DD2L|:
Hey <@U03JK6RTGSJ>! Thanks for the feedback. I would recommend sending us a bug report — and post the Feedback ID here afterwards so we can take a look. There are great guidelines on how to file an actionable bug report <https://developer.apple.com/news/?id=vvrgkboh|here>.

|U03JUCK9B16|:
I’ve seen that too on Xcode 13.

|U03JK6RTGSJ|:
Thanks for the reply, I actually did: FB10015071

|U03JK6RTGSJ|:
<https://feedbackassistant.apple.com/feedback/10015071>

--- 
> ####  Any chance Xcode 15 could continue to support macOS Monterey given that macOS Ventura does not support the 2015 MacBook Pro?


|U03HL553PNG|:
That's always a tough decision for us each year and we weigh those trade-offs very carefully.  Please file a feedback for us at <http://feedbackassistant.apple.com|feedbackassistant.apple.com> so we can track that.

--- 
> ####  I’ve always wondered what is the use case for the option to change Xcode Project format version. It doesn’t seem to change anything in my project file. What it is useful for?


|U03J7H4LJ5N|:
Certain features require newer versions of the project format, typically Xcode will automatically upgrade you to the appropriate format version. There isn’t really a reason to select a newer version than what was automatically chosen, but it can be useful to pick an older one if you need your project to be compatible with older tools.

|U03JUCK9B16|:
Would be nice to have a project format that is more friendly to branch merges

|U03JPFQNX5K|:
For that case, I'd really recommend exporting as many files &amp; modules as possible to Swift packages. :ok_hand: We currently only have `AppDelegate.swift` directly in the project. :sunglasses:

|U03JUCK9B16|:
Wish I could, but I’ve forked a LARGE open source project and often need to modify the main project for my own tweak.

|U03JLV0E8RJ|:
Thanks for your advice <@U03JPFQNX5K> but..  Isn't that a little too… “extreme”?
Is there any doc/post/tutorial on how/why such approach is beneficial to such extent?

|U03JK6RTGSJ|:
<@U03JLV0E8RJ> check pointfreeco isowords tour, they explain this “extreme” approach thoroughly and its benefits 

|U03JPFQNX5K|:
It's always open to discussion :thinking_face: but shortly from our perspective: :one: it got us rid of most of project file issues, :two: it helped us greatly with modularisation &amp; domains separation (which we'll definitely find even more useful when thinking about different platforms) and :three: in ideal case, if everything links nicely statically (all package deps resolve as static), it will be all linked into a single binary, reducing any possible dylib/dynamic frameworks count cons (which are probably already a thing of a past).

I remember some articles about Airbnb or Uber having hundreds of modules :sweat_smile: that was a bit too much, but we're around 30 and it really helped us significantly.

--- 
> ####  I would like the ability to import a swift playground app (.swiftpm) into Xcode and provide the functionality to have Xcode convert it to an Xcode project. I know we can already open a Swift Playgrounds app (.swiftpm) in Xcode, but it opens as a playground and not an Xcode project. Currently, I have had to manually create a new Xcode project and copy in the playground (.swiftpm) file content. Can Xcode import and convert (.swiftpm) into an Xcode project with Xcode 13 and Swift Playgrounds 4.1 or with Xcode 14 and Swift Playgrounds 4.1?  If not I’ve created a feedback request: FB10080022


|U03HB5N9QBY|:
There isn't currently a way to convert a swiftpm app project to an Xcode project. Thanks for filing the request.

Can you share why you'd like to convert from one project type to another? Perhaps there are other ways to help solve what you need. For example, you could put shared code in a package and both your swiftpm app project and Xcode project could import the package as a dependency.

|U03J4EJ0CVA|:
Main reason is that I don't necessarily want to manually have to create a new Xcode project than copy swiftpm file content into the new Xcode project. I want to continue working on my app in Xcode because I plan to add Core Data and CloudKit functionality to my app and I can't do that on my iPad in Swift Playgrounds 4.1. 

|U03HB5N9QBY|:
Ok, makes sense. Thanks for providing more details.

|U03J4EJ0CVA|:
Also, I'd like to use source control from Xcode so I'm not keeping app up to date in both Xcode and Swift Playgrounds. 

|U03J4EJ0CVA|:
I was waiting on WWDC22 to see if any new API functionality was introduced for Swift Playgrounds for Core Data, CloudKit and source control. Thanks for clarifying for me. :+1::skin-tone-2:

--- 
> ####  Might be more of a SPM question, but with Xcode 14 I see about 30 times every time that "Resolve Packages" runs:   Showing Recent Messages Usage of /Users/me/Library/org.swift.swiftpm/collections.json has been deprecated. Please delete it and use the new /Users/me/Library/org.swift.swiftpm/configuration/collections.json instead.  I don't mind deleting that file, but will it mess up my Xcode 13 builds?


|U03J7H4LJ5N|:
The message showing up multiple times is a known issue in Xcode 14 beta 1. This file contains the list of configured package collections, deleting it will only affect which package collections are visible during “Add Package”, so it is safe to delete it. The warning is there to make you aware that a collection added using Xcode 14 will not appear in earlier versions and vice versa. So it is also safe to ignore the warning until you’re using Xcode 14 exclusively.

|U03J22AU6DQ|:
<@U03J7H4LJ5N> So, is it correct that `collections.json` was copied to a new location thus the old one can be deleted or shall any additional changes be made to preserve existing packages collections?

|U03J7H4LJ5N|:
Yes, that is correct. This is part of a one-time reorganization of these configuration files.

--- 
> ####  With this gorgeous Build Timeline I'm really curious if there are any advices on how to display the dependency graph for a Swift Package. The idea can be described as a diagram with displays how targets depend on one another. I've been working on a pet-project tool to display it with some solutions that do not look quite right since the Package is a struct and it has `targets` property and SPM does package resolution anyway. Sooo, I've been wondering maybe there is some approach that I do not know about? :sweat_smile: What would be your general advices to achieve such a behaviour?  (And once again, Build Timeline is magnificent, many thanks for that, basically a piece of art)


|U03HES8111T|:
This is a great idea — please file feedback at <http://feedbackassistant.apple.com> describing what information you would want to see in such a graph in Xcode.

For a pet project, one approach that might be useful would be to invoke `swift package describe --type json` to get structured output for each package (including target dependencies), and then to use that to construct a graph.  There should be enough information there to represent all the dependencies.

|U03J22AU6DQ|:
Yeah, parsing JSON is exactly the approach that made from prototype to some actual use :sweat_smile:
Thank you, I’ll file a feedback then :raised_hands:

|U03H36R5MST|:
Hey <@U03J22AU6DQ>, sorry just got in. Awesome that the build timeline is helping you, that's really great to see! For the target dependency graph of a Swift package, that actually exists: `swift package show-dependencies --format json`. Does that miss anything you're looking for?

--- 
> ####  Is it possible to build an iOS app with SPM only without xcworkspace / xcproject and without falling back to generating one?


|U03J7H5DD2L|:
Yes, it’s possible! In Xcode, choose File &gt; New &gt; Project… and pick the Swift Playgrounds App template. These types of apps use the SwiftPM format rather than Xcode-style settings, so you’ll get a `Package.swift` file instead of an `xcodproj`.

You could even use the Swift Playgrounds app on iPadOS for this! Here’s a <https://support.apple.com/guide/playgrounds-ipad/itc2207c0870/ipados|guide> to get you started in that direction if you want.

|U03JSFUKL2U|:
But how to manage diff for project with .spm extension? Looks like my project on github will contains readme and spm folder)

|U03JSFUKL2U|:
Also, any chance that Swift Playground App will support Plugins?

|U03JGEZM5K5|:
:sunglasses: Awesome! I somehow considered playgrounds for … playing / experimenting? Are playgrounds a new preferred way to do big production projects?

|U03JPFQNX5K|:
I only wonder how managing Unit/UI/Snapshot tests (or even Test plans) in such configuration would work, but sounds like something I should spend a couple of moments try-failing. :eyes:

|U03JSFUKL2U|:
In my opinion - isn’t. To be an enterprise application, we should use some devtools, like linting/codegen or different package managers (CocoaPods).

Playground App format is a good first step for replacing a heavy xcodeproj files to something modern. But we don’t have enough tools for that.

|U03J7H5DD2L|:
Wow, lots of replies! I would recommend asking new questions via the :workflowbolt: workflow rather than in this thread so you can get more eyes on it!

--- 
> ####  In Xcode, is it possible to see what updates are available for SPM packages without actually updating them? Would be nice if there was a visual indication of out of date packages in the Packages section


|U03J7H4LJ5N|:
This is a great idea — please file feedback at <http://feedbackassistant.apple.com> describing how you’d think this information should be presented.

|U03JUCK9B16|:
Sometimes features seem so obvious I assume its already on some Jira board somewhere :grinning:

--- 
> ####  Should it be up to the consumer or producer of a package to decide how it should be linked and why? As of today, SPM looks like missing a way to say "I want this to be linked in ... way" without forking a third-party package and modifying `Package.swift`.


|U03HES8111T|:
Yes, this would be a very good enhancement, at least for some kinds of properties (such as whether to build a library as static or dynamic).

Would you mind please filing a bug report at <http://feedbackassistant.appe.com|feedbackassistant.appe.com> and describing in more detail what you would want to configure about how the library is linked? (beyond just dynamic vs static, for example would the consumer target need to affect the build parameters that the library is built with).

|U03JGEZM5K5|:
I will. However, I dream of solution where I do not need to know what is static and dynamic and making any of such decisions :slightly_smiling_face:

--- 
> ####  Can you help me with a crash with XcodePreview ? When I try to load an asset that is located in another package it crashes. Will it be possible to load assets from other local package ?  <https://forums.swift.org/t/xcode-previews-swiftpm-resources-xcpreviewagent-crashed/51680|https://forums.swift.org/t/xcode-previews-swiftpm-resources-xcpreviewagent-crashed/51680> <https://stackoverflow.com/q/64540082|https://stackoverflow.com/q/64540082> <https://feedbackassistant.apple.com/feedback/10102512|https://feedbackassistant.apple.com/feedback/10102512> <https://feedbackassistant.apple.com/feedback/10104291|https://feedbackassistant.apple.com/feedback/10104291>


|U03J7H4LJ5N|:
This is a known issue when using resources of transitive dependencies while previewing a standalone package. Using an app as the preview host as discussed in the forum thread is the recommended workaround at the moment.

|U03J4DF6D6G|:
Thanks for taking the time to respond. I’m more confident now that this bug will be fixed. I’m not really sure what this(*Using an app as the preview host*) means. Because when I select the app and run the Preview it doesn’t crash but nothing seems to be happening. And I have this in the run console. `*2022-06-09 00:41:53.104293+0200 TestCrashXcode[99836:1805516] [UIFocus] _TtGC7SwiftUI14_UIHostingViewGVS_15ModifiedContentVS_7AnyViewVS_12RootModifier__ implements focusItemsInRect: - caching for linear focus movement is limited as long as this view is on screen.*`

|U03J4DF6D6G|:
<@U03J7H4LJ5N> sorry to ping but did you see my response ?

|U03J7H4LJ5N|:
Yes, sorry, I was trying to find an answer for you, but it might be better to ask this in the SwiftUI labs <https://developer.apple.com/wwdc22/labs-and-lounges/dashboard/P946NZ7RP4/dashboard>

|U03J7H4LJ5N|:
I’m only familiar with the Swift package part of this issue

|U03J7H5DD2L|:
Right, the SwiftUI folks staffing that lab from 3–5PM (California time) tomorrow might be able to help — or at least, they could walk you through submitting another bug report since it sounds like we couldn’t find a good workaround for you!

|U03J4DF6D6G|:
Thanks I did ask for a lab session.

--- 
> ####  When trying to compile our app with Xcode 14b1, we have a build failure due to ibtoold/AssetCatalogSimulatorAgent crashing when compiling the asset catalog. This did not happen with Xcode 13.x.  Except submitting a feedback, is there any known workaround?  :point_right: <https://feedbackassistant.apple.com/feedback/10104592|https://feedbackassistant.apple.com/feedback/10104592>


|U03HB5P8SB0|:
Thank you for reporting this! We are looking into issues with the asset catalog compiler and will provide updates through your feedback.

--- 
> ####  What is your advice for handling teams where some developers are on Intel machines and some on Apple Silicon?  We run Xcode through Rosetta consistently, but wondering if you had best practices.


|U03HHPY6YQ2|:
Hi Brandon, I'd be interested in understanding your team's use case for running Xcode under Rosetta

|U03HL5G4QFN|:
~Guessing this is they produce Intel Debug builds? (which would run on all systems)~

|U03J20E7UBV|:
<@U03HHPY6YQ2> We have some internal, third-party dependencies that throw an `Unsupported Swift architecture` and `Could not build Objective-C module...` when running without Rosetta.  Our builds typically go through a CI pipeline, so there's little need for actually using Rosetta if there was an easy way of resolving the aforementioned errors, assuming we could run Xcode on both Intel and Apple Silicon machines.

|U03HHPY6YQ2|:
Are these internal dependencies fat binaries that have slices for both Intel and Apple silicon?

|U03J20E7UBV|:
<@U03HHPY6YQ2> Regretfully, your question goes a bit over my head, so I can't say with confidence.  If you have guidance or documentation on where to look into this, I can start investigating and speak with the teams responsible for said dependency.

|U03HHPY6YQ2|:
Let's start simple: what platforms do your app build for?

|U03J20E7UBV|:
Our app is iPhone (iOS 13+) only.  Our internal developers work on macOS 12, all of us on Xcode 13.4.1, as noted some on Intel, some on Apple Silicon.

|U03HHPY6YQ2|:
Do you have the "excluded architectures" build setting set in your project?

|U03J20E7UBV|:
Excellent question; I was just exploring that area before this Q&amp;A started.  Yes, to your question.  We have `arm64` listed for each scheme next to "Any iOS Simulator SDK".

|U03HHPY6YQ2|:
Well, I'm guessing that's your problem. You have set Xcode to build only for Intel

|U03HHPY6YQ2|:
If you run Xcode without Rosetta, then the current architecture will be `arm64`, but you have set Xcode not to build for that architecture

|U03HHPY6YQ2|:
Hence why you have found you need to run under Rosetta

|U03HHPY6YQ2|:
We strongly suggest _not_ excluding architectures and instead building your app for every supported CPU type

|U03J20E7UBV|:
To that end (and totally understand we're deep-diving here, so I know you might only be able to help so far - thank you for this so far), if I remove `arm64` from the Excluded Architectures, I end up with a familiar _...building for iOS Simulator, but linking object file built for iOS...for architecture arm64_ (this with Rosetta disabled and running Xcode 14 natively).

|U03HHPY6YQ2|:
The iOS simulator and iOS on-device are two distinct platforms, even though they may share a CPU architecture (arm64). You need to make sure that your dependency is configured to build with both native (device) and Simulator slices

|U03J20E7UBV|:
Got it, that makes sense.  Last question, if you don't mind, but do you have any guidance on where I can turn to learn more about what slices my dependency is designed to build for?  This is a little outside of my wheel house, but I'm sure with some guidance, I can figure it out and learn what steps are needed.

|U03HHPY6YQ2|:
Do you have the source for this dependency, or are you building (linking) against a pre-compiled library?

|U03J20E7UBV|:
I have the source.  It's an internal dependency that I can view/modify as needed :slightly_smiling_face:

|U03HHPY6YQ2|:
The first thing I would do is read this documentation page: <https://developer.apple.com/documentation/apple-silicon/building-a-universal-macos-binary>

|U03HHPY6YQ2|:
I strongly suggest not running Xcode under Rosetta. This tends to mask issues in your project configuration. Instead, have Xcode build for all architectures, at least for release builds. Since no iOS device runs on Intel, the fact that you have to compile for Intel suggests to me that your app's code is not properly set up to support Simulator. You might be hard-coding a particular architecture or SDK somewhere in your project

|U03HHPY6YQ2|:
Unfortunately, I don't think I can help you much further over Slack. You should check if there are any more lab sessions available, or try posting on the developer forums

|U03J20E7UBV|:
Got it; thank you <@U03HHPY6YQ2>.  Again, this goes a bit over my head, but I'm grateful for your help.  Our core app and dependencies only run on iOS, so I'm not sure why we'd need to support Intel.  I will connect with the developers of the dependency in question, but will also schedule a lab if times exist as I know our team would be thrilled to get our own building not just on Xcode 14, but natively on Apple Silicon, as well.

|U03J20E7UBV|:
Thanks so much for your in-depth troubleshooting!

|U03HHPY6YQ2|:
Yeah, I suspect that your project's Simulator configuration had a hard dependency on compiling on Intel, and when Apple silicon came around, someone just ran Xcode through Rosetta (which you really, _really_, *really* should not do) instead of fixing the underlying issue

|U03HHPY6YQ2|:
And no problem at all! We are happy to help; it's what we are here for!

|U03J20E7UBV|:
Thanks so much for all of the help!  Greatly, greatly appreciated!

|U03J4D1FEP6|:
I experienced the exact issue above with my app’s OpenSSL dependency as soon as I moved to Apple Silicon. The final step in the solution was realising that while you can _compile_ a single static library that contains both the arm64 code for running on iOS and also the arm64 code for running on the Simulator on an Apple Silicon Mac, as far as I’m aware Xcode will not/cannot select the simulator version of the fat binary when building the app to run on the Simulator.

The solution instead was to package the dependency in _Frameworks,_ containing separate universal libraries for Simulator vs Device, and so will vend the correct required code in each environment. In my case, someone else had already written a tool to package OpenSSL that way, which made my life easier, but it was uncovering the need to do this that was the breakthrough. Sounds like you’re describing the exact same scenario to me.

|U03HHPY6YQ2|:
I'm going to second Duncan's suggestion here. If you have not looked into XCFrameworks, now is a great time to do so!

|U03J4D1FEP6|:
Example: <https://github.com/jasonacox/Build-OpenSSL-cURL>

|U03J20E7UBV|:
Thanks for the detail, <@U03J4D1FEP6> <@U03HHPY6YQ2>.  So much of this is handled outside of my own purview, similar to your OpenSSL experience, <@U03J4D1FEP6>, that I have a bit of a learning curve ahead of me to understand how to implement this.  But it is critically important to me, and my team, to get our app running natively on Apple Silicon (and Xcode 14, for that matter), so these challenges are worth learning and overcoming.

|U03J4D1FEP6|:
There is a lot of work in that repo and setting up the XCFrameworks you need is likely to be a lot simpler than producing something like that! The key point is the XCFramework packaging that lets you vend the correct version of your dependency for the two environments. You got this. :+1::skin-tone-3:

|U03J20E7UBV|:
Thanks, <@U03J4D1FEP6>!  I appreciate the vote of confidence and will be checking that out now!  I am going to get to the bottom of it!

--- 
> ####  I’ve been re-creating SwiftLint SPM Plugin from the recent session and while I did manage to do that the experience was not ideal. I had to to a clean build of my target to re-build the plugin and moreover errors and warnings were not displayed. I’m assuming that has to something to do with my setup. Setup was: Plugin was created as a plugin target only, re-building the target it connected to for changes. So, targets are in the same Package and there are no libraries for the plugin. I do recall that there are going to be sessions on this topic tomorrow, but maybe any advice while we’re in Q&amp;A space? :sweat_smile:


|U03HES8111T|:
We would very much like to know more about the errors you are encountering.  There should be no problem having just a plugin target, and a corresponding package product shouldn't be needed unless the plugin is used outside of the package.

If a clean build was needed in order to get the plugin to work, that sounds like a bug.  It doesn't sound as if there is anything wrong with the setup.

If you have the time, would you want to make an appointment for the Xcode Open Hours Lab from 1pm-4pm on Friday?  Alternatively, a bug report at <https://feedbackassistant.apple.com> with as much detail as you can provide would be greatly appreciated.

|U03J22AU6DQ|:
Okay, before I file a bug report let me just describe this issue:
1. Plugin contains an issue which should result in a compiler error
2. I try to build the source target this plugin is connected to(not plugin target)
3. I do not see the error in the Issue navigator or in status bar, but if I go to Report navigator, select failed build and check the errors there
Also, when:
1. Change was made to a plugin, e.g. `print(“Hello,` `World!”)` was changed to `print(“Hello, WWDC!”)`
2. I build the source target plugin is connected to
3. I still see `print(“Hello, World!”)` like the plugin is not being re-built
4. After clean build everything is expected

|U03HES8111T|:
Thank you for the additional detail, I had misunderstood that it was an issue from the plugin itself that was not being shown.  I just want to confirm that you are seeing this with Xcode 14 Beta 1?  If so, is this a package that you would be willing to attach to a Feedback Assistant report so we can look at the details?

|U03J22AU6DQ|:
Yep, it’s in beta. The package cannot be attached to a Feedback Assistant as is, but I can create a sample and try reproducing it there. If my understanding is correct, it should not be a problem since our main package is nothing out of ordinary.

|U03HES8111T|:
That would be great — thank you very much!  You might have seen this already but in Xcode 14 Beta 1 there are additional entries in the build log for compiling and running build tool plugins, and that is where any errors in compiling the plugins should appear.

|U03HES8111T|:
If you do have time to sign up for a slot in the Xcode Open Hours Lab from 1pm-4pm on Friday, that might be another good way to go into more detail about the problems you're encountering.

|U03J22AU6DQ|:
Got it, thank you <@U03HES8111T>!

--- 
> ####  Hi! In preparation for WWDC, we updated all our app release branches to build with Xcode 13.4! This went fine for most of our apps' branches, but one of those branches (with `IPHONEOS_DEPLOYMENT_TARGET = 14.0`) started producing archive builds which fail to upload to App Store Connect. The error we're getting is:  ``` ITMS-90427: Invalid Swift Support  …  We identified one or more issues with a recent delivery for your app, "OmniFocus 3 Enterprise" 3.13 (151.23.24). Please correct the following issues, then upload again.  ITMS-90427: Invalid Swift Support - The expected dylibs are missing from the app’s Framework location, such as /Payload/OmniFocus.app/Frameworks. ```  Going back through builds of Xcode, it looks like this error started with Xcode 13.3, and seems to be associated with its bundling of:  ``` SwiftSupport/iphoneos/libswift_Concurrency.dylib ```  We don't actually reference `libswift_Concurrency.dylib` in this release branch at all, so as far as we know we shouldn't need that `dylib`. But is there something we can do to make App Store Connect accept our upload (other than reverting to Xcode 13.2 or earlier)?


|U03HHPCVCLT|:
The `libswift_Concurrency.dylib` is a backwards compatibility library that is necessary when using Swift's async functionality when deploying to a platform that does not support concurrency natively. Are you removing this library from your app bundle?

We will likely need a feedback created that contains the build log for the archive process of your app to see if there is an issue with this library being bundled correctly.

|U03KGEZNC3S|:
We're not removing the library from our app bundle, but it doesn't appear to ever get added (which seems reasonable, since none of the binaries included with our app link against it).

|U03KGEZNC3S|:
(We're not using any `async` functionality in this old release branch of our app.)

|U03KGEZNC3S|:
Writing up a feedback report now. Thanks!

|U03KGEZNC3S|:
Filed as `FB10115173`. Thanks again!

|U03HHPCVCLT|:
Thank you! I will make sure it is routed appropriately.

|U03JQC37CR1|:
Yes, I am facing the exact same problem. I got an appointment for a lab but unfortunately the engineers couldn't figure out what's going on. They recommended to try downgrading to Xcode 13.2 and I did but unfortunately it didn't help. Any pointers?

We did check the binary and the library is indeed there. Yet TestFlight is rejecting the build saying it's missing the concurrency library

|U03JQC37CR1|:
With 13.2 I'm not able sign but it is only related to the concurrency library. 

‘error: Couldn't codesign /Users/xxxxx/Library/Developer/Xcode/DerivedData/xxxxxxx-bdejxonpdwrjtwgwgskakzwsmprg/Build/Intermediates.noindex/ArchiveIntermediates/xxxxxx/InstallationBuildProductsLocation/Applications/xxxxxx.app/Frameworks/Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/lib/swift-5.5/iphoneos/libswift_Concurrency.dylib: codesign failed with exit code 1
Command PhaseScriptExecution failed with a nonzero exit code’

--- 
> ####  I can understand newest versions of Xcode being picky about the minimum macOS version required to run, but why recent macOS version can’t run older versions of Xcode?


|U03H36RCYJK|:
We want to ensure the best experience for our developers. We explicitly qualify each version of Xcode with the two most recent versions macOS. We don't qualify older Xcode's with newer versions of macOS, and want to ensure you don't run into problems using an unsupported configuration.

--- 
> ####  We’re having an issue with SPM packages on Xcode 13, where our local packages fail to load when switching branches, giving us Missing Package Product errors. The only workaround we’ve found is to reset package cache and restart Xcode. Is this a known issue?


|U03J7H4LJ5N|:
We would like to know more about the errors you are encountering since this sounds like a bug.  If you have a reproducible case and have the time, would you want to make an appointment for the Xcode Open Hours Lab from 1pm-4pm on Friday?  Alternatively, a bug report at <https://feedbackassistant.apple.com> would be greatly appreciated.

|U03JUCK9B16|:
bug reports for this kind of thing are tricky as often it is project specific and uploading the entire source tree is not an option, due to company restrictions.

I too have seen similar issues, so if Jean can’t do the Lab, I’d be able to.

|U03JE7VT43Z|:
I wouldn't be able to do the lab, but I will gladly file a feedback. Pls feel free to do the lab in my stead <@U03JUCK9B16> 

|U03JUCK9B16|:
OK, let me check my app still has the issue and if so, I will

|U03J22AU6DQ|:
There seems to be an issue with the way Xcode updates SPM stuff. In my experience deadblock are caused when dependency was added. Especially if it is not cached yet. Meanwhile, CLI works fine. No sure about Xcode 14.

I’d recommend having something like this at hand and run it after you switch branch where something in the dependancy graph changed. With Xcode closed, of course.
`xcodebuild -resolvePackageDependencies -scheme YourScheme -project YourProject.xcodeproj -configuration YourConfiguration`

|U03JE7VT43Z|:
This link is very familiar to my issue: <https://forums.swift.org/t/missing-package-product-error-for-all-local-swift-packages-when-switching-git-branches/38041|https://forums.swift.org/t/missing-package-product-error-for-all-local-swift-packages-when-switching-git-branches/38041>

|U03JE7VT43Z|:
I have not tried the file:// hack yet

|U03JUCK9B16|:
FYI Yes, my app still has the same problem on Xocde 14. I’ll sign up for the lab

|U03JUCK9B16|:
Lab request sent

|U03JUCK9B16|:
<@U03J7H4LJ5N> My Lab request was accepted, but its right at the start of the session. Any chance of moving it to the end as I’ve got to travel back over 200 miles from somewhere a during that time.

|U03JUCK9B16|:
Actually I haven’t go the acceptance yet, just the standard “We’ll let you know” If you have any control over it, please make it sometime after 8am PDT, otherwise I’ll have to decline

|U03HES8111T|:
We'll try to make sure it's at a time slot that you can make.  Just so I understand, is it the 1pm-4pm PDT Xcode Open Hours Lab on Friday that you signed up for?

|U03JUCK9B16|:
Ah no - I did the first one (6am PDT) I saw. I hadn’t noticed the other one. Can you shift me to the later one or should I cancel and create a new request?

|U03HES8111T|:
I checked on whether it's possible to do a transfer, but it looks as if the appointment is already in the correct lab now (<https://developer.apple.com/wwdc22/labs-and-lounges/dashboard/CM93AQ7FJK/dashboard>).  Were you able to move it?

|U03JUCK9B16|:
yes, I just canceled and resubmitted

|U03JUCK9B16|:
Any idea when confirmation of labs comes through? I made a request yesterday for a lab tomorrow and have not heard anything yet other than they’ll get back to me

|U03J7H4LJ5N|:
They typically get assigned later in the day on the day before after signups close, I believe

|U03HES8111T|:
Yes, signups for a lab close at 6pm PDT the day before the lab, so I would not expect a confirmation until after that.

|U03JUCK9B16|:
OK, just that tomorrow I’m travelling to the place I’m coming back from on Friday, so don’t want to rush if I’m not in the lab

|U03JE7VT43Z|:
Feedback submitted: FB10116194

|U03HZ6C9S21|:
We had the same issue and got fixed when we stopped using strings to declare targets and packages and used the respective .package and .product and relative path for local packages like ../../dependencyProduct

--- 
> ####  I'm working on an app right now that I anticipate releasing before fall. I know that the new Xcode can make things for TestFlight, but what about for the app store?  I'm not sure I want to use beta Xcode for a released app. Can I even do so?


|U03HB5P2UTY|:
You can submit apps to TestFlight using Xcode 14 beta. However you won't be able to release apps built with Xcode 14 beta to the App Store.

|U03JF7VN2SE|:
roger, thanks!

--- 
> ####  Can you advise if there have been any changes from Xcode 13.4.1 to Xcode 14.0 that would perpetuate a new;  ``` Undefined symbols for architecture x86_64:   "_OBJC_CLASS_$_ADBMobile", referenced from:       objc-class-ref in OmnitureWrapper.o ld: symbol(s) not found for architecture x86_64 ```  I know that's overly specifically, but just curious what might have changed under the hood that would make building on Xcode 14 hit a build error!


|U03HL5H29CL|:
Hi Brandon! Looks like a missing symbol in your build - Please file feedback at <http://feedbackassistant.apple.com> and attach any details or an example project that can reproduce the issue.

|U03J20E7UBV|:
Thank you, <@U03HL5H29CL>!  Will do!

--- 
> ####  Deleting ~/Library/Developer/Xcode/DerivedData is a common workaround for various Xcode problems, at least according to community wisdom. Does this make Xcode engineers grit their teeth, and if so, what should we be doing instead?


|U03H36RCYJK|:
When deleting your derived data fixes a problem, it means that there's a dependency in your sources that either Xcode doesn't see but should, or that you haven't declared. In either case, if you could isolate the problem and report it in feedback, we'd use that to improve Xcode.

|U03HMD22287|:
Thank you, I will do that!

--- 
> ####  I usually like my assistant editor to the right, but I prefer the new build timeline below. Is there a way to save per-assistant position preferences?


|U03HB5P0B5L|:
This is not possible today, but is a great idea - if you could file a feedback at <http://feedbackassistant.apple.com> and post the FB number that would be greatly appreciated :slightly_smiling_face:

|U03JPFQNX5K|:
Historic reminder of a millennial developer: Xcode version 2 or 3 used to have no columns, just the project files and targets listing above the source editor (vertical split) → basically not enough space for either :sweat_smile: – those were some wild development times! :smile:

|U03JUCK9B16|:
Pah - MPW had individual windows for every file

|U03HMD22287|:
Thank you <@U03HB5P0B5L>, I will.

|U03HVD5CL5C|:
Kinda related: I had a very hard time finding the Build Timeline. Not sure if it living in the Assistant is so obvious.

|U03JRPTG8BS|:
I searched this Slack for “Build Timeline” because I was trying to find this feature in Xcode and failed. Definitely not obvious that it is hidden in the Assistant.

--- 
> ####  I recall it used to be you could not use a beta macOS version and the current public version of Xcode to distribute an app on the App Store. Is that still the case? Recognizing the current macOS beta doesn't support the public Xcode, it's not super relevant right now, but still curious if this remains true.


|U03HB5P2UTY|:
Xcode 13 will not run on macOS Ventura, There is no restriction on distributing apps to the App Store that were built with released tools on a beta macOS. However, we recommend using a released macOS whenever you build software that you intend to distribute.

--- 
> ####  I am using gRPC is there a way to monitor all the active gRPC connections? Not sure how accurate is the Active connection in the network report is.


|U03HES8U8FP|:
The network gauge in the debug navigator shows a list of all the active connections. You can see the local and remote addresses and ports, state and the traffic for each connection. The data you see there should be accurate, if that’s not the case, please send us a feedback!

--- 
> ####  Running a macOS catalyst app in Xcode in release mode creates two builds even though only one build is needed. This doubles the compile time. Any chance this could be fixed without affecting archive release builds?


|U03HB5P0B5L|:
Using generic run destinations will always build for all architectures, and because archiving can only be done with a generic run destinations, it will always build for all architectures. If you want to build for only the device's architecture in `Release`, you can do so by setting `ONLY_ACTIVE_ARCH` to `YES` for that (or all) configurations - it's set by default for `Debug`. This will not impact archiving.

|U03HZ5CEBKP|:
So why isn’t this done by default given that archiving would not be affected?

|U03HB5P0B5L|:
Thanks for the feedback! If you could file feedback at <http://feedbackassistant.apple.com> and post the FB number here, we'd really appreciate it.

|U03HZ5CEBKP|:
Done. <https://feedbackassistant.apple.com/feedback/10116446>

--- 
> ####  This came up in a lab today with Anders and he said to post here:  Tickets:  FB10107786 FB10107712  Is there currently any way to flip on the “this is extension safe” bit when building a Dylib entirely in SPM?


|U03HES8111T|:
There is currently no way to set the "extension safe" bit when building dynamic libraries from packages.

Thank you very much for providing the FB number here for that issue and for the bug with the conditional dependencies so that we can track them.

--- 
> ####  What plans to improvement Xcode Plugins? Can we see something like in VSCode Extensions or Jetbrains Plugins?


|U03J7H5DD2L|:
Hi <@U03JSFUKL2U>, hope you’re having a great WWDC so far! We can’t comment on the future, but we would be very interested to learn more about your use case and what problems you’re facing that you think such an idea could help with.

Could you please <https://developer.apple.com/news/?id=vvrgkboh|file a bug report> — and in the meantime, maybe the brand-new <https://developer.apple.com/videos/play/wwdc2022/110359|Swift Package plugins> will be of interest to you?

|U03JPFQNX5K|:
As somebody who remembers Alcatraz, I'd really like to see that level of extendability again. :thinking_face: I know there were security concerns then, but as something that could be fully optional, up to the developer (as much as we can disable SIP &amp; hook different scopes of OS), it'd be great to give this to the community again.

|U03JSFUKL2U|:
<@U03J7H5DD2L>  In my case - extend xcode terminal, like coloring, filter by tag or use system shell out of the box. Or write my own wrapper around something - git or gradle. 
I wonder to see same extensions API like in Safari where I can write SwiftUI extension and launch it in xcode! 

|U03J7H5DD2L|:
Ooh, those are cool ideas! Since this Digital Lounge will be shutting down pretty soon, a bug report might be the best place to write down these use case!

--- 
> ####  My project compile in Xcode 13.4.1 and in Xcode 14b1 it doesn't I have "The compiler is unable to type-check this expression in reasonable time; try breaking up the expression into distinct sub-expressions"  error in two different SwiftUI files that compile in Xcode 13.4.1.


|U03J7H5DD2L|:
Oh no! Sounds like you may have found a regression in Xcode 14. Could you please file a <https://developer.apple.com/news/?id=vvrgkboh|bug report> for that? It would be especially helpful to include a sample project that exhibits the issue. Try reducing it down into a very simple case so we can use it to reproduce.

If you’re unable to share a whole project, the SwiftUI files in question could also be helpful — or even a snippet of the complex expression.

Also, please share the Feedback ID here if you have time before the digital lounge ends!

|U03J4DF6D6G|:
I will share the code of my project.

|U03J4DF6D6G|:
I’m filing the radar now

|U03J4DF6D6G|:
<@U03J7H5DD2L> FB10115879

|U03J4DF6D6G|:
I hope it help

|U03J7H5DD2L|:
Thank you!!

--- 
> ####  I didn't want to derail another thread about differences between Xcode running on Apple Silicon vs. Intel: the main problem we face are snapshot tests that produce slightly different results when executed on simulators running on AS vs. Intel. (These tend to be sub-pixel differences in the rendered output). Are you aware of any way we can launch simulators to mitigate these differences?


|U03HHPY6YQ2|:
Hi Nathan. I'm not aware of any specific reason why running on Intel vs on Apple silicon would produce differences in the rendered output. You may want to check if your app is doing some kind of conditional compilation of code that might be affecting the layout or the rendering

|U03HHPY6YQ2|:
Barring that, I think this would be a bug that our OS teams would be very interested in looking at. You can use Feedback Assistant to file the bug report. A sample project that reproduces the particular issue, alongside with a description of your hardware, would be helpful

|U03JPFQNX5K|:
We faced this as well and basically ended up keeping everything related to snapshots to our CIs – so that only CI machines generate &amp; validate snapshots during the acceptance pipeline.

|U03JGEZM5K5|:
Curious what could be a reason. Can CPU architecture introduce different rounding issues that as result will produce subtle pixel differences in snapshots?

|U03HHPY6YQ2|:
Yes, floating-point operations on different CPUs could be a potential source

|U03JGH0JLMQ|:
Thanks for the reply! I don’t think it’s in our code because it’s reproducible on pretty trivial labels, but happy to check. There are a few discussions in <https://github.com/pointfreeco/swift-snapshot-testing/issues/424|snapshot libraries>

It’s also an issue that impacts <https://github.com/pedrovgs/Shot/issues/265|Android Emulators> so it’s definitely a problem much closer to the CPU! I guess I was wondering if there are potentially some secret render flags when instantiating a simulator that might trigger a different codepath. Happy to file a FB with the OS teams

|U03JGEZM5K5|:
This would be a legit use of precision in snapshot testing configuration, the same as where you compare two floats you do not compare them all way in. Most of snapshot test frameworks do have an option to configure precision. Setting something like 0.99 probably will solve the problem without allowing any significant degradations to sneak in.

|U03HHPY6YQ2|:
I was going to say essentially what Nikita said -- due to the nature of floating point operations, especially across CPU architectures, you can't really do a straight binary diff to determine whether things have changed. You need to choose a threshold for acceptable differences (deltas)

|U03HHPY6YQ2|:
With regard to any "secret render flags" -- that's a great question for the OS teams, but I think what they will tell you is that you won't always get 100% perfect output, and you need to decide what level of difference between two screenshots you are willing to tolerate

|U03JGH0JLMQ|:
We’ve seen some very high precision deltas sometimes (&gt; 10%) that would effectively make the tests useless, sadly. Though yes, in general, the libraries seem to be evolving so that they can quantify the difference between two pixels with more nuance than just being a match or not.

|U03HHPY6YQ2|:
Is it possible for you to standardize on a specific platform and CPU for taking the snapshots?

|U03JGH0JLMQ|:
Sure. At the moment anyone with an M1 asks someone on an Intel mac to update their PR with the snapshot tests :slightly_smiling_face:

|U03K840U3L1|:
Thanks for discussing this, we also are facing this issue in a team with mixed machines.

--- 
> ####  With TestFlight being an iOS 13+ app, how can I distribute my beta builds to users on iOS 12 which we are still supporting?


|U03HB5P2UTY|:
You can submit an app with an iOS 12 deployment target to TestFlight. In order to install the TestFlight client on devices with an older OS, you'll need to install a previous version from the Purchased list available in the App Store client.

--- 
> ####  I've heard mixed reports about bitcode being deprecated for all platforms vs just watchOS and tvOS. Could you clarify which platforms bitcode has been deprecated for? Also, is it right that a target will just need BITCODE_ENABLED set to NO.


|U03HB5P2UTY|:
Bitcode is deprecated for all platforms. You can set BITCODE_ENABLED to NO in your projects to disable building with it. Xcode will also automatically compile bitcode into machine code when you upload your app to App Store Connect.

--- 
> ####  In the enterprise setup for an iOS app where there is heavy use of unit tests, linters, code generation, etc is it possible to drive all of it with SPM, or fall back to the existing built-in Xcode build orchestration with xcproject and xcworkspace? There was a recommendation to use playground templates, can you confirm that it is solution for all the cases above?


|U03J7H5DD2L|:
Hi again! Sorry for the delay — I checked with some other engineers and some Xcode features like custom build phases and unit tests aren’t available in Swift Playgrounds apps at this time.

|U03JGEZM5K5|:
:hugging_face: thank you very much for checking on this. We will keep our fingers crossed that someday Xcode will be able to build , test, validate and run on simulator a standalone package without jumps through the hoops.

|U03JGEZM5K5|:
It seems like SPM package for any other platform but iOS is first class citizen in Xcode. You can open it in Xcode, build, run, run plugins etc. The only problem is that for iOS it is not working yet as there is no iOS targets in destinations yet even if you specify iOS only platform support in package.

|U03JGEZM5K5|:
ps but I am sure this is known :slightly_smiling_face:

--- 
> ####  Hi Guys, hopefully a simple question. In 13+ many times when I debug and try to see the values in variables it since they have been optimized out. Is there a way that this will not be done while debugging? I can t figure out what causes it and or iof there is a switch to stop this behaviour.


|U03HES8U8FP|:
This could happen because the debugger doesn’t have the symbols to read a particular variable from memory. We made a lot of improvements this year when it comes to the variables view in Xcode 14 and lldb. If you can isolate problems like these, please send us a feedback with a sample project!

|U03HHPTMS74|:
You've probably done this already, but do make sure that the SWIFT_OPTIMIZATION_LEVEL setting in your project is set to `none` in the build you are trying to debug.  Debugging optimized Swift code is not supported at present.

|U03HT20UKFZ|:
Ill make sure that is set, I think I did this a while ago. I am going to start using 14 next week and will file a report if it continues there

|U03HT20UKFZ|:
Thanks!

--- 
> ####  We have a large project that seems to crash Xcode frequently when switching branches. Is there anything in the project file that might cause this? We use SPM for dependencies, and it seems to possibly be correlated to when we switched from Cocoapods


|U03J7H4LJ5N|:
Which version of Xcode are you seeing this with and could you share a crash log?

|U03JRNYGBNC|:
xcode 13.4, and maybe 13.4.1

|U03JRNYGBNC|:
i don't have one at the moment, but i've shared many through the automated thing that pops up when it crashes

|U03J7H4LJ5N|:
Would you mind also filing a bug report at <https://feedbackassistant.apple.com> with the attached crash log, that would be greatly appreciated. That way we will have an avenue to follow-up with you.

|U03JRNYGBNC|:
ok, was able to just trigger in 13.4.1

|U03JRNYGBNC|:
yeah, i'll definitely file a report

|U03J7H4LJ5N|:
Thanks for the log, I can check whether this is something we already know more about

|U03J7H4LJ5N|:
It doesn’t look like we do, so the report would definitely be helpful, thanks!

|U03JRNYGBNC|:
i guess one followup question would be, do you have any idea whether corrupt project files in the wild is much of an issue?

|U03JRNYGBNC|:
i'm wondering if we had a bad merge at some point which created some weird thing in the project file

|U03JRNYGBNC|:
which might be fixed by rebuilding it

|U03J7H4LJ5N|:
It’s a pretty rare issue, so I wouldn’t think it’s the most likely explanation. But from a cursory look at the crash log, I also wouldn’t entirely rule out that possibility.

--- 
> ####  Our "Create build description" step on a clean build takes 145 seconds, is there a way to speed that up?


|U03HHPCVCLT|:
Please submit a feedback with a spindump during the build when the build description is being created. Any additional information, such as the size and number of files in your project would be useful to know as well.

Also, please consider signing up for a Build Performance Lab to speak directly with an engineer about the issue.

Link to the next lab:
<https://developer.apple.com/wwdc22/labs-and-lounges/dashboard/44T86K56S9/dashboard>

--- 
> ####  Not a question, but a thank you for making previous errors in the build navigator turn grey while doing a new build to make it clear that they may be stale


|U03HB5P0B5L|:
Thanks! As you get to know the feature, if you have any suggestions for improving it further, please do send us your feedback :slightly_smiling_face:.

|U03HMD22287|:
Yes, thank you, this feature is wonderful! :heart_eyes:

--- 
> ####  I use VIM on a daily basis, and I find frustrating the lack of functionality on XCode. Are there plans to improve VIM support on Xcode 14 or later?


|U03HHPY6YQ2|:
Hi Juan, I'm sorry to hear that you are having issues. Could you file a feature request using Feedback Assistant detailing the specific things that you feel Vim support in Xcode is missing?

|U03HX9ZTNQ7|:
OK, I'll do it. Thanks for the quick answer.

--- 
> ####  Is it normal to see multiple "Resolve Packages" tasks in the Report navigator when opening a project with lots of SPM dependencies? If so, why?


|U03HES8111T|:
This is not normally expected.  Are you seeing any warnings or other messages in the previous Package Resolution logs?

It is also possible that something is causing a change to the files in the package that is triggering second package resolution after the first one completes.

Would you mind filing a bug report at <http://feedbackassistant.apple.com|feedbackassistant.apple.com> with as much information as you can provide from the package resolution log?

|U03JRNYGBNC|:
usually we don't see any issues when this happens, though occasionally authentication fails to a git repo with private dependencies

|U03JRNYGBNC|:
i'll file a feedback though, yes

|U03JPFQNX5K|:
In my case, this is triggered when opening a Swift package directly, having DerivedData folder set to be placed relatively, that means, inside the package folder. :boom: Just to put another reproduction way here. :ok_hand:

|U03HES8111T|:
Thank you for the additional information.  I think a bug report with as much information as you can share would be helpful.  The part about the DerivedData folder is particularly interesting, and would suggest that it's the creation of intermediate package resolution files that's causing the reresolve.

|U03HES8111T|:
There is provision for avoiding such a feedback loop, but it definitely sounds as if there is a bug here.

--- 
> ####  Whats the recommended way to package up internal dependencies (ex: model-layer code, etc) in Xcode? Is it SPM or Frameworks?


|U03J7H5DD2L|:
The answer to this will depend on your team and your needs. Packages will make it easier to break your dependencies up into smaller chunks, store them in separate repos, and version them independently — but frameworks will give you things like advanced project customisation and Objective-C interoperability.

|U03JK060ZNZ|:
Adding to this thread: if our framework uses Objective-C code, and users of the framework (customers) are asking us for a Swift package, can we give them what they want by bundling the framework as a binary wrapped in a Swift package?

|U03JK060ZNZ|:
Reposted my question in its own thread in case this thread is marked as already answered… apparently cannot delete here.

|U03J7H5DD2L|:
No worries — that will definitely get more eyeballs on your question!

|U03J4D1FEP6|:
<@U03JK060ZNZ> not sure if your reposted question got answered in the end, so just coming back here to comment: I was really surprised to discover a while back that you can actually vend a mixture of Swift and Objective-C code in a single Swift Package. If the situation hasn’t changed, each _target_ within the Package can only be Swift or Objective-C. But you can put your Objective-C classes in a single target, and make that an internal dependency of your Swift library. So all up, the point is you don’t have to bundle it as a binary if you don’t want to, in order to give your customers the Swift package.

Maybe this isn’t what you were asking… but just in case it was.

My use case was because I needed to include a helper class in Objective-C to call an iOS API that currently cannot be called from Swift, only from Objective-C. (Weird.)

--- 
> ####  We've got an app that's split up into many frameworks. There's a single project that contains all the targets. I've seen examples of apps that make multiple single-target projects in a workspace. Other than organization, is there any difference between those two approaches or are they functionally the same.


|U03H36RCYJK|:
The two arrangements support the same features. If you'd like to open subsets of the projects without seeing all of the sources, you might prefer the organization with multiple project files.

For example, if you later have two top level apps that use different subsets of the targets, they can each have a more focused workspace with the many projects approach.

--- 
> ####  I keep getting "Could not resolve package dependencies error" for Xcode Cloud last 2 days. I am pretty sure I have Package.resolved checked in to the repo. anything I can do to resolve this issue? everything build fine on my Xcode


|U03J7H4LJ5N|:
If you take the xcodebuild command from the Resolve package dependencies step of your Xcode Cloud logs and add the option -IDEPackageOnlyUseVersionsFromResolvedFile=YES, can you reproduce the issue locally?

|U03J7H4LJ5N|:
Alternatively, feel free to submit a bug report at <https://feedbackassistant.apple.com>

|U03JQ7PU1L1|:
I got a provision error when running locally saying provision  doesn't include the currently selected device

--- 
> ####  Monterey does not allow Xcode 12.x to run. Is it safe to assume that like 12, no prior versions of Xcode before 12 run on Monterey?


|U03HHPY6YQ2|:
Yes, I believe that is correct

--- 
> ####  Why do we keep getting code signing warning when validating a project in Xcode, if we have the settings applied at the project (not target) level?


|U03HB5P2UTY|:
This is most likely a bug. Could you file a bug report about this and provide the ID number to me?

|U03HZ4PT2ER|:
<@U03HB5P2UTY> FB10116268

|U03HB5P2UTY|:
Thank you, we'll take a look. From your description, it sounds like an Xcode bug and not a project configuration issue.

--- 
> ####  Are planning to add support “Signing and Run locally” to iOS targets?


|U03HB5P2UTY|:
iOS requires apps to be signed with a trusted certificate in order to install and run.

|U03JSFUKL2U|:
Ok, mac can run app without trusted cert? what about generating one day cert? Or it's more complicated than I thought?

|U03HB5P2UTY|:
The macOS runtime environment differs substantially from iOS. However, if this is something that would benefit your work we'd welcome a feedback report with more details.

--- 
> ####  The new primary associated types are really cool. I immediately tried it with `Publisher` eg: `some Publisher&lt;String, Never` but found this protocol does not support this. I've read SE-0358 but I'm still not clear on why this protocol shouldn't or couldn't use this new feature?


|U03HWDCSCHX|:
Thanks for your interest in using Combine with primary associated types. We share your excitement, and think this would be a great direction forward. Please file feedback at <https://feedbackassistant.apple.com> against Combine to add this as an enhancement.

|U03HWDCSCHX|:
To answer your question, there is nothing stopping Combine from adopting this feature. You can even try it out yourself if you want to by defining your own protocol that refines Publisher and making combines existing publisher types conform to it.

```
protocol _Publisher&lt;Output, Failure&gt;: Combine.Publisher {
  associatedtype Output
  associatedtype Failure
}

extension Just: _Publisher {}
/* etc */
```
It's a bit tedious but each conformance should take no code outside of the extension.

--- 
> ####  If our framework uses Objective-C code, and users of the framework (customers) are asking us for a Swift package, can we give them what they want by bundling the framework as a binary wrapped in a Swift package?


|U03HES8111T|:
Swift Packages can contain targets that are comprised of C or Objective-C code and that are compiled with the Clang compiler.  If the Objective-C target has modularized headers and does not require special compiler options that are not supported by Swift Package, then a good solution would be to build the library as a Swift Package with an Objective-C target.

--- 
> ####  We’re building a location tracking app that collects user locations in background and periodically sends them to our backend for further processing. It’s important for us that the app is as power efficient as possible. Which means 1. optimizing location data collecting 2. network usage optimization (choosing the right network protocol/time intervals and so on).   The question is what’s the best way to test different location/network settings for power consumption? Is there a way to measure device power consumption in mA or watts?  We tried using xCode’s energy impact gauge, but it doesn’t give enough precision to distinguish between different CLLocationManager settings. We also tried running tests on a physical device and see what battery drainage per hour is. But it’s terribly inefficient and the results are not as precise as we would like them to be. 


|U03J7H4CG6L|:
Hi Mykyta! I'd recommend that you ask this question in the "Performance, power, and stability lab" tomorrow, as the folks there should have some good recommendations for your use cases. I'd also recommend checking out the WWDC19 session "Designing for Adverse Network and Temperature Conditions". As that session will explain, it's possible to use Xcode to simulate adverse network conditions, which may be useful in this case.

|U03J01RLBBR|:
<@U03JWUYKJLS> I’d also be interested in what you are able to find out from the Performance, Power and Stability lab as these are important things we are attempting to inspect and evaluate as well.

|U03HL5AMFCL|:
Instruments has a Location Energy Model tool in the library that you can use to track your CoreLocation precision usage over time. This is helpful because as you point out, the best way to manage the power usage from your code is with the location manager settings.

Measuring actual power drain is much more complicated, as there are a myriad of factors — device model, network conditions, other device services, temperature, etc. and it's hard to measure precisely in live debugging conditions or predict/extrapolate to what users will experience when they're out and about in different conditions. MetricKit can help with the aggregate statistics though to show you usage trends over time,

--- 
> ####  When managing workflow for Xcode Cloud the scheme I selected have a yellow exclamation mark saying "The scheme may only exist locally". I am pretty sure this scheme have shared checked and the xcsharedata folder is in the repo. I am wondering is because of the having this exclamation mark my xcode cloud build always failed with message "An internal error has occurred. This operation will be retried on another build worker"


|U03J7H8QABS|:
The warning that “the scheme may only exist locally” shows up if you configure a workflow with a scheme that Xcode Cloud has never seen in a cloud build. Every time we run a build we record what public schemes were in your project. If you add a new scheme in a commit, Xcode on your machine knows about it but our servers don’t. You can go ahead and change the workflow anyways, it’s just warning you that builds might fail on branches that don’t have the scheme.

As for the “an internal error occurred message,” please file a feedback with a link to the build. It might be the cause of the warning, if there’s a failure before we record the schemes.

|U03JQ7PU1L1|:
Now I keep getting "Could not resolve package dependencies" error. executing the same command in local is fine

|U03JQ7PU1L1|:
I added -IDEPackageOnlyUseVersionsFromResolvedFile=YES suggested by Boris yesterday to run it locally and is running fine as well

|U03J7H8QABS|:
The most common cause of package dependency issues is if Xcode Cloud can't access the package.resolved file in your repo, see
<https://developer.apple.com/documentation/xcode/making-dependencies-available-to-xcode-cloud#Use-Swift-package-dependencies-and-Git-submodules>
If that command is working locally, it means the file is available locally, but make sure it's checked in as well.

|U03JQ7PU1L1|:
Yes, I am 100% sure it's checked in.

|U03JQ7PU1L1|:
Okay I just double check I think it didn't get checked in. Thanks for the help

|U03J7H8QABS|:
Glad that helped!

--- 
> ####  I'm interested in introducing Xcode Cloud for a public sector project. Any suggested pathways towards having it approved for government use? Or insight into FISMA rating?


|U03HL553PNG|:
Hi Clint!  We're all engineers here and this is more of a question for WWDR or possibly our Legal team.  Can you reach out through <http://developer.apple.com/contact|developer.apple.com/contact> and ask your question there?  We'll try to find the right answer for you there.

|U03JKEKF1TP|:
ok thanks

--- 
> ####  Xcode Cloud: is it possible to build and export a notarized Mac app on Xcode Cloud?


|U03HB5P2UTY|:
Support for notarizing Mac apps is not available in Xcode Cloud. We would welcome a feedback report from you if this is a feature you'd like to see in the future.

|U03J1UX2CQK|:
Sure thing! FB9961116

|U03HB5P2UTY|:
Thank you!

--- 
> ####  When adding [ci skip] in my commit for Xcode Cloud, will this string also appear in our GitHub repository?


|U03J7H8KC8Y|:
Yes, the full content of your commit message, including the `[ci skip]` tag, will be present in your repository.

|U03J7H8KC8Y|:
Xcode Cloud reads the content of the commit message, but it cannot filter or modify it.

|U03JHQDJ31C|:
I see, makes sense. Thanks for answering.

--- 
> ####  Can i make only adhoc ipa with Xcode cloud? All types of ipa in develop/adhoc/store are output to the deliverables. In the Scheme specified in the workflow, adhoc is set in Archive. Want to reduce build time.


|U03HB5P2UTY|:
There is no option to customize which IPAs are output at this time. If you are experiencing increased build times as a result of this, we'd welcome a feedback report.

|U03JJJQ411C|:
That clears things up, thanks.
I understand that the specification is that all types of ipa will be output.

In addition, please let me ask.
Do you have any plans to add such options in the future?

|U03HB5P2UTY|:
I can't comment on future plans, but we always give a lot of weight to the requests we get from our developers via feedback reports :smile:

|U03JJJQ411C|:
Thank you for taking the time to help me.
It was very helpful.

--- 
> ####  Hello! My team is very interested in switching to Xcode Cloud for CI/CD. However, we're currently using Azure DevOps for both SCM and CI/CD, and I don't see that Azure DevOps is a supported SCM provider. Is there a way to use Xcode Cloud with AzDO as the SCM provider? We would not likely be able to use another SCM provider.


|U03J7H65WGY|:
We don't support Azure DevOps. Xcode Cloud only supports Bitbucket, Github, and Gitlab SCM providers. Please send feedback with Feedback Assistant to suggest adding support for Azure DevOps.

|U03HMD22287|:
Thank you, I'll do that.

--- 
> ####  What's a reasonable expectation on the backwards compatibility for `xctestrun`? I'm creating mostly macOS apps, and I need to keep a couple of versions of macOS supported to keep with my users. Right now (Xcode 13.4), I can use build_for_testing and test_without_building to test under macOS 11 and macOS 12, but not macOS 10.15 (our oldest supported version. Going forward, what can we expect for backwards testing compatibility?


|U03J7H4CG6L|:
Regardless of the project's deployment target, the products of build-for-testing are only valid for testing on macOS versions that are supported by the Xcode that produced those products.

|U03HVC2HDFG|:
Is there any indication which versions are supported by Xcode for Xcode 14?

|U03J7H4CG6L|:
Since you'd like to test older macOS versions than the ones that are supported by the Xcode that's producing the test build products, could you file a feedback assistant requesting this capability?

|U03J7H4CG6L|:
Xcode 14 is supported on macOS 12.4 and later.

|U03HVC2HDFG|:
It would be helpful to have test-without-building support os versions that represent a substantial portion of the customer population. For iOS, it's easy to expect people to move forward, because of the high percentage of users who move forward annually. I don't see the stats published by Apple, but from our customers we don't see the same quick adoption on macOS. (Yes, I'll put that in the feedback).

|U03J7H4CG6L|:
The use case of testing older macOS versions is definitely valid, and it's awesome that you are focused on expanding your test coverage across various OS versions. Your feedback here will help us prioritize our offerings in this area for the future!

--- 
> ####  Using Xcode Cloud, is there any way to test with physical devices as well as simulators?


|U03HL5H29CL|:
Hi Sara! No there isn't. But if this is a use case that you need please file a feedback at <http://feedbackassistant.apple.com|feedbackassistant.apple.com> with your use case and details.

|U03JE7PBQPP|:
Gotcha. There's some notable differences between simulators and real devices with lack of some default apps or lack of features in them, Cellular behavior, inability to change some system configurations, UI differences from a Simulator rendering and a physical device, etc.

|U03J01RLBBR|:
Yes, we experience this as well. We build a framework that id heavily dependent on the various CoreMotion sensors which are unavailable in the Simulators, so on device testing even for simple unit tests is necessary.

--- 
> ####  How do Xcode Cloud and Xcode Server "relate to" and "work with" each other? Would it make sense to have an Xcode Server setup for small/PoC projects/teams and an Xcode Cloud setup for bigger/productive projects/teams? Would it be easy to move projects and configuration files between Xcode Cloud and (on premise) Xcode Server? In case this would indeed be an option, is there any documentation how to best practice such a dual CI/CD setup approach?


|U03HWD8L36V|:
Xcode Server has been deprecated this year, see <https://developer.apple.com/documentation/xcode-release-notes/xcode-14-release-notes>. We encourage developers to try out Xcode Cloud. If you find functionality missing from Xcode Cloud that was present in Xcode Server, please file a feedback.

--- 
> ####  Is Xcode Cloud available for enterprise apps?


|U03HB5P2UTY|:
Xcode Cloud is not available for the Apple Developer Enterprise Program. If this is a feature you'd like to see in the future, we'd welcome a feedback report from you.

|U03HL553PNG|:
<http://feedbackassistant.apple.com|feedbackassistant.apple.com>

|U03HB5P2UTY|:
Also I should mention that *cloud signing*, which is available in both Xcode Cloud and in Xcode, *does* support enterprise apps. So you can export your apps from Xcode's Organizer and take advantage of cloud managed signing certificates, just like Xcode Cloud uses.

|U03HB5P2UTY|:
You can learn more about that in <https://developer.apple.com/videos/play/wwdc2021/10204|Distribute apps in Xcode with cloud signing> from WWDC21.

--- 
> ####  Will/does Xcode Cloud have the ability to auto-update signing certificates when they will expire?


|U03HB5P2UTY|:
Xcode Cloud uses cloud signing certificates that it automatically manages on your behalf. It will renew certificates a few months before they expire so that your builds have valid signatures while you test and distribute them.

--- 
> ####  I have a problem with xcode cloud. In certain cases, branches are not recognized and workflow cannot be started. Problems occur when trying to start a workflow directly from AppStoreConnect and Xcode using the "Start Build" button. If the workflow is initiated by a StartConditions decision, the problem does not occur. We use bitbucket.`master` branch is fine. `develop_xxx` branch is fine too. Problems occur with `develop/xxx`, `release/xxx` . If the branch name (path) contains `/`, it may cause problems.


|U03J7H4CG6L|:
Could you please file a feedback assistant for this issue, and include the link to the Xcode Cloud job and workflow in the description?

|U03J7H4CG6L|:
Also, when you try to manually start a build for a branch with a "/" in the name, does the build start and then fail? Or does no build start at all?

|U03J7H4CG6L|:
If you could also paste the feedback assistant number here, I can make sure it gets routed to the correct folks.

|U03JJJQ411C|:
Thanks.
We had not yet filed for a feedback assistant, so we will file for a feedback assistant after this.

&gt; does the build start and then fail? Or does no build start at all?
It does no build start at all. And As shown in the attached image, commit information and other information are not loaded, and the display is different from norma

|U03J7H4CG6L|:
I see. If you copy the build number link (the "124" in that image) and paste that in the feedback assistant, that would be very useful!

|U03JJJQ411C|:
Thanks for the advice. I'll do that when I file the feedback assistant.

|U03JJJQ411C|:
<@U03J7H4CG6L>
I filed feedback assistant. Number is 10145252.

--- 
> ####  If I used Xcode Cloud to build the app, do I still need to adjust the build number in the future? Or just leave it as 1 is fine?


|U03HB5P2UTY|:
Xcode Cloud can manage your build number for you. For a new app, you can leave the build number at 1. If you have an existing app, we suggest reading <https://developer.apple.com/documentation/xcode/setting-the-next-build-number-for-xcode-cloud-builds|this documentation> for more information about how Xcode Cloud manages build numbers.

|U03HVCK66P8|:
Sure. No problem

--- 
> ####  Hello. The Xcode cloud is not so stable and always 'down'. After committed the files, it takes a while to show-up, but just queuing. 🫠


|U03HVCK66P8|:
Hi. I just tried again, and it seems the server issue is fixed.

|U03J7H4CG6L|:
Could you file a feedback assistant report at <https://feedbackassistant.apple.com>? If you could include the link to your Xcode Cloud workflow, and any builds where you are noticing issues/reduced performance, an Xcode Cloud engineer will be able to take a look.

|U03HVCK66P8|:
Okay!

|U03J7H4CG6L|:
Thank you!

--- 
> ####  Does Xcode Cloud integrate with other CI/CD workflows like Jenkins or is it considered standalone?


|U03HB5RVBQW|:
You can connect Xcode Cloud to other services using webhooks directly (<https://developer.apple.com/documentation/xcode/configuring-webhooks-in-xcode-cloud>), and Xcode Cloud is also part of the App Store Connect API (<https://developer.apple.com/documentation/appstoreconnectapi/xcode_cloud_workflows_and_builds>)

--- 
> ####  Can you integrate Xcode cloud with Fastlane?


|U03HB5RVBQW|:
You can connect Xcode Cloud to other services using webhooks: <https://developer.apple.com/documentation/xcode/configuring-webhooks-in-xcode-cloud>

If there's more behavior you'd like to see supported in Xcode Cloud for your development workflow, please file a Feedback report at <http://feedbackassistant.apple.com>.

|U03HB5RVBQW|:
Xcode Cloud is also part of the App Store Connect API (<https://developer.apple.com/documentation/appstoreconnectapi/xcode_cloud_workflows_and_builds>)

|U03HB5RVBQW|:
Alternatively, if you're trying to call Fastlane from Xcode Cloud, check out "Writing custom build scripts": <https://developer.apple.com/documentation/xcode/writing-custom-build-scripts>

--- 
> ####  Is Xcode Cloud possible to cache a specific directory for next build? I want to cache libraries built by Carthage and software installed with brew such as aws cli. Our Workflow takes a long time to build an environment and I want to solve this problem. We are still in need of a library that only supports carthage. There are plans to use SPM, but we still need more time.


|U03J7H8KC8Y|:
Xcode Cloud doesn’t cache anything outside of Xcode’s derived data, but if that’s a feature that would be helpful for you, please file a feedback at <https://feedbackassistant.apple.com> describing your use case.

|U03HVCY3170|:
Since I’ve done similar things myself in the past it might make sense to cache expensive to build artefacts in your own storage and load them from there

|U03J2RRKF9U|:
This is something my team would love to see too! Submitted FB10135438 :thumbsup:

|U03JJJQ411C|:
Thank you, understood.
We will file a feedback assistant, stating the status of use.

--- 
> ####  We have a large development organization with many apps and teams.  I have two Xcode Cloud subscription questions:  1. What happens if we exceed 1000 hours of compute time a month?  2. Would it be possible to have multiple subscriptions, each for  specific apps/teams?


|U03J7H65WGY|:
&gt; 1. What happens if we exceed 1000 hours of compute time a month?
Any builds in flight will complete, however no new builds will kick-off until the cycle resets on the "monthly anniversary" date.

&gt; 2. Would it be possible to have multiple subscriptions, each for  specific apps/teams?
Subscriptions are per Developer team.

--- 
> ####  Does Xcode Cloud offer a choice between Intel or Apple Silicon hardware?


|U03HL5G4QFN|:
We do not offer a choice at this time. If you require Apple Silicon machines please file a feedback

|U03JE7PBQPP|:
Does that mean Xcode Cloud runs on Intel?

|U03HL5G4QFN|:
At this time Xcode Cloud provides a x86_64 environment

--- 
> ####  Can Xcode Cloud support ssh package urls to GitHub package repos?


|U03J7H8QABS|:
Yes, both https and ssh URLs are supported for cloning both the primary repo and package dependencies.

--- 
> ####  I am getting started with Xcode Cloud, but can't get build caching to work. All builds take the same time as the first, "clean" build, and the `DerivedData` folder is empty at the start of each build. In addition, I tried caching my CocoaPods by symlinking them into `DerivedData` as suggested at <https://developer.apple.com/forums/thread/695634,|https://developer.apple.com/forums/thread/695634,> but that doesn't seem to work, either.


|U03HL5G4QFN|:
Please <http://feedbackassistant.apple.com|file a feedback> describing what you are seeing so the team can take a look

--- 
> ####  This is perhaps not a question for the engineering team but I'm wondering how build hours are billed beyond 1000 a month. Doing some napkin math, I think my team might be in that ballpark.


|U03HL5G4QFN|:
We do not currently offer plans beyond 1000 hours. If you need more than 1000 hours please file a feedback so the team can understand your needs. But we would recommend trying the product to see how the back of the napkin math compares to reality

|U03HMD22287|:
Yes, measuring in the real world is good! Thank you.

--- 
> ####  To build my project, I need to first build some tools using Mint (<https://github.com/yonaskolb/Mint)|https://github.com/yonaskolb/Mint)> such as swift-format and XcodeGen. This dramatically increases job times because the extra tools need to be rebuilt each run. Do you have any recommendations on how to reduce the time needed for this? Is there a way to cache custom directories created during the job such as ~/.mint (where Mint puts the built products) to re-use on subsequent jobs?


|U03J7H4CG6L|:
One thing you could try here is to install these tools via homebrew in `ci_pre_xcodebuild.sh`, which is a script that runs prior to xcodebuild in Xcode Cloud (see the documentation <https://developer.apple.com/documentation/xcode/writing-custom-build-scripts|here>).

|U03J2RRKF9U|:
Thanks for the suggestion! We would, but Homebrew doesn’t let us control the version of the tool, we can only get the latest. We want to make sure all the devs &amp; CI are using the same versions to avoid any inconsistencies :smile:

|U03J7H4CG6L|:
In that case, another option is to have the versions of the tools you need on some content storage, and then download those tools in `ci_pre_xcodebuild.sh`.

|U03J2RRKF9U|:
<@U03J7H4CG6L> do you know if Swift Package plugins would possibly help with this? A lot of our caching use case has to do with tools related to code generation and linting/formatting. I haven’t had time to explore Package plugins yet, but I wonder if we could pull those tool dependencies into our app’s SPM dependencies, and write some Package plugins to execute them. At that point maybe the tools would be cached in DerivedData?

Most of the examples I’ve seen for Package plugins seemed to execute a CLI tool though, not run other Swift code, so maybe this wouldn’t work :thinking_face:

|U03J7H4CG6L|:
If that also doesn't work, we recommend storing the build products in DerivedData. The contents of DerivedData should be cached for future runs. If you notice any issues with that, please file a feedback assistant!

|U03J2RRKF9U|:
OK, we can give that a try :thumbsup:
I’ll also try to play around with Package plugins to see if they’d fit what we’re trying to do.

Thanks!

--- 
> ####  On the iOS 16 beta, we noticed a new _UIWindowSystemOverlayWindow, which interfered with some of our UI tests. Could it be related to the new Stage Manager?


|U03HL5H29CL|:
Hey Barry! Can you provide some more specifics around this? Which UI are you seeing?

|U03J4EW62N8|:
<@U03HL5H29CL> We don’t see it visually but it was detected during  debugging an issue where some view of ours failed to be tapped due to “not hittable” and we found that system window was the reason

|U03HL5H29CL|:
Interesting! Could you file a Feedback Report a <http://feedbackassistant.apple.com|feedbackassistant.apple.com> with an example project or some good details of what your test is attempting to do when hitting it?

|U03J4EW62N8|:
Will do. Thanks

--- 
> ####  Does Xcode Cloud run on Intel or Apple Silicon or a somewhat-random hybrid (you don't know what you'll build on each time)?


|U03HL5G4QFN|:
At this time Xcode Cloud provides a x86_64 environment. If you require Apple Silicon machines please <http://feedbackassistant.apple.com|file a feedback>

--- 
> ####  For xcode Cloud when i add my github it take forevet to get access  to github why is this?


|U03J7H4CG6L|:
Can you file a feedback assistant on <https://feedbackassistant.apple.com> and include the link to your Xcode Cloud workflow and build execution?

|U03J7H4CG6L|:
If you paste the feedback assistant number here, we can get it routed to an Xcode Cloud engineer to take a look.

|U03HVBSNM6J|:
Ok,Sorry new to xcode cloud.

|U03J7H4CG6L|:
No worries! I think I may have misunderstood your question actually. Are you asking about connecting to github when you first create a workflow?

|U03HVBSNM6J|:
Yes

|U03HVBSNM6J|:
Sorry i was not clear about it.

|U03HWD8L36V|:
Hi Craig - are you connecting to GitHub or GitHub Enterprise?

|U03HVBSNM6J|:
Github personal  i use

|U03HWD8L36V|:
So <http://github.com|github.com>?

|U03HVBSNM6J|:
ok

|U03HWD8L36V|:
Okay, see these steps for connecting Xcode Cloud with your GitHub repository: <https://developer.apple.com/documentation/xcode/connecting-xcode-cloud-to-github>

|U03HWD8L36V|:
In the doc, see item 5 to answer your question about creating a GitHub app:
&gt; 5. Review the GitHub app that Apple provides on the GitHub website and choose to only install it for your project’s repository. Don’t install it for every repository. GitHub uses the app to grant Xcode Cloud access to your repository.

|U03HVBSNM6J|:
Add remote for github but when i get to  try to access  github it keep lading.

|U03HWD8L36V|:
You mean when Xcode Cloud runs a job where it tries to clone your repository, that it keeps loading there? Or that in Xcode, when you e.g. push/pull to your GitHub repository, that it keeps loading? Or when you're creating an Xcode Cloud workflow and Xcode asks you to authorize your GitHub repository with Xcode Cloud? Just so that I understand where the issue might be.

|U03HVBSNM6J|:
Yes

|U03HWD8L36V|:
Which of the 3 scenarios are you seeing the issue in?

|U03HVBSNM6J|:
It working now

|U03HWD8L36V|:
Great!

|U03HVBSNM6J|:
Thank You

|U03HWD8L36V|:
You're welcome!

|U03HVBSNM6J|:
:slightly_smiling_face:

--- 
> ####  Is there a workflow that allows for manually building a build off of a specific branch?  More specifically, if I had a feature branch and wanted to generate a build for a non-developer to test through TestFlight, is this possible?


|U03J7H65WGY|:
Yes! You can create a workflow with an archive action and any start condition. To prevent the start condition from running and make the workflow only trigger manually, you can disable the workflow. Disabling the workflow will stop the start condition from working, but will still let you start builds manually. You can start a build manually by using the Start Build button. I realize that this flow is  involved. Please file a feedback report with <http://feedbackassistant.apple.com/|Feedback Assistant> and provide feedback / suggestions for making this flow better.

|U03J20E7UBV|:
Thank you, <@U03J7H65WGY>!

|U03J7H65WGY|:
No problem. I hope you have a great rest of your day!

|U03J20E7UBV|:
You as well!

--- 
> ####  In Xcode cloud workflow there is an option to select Deployment preparation for internal or App Store. and In Post-Action it's also separate between internal and app store. My question is from my understand in Appstore connect dashboard there is only one place showing Testflight how is internal and appstore build being differentiate? or how can I tell which one is for internal or which one is for app store


|U03HB5P2UTY|:
If you want to check if an Xcode Cloud build is Internal Only or App Store Eligible, you can go to Build Groups tab &gt; click on a build and check the Build Type field. Also, you will not be able to add any internal only builds to an external group, they will be disabled in the Add Build UI.

|U03JQ7PU1L1|:
Thanks. curious did this ever get mentioned in any document or session ?

|U03HB5P2UTY|:
Deploying to internal testers was covered in <https://developer.apple.com/wwdc21/10268|Explore Xcode Cloud workflows>

--- 
> ####  I have a lot of extensions in our app so updating them for a new build requires going in to each one to update the version number and build number. Is there a way to automate this? using custom script?


|U03HB5P2UTY|:
When distributing your app from either Xcode or Xcode Cloud, the build numbers in your app and all of your app extensions will automatically be updated. For more information, watch <https://developer.apple.com/wwdc21/10204|Distribute apps in Xcode with cloud signing> or check out <https://developer.apple.com/documentation/xcode/setting-the-next-build-number-for-xcode-cloud-builds|this documentation> for Xcode Cloud. You will still need to manage version numbers yourself, since those are more correlated to your own product release plans.

|U03JQ7PU1L1|:
Thanks Itai

--- 
> ####  Does Xcode Cloud support custom release notes during a build step? I could be missing a toggle somewhere but feels like a great feature to include, e.g. branch names or commit msg.


|U03J7H8QABS|:
Xcode Cloud doesn’t have any kind of release notes support, but it sounds like a great idea. Could you <http://feedbackassistant.apple.com/|file a feedback> explaining what you'd like to see and how you might use it?

|U03K9D00WLR|:
:+1: 

--- 
> ####  Is it possible to disable `precondition` checks when running XCTests? where should I pass the compile flag?


|U03J7H8KC8Y|:
This is possible by adding `-Ounchecked` to the Other Swift Flags build setting, however this would not be specific to running tests, and is not generally recommended.

--- 
> ####  I am working at an app agency, building apps for 50+ customers, all with their own developer accounts. What is the recommended way of managing Xcode Cloud setups &amp; builds across multiple developer accounts or teams?


|U03HL5G4QFN|:
Xcode Cloud is per WWDR team. You would have to onboard your apps in Xcode for each respective app/team. We don't really have advice for the managing multiple WWDR teams.

--- 
> ####  When trying to set up Xcode Cloud for our iOS app containing private Swift packages, we see the error  GitLab installation was incomplete. Repository was not found. Either the repository does not exist or you do not have permission to access it.  Is there something special to do for Xcode Cloud to get access to private Swift packages hosted on GitLab?


|U03HWD8L36V|:
Please make sure to follow the steps described in connecting Xcode Cloud with GitLab (<https://developer.apple.com/documentation/xcode/connecting-xcode-cloud-to-gitlab>) or self-hosted GitLab (<https://developer.apple.com/documentation/xcode/connecting-xcode-cloud-to-a-self-managed-gitlab-instance>), depending on which one you're using.

For that, you need to be an admin of the repositories.

If the issue persists, please file a feedback at <http://feedbackassistant.apple.com|feedbackassistant.apple.com>.

|U03JEMQ66C9|:
Thanks <@U03HWD8L36V>, I will double check!

|U03HWD8L36V|:
You can also try to remove the connection and start over. For the removal, see <https://developer.apple.com/documentation/xcode/removing-your-project-from-xcode-cloud#Disconnect-your-Git-repository-in-Xcode-Cloud>

--- 
> ####  Any chance to get Xcode Cloud enabled for Enterprise/In-House builds? That would be our main use-case.


|U03HB5P2UTY|:
Thank you for your question! This was answered above in <https://wwdc22.slack.com/archives/C03H0JN431U/p1654792281999369>

--- 
> ####  Is there a tool to help track Hangs with Enterprise Apps ?


|U03HHPU5NLS|:
Hi - thanks for the question! You should be able to use MetricKit to collect hang reports from enterprise apps on iOS and macOS. Please also give Instruments with Hang Reporting a try.

|U03HVD5FL1L|:
Thanks !

--- 
> ####  Hi! I hope this is the right room for this question. When releasing an app on the App Store, we can conveniently receive symbolicated crash reports via the App Store itself. Instead, when releasing apps by any other mean, like enterprise, I currently rely on Firebase Crashlytics, but it's not as convenient because since lately (a year or more) it's always missing dSYMs. Is there any official way that maybe I don't know to get crash reports from enterprise apps? Thank you!


|U03HHPU5NLS|:
Hello. I'm glad to hear you're making use of the Xcode Organizer window!

Crash reporting in the Organizer can be used for TestFlight, App Store, and Custom App distributions. If it makes sense, I would consider using TestFlight or Custom App distribution where you may be using Enterprise or Ad Hoc distribution.

For distributions outside the App Store or TestFlight, you can collect Crash Logs from your users via the MetricKit framework within your code or ask the user to send you the log via the Settings menu. These latter options require keeping your dSYMs and performing local symbolication manually.

For more information on crash reporting in the Xcode Organizer window, dSYMs, and MetricKit, I recommend the following sessions:

<https://developer.apple.com/videos/play/wwdc2021/10203/>
<https://developer.apple.com/videos/play/wwdc2021/10211/>
<https://developer.apple.com/videos/play/wwdc2018/414/>

These sessions include instructions on how to perform symbolication manually. If you can make use of the Organizer window symbolication is automatic.

|U03HMBPKU0P|:
Thanks a lot! This is great! :+1::skin-tone-2:

|U03HMBPKU0P|:
Just to be sure, <@U03HHPU5NLS>, are reports collected with MetricKit delivered to the organizer window as well? Is it because I’m signed into Xcode with an account belonging to the app’s signing team?

|U03HHPU5NLS|:
MetricKit reports are delivered directly to your application via the framework. They're separate from the Organizer reports insofar that you'll need to upload and process them yourself. The reports you'll see in the Organizer window are coming from the same source as MetricKit (i.e., user devices,) but they are processed by Apple on your behalf and delivered to the Organizer if you are signed into Xcode with a developer account with access to the associated app.

|U03HMBPKU0P|:
Ok, clear, so I can manually deliver them wherever I want, also on my server, am I right?

|U03HHPU5NLS|:
Thats correct. MetricKit will provide you the logs themselves as objects defined in the API, and you can convert them to JSON objects for transport off device.

|U03HMBPKU0P|:
Perfect, thank you very much!!

--- 
> ####  We release builds with a roughly 2-3 week cadence. App Store Connect opt-in metrics show many active devices + sessions each week. When I open the Xcode 13 organizer, I see crash reports but all the metrics views are marked "Insufficient Metrics Data." Is there a way to tell what the internal threshold is? Any tips on getting performance data from the wild? It seems like a very useful feature we've never been able to take advantage of


|U03HHPU5NLS|:
Hi - thanks for the question! It would be great if we could have your apps bundleID to help investigate this. In the meantime, MetricKit is a good alternative to collect data from the wild on iOS and macOS. You'll be able to capture most of the same metrics/reports directly from the framework. For more information on MetricKit check out the documentation here: <https://developer.apple.com/documentation/metrickit?language=objc>

|U03JLV0E8RJ|:
Same exact scenario here :man-raising-hand:
Should we file a FR?

|U03HHPU5NLS|:
Yes please!

|U03J1UX2CQK|:
Thanks <@U03HHPU5NLS>! Here's my Feedback with bundle ID: FB10140258

|U03JLV0E8RJ|:
And here’s mine: FB10148972

--- 
> ####  This is probably very basic, but I have an app that hangs after a few minutes of use and I’m having trouble tracking down the problem. It seems to have some kind of memory leak where some objects aren’t being deallocated as I would expect, but I’m not sure how to resolve that or even if that is the reason for the hang (the app isn’t crashing).


|U03HB5HL8NS|:
Hi Ian,
Memory leaks usually don't cause *long* hangs. A hang occurs when the main thread is not able to process incoming events. This is either because it is busy doing other things or because it is blocked waiting on something else to finish.

This year, we added Hang detection to Instruments. Just choose the "Time Profiler" template in Instruments and profile your app using it. If a hang occurs, it will show up in your process' track, right under the application lifecycle lane (the one showing whether your app is running in the foreground or in the background).

We also published <https://developer.apple.com/videos/play/wwdc2022/10082/|a session >*<https://developer.apple.com/videos/play/wwdc2022/10082/|today>* that goes into more detail about a bunch of tools for tracking down hangs this year.
E.g. when you run your app from Xcode, Xcode will highlight common hang causes as runtime issues. Also, you can enable on-device hang detection in iOS to get notified of hangs while you are using the device and record them directly to export logs/traces and analyze them later. This way you don't need to have your device attached to Instruments all the time if it takes longer to reproduce.

Lastly, feel free to sign up for tomorrow's <https://developer.apple.com/wwdc22/labs-and-lounges/dashboard/TJ7F5J2CSP/dashboard|Performance, power, and stability lab> and we are happy to take a look at this together with you. If you can reproduce the issue and share your screen during the Webex lab we can take a look together.

|U03J4D1FEP6|:
```
This year, we added Hang detection to Instruments.
```

|U03J4D1FEP6|:
Noice. :clap::skin-tone-3:

|U03JK5F8LQ2|:
Excellent. Thanks. To get the new version of instruments do I need the new XCode beta?

|U03HB5HL8NS|:
Yes, exactly, Instruments is bundled with Xcode.
To get the Hang detection (both the one that shows up in Instruments as well as the on-device hang detection), you'll also need to run your app on the new operating systems (macOS Ventura, iOS 16, etc. depending on where your app lives), as only those have the corresponding instrumentation.

--- 
> ####  Hey all, I saw that `Contents/SharedFrameworks/DVTFoundation.framework/Versions/A/Resources/symbolicatecrash` is deprecated and I was wondering if there is a convenient alternative that can symbolicate a whole .crash file in a single command?


|U03HHP75UJF|:
Hi. With new Xcode 14 Beta `xcrun crashlog &lt;path/to/crash&gt;` can be used to symbolicate crashes. To learn more take a look at help page `xcrun crashlog --help`.

|U03JE7PBQPP|:
I should use that instead of `atos`?

|U03HHPUC3K4|:
`atos` can be used for single address to symbol lookup. It is not meant to be used to symbolicate entire files by itself. `xcrun crashlog` is the appropriate tool to use to symbolicate an entire file.

|U03JE7PBQPP|:
Cool! Where's that documented? Last I could find in the Xcode 13.3 release notes was to use `atos` instead of `symbolicatecrash`

|U03HHPUC3K4|:
You can find this in the Xcode 14 release notes: <https://developer.apple.com/documentation/Xcode-Release-Notes/xcode-14-release-notes>

|U03HHPUC3K4|:
Hope that helps :smile:

|U03JE7PBQPP|:
Great! Will <https://developer.apple.com/documentation/Xcode/adding-identifiable-symbol-names-to-a-crash-report> be updated as well? Not that it wasn't fun with `atos` in it but...(it wasn't :smile: )

|U03HB5HL8NS|:
That's a great point Sara, this documentation should definitely be updated. Could you please file a feedback?
If you do, feel free to post the feedback ID here and we'll make sure it gets into the right hands.

|U03K840U3L1|:
Thanks <@U03HHP75UJF>, <@U03JE7PBQPP> &amp; <@U03HB5HL8NS>, That is super helpful

--- 
> ####  Hey all. Is there a tool for parsing .crash files into JSON (or some other format) that's easy to put into a database? We're trying to do some analysis over .crash files we've received in the organiser.


|U03HES3E8DT|:
Recent versions of macOS, iOS, watchOS and tvOS generate crash logs by default in a JSON format. You should be able to create scripts to parse these logs and manage them in a database of your choosing. While new information may be added to these crash logs across releases, the format of existing fields in JSON logs will generally be stable over releases.

If you would like to symbolicate crash logs before processing them further in your script, you may invoke `xcrun crashlog`. See the output of `xcrun crashlog -h` for more information on usage.

We do not offer a tool to convert a .crash file generated by older OS versions to JSON. However, you may refer to the parser in the `crashlog` script to create a solution that works best for you.

|U03K840U3L1|:
Hey <@U03HES3E8DT> thanks. How do I get hold of JSON based crash logs from users? In the organiser I can see .crashpoints &amp; .crash files?

|U03HES3E8DT|:
That's a great question.

JSON crashlogs are currently only available if you have direct access to the device on which the crash occurred. The crash logs can be shared or airdropped from the device, or they can be synced to a mac by connecting the device to a mac. See <https://help.apple.com/xcode/mac/current/#/dev0f3181c2c>

Please file a feedback report requesting access to JSON crash logs in organizer from customer devices.

|U03K840U3L1|:
Thanks <@U03HES3E8DT>, I've filled FB10139088 requesting them in Xcode / AppStoreConnect API.

--- 
> ####  What makes for a great bug report about a hang or performance issues found in beta Apple OSes and apps? What information should be attached to them?


|U03HB5HL8NS|:
Thank you so much for this question and wanting to file great bug reports!
One great option for iOS is to use "Performance Trace." To do so, you install a profile that will add a "Performance Trace" button to control center. You can press it to start capturing a trace. It's limited to 30s, but if you can reproduce the issue during this time it will give us a lot of data on what's going on and why this might be happening.
There are detailed instructions here how to enable it and export the resulting trace file: <https://developer.apple.com/bug-reporting/profiles-and-logs/?name=performance>

Another option that works for all our operating systems is Instruments. If you have Instruments installed, just connect the device you want to profile to your Mac (or target your local mac), then open a new document and choose the "Time Profiler" template. Ideally, choose to record "All Processes," then start recording when your are ready to reproduce the issue. Reproduce the issue and stop recording.
The hang should show up in the relevant process' track.
Then save the Instruments document and attach the resulting `.trace` file to your Feedback report.

--- 
> ####  Is there a good way to simulate or trigger an app termination due to memory pressure?  We're trying to clean up issues with static c++ variables being used after destruction in our app, called by a 3rd party sdk which appears to make use of atexit().  Architectural problems aside, it'd be nice to be able to reproduce the issue reliably.


|U03HES3E8DT|:
You may induce memory pressure during while debugging in iOS Simulator, or while profiling in Instruments. This emulated condition is a good way to test your memory pressure notification handler, and a great way to ensure that state which you are managing using datatypes like NSCache are being released properly.

However this emulated condition doesn't alter your app's memory usage.

It is also worth noting that when the OS chooses to terminate your process for excessive memory usage, the process will not be able to execute any more code. Therefore, the `atexit` handler will not run during this termination condition.

The best way to debug any issues you may suspect in your `atexit` handler in non-fatal termination situations is to call `exit` at an opportune time.

--- 
> ####  My manager wants our apps to use Firebase Crashlytics because it means they can see crashes and get notified through email and slack. But I want to ditch Firebase Crashlytics in all our apps for various reasons.   So is there a way to get the crashes that are displayed in the organizer through command line or some kind of API?  I'm aware of being able to do my own implementation with MetricKit, but we don't want to manage a server where the crashes are collected.


|U03HB5HAZKQ|:
Thanks for the question Rens! This is not something we currently support but please file a feedback with <https://developer.apple.com/bug-reporting/|Feedback Assistant> for us to hear your thoughts! In the meantime, you can go through the file system in the Crashes Organizer to get logs by right-clicking the crash and selecting "Show in Finder". Also feel free sign up for a 1-1 lab tomorrow from 9 AM to 1 PM PST to discuss your use case in depth with us!

--- 
> ####  Is Instruments data collection still memory bound? I've had long running tests crash because Instruments ran out of memory and didn't cache to disk.


|U03HHP75UJF|:
Hi, Sara. Thanks for asking — that’s definitely unexpected. Could you please file a feedback with issue description and sysdiagnose from target and host devices attached. To collect sysdiagnoses please reproduce the issue and follow instruction from <https://developer.apple.com/bug-reporting/profiles-and-logs/>.

--- 
> ####  Quick question: What does IPS stand for? Is there any official documentation of the format?


|U03HL5AMFCL|:
This is a great question — you almost stumped us all, we had to do a bit of asking around to find someone who knew the answer on this one! :laughing:
Apparently it once stood for .ips = "iPhone Submissions" to the crash handling pipeline, but doesn't mean that anymore since these files are used for other platforms now, and most of us had no idea of the original acronym.
As to your question about what's in them — there's some good information on the fields you'll find in these crash reports here: <https://developer.apple.com/documentation/xcode/examining-the-fields-in-a-crash-report>.

|U03HZ39CGAH|:
RIP Stump the Experts. :headstone:

|U03HL553PNG|:
If you like trivia, also check out our Trivia Night starting at 6pm Pacific Time tonight in <#C03HX3KRW2D|>.  If you're not in the channel yet, <https://developer.apple.com/wwdc22/labs-and-lounges/dashboard/upcoming/digital-lounges|sign up now>!

|U03HZ4PT2ER|:
<@U03HL5AMFCL> Thanks, but that link doesn't seem to relate directly to the IPS JSON?

|U03HB5P2UTY|:
The IPS format is a wrapper around a crash log. It adds a JSON header that is not documented. The crash log itself can be either plain text or JSON. The plain text format is documented at the link above, but the JSON format has not been documented. If you're interested in seeing more documentation for these, please file a feedback report.

|U03HB5P2UTY|:
I accidentally dropped the link on that, you can file feedback at <https://feedbackassistant.apple.com>

--- 
> ####  Is there a way to obtain power draw (like powermetrics does on macOS) on iPadOS when profiling an application?


|U03HES3E8DT|:
There currently isn't a tool which reports power usage from an iOS or watchOS device in watts. The Xcode energy gauge offers a way to understand application impact on energy usage based on the app's behavior and an abstract cost semantics.

We typically find this to be effective for many software development scenarios because optimizing based on abstract cost semantics allows developers to follow best practices that are applicable to many devices, whereas exact power measurements are highly specific to the hardware configuration and test conditions of the device under test.

We'd love to hear your feedback on any use cases and workflows that may not be addressed by existing tools.

|U03J07WJLRK|:
Thank you for the answer. The use case of mine is profiling GPU power draw to optimise battery life (per-device tuning configuration).

|U03HES3E8DT|:
Thank you for sharing that use case. Please file a feedback report requesting a tool to profile GPU power usage on iOS devices and share the feedback number.

|U03J07WJLRK|:
FB10138914

--- 
> ####  Wild shot in the dark - I have a SwiftUI app for iPad and Mac Catalyst. It performs no networking or file operations, though it has a simple Core Data store (it's essentially a local card game). It's always performant on iPad. On Mac, most of the time there's a multiple-second beachball hang on launch before it becomes responsive, and animations are laggy. But here's the weird thing - after any OS update, or rebooting in Safe Mode, it becomes performant again for usually about 1-2 days or so. I've profiled it in Instruments and can clearly see the hang, but the deepest stack trace is all SwiftUI system calls, so I have no idea how further to debug. Any ideas for a SwiftUI performance issue that temporarily goes away on macOS after an OS update or Safe Mode reboot?


|U03HB5HL8NS|:
Hi Jason,

at first glance, this sounds like it might be an issue with SwiftUI or even a system performance issue, but it's obviously hard to tell via Slack. We'd like to invite you to <http://feedbackassistant.apple.com|file a feedback> and register for the <https://developer.apple.com/wwdc22/labs-and-lounges/dashboard/TJ7F5J2CSP/dashboard|Performance, power and stability lab> tomorrow, so we can take a look together with you and help you figure out why this is happening and whether it's a bug to file or something you can resolve.

If you can either reproduce the issue during the lab or bring a .trace file, that would hopefully allow us to investigate this together.

|U03J1UAEU4B|:
Thanks!

|U03HB5HL8NS|:
Thanks for signing up for the lab, looking forward to chat with you!

If you can, it would be awesome if you can bring the following: A device with the new macOS Ventura Beta + Xcode 14 beta on which you can reproduce the issue. macOS + Instruments have new hang tracing functionality that will make it easier for us to see what's going on.

However, your question suggests that updating the OS would make the issue go away for a few days so if you don't have a 2nd device to try this on, having the current macOS version + Instruments is totally fine as well. Having a reproduceable issue is more important than being on the betas. We'll just see what we can find together starting from there.

|U03J1UAEU4B|:
Hi, thank you! Yes, because updating the OS would make the issue go away, and I do not have a second device, I will only have my current MacBook Pro running Monterey 12.4, and which the issue is currently happening on. I attached a trace to the Lab request - hope you were able to get it, but I should also be able to regenerate it in the Lab if necessary.

--- 
> ####  :workflowbolt: When debugging Swift, is there anything I could do to fix the following error: error: virtual filesystem overlay file ‘…/iOS-ReferenceData/DerivedData/Build/Intermediates.noindex/ArchiveIntermediates/ReferenceData/IntermediateBuildFilesPath/ReferenceData.build/Release-iphonesimulator/ReferenceData.build/all-product-headers.yaml’ not found. error: couldn’t IRGen expression. Please check the above error messages for possible root causes.


|U03HERYFK6H|:
LLDB's Swift expression evaluator wants to import the Swift module of the current context. Swift modules can depend on Clang modules. In this case it looks like a temporary build product cannot be found during debugging. If you built the code on a different machine, one way to work around this is to turn off search path serialization when building Swift modules. Please check out <https://developer.apple.com/wwdc22/110370> for more details on how to do this!

--- 
> ####  Hi team! A recurring problem for many people with diverse code bases seems to be a symptom in which lldb running under Xcode just "hangs". What can we do to gather pertinent debug information in this scenario and help either the open source lldb teams, or Apple, to address the issues that are affecting us?


|U03HWCZC2HX|:
The first thing to determine is whether this is indeed a hang or a (very) slow operation. The easiest way to do that is probably to sit back and see if it completes within a "reasonable" amount of time (e.g. 10 minutes). The best way to help us figure out what's causing this is to grab a sample of `lldb-rpc-server` with the following command `sample lldb-rpc-server 60` . The last value specifies the length of the sample in seconds, which you'll want to make as large as possible. Please file a feedback report with that information and post the number here so we can follow up.

|U03HZ39CGAH|:
Thank you!

--- 
> ####  In a modular project setup that has subprojects that generate frameworks, are there any recommendations/suggestions around a number of these subprojects that generate dylibs might be? I’ve heard of suggestions of staying near 6 (pre-dyld3) but am curious if there is still a suggested ceiling with all of the recent improvements over the past few years?


|U03HHP75UJF|:
Hi, Jordan.
That’s a good question. We don’t have a specific recommendation on the number of dynamic libraries. As a rule of thumb it’s always a good idea to measure performance and bottlenecks first and make a decision based on results. App Launch template in Instruments can be used to profile app launch time on multiple physical device configurations. Don’t forget to avoid using any `DYLD_*` environment variables during these tests. To learn more about dyld enhancements this year: <https://developer.apple.com/videos/play/wwdc2022/110362/>

|U03JS1GFH44|:
:+1:

|U03J0QXPYA3|:
This was a fantastic video and lead me to ask the question! I always cautiously approach the idea of adding a new subproject that needs to generate a dylib, and it can be a lot of work to walk it back if it were to have devastating effects :joy:

Thanks for the answer. Where my thought pattern was headed (too much variability involved) but the confirmation is helpful.

--- 
> ####  Hey there! I am looking to understand what kinds of things I can do with lldb in terms of  stack trace recording, demangling, and making things human readable. This is a long, wild shot in the dark: is it at all possible to 'record' the execution of a running application, persist it to a file or series of files somewhere, and then use it as a backing store to piece through the execution trace? There are tools like <https://github.com/johnno1962/SwiftTrace|https://github.com/johnno1962/SwiftTrace> that are extremely powerful, but a bit difficult to clear up data and get specific information ordering, timing, etc. Very curious what tools might already exist for this use case, as I'm sure this isn't a new idea, haha!


|U03HWCZC2HX|:
We currently do not support recording or examining execution traces. You could use LLDB's scripting capabilities to record arbitrary information at certain points with the help of breakpoints callbacks and implement your own tracing framework that way, but it would be less efficient than modifying the code, if that's an option. Here's a link to LLDB's scripting documentation: <https://lldb.llvm.org/use/python.html>

|U03HES2QKGV|:
Another option is to use Instruments to profile the app, specifically the "Time Profiler" template. This will collect samples at fast rate.

|U03JRPTDF6U|:
Thank you so much for the feedback!

I'm curious: since it seems there is plenty of tooling for code generation, do you think it's a reasonable path to use something ‘swift-syntax’ to rewrite or regenerate a set of code files that have tracing calls by default? For example, I have a sample that simply adds a function call to a log output, which I can then write to disk and read. Using macros like #line and #file, I can pseudo trace execution and args. Should I explore that more do you think?

<@U03HES2QKGV> , thank you! Is the profile trace a manageable file format of some kind that I could parse out and read from? Even it requires manual formatting, that would be a wonderful solution to experiment with!

|U03HES2QKGV|:
Instruments has some ability to get at the data, though I think it will require some investigation, trial and error. Instruments has a command line tool, `xcrun xtrace`. One of its subcommands is `export`.  If you run `xcrun xctrace export` without any arguments, it will print the help. The help shows you that you want to look at the `--toc` (table of contents) and then from there select which data to export using the `--xpath` flag. Beyond this, check if there are labs or Q&amp;A with Instruments engineers to get more help.

|U03JRPTDF6U|:
That’s fantastic, I’ll give the CLI a good scouring tonight. I appreciate the heads up on the arguments and expected output, puts a nice target at the end of the tunnel :smiley: There’s an Xcode open chat tomorrow, and I’ll see if I can find specific tooling ones too.

|U03HES2QKGV|:
Good luck I hope you get the data you're looking for!

--- 
> ####  We're starting to see some app hangs already come in from the Xcode Organizer!  Do we know how far back has the system been tracking this information?


|U03HB5P2UTY|:
Great to hear that you're already finding the new hang reports useful! You should be able to see data collected over the last two months from devices running iOS 14.4 or newer.

Also in case you want more information about investigating hangs in your app, please check out <https://developer.apple.com/wwdc22/10082|Track down hangs with Xcode and on-device detection> from this morning.

|U03JGKAB4AE|:
Thanks!

--- 
> ####  I've found at time that pausing at a breakpoint in Xcode is really slow to update the variables views, etc. (10-60 secs at times), which makes debugging difficult and breaks the workflow. Has there been improvements to Xcode on that front?


|U03HERYFK6H|:
We made several performance improvements in Xcode 14. If it's still slow in the Xcode 14 beta, could you run `sample lldb-rpc-server 60` in Terminal before launching your process and send a Developer Feedback our way?

|U03K3R7RP6C|:
Sounds good - I’ll keep an eye out with XC14 and keep the sampling in mind if it ever happens. Thx!

|U03HERYFK6H|:
Bridging headers can also contribute to slow debugger startup times. In contrast to Clang Modules, bridging headers cannot be cached, so LLDB has to recompile them from source every time the project launches. Where possible, we encourage migrating to Clang Modules.

|U03J22A0C4S|:
How would you migrate? Those seem to be two different things?

|U03HERYFK6H|:
You can define new Clang modules by putting a `modules.modulemap` file into the same directory that describes which header files form one module. Then you can importthem in Swift with `import ModuleName`.
<https://clang.llvm.org/docs/Modules.html>

|U03HERYFK6H|:
In the simplest case something like:
```
module MyModule { header "MyHeader.h" }
```
would be sufficient.

|U03HERYFK6H|:
If you put this module map next to `MyHeader.h` you will be able to import it into Swift via `import MyModule`.

--- 
> ####  I’ve been SO busy this week!  Is there a plan for capturing all of the Q&amp;A across all of the Digital Lounges so I can read through them after WWDC closes?


|U03HL553PNG|:
I'm glad you're enjoying WWDC!  I can relate to it being a busy week. :slightly_smiling_face:  I just checked and yes, the Slack Digital Lounges will remain open and read-only for a little bit after this week so you can still read through everything.

|U03J21ZK802|:
Someone did a nice thing and captured last year's, recent repo history suggests they may be prepared to do the same this year. <https://roblack.github.io/WWDCLounges/>

|U03J201SFAP|:
BUMP!!!

--- 
> ####  When writing concurrent swift, I find methods and variables tend to get sucked into @MainActor  I mark one @Published variable as @MainActor, then any function which writes to that needs to be @MainActor and so on  However - this even includes functions which only _read_ from my @Published variable.  I feel like there should be a pattern where I can read various state from different threads - and only need to constrain my writing to @MainThread.  Is there an idiomatic way to approach this so as to minimise the functions which have to be @MainActor ?


|U03H36UD5QX|:
I presume the question that it is not really a Combine issue but more so an issue generally about constraints about `@MainActor`.  You can mark whole types as `@MainActor` but it often infers that if you are needing to mark things included into that grouping of only being bound to the main actor with things that clearly don't belong in that isolation group. It means that those functions that do that work may be better suited for their own type that is outside of that isolation.

|U03J214MNE6|:
It's more that I mark something as @MainActor because it needs to be written on @MainActor - but that inevitably sucks in any dependant functions.
However if those functions are only reading the attribute, then they could (probably??) do that safely on any thread.
Is there a way to split out the all or nothing nature of @MainActor into read/write ?

--- 
> ####  I just wrote the following code:      private func monitorCoordinatorUntilWeCanConnect() {         coordinatorMonitor = appCoordinator.objectWillChange             .receive(on: DispatchQueue.main)             .sink(receiveValue: { [weak self] _ in                 DispatchQueue.main.async {                     self?.checkOnboarding()                 }         })     }  checkOnboarding() is marked as @MainActor, so the compiler insists that I call it from DispatchQueue.main  However I have already asked combine to deliver any notifications on the main thread   .receive(on: DispatchQueue.main)  so the DispatchQueue.main is actually not needed  Is there any way to say  //Hey - I promise I'm on the main thread already!  More generally, when writing a function which always calls a block on the main thread, is there any way to annotate it so that the compiler knows that it can stop stressing?  e.g. I have a WeakTimer class which works much like Timer except that it automatically invalidates when released, and always calls the block on the main thread.  It would be great if I could tell the compiler that          let timer = WeakTimer.scheduledTimer(withTimeInterval: 1, repeats: true) { timer in             //This is guaranteed to be on the main thread already         }   something like   public class func scheduledTimer(withTimeInterval interval: TimeInterval,                                      repeats: Bool,                                      block: @escaping @GuaranteedMainActor (Timer) - Void) - WeakTimer   is there anything like that available, or is DispatchQueue.main just special-cased in the compiler?


|U03H36UD5QX|:
There is an attribute for that `@MainActor(*unsafe*)` that tells the compiler; "Yes, _I_ know it is on the main actor and I take the safety of that into my own hands"

|U03J214MNE6|:
Thanks for the response, can you point to any docs on `@MainActor(*unsafe*)` vs `@MainActor`

|U03J214MNE6|:
with some further experimentation, it seems I can do the following:

&gt; `*class* MainRunner {`
&gt;   *`static`* `*func* run(_ block:*@escaping* @MainActor ()-&gt;Void) {`
&gt;     `DispatchQueue.main.async {`
&gt;       `block()`
&gt;     `}`
&gt;   `}`
&gt; `}`
&gt; 
&gt; `*class* Foo {`
&gt;   `@MainActor *var* bar:String = "start"`
&gt;    
&gt;   `*func* modify() {`
&gt;     `MainRunner.run {`
&gt;       `*self*.bar = "Test"`
&gt;     `}`
&gt;   `}`
&gt; `}`

--- 
> ####  I need to access an async property in a synchronous delegate method. Example: I have an async Bool I need to access to determine what to return in application(_:shouldRestoreApplicationState:) - Bool. How can I do so?


|U03HWDD6RED|:
Hi! So:

|U03HWDD6RED|:
when you’re inside a synchronous method, in general, you cannot suspend for an `async` thing — this lets the caller know that you will complete this call on the same thread and prevents it from blocking.

|U03HWDD6RED|:
do you need the boolean to return the value from this method? (and, if you can share: where is this boolean coming from?)

|U03HMDG985D|:
I’m working on a refactor of an app where we use Realm. You can’t work with Realm objects across threads, and trying to do so across Tasks has been a headache. So I’ve started refactoring with a custom global actor so all access to Realm is controlled. Unfortunately, this means all access to this property is now async.

In our case, this is a user object, and I’m trying to see if `ourAppState.user == nil` as part of our logic for state restoration.

|U03HMDG985D|:
(So it’s not really an async bool now that I think about it; it’s accessing a property that’s marked with an actor and has become accessible only via async.)

|U03HWDD6RED|:
There is little difference between the two, which is why `await` is used — both require a potential suspension point to occur.

|U03HWDD6RED|:
You may need to think about how to cache this value before transferring the Realm objects to the actor, or work with the Realm objects on the main thread instead.

|U03HMDG985D|:
Right now, I have annotated the property itself with `@OurActor`.

|U03HMDG985D|:
Can that be a computed setter/getter? In other words, if I refactor it as such in our state controller:
```
@OurActor
var user: User? {
  get { localUser }
  set { localUser = $0 }
}

private var localUser: User?
```
Would that be legal?

|U03HWDD6RED|:
It would, but you still need to `await` it — presenting the same problem. There’s no way to both perform an `await` in the body of that method _and_ return that value synchronously, on purpose: because a task cannot block a synchronous call.

|U03HMDG985D|:
In my case, presumably I don’t need to worry about the actor just to check if the user is nil.  (Hopefully it’ll just check the Optional and not the Realm object itself), so just checking if `localUser` is nil might be enough in this case, while still having the public `user` var to use in a thread safe way.

|U03HWDD6RED|:
You may find that the code you wrote doesn’t compile, and requires you to `await`, unless the `localUser` variable is a global variable, which is generally not a kind of pattern that is encouraged.

|U03HWDD6RED|:
in this case, serializing access through your global actor doesn’t do much anyway.

|U03HWDD6RED|:
I bet that some 1:1 time at a lab would help — if you want to go a little deeper, there is a Swift open hours lab tomorrow you can sign up for today.

|U03HWDD6RED|:
(in a lab context, you can share your screen and code with an engineer, which makes this kind of question a lot easier to deep-dive in.)

|U03HMDG985D|:
If I end up failing miserably with this approach, I’ll definitely book some time. Thank you for your time right now, <@U03HWDD6RED> :smile:

--- 
> ####  Is there a way to tell URLSession/ background processing that a set of uploads are critical and shouldn't be killed across force quit?


|U03J2SE7Z97|:
Unfortunately, no. Force quitting (swiping up from app switcher) implies the user intent that all background activities associated with the app should stop.

|U03JHE95K7C|:
okay, so my popup notification in appWillTerminate is really the only way to (hopefully) get the user to continue the app - got it, thanks

|U03JHE95K7C|:
coincidentally, just finished watching your video on async/await with URLSession :slightly_smiling_face:

|U03J2SE7Z97|:
Hope you liked it

|U03JHE95K7C|:
yeah, going to start experimenting with concurrency, so the video was a great starting point, looking forward to cutting my 1200 line objective-c file in half or better

--- 
> ####  What is the preferred way to set a basic auth header in a URLSession request? I have been setting the 'Authorization' header of the URLRequest, but according to the docs this is reserved header and should not be set (<https://developer.apple.com/documentation/foundation/nsurlrequest#1776617).|https://developer.apple.com/documentation/foundation/nsurlrequest#1776617).>  ``` extension URLRequest {     mutating func setBasicAuth(username: String, password: String) {         guard let basicAuthData = "\(username):\(password)".data(using: .utf8)?.base64EncodedString() else { return }         setValue("Basic \(basicAuthData)", forHTTPHeaderField: "Authorization")     } } ```


|U03J2SE7Z97|:
URLSession supports HTTP authentication. If you implement `didReceiveChallenge` delegate method, URLSession will call it to request a credential when a 401 response is received, then we will generate the appropriate `Authorization` header for the particular type of authentication challenge when sending a new request.

However, if you want to manually handle authentication, adding an `Authorization` header to the request is allowed.

--- 
> ####  Are there built in facilities for parsing user-generated URLs? I've found URL and URLComponents are both generally too strict for user-provided content when it comes to expecting the string inputs to be perfectly spec-compliant. Right now I'm using a (wonderful) 3rd party Swift package that implements the WHATWG standard (which is significantly more forgiving), but it seems like something that's common enough that it should be builtin.


|U03HBMCMGBG|:
Hi <@U03J4DRK4SY> ! This year Foundation introduced `URL.ParseStrategy` (alongside with `URL.FormatStyle`) that can be used to parse user generated URLs. You can use it the same way as the rest of Foundation's parse strategies. Here's an example:
```
let strategy = URL.ParseStrategy()
    .scheme(.defaultValue("https"))
    .user(.optional)
    .password(.optional)
    .host(.required)
    .port(.defaultValue(8080))
    .path(.optional)
    .query(.optional)
    .fragment(.optional)
let text = "<http://www.watermelon.com/about|www.watermelon.com/about>"
let url = try? strategy.parse(text) // <https://www.watermelon.com:8080/about>
```

|U03HBMCMGBG|:
This new `ParseStrategy` supports Internationalized Domain Names (IDNs), so you'll be able to parse URLs like this:
```
let strategy = URL.ParseStrategy()
// Yes, these are real URLs in use :P
try? strategy.parse("https://👁👄👁.fm") // returns an URL instance
```

|U03HWDD6RED|:
The parse strategy doesn’t implement the full WHATWG standard, but it can be used as a start if all you have access to is built-in functionality. If you’d like the standard to be in Foundation, the best way to track that is to send us a feedback!

|U03J4DRK4SY|:
Ooh, that's very interesting, IDNA support was a big problem for me before! Can the ParseStrategy handle escaping invalid characters in the URL?

|U03J4DRK4SY|:
The other big issue I've encountered in the past was that some remote services will produce technically malformed URLs (e.g., with an "ü" in the path but not percent-encoded) that make their way to my app and that I need to be able to parse

|U03HBMCMGBG|:
&gt; Can the ParseStrategy handle escaping invalid characters in the URL?
Yes! For example
```
try? URL.ParseStrategy().parse("<http://見.香港/热狗/🌭>")```
should return an URL instance like this:
```<http://xn--nw2a.xn--j6w193g/%E7%83%AD%E7%8B%97/%F0%9F%8C%AD>
```
Note that the host name has been automatically Punycode encoded while the path has been percent encoded.

|U03J4DRK4SY|:
:heart_eyes: That is fantastic!

--- 
> ####  With combine, when do subscribers' closures get called? and in what order are the subscribers called with respect to each other?  suppose i have a publisher that sends values on main. will the subscriber's closures get called before `Publisher.send()` returns? or does `send` just schedule the subscribers to receive events on the same queue as the sender?


|U03H36UD5QX|:
Values are delivered as soon as there is demand for them; this means that when demand is applied to a publisher any values that would be sent would be emitted immediately (barring any scheduler based operator in the chain). In short; if there is demand, you can rely on delivery to be synchronous.

|U03HWEGHRKR|:
is there anyway to order the subscribers? I assume that if a value is sent on the main thread, not all subscribers can receive the value at once.

|U03H36UD5QX|:
how do you have more than one? are you using share?

|U03HWDD6RED|:
If you are using something like `share()` or a built-in Subject type to share the value among multiple subscribers, none of those in the Combine framework proper have guarantees for the order in which subscribers receive the value.

|U03HWEGHRKR|:
maybe i’m not understanding combine correctly :sweat_smile:
something like this
```
@Published var myVar: Bool = false

let sub1 = $myVar.sink {
    print($0)
}
let sub2 = $myVar.sink {
    print(!$0)
}
```

|U03HELZ3GUD|:
first subscription with `sub1` would subscribe and request it’s demand and receive the current value (cause `@Published` is like a `CurrentValueSubject` ) then `sub2` would subscribe and request it’s demand and get the current value. After that the subscriptions (so long as both `sub1` and `sub2` have an active reference count) would be setup so only demand and values would be exchanged

|U03HELZ3GUD|:
does that make sense?

|U03HWDD6RED|:
In general, though, there’s no real guarantee which closure will fire first once that first pass is done.

|U03HELZ3GUD|:
Right

|U03HWEGHRKR|:
makes sense. thank you both :smile:

--- 
> ####  Taking off Tony's question, is there actually a replacement for the Virtual Keyboard enum in Carbon? (`kVK_Escape`...)


|U03HL5K6L04|:
Ok I actually don’t know the answer, but since this is my fault I thought I should say that in public here. :slightly_smiling_face:

|U03HL5K6L04|:
I will say however that some of the engineers over in the uiframeworks-lounge have been around long enough to probably know if you’re curious!

|U03HZ4PT2ER|:
e.g. what would you do in Swift if you needed to map the key code of an `NSEvent`?

|U03HWDD6RED|:
pssst: <https://developer.apple.com/documentation/uikit/uikeyboardhidusage> for Catalyst and iOS

|U03HZ4PT2ER|:
<@U03HWDD6RED> I should have mentioned macOS

|U03HWDD6RED|:
oh! easy: that’s <https://developer.apple.com/documentation/appkit/nsevent/specialkey/>

|U03HWDD6RED|:
which I definitely wasn’t googling around for while you were reading my previous message :stuck_out_tongue_winking_eye:

|U03HWDD6RED|:
(of course, these aren’t the only ways you can map a key to its intent; UIKey and NSEvent are your starting points there.)

|U03HWDD6RED|:
(and UIKeyCommand. But the UI Frameworks labs and lounges definitely can be more helpful there!)

|U03HZ4PT2ER|:
I still use `kVK...` , old habit

--- 
> ####  Do you have any favorite, oft-overlooked features of Foundation?


|U03HL5K6L04|:
Good question!

|U03HL5K6L04|:
I like `Progress` — it’s a lot more than just a double or a fraction

|U03HZ4PT2ER|:
To that I would add all the implicit `Progress` you get for free, even in `NSImage`

|U03HL5K6L04|:
And it has a *lot* of totally localized output that you can take advantage of

|U03HWDD6RED|:
`Scanner` is a little old-fashioned — and certainly a lot of what it does can be done with the new Swift Regex support these days — but sometimes your state machine really needs to be coded by hand :stuck_out_tongue:

|U03HWDD6RED|:
also, I’m biased, but that beautiful `AttributedString(markdown:…)` initializer is just splendid.

|U03HELZ3GUD|:
for me personally it would be the objc literals like `@[ @1 ]` etc

|U03JSFUKL2U|:
Each example with mapping hex to UIColor using `Scanner`

|U03HZ4PT2ER|:
Using `NSExpression` to let users type in basic math in your fields

|U03HELZ3GUD|:
reminder that you can use `plutil -convert objc myPlist.plist` to convert it to a compile time constant `.m` file to avoid disk overhead

|U03JSFUKL2U|:
`NSClassFromString` and `NSStringFromClass`  my favorites!

|U03J5J0TUN9|:
`Formatter` is powerful and underused. Wish SwiftUI would use more of it. (and that the newer formatters would properly support parsing and partial parsing, not just rendering)

|U03HJ9NBK7D|:
`CFRunLoopSourceRef` for me

|U03HESGTE77|:
Also biased, but I think all the ways you can access, mutate, and enumerate attributes on `AttributedString` is pretty neat, with some really nice syntax.

|U03HL5K6L04|:
A lot of great advances in parsing strings this year with swift RegEx, but happy to accept feedback on more

|U03HL5K6L04|:
(including integration with the FormatStyles/ParseStrategies in Foundation)

|U03H3NETJ7R|:
I think all of our formatters, even the more obscure ones, are really cool! Especially the format styles that allow you to receive an `AttributedString` as output

--- 
> ####  Should I use Foundation in SPM package for non Darwin platforms?


|U03HWDD6RED|:
A subset of Foundation is available in Swift for Linux and Swift for Windows. Some functionality is Darwin-only, but much of what you’ll need is there — URL, FileManager, AttributedString, UserDefaults, NotificationCenter and more. Make sure you test your package with Swift on the platform you want to run it on, as there are some important differences.

|U03JSFUKL2U|:
Thats great news! if I understand it right, I can see sources in apple/swiftfoundation repo?

|U03HWDD6RED|:
Correct! Check out <http://github.com/apple/swift-corelibs-foundation|github.com/apple/swift-corelibs-foundation>.

|U03JSFUKL2U|:
Thank you a lot!

--- 
> ####  The session on creating a Swift package plugin mentions no network access for plugins. Is this functionality coming in a future release by chance, or should I create a feature request? I'd love to move our theming code into a package, but we utilize a JSON payload from a web service call in order to do our code generation.


|U03HES7M40M|:
Great question. Generally swift plugins are running inside a sandbox for security reasons. This sandbox does not allow network access. If you have created your own plugin, which you trust, you can disable the sandbox on the command line with:

```
swift package --disable-sandbox yourPluginName
```
If you are interested in allowing plugins specialized network access while not disabling the sandbox, we would encourage you to post a swift forums thread describing your use case.

|U03HVE965FY|:
is there any option to "trust" a plugin through xcode to disable the sandbox through the xcode integration?

|U03HES7M40M|:
This is not possible today… If this is something that is needed for your use-case we encourage you to open a feedback.

|U03KVQZRF7S|:
Ah, okay. Sounds like I can still accomplish what I want, I'll just need to disable the sandbox. I'll give that a shot then, thank you!

|U03HES8111T|:
Please do file a bug at <https://feedbackassistant.apple.com/> or post in the open source Swift forums with the specific requirements, e.g. whether you'd want an allow-list of hosts etc.

|U03HES8111T|:
It would be useful to better understand the requirements so you wouldn't have to disable the whole sandbox.

|U03KVQZRF7S|:
I will do that. The basic requirements are that we have a public endpoint that provides a JSON payload that we use to generate code for theme information as part of our deployment process. We deploy an app for ~600 financial institutions in the U.S. and release once a month. An allow-list would be a great solution for this.

|U03KVQZRF7S|:
I do have a good majority of our generated theme code in a local package already, but have been using a project pre-build script to accomplish what I need. I'd like to make the package remote at some point, because we have additional frameworks that we currently inject the Theme info into, and I'm keen to stop doing that. :smile:

|U03HES8111T|:
Thanks for the additional information.  I think the balance here that will be interesting to find will be to allow only as much network access as you need while keeping the rest of the sandbox in place.

--- 
> ####  Hi! During the Meet Swift Package Plugins session yesterday, in the code section of the Apple Developer App, there seemed to be a code example showing how to add support for an XcodeBuildToolPlugin, but that protocol doesn't seem to be available in the Xcode 14 beta.  Does this mean there will be support for Build Tool Plugins (not Command) to be run as part of an Xcode build? If so, how would we define the build step?


|U03HES8111T|:
Thanks for the report.  The absence of this protocol in Beta 1 is a known issue that is mentioned under the Known Issues at <https://developer.apple.com/documentation/Xcode-Release-Notes/xcode-14-release-notes>.

|U03HZ4JN7D3|:
Awesome! Thanks <@U03HES8111T> :raised_hands: Am I right in thinking we’ll be able to use build tool plugins in Xcode projects? As in, just run them as a build phase or something similar? 

--- 
> ####  When using swift package manager for local packages, where is the best place to put them? Inside the project? Inside the workspace? Somewhere else?


|U03J7H4LJ5N|:
In terms of where to put them on the filesystem, adding them at the top level of your existing project makes the most sense so that it can become part of your existing SCM repository. For the references in Xcode, it depends a lot on the general organization of your project. If the given package is used by a single project, I would add it to that, but if it is used broadly across a larger workspace, it rather belongs there.

|U03K1K31P8Q|:
One of the issues I run into constantly is opening the same workspace from different repo copies that have local SPMs referenced but Xcode tells me that the second opened copy of the workspace can’t open our local packages. Not an issue for the ones added from GitHub. Is there a way around this?

|U03HES8111T|:
I believe this is an Xcode limitation, and unfortunately I don't know of a way around it.

|U03K1K31P8Q|:
Thanks!

--- 
> ####  Hi,  In a xcworkspace that have client and backend code. Most of client and backend code live in SPM modules. Why when I compile only the client xcode build the server executable too ? Knowing that the App depend only the client code. The project is available in this feedback FB10115879.  Thanks,


|U03H36Q9X3R|:
Building the client should not also build the server code, unless there is a dependency defined between the client and the server modules. Thank you for filing the FB10115879, we will provide additional details there.

--- 
> ####  Hi! I'd like to start using SPM in a bigger project for app development. Right now we cannot use a Package.swift for this purpose and instead need to rely on Xcodes SPM version. This becomes problematic in code reviews where it would be beneficial to have a clearly readable Package.swift update for dependencies instead of some XML file. Is there any way to handle this differently? E.g. adding a separate project inside the same workspace which is using a Package.swift and contains all dependencies. Then using that package within the actual project?


|U03HL5FTVHS|:
Hi Roland! Generally I would recommend using using local Swift package dependencies as a solution for this problem.

There’s some documentation on <https://developer.apple.com/documentation/xcode/organizing-your-code-with-local-packages|that workflow here>.

--- 
> ####  Can build plugins run on Xcode projects?  I have seen the following code in material for WWDC session "Meet Swift Package plugins" but the code does not compile because  `XcodeBuildToolPlugin ` does not exist  ```swift #if canImport(XcodeProjectPlugin) import XcodeProjectPlugin  extension MyPlugin: XcodeBuildToolPlugin {      /// This entry point is called when operating on an Xcode project.     func createBuildCommands(context: XcodePluginContext, target: XcodeTarget) throws - [Command]         debugPrint(context)         return []     } } #endif ```  Same question raised in Developer forum: <https://developer.apple.com/forums/thread/707813|https://developer.apple.com/forums/thread/707813>


|U03HVE01C6A|:
Answered in <https://wwdc22.slack.com/archives/C03H0JN431U/p1654877196432919>

|U03HVE01C6A|:
Answer

&gt; Thanks for the report.  The absence of this protocol in Beta 1 is a known issue that is mentioned under the Known Issues at <https://developer.apple.com/documentation/Xcode-Release-Notes/xcode-14-release-notes>.


|U03HES7M40M|:
Awesome!

--- 
> ####  Adoption of `XcodeProjectPlugin` in Swift package plugins is needed so that command plugins (and build plugins in future betas) can run on Xcode projects? Existing command plugin(s), used in a Swift package (!!!), can be triggered interactively in Xcode 14 out-of-the-box. Is that right?


|U03HES8111T|:
Yes, that is correct:  existing command and build tool package plugins can be invoked on regular packages, but in order to be invoked on an Xcode project, a package plugin has to use APIs from the XcodeProjectPlugin module.  This module extends the APIs in the regular PackagePlugin module.

|U03HES8111T|:
This is described in a little more detail here:  <https://github.com/apple/swift-package-manager/blob/main/Documentation/Plugins.md>

--- 
> ####  Hi! Many thanks for creating Swift Package plugins API, it seems to be very powerful, looking forward to using those integrations for dev tools. I was wondering if it's possible to use Swift Package plugins in ordinary .xcodeproj (non-SPM) projects too?


|U03J7H4LJ5N|:
Yes, there’s a separate XcodeProjectPlugin module for this in Xcode 14. Using this API, the plugin can get a simplified description of the Xcode project’s structure. This lets adapted plugins running in Xcode take advantage of using this API on Xcode projects. You can still use packages that haven’t been adapted to support Xcode projects with Swift Packages in Xcode. Note that build tool plugins that are adapted to support Xcode projects can’t yet be used with Xcode targets to generate source code and resources. This functionality is expected to be available in a future Xcode 14 beta.

|U03JRQGTPB2|:
Sounds great, thank you very much for the answer!

--- 
> ####  I've seen references of folks online moving almost all their app code into a Swift Package, and then having the App module/top level Xcode project just contain a little configuration (build phase scripts, etc). The AppDelegate lives in said Xcode project and just calls into this jumbo swift package where most of the app lives. As most files aren't in the xcodeproj file they avoid merge conflicts here. What do y'all think of this approach/does anything jump out around launch time/linking or SPM gotchas?


|U03J7H4LJ5N|:
Typically, this will have no effect on launch time since the packages will get linked statically and can be a reasonable approach to organize your project.

|U03JELM0ZNV|:
Does this approach have any implications for code reuse between an iMessage app extension and main app? I have about 20KLoC in core logic am about to start sharing with my main app.

Also, as an old-timer, :smiling_face_with_tear: at seeing the dogcow avatar.

--- 
> ####  After that the great talk around linking static and dynamic libraries, how does SPM decide on how package products are built? If our 3rd party libraries don't explicitly specify dynamic or static in their package manifest, will it just "work it out" (like building them dynamically if a lib is used by two packages Or statically if it's only used by one or just the app target)?


|U03J7H4LJ5N|:
If a product does use automatic linkage, Xcode will built it dynamically depending on the situation, e.g. if it is used by the app and its test bundle — this is done to avoid duplicated symbols at runtime. In `swift build`, automatic products always build statically.

|U03KC4368BS|:
Is there any way to have more direct control of this? Or visibility into why the build system chooses one over the other in case we want different behavior?

|U03J7H4LJ5N|:
There’s no way to control it from the client side, only the `type` parameter on the package product itself.

--- 
> ####  I have another question for Xcode Q&amp;A. I have a simple iOS app with one heavy SPM dependency — Firebase.  When I switch between branches/commits, Xcode starts to “Resolving package graph” and use 100% CPU, next “Preparing editor functionality” and use 750% CPU. It freezes my Mac for about 10-15 seconds and I can’t do anything.  So, I have 3 questions: 1. Why Xcode starts to “Resolving package graph” when I switch git branch/commit? Is it a bug? 2. Why Xcode uses 750% CPU when doing “Preparing editor functionality”? Is it a bug? 3. It seems that Xcode rebuilding SymbolCache when I change git branch/commit. Is it a bug?  I’ve found two issues posted of the developer forum that describe my issue: <https://developer.apple.com/forums/thread/699112|https://developer.apple.com/forums/thread/699112> <https://developer.apple.com/forums/thread/694058|https://developer.apple.com/forums/thread/694058>


|U03HES8111T|:
Switching branches can cause packages to be re-resolved if there are any changes to the package manifests or to the structure of the packages.

That said, this shouldn't cause the user interface to become unresponsive.  This sounds like a bug, and it would be very appreciated if you could file a bug report at <https://feedbackassistant.apple.com/> and attach a sysdiagnose that shows what Xcode is doing that is taking so long.

|U03K1K31P8Q|:
I’ve been seeing this issue for many versions of Xcode 13. The “resolving package graph” also is very slow to cancel, if it works at all. Xcode 14 beta 1 is performing similarly for me.

|U03J22A0C4S|:
Yeah, Xcode 14 has all of the same, existing SPM bugs, including breaking package resolution when switching branches or even when just resetting the project file on the existing branch with Xcode open.

|U03J22A0C4S|:
In none of these cases have the dependency versions changed.

|U03HZ3N1QFP|:
Definitely see this too

|U03HES8111T|:
Thanks for the additional reports.  If you have already filed feedback tickets, would you mind pasting the numbers here so we can follow up?  Having sysdiagnoses for these cases to see what Xcode is doing would be really helpful in trying to track it down.

|U03HZ3E3GTF|:
<@U03HES8111T> I've filed a feedback and attach a sysdiagnose file. Ticket FB10163853. Hope this issue will be fixed in the release version of Xcode 14. If you need additional info contact me.

|U03HES8111T|:
Thank you very much!  The attached project and sysdiagnose are really helpful!  I'll make sure this gets routed to the right team, and will reach out through the feedback system if there is anything else that would be helpful.  Much appreciated!

--- 
> ####  Pre-compiled version of module(s) in Swift package are often preferred by consumers. Are there out-of-the-box capabilities by Apple SPM or other tooling which help with that?


|U03HES7M40M|:
SPM offers the option to consume pre-compiled xcframeworks. Learn more about it here:

<https://developer.apple.com/documentation/xcode/distributing-binary-frameworks-as-swift-packages>

|U03HVE01C6A|:
My question is different. Swift packages are compiled by consumers in their app. Ideally the same package could be provided as xcframework.

Prominent example is Firebase iOS SDK which is open-source and made out of various Swift packages. But they provide for each target(s) in their Swift packages a binary framework. Firebase is achieving this through a custom build process involving CocoaPods

Ideally Apple would provide capabilities of converting a Swift package to binary framework(s) to speed up compilation time for consumers

|U03HES7M40M|:
This sounds like an interesting use-case. Would you mind opening a feedback for this?

|U03JGH0JLMQ|:
ooi which Feedback category would be appropriate for something like this?  I’m not sure if it would be “Swift compiler” or “pkg Authoring” or something else

|U03HVE01C6A|:
I created FB10160671. Please change category (Swift compiler) if necessary.

<@U03JGH0JLMQ> please create a feedback as well as it bumps up visibility :)

--- 
> ####  I see in the Package Plugins session that in case it needs disk access it will show a pop up to the user. Can this be managed programatically? (Thinking of running some of these tools in the CI) Does disabling the sandbox automatically enable write access to the disk too?


|U03JE2LACMS|:
I wrote this question as I was watching the video. Just 2 minutes after sending it I saw that you can pass in a parameter to enable write access :man-facepalming:

|U03JE2LACMS|:
`--allow-writing-to-package-directory`

|U03HES7M40M|:
The better options here is to allow only write access to your files instead of disabling the sandbox altogether. This will ensure the plugin still can not communicate with the network/internet, but allows write access…

```
swift package --allow-writing-to-package-directory yourPluginThatWantsWriteAccess
```
You can learn more about it here:
<https://github.com/apple/swift-package-manager/blob/main/Documentation/Plugins.md>

|U03JE2LACMS|:
Thanks a bunch Fabian

|U03JE2LACMS|:
really looking forward to migrating to plugins

--- 
> ####  Xcode SPM Question:   I am trying to run server side app in the `Release` build configuration, and I have test cases in which I am importing the app using  `@testable import App`  When I try to run it is giving   `Module 'App' was not compiled for testing`. Is this a bug or I am missing some configurations? 


|U03HES7M40M|:
Hi! It sounds like you try to run your tests in release configurations to test/measure performance. In those cases we recommend setting up two different test targets:

1. One for unit tests that uses `@testable import App`
2. One for performance tests that does not use `@testable`. 
This way you can only run the performance test target with release configuration.

Is this a workable solution for you?

|U03JRQ81NEL|:
Thanks for the reply. Sure, I will try. One follow up question here, for the unit test target in spm package, how I can avoid that not to compile for release config in Xcode?
Right now I am using `#if DEBUG` flag to bypass the tests. Are there any other options?

|U03HES7M40M|:
Did you create a xcodeproj file from the `Package.swift`? If yes, we don’t recommend this approach anymore. Instead open the Package.swift with Xcode directly.

|U03JRQ81NEL|:
I am having a Stand-alone spm project with direct opening the `Package.swift` file.

|U03HES7M40M|:
I would use two different xcode schemes to toggle between unit and performance tests

|U03JRQ81NEL|:
Thanks :bow:. I will try this.

--- 
> ####  Am I missing something? I have a Swift package with some SwiftUI views that load assets from an asset catalog... but when I add that package to a project the images can't be found...


|U03J7H4LJ5N|:
I am assuming you are already using `Bundle.module` to reference the asset. Does this work when previewing from within the module itself but not when using it in a larger project?

|U03HVC6M81L|:
When I use Bundle.module I get the following error:
`MyApplePie/Sources/MyApplePie/TreeView.swift:12:58: Type 'Bundle' has no member 'module'`

When I use Bundle(for:Self.self) it still doesn’t find them. In the project I add the package to I *do* see the resources in the Asset catalog, I just can’t figure out how to refer to them…

|U03J7H4LJ5N|:
Is `TreeView.swift` part of the package that contains the assets? Or outside of it?

|U03HVC6M81L|:
It’s part of the package that contains the assets.
<https://github.com/mhanlon/MyApplePie>

|U03J7H4LJ5N|:
Ah I think I see the issue, you need to use tools-version 5.3 or newer for your package, since that is when resources were introduced

|U03J7H4LJ5N|:
Once you do that, the `Bundle.module` accessor should be generated for you at build time

|U03HVC6M81L|:
Got it. Excellent. Thanks!

|U03HVC6M81L|:
Oh man, you’ve saved me so so so much time. Thanks a million.

--- 
> ####  Is a package registry service for Swift Package Manager still moving forward? This was proposed and accepted in SE-0292 but it doesn't seem like there has been any more concrete implementation in the tooling.


|U03H36Q1EH5|:
The 5.7 release of SwiftPM will support package registry servers that are compliant with SE-0292. We suggest checking the forums for any future announcement of server implementations, for that will most likely be the place where people would make such news public.

|U03JGH0JLMQ|:
<@U03JRP6568Y> This popped up a couple of days ago: <https://jfrog.com/blog/artifactory-your-swift-package-repository/>, which I assume is an implementation of the service.

|U03JRP6568Y|:
Oh very exciting, thank you! We use Artifactory so extra exciting.

|U03JRP6568Y|:
Is this still only for source packages or will binary packages be supported? The original evolution thread didn't mention binary.

|U03JGH0JLMQ|:
The blog post mentions binary :crossed_fingers:

|U03JRP6568Y|:
Thank you. This will make our devops team super happy.

--- 
> ####  Trying to make an XcodeCommandPlugin for my iOS app. The plugin has a dependency on swift format that has a platform requirement for `.macOS(.v10_11)`. This is causing the build plugin to be compiled into inaccessible build folder in derived data. Any thoughts on how I can avoid this?


|U03HES8111T|:
This sounds like a bug — dependencies of the plugin, such as SwiftFormat in this case, should be getting compiled for the host in a way that makes it reachable from the plugin via the `tool(named:)` API.  It should not matter that the package has a lower platform requirement than what you are currently using.

Would you mind filing a bug report at <https://feedbackassistant.apple.com/> with as much information as you can provide so that we can investigate?  If you are able to attach the plugin, that would be ideal.

|U03JETJ7UU9|:
Hmm okay I can definitely make a feedback. Should we have a specific target in Xcode? I point to the iOS sim right now?

|U03JETJ7UU9|:
Or perhaps specify a `platform` in my plugins package manifest?

|U03JETJ7UU9|:
Also I should note that it's `swift-format` rather than `SwiftFormat`  :slightly_smiling_face:

|U03HES8111T|:
Thank you for clarifying — I misunderstood which package your were referring to.  It should still work the same, so a bug report with as much information as you can provide would be great.  It shouldn't matter whether you are building for the simulator or device — the dependencies of the plugin should always be getting built for the macOS platform on which they will run.

|U03HES8111T|:
The question about the `platform` is a good one — but it doesn't actually affect the way in which the plugin is built, and so only the `platform` in the package that defines the executable would affect it.

|U03HES8111T|:
In this case, I would be particularly interested in seeing what local path the executable is getting built into such that the plugin cannot find it.

|U03JETJ7UU9|:
Ah gotcha. That makes sense. Yes it seems `swift-format` is located in `/DerivedData/MyApp-baylwqivgphkmvgkekypuexdjmvt/Build/Products/Debug`  but the context running the actual plugin is looking in `/DerivedData/MyApp-baylwqivgphkmvgkekypuexdjmvt/Build/Products/Debug-iphonesimulator`

|U03JETJ7UU9|:
I've got it working in a very small sample app. I'll include that in the feedback

|U03JETJ7UU9|:
Sorry by working I'm mean repro'd :sweat_smile:

|U03HES8111T|:
This sounds like a bug, and so a feedback ticket would be very much appreciated!  If you have the FB number and can post it here, that would be great.  Thank you!

|U03JETJ7UU9|:
<@U03HES8111T> just submitted' it! `FB10162156`

|U03JETJ7UU9|:
Demo project attached

|U03HES8111T|:
Thank you very much!

--- 
> ####  Could you clarify what the Package.resolved is used for by SPM?   I keep thinking it's like a .lock file used by other package managers, but SPM seems to use it more like a record of what was installed rather than specifying the exact versions to install.


|U03HL5FTVHS|:
Hi Craig! You’re correct in that the Package.resolved file is designed to be distinct from a .lock file but it does have some overlap. Here’s a snippet from the proposal:

&gt; The version of every resolved dependency will be recorded in a Package.resolved file in the top-level package, and when this file is present in the top-level package it will be used when performing dependency resolution, rather than the package manager finding the latest eligible version of each package. swift package update will update all dependencies to the latest eligible versions and update the Package.resolved file accordingly.
You can read more about the detailed design of the Package.resolved file in this <https://github.com/apple/swift-evolution/blob/main/proposals/0175-package-manager-revised-dependency-resolution.md|Swift Evolution proposal>.

By default the version in the Package.resolved is used as a base when resolving existing dependencies but if you want to enforce the usage of the Package.resolved file as a true lock, you can pass the `--only-use-versions-from-resolved-file` to SwiftPM or the `-onlyUsePackageVersionsFromResolvedFile` flag to `xcodebuild`.

--- 
> ####  We distribute a large number of binary Swift frameworks, and would love to be able to distribute them using SPM. How would you recommend constructing our package definitions so that our packages can correctly declare dependencies on each other?


|U03J7H4LJ5N|:
I am assuming you are talking about the fact that a binary target cannot declare dependencies in the package manifest. This is a known issue we are already tracking, in the meantime, there is some discussion around a workaround on the Swift forums here: <https://forums.swift.org/t/swiftpm-binary-target-with-sub-dependencies/40197/5>

|U03JGH0JLMQ|:
Thanks for the reply! I was, though hadn’t found that page. Was wondering if there was a better workaround, but glad I hadn’t missed anything obvious

--- 
> ####  Can image assets that are part of a package be used in storyboards and nibs outside of that package? They display fine in IB, but when running in the simulator or device I always get an error that the resource can't be found.


|U03HES8111T|:
A packages target's resources can only be accessed from within the package target that declares them.  This is done using the `Bundle.module` accessor that is synthesized for each package target.

To access these resources from outside the package target, the package target can implement an accessor that returns the target's own resource bundle to the caller, but there isn't currently a way to do that directly in Storyboards and Nibs.

|U03KVQZRF7S|:
Ok, confirms what I'm seeing, thank you!

|U03HES8111T|:
If you have time, would you mind filing a feedback ticket at <https://feedbackassistant.apple.com/> since it seems like a bug that there is a discrepancy between what you see at editing time and at runtime?

|U03HES8111T|:
That would be very appreciated!

|U03KVQZRF7S|:
Yes, I can do that!

|U03HES8111T|:
Thank you!

--- 
> ####  Is there any modern guidance on when to use Swift Packages vs. dynamically linked frameworks when sharing code *internally* across multiple targets? Say I have an iOS app, a Siri intent, and a widget - I've always relied on making a MyCoolKit framework with shared code and importing that framework into each user-facing target. Should I migrate to a Swift package instead? Are there pros and cons or tradeoffs to consider?


|U03J7H4LJ5N|:
Packages can be a good way to organize your internal modules, especially if you may plan on splitting them out into separate SCM repositories in the future, e.g. to share them between multiple apps. For the concrete case of sharing code between an app and app extensions, frameworks would still be the tool of choice to share code at runtime. You are able to take advantage of both by having a shared framework between your app and extensions which links any local packages statically.

|U03J1UAEU4B|:
Ah, great idea, thank you!

|U03J1UAEU4B|:
I have had that happen before, where I have code within my framework that ultimately I want to split out into a library - in the past that's required refactoring, moving it out of the repo, making it a cocoapod.

|U03J1UAEU4B|:
I'll try that approach instead next time

|U03HVE01C6A|:
<@U03J7H4LJ5N>  can you elaborate further on why using a framework is the tool of choice here?

&gt; For the concrete case of sharing code between an app and app extensions, frameworks would still be the tool of choice to share code at runtime. You are able to take advantage of both by having a shared framework between your app and extensions which links any local packages statically.

|U03J4DRK4SY|:
I'd love to know more about this ^ as well. I'm using a Swift package for my app's shared code, because otherwise depending on another package in both my app target and the shared framework causes the package to be linked dynamically resulting in a big file size increase since dead code stripping isn't performed on it (I think)

|U03J1UAEU4B|:
If your framework links the package, does the app target need to link it too? You could just import your framework anytime you need the stuff from the package.

--- 
> ####  Hi! I'm in large scale apps we want to optimize as aggressively as possible to have a smaller app size. We'd like to switch to SPM to manage our dependencies but there are a few restrictions in flexibility that impeded us from doing so: * We need to use the -Oz optimization flag, without using unsafeFlags (for release builds) * We need to be able to produce LLVM BC instead of Mach-O files, as we have additional size optimization steps at that level. Are these things something that can be done now with the new announcements or is in the roadmap for SPM?


|U03HES8111T|:
This customization isn't currently possible with Swift Packages, so for now staying with Xcode projects is probably the best option.

To help shape the future of Swift Packages  customization, the best way is to get involved in the package-specific forums at <https://forums.swift.org>.  That would be a great way to discuss what kinds of customization you need and suggestions for how you would like to express that customization.

--- 
> ####  Is there a way to set "Valid Archs"/"Excluded Architectures" for a SPM target or the whole library for simulator builds?


|U03J7H4LJ5N|:
There is no way to currently do this, but we consider it a known issue that package products do not build for the same set of architectures as their clients automatically.

|U03J22AU6DQ|:
Got it, thank you!

--- 
> ####  Swift package plugins might rely on executables which may or may not exist on the local machine, e.g SwiftLint. Can those be bundled as part of the package (if not locally available) ?


|U03HES8111T|:
Yes they can.  Our recommendation is to have a dependency on a source package if the executable is built using SwiftPM and the command is used as a regular build command.  If the executable is built using a different tool than SwiftPM or needs to be invoked from a prebuild command, the executable can also be bundled as a binary artifact archive.

For more information see <https://github.com/apple/swift-package-manager/blob/main/Documentation/Plugins.md>

--- 
> ####  Does Apple provide a set of own, open-source Swift package plugins to the community?


|U03HL5FTVHS|:
Apple routinely provides Swift open source libraries, and some of them could include plugins. For example, Apple recently opened sourced a <https://github.com/apple/swift-docc-plugin|Swift-DocC plugin> as part of Swift 5.6. The plugin provides an easy way to generate documentation for Swift Package libraries and executables from the command-line.

--- 
> ####  I work on a complex enterprise app that uses many SPM dependencies under the hood (over 100 packages). Since we've onboarded to building our app with SPM, our engineers are complaining about slow and error prone package resolution times which require multiple cycles of   File  Packages  Reset Package Caches / Resolve Package Versions  Should we expect Xcode 14 to resolve many of these issues? We are currently using Xcode 13.3 and 13.4.


|U03H36Q9X3R|:
Thanks for reporting. We would need to work together with you to collect additional information about these issues. Please file a feedback (<https://feedbackassistant.apple.com/>) with as many technical details as possible. For example, a sysdiganose would help understand the performance issues.

|U03H36Q9X3R|:
Please post the feedback ID here once filed so we can follow up on it

--- 
> ####  Is it possible to use Xcode's Package Collections UI for a Swift Package without an xcodeproj? The only way I managed to find is manually adding a dependency in code.


|U03H36Q9X3R|:
Package manifests are code, and could be arbitrarily complex, so there is currently no way to programmatically modify the package manifest. As such the collection UI only offers a copy-to-clipboard helper and the manifest changes have to be done manually

--- 
> ####  How can SPM subcommands like diagnose-api-breaking-changes be used for Swift packages which require xcodebuild for compilation (example: a package targeting iOS only with heavy use of UIKit) ?


|U03HES7M40M|:
The SwiftPM cli tools are currently not supported for iOS projects. `xcodebuild` is the CLI tool for iOS projects. `xcodebuild` does not provide the diagnose-api-breaking-changes feature. If you would like to use such a feature for iOS projects, we recommend you to open a feedback.

--- 
> ####  Hey there! :wave: In our company we have a project basically split between a bunch of Swift packages, including a top-level one wrapping all of these into an app target, also delivering some external packages by providing wrapping protocols to have it properly abstracted.  The issue we have is that basically most of our packages are resolved automatically (products with no explicit `type` setting) so that these can be linked either statically or dynamically, yet a couple of these packages needs to be kept explicitly as `.dynamic` due to an issue with tests. Building the app for running compiles perfectly, with all our packages being linked statically to the app binary, keeping only a few 3rd-party frameworks aside in the Frameworks folder. Once there are some specific Test targets involved, we're getting a “Swift package product 'OrbitStatic' is linked as a static library by 'SnapshotTests' and 'App'. This will result in duplication of library code.” – The mentioned package is being `@_exported import`ed and statically linked by our wrapping UI package (SharedUI) so that it bundles our external package with basic UI kit (Orbit), another package with SwiftUI (SharedSwiftUI) views and basic legacy code with UIKit (SharedUI, the wrapping one). This SharedUI package is linked both by the App target and SnapshotTests, I'd say the problem is being in the build list by both targets while requiring different linking type for each one of them. Could you suggest anything related to this specific error message? Possible solutions? Thanks! :raised_hands:


|U03J7H4LJ5N|:
This error message suggests that Xcodes automatic handling for the duplicated symbol issue has failed. Are you marking any package products explicitly as static by any chance? You also have the option of setting `DISABLE_DIAMOND_PROBLEM_DIAGNOSTIC` in the client target to suppress the diagnostic, however this means that you will potentially encounter a duplicated symbol issue at runtime.

|U03JPFQNX5K|:
Yea, definitely, the `SharedUI` one links those underlying `Orbit` and two others in the `SharedSwiftUI` statically (references the explicit `OrbitStatic`, `AccessibilityStringsStatic`, … products). If the error basically causes the duplication between different targets (App and Tests), could it be safe to use tle flag then?

|U03J7H4LJ5N|:
It’s difficult to say, but it is reasonably safe to try since any issues would surface at launch time. If there’s actual duplication, you might see a crash on launch due to it or potentially warnings that there is ambiguity with duplicated types.

|U03JPFQNX5K|:
We'll definitely try :eyes: we lived with some symbols duplication (having the whole Orbit duplicated in ealier-separated `Shared*UI` packages twice) for some time, eventually discovered this static encapsulation solving the bundle size troubles quite nicely. This is a quickly drawn scheme just to illustrate it better (green-suffixed explicitly `static`, intermediates all `auto`, SharedUI currently `dynamic` and working with all targets, setting it `auto` will result in that duplication error).

|U03JPFQNX5K|:
The duplication shouldn't really be in a single binary, my guesses were that :one: app resolves statically and SnapshotTests dynamically for some reason and building both would be a duplicate building or :two: something more internal I can't really think about. I'd say if both `App` and `SnapshotTests` targets would ask for a `static` build of `SharedUI`, using a single one to link into both should be okay. :thinking_face: `SnapshotTests` have a Build phase dependency on `App` but only because it uses it to run the tests, there's no linking in between them.

|U03J7H4LJ5N|:
So during test execution the test bundle will be loaded into the app’s address space, so that’s where duplicated symbols *can* be an issue. We are bit careful about these issues because they can surface as crashes on launch when executing tests that are very confusing to developers.

|U03JPFQNX5K|:
Ah, I see, different setting of Host App unlike Target App in UI tests, that makes total sense then :thinking_face: I'll definitely try more shuffling with configuration, maybe some workaround will be possible. Not sure whether we're just doing something unique, or just can't figure out how to put stuff together correctly, as the linker expects. :smile: Anyways, thanks very much for your precious help and insight during these two days :raised_hands: Have a nice rest of the Dub Dub!

--- 
> ####  Are you planing to add support for importing custom modules to Package.swift manifest file? <http://tuist.io|tuist.io> can handle PackageDescriptionHelpers where user can write complicated scenario and in the end import to manifest file and call smth like "<http://Project.app|Project.app>(liOS], named: "MyGreatApp")  The reason is to avoid massive files with manifests. Some manifest can contains more than 1000 loc (Firebase/AppCenter and etc)!


|U03HES8111T|:
This sounds like a great idea, and it is something that has had some discussion in the Package Manager section of the Swift forums in the past.

Any developments in this area would go through the Swift Evolution Process, and getting involved in the discussions at <https://forums.swift.org> would be a good way to influence such a feature.

|U03JSFUKL2U|:
Ok, I will!

--- 
> ####  Can I add a new platform support and contribute it to SPM? Or new features in SPM can be added only by SPM Workgroup member?


|U03H36Q9X3R|:
SwiftPM, and more broadly the Swift project, are open source projects, and folks are welcome to make contributions. Adding a new platform support may require changes beyond SwiftPM itself, and this is something we can discuss on the Swift forums <http://forums.swift.org|forums.swift.org> or on github <https://github.com/apple/swift-package-manager/>.

|U03JSFUKL2U|:
Great! Thanks a lot!

--- 
> ####  Does the iOS simulator run in x86_64?  I notice that I get one set of build errors when building to a Simulator but not to a practical device?  If it helps, I’m on Apple Silicon running Xcode natively.


|U03HES8U8FP|:
The simulator runs in the native architecture. Sounds like you are using a third party library that hasn’t been updated to support arm64 in the simulator. If that’s the case, we encourage you to contact the developers of this library to make sure they update for arm64 when targeting the simulator. In the meanwhile, if that library is not essential to build your app in the simulator, you can conditionally remove this library when targeting the simulator with `#if targetEnvironment(simulator)`.

|U03J20E7UBV|:
Thank you!!!

|U03J22A0C4S|:
Aren't older sims (&lt;=iOS 13) x86 only?

--- 
> ####  Hey there! We have automated the process of creating VM images of macOS to use in our CI/CD pipeline. This includes multiple Xcode versions being installed using cli tools like xcode-install or xcodes.   Since Xcode 14 doesn't ship with all simulator runtimes by default (watchOS/tvOS), is there a easy way to download the missing runtimes using a command?  :)


|U03HB5P2UTY|:
There are a few ways to do this. The simplest would be to  use the `xcodebuild -downloadAllPlatforms` command to download and install the additional simulator runtimes. Alternatively, the individual runtime images are available to download from <https://developer.apple.com/download/all>. You can install them via the `xcrun simctl runtime add` command. More information and instructions are available here: <https://developer.apple.com/documentation/xcode/installing-additional-simulator-runtimes>

|U03J22A0C4S|:
Similarly, is there a way to remove runtimes when added?

|U03HB5P2UTY|:
Yes! You can use the `xcrun simctl runtime delete` command to do so.

|U03HB5P2UTY|:
Or you can delete them via the Platforms preference pane in Xcode.

--- 
> ####  Using codesign to extract the entitlements plist from an app is broken on Monterey, we are getting a weird dictionary like this. Is there an alternative to codesign for this workflow? Is this expected to fix in Ventura?  Command used: `/usr/bin/codesign -d --entitlements - &lt;appName.app`  ``` [Dict] 	[Key] application-identifier 	[Value] 		[String] &lt;appID 	[Key] com.apple.developer.team-identifier 	[Value] 		[String] &lt;teamID 	[Key] get-task-allow 	[Value] 		[Bool] true 	[Key] keychain-access-groups 	[Value] 		[Array] 			[String] &lt;appID ```


|U03HB5P2UTY|:
The output you're seeing is a more user readable form that was introduced in Monterey. You can still get the XML output you're used to by passing the `--xml` flag to `codesign`.

|U03K8US1GD7|:
But that output is a single line xml and not properly formatted as it used to be in bigsur. Outputting that into a plist file file is creating a corrupted file.

|U03HB5P2UTY|:
There is an issue we're tracking where the output contains a NUL character at the end, but if you remove that you'll have a valid plist. Another option is to use the `SecStaticCode` API to retrieve this programatically.

|U03K8US1GD7|:
Ok. `SecStaticCode` won’t work for our workflow. Looks like removing the final Nul character is working. Maybe we can work that into our script to get a valid plist.

--- 
> ####  I see there's now a way to submit builds for notarization from non-Macs (the new REST api). That's super awesome &amp; will definitely unblock some work I've been doing on our distribution process. Is there a way to staple the notarization ticket to our binary on Linux/non-Macs in general?   If not, notarization workflows still need to find a cloud-based Mac somewhere.


|U03HB5P2UTY|:
If you're notarizing an application package, you can grab the ticket directly from CloudKit by reproducing the call that `stapler` uses. You can see that call by running the tool in verbose mode. However, you'd have to calculate the cdHash manually. We'd welcome your <https://feedbackassistant.apple.com|feedback> if you'd like to see improvements to this process .

|U03J1UX2CQK|:
Thank you! Feedback: FB10162550 -- apologies for the poor title, I hit submit too early :grimacing:

|U03HB5P2UTY|:
Thank you!

--- 
> ####  I'm looking to add testing to my SwiftUI multiplatform app. What's the best approach. Is it XCTest? XCode Cloud? Or something else?


|U03HES8U8FP|:
You should start locally with XCTest before using Xcode Cloud. We recommend you to add some unit tests and UI tests first and once you are happy with that and your tests pass in your Mac, you can configure Xcode Cloud to run your tests.
Remember writing tests is a process that never ends, so do it incrementally, here are some tips:
• identify what’s most important to test on your app and start from there
• when you add new features or fix some bugs in your app, make sure you also write tests to avoid regressions in the future

|U03JRPWSDJ4|:
Perfect. Thank you

--- 
> ####  Does Apple have a supported mechanism for re-signing already archived and signed Ad Hoc IPA apps? For example, if we archive an (ad hoc) IPA, can it be re-signed by someone else if they have an Enterprise Distribution Certificate?


|U03HB5P2UTY|:
We don't provide a supported mechanism for accomplishing this, but we'd welcome a <https://feedbackassistant.apple.com|feedback> report describing the details of your use case.

|U03JEEUJPMJ|:
(Ahmed– forgive me if you already knew this, but: an IPA file is a zip archive, with the .app directory nested inside. Unless I’m missing something– when we’ve needed to do something similar: the .ipa can be extracted, Apple’s `codesign` program could be run on the .app directory, and the .ipa re-created by zipping back up the directory)

--- 
> ####  I am writing an iOS app using SwiftUI and was wondering what framework do you recommend to call Rest APIs from AWS? sample code would be good


|U03HES8U8FP|:
You should take a look at <https://developer.apple.com/documentation/foundation/urlsession|URLSession> in Foundation. There are also several open-source frameworks from the community that wrap URLSession in different ways. Take a look at <https://developer.apple.com/wwdc21/10095|Use async/await with URLSession> from WWDC 2021 for some code samples.

|U03JSPSLWG3|:
Thanks!

--- 
> ####  Hello! I'm really excited to use the new features in Swift 5.7 that improve the usability of existential types. Do these features require increasing my deployment target to iOS 16, or are they all at the compiler level?


|U03HES8U8FP|:
It’s a Swift feature, so it doesn’t require increasing the deployment target. You should be fine as long as you build using Swift 5.7+

|U03HWDCSCHX|:
I should note that using generics in concert with the new existential type features _may_ require you to increase your deployment target. Essentially, if Swift can determine something statically you're free to use it on whatever OS you'd like. However, when Swift needs to make runtime calls (the big ones are casting with `is/as!/as?`!) you will need to use availability guards.

|U03HWDCSCHX|:
So something that compiles:
```
let xs = [ "Hello" ] as any Collection&lt;String&gt;
```


|U03HWDCSCHX|:
But something that requires a deployment target bump:
```
let fail = Array&lt;any Collection&lt;String&gt;&gt;() // Requires runtime support to know the layout of `any Collection&lt;String&gt;`
```

--- 
> ####  I have a question for Xcode Q&amp;A. I want to reset permission status for booted simulator. When I call `xcrun simctl privacy` commands, nothing happens. Example commands: `xcrun simctl privacy booted reset contacts MY_APP_ID` `xcrun simctl privacy booted reset all` This issue happens for all types of permissions: contacts, notifications, reminders etc. I submitted a bug report last year FB9737211 but the issue is not fixed in Xcode 14. When the issue will be fixed?


|U03H36PM1BR|:
Thanks so much for filing a great Feedback for this. It looks like this issue is still being looked at internally. I see you've already provided sysdiagnoses, so thank you for this!! Could you please reproduce this issue again and this time run `xcrun simctl diagnose` and attach that to the Feedback issue? That might give us some more information to look at. Thank you!!

|U03HZ3E3GTF|:
Yes, I will do it within several hours.

|U03HZ3E3GTF|:
<@U03H36PM1BR> I've reproduced the issue and added a sysdiagnose file to the FB9737211.

|U03H36PM1BR|:
Thanks Daniil. To clarify, did you attach the output of `xcrun simctl diagnose`  or a regular `sysdiagnose` output? The 2 are different things and the former gives us a bit more information from the Simulator side of things. Both are important here. :slightly_smiling_face:

--- 
> ####  I was just flicking through the Xcode14 release notes and spotted this :  Deprecations Building iOS projects with deployment targets for the armv7, armv7s, and i386 architectures is no longer supported. (92831716)  So once Apple require submissions to use Xcode14 (any idea when this might be?) will that mean we won't be able to deploy to devices older than iPhone 5s (the first arm8 phone) eg 5c, 5, 4s etc (that were all arm7) are excluded?


|U03HHPDDRK5|:
That’s correct.

|U03JEEUJPMJ|:
Edit: oh gosh, App Store submissions will cut off support!

[Previously: what’s the recommended route forward for building for older devices, given that older versions of Xcode are prevented from running on newer macOS versions, and vice-versa?– somehow obtain older versions of macOS, virtualise them, and use an older Xcode version?]

|U03JVMGR3RT|:
Thanks <@U03HHPDDRK5> - when do you think requirement to submit with Xcode14 would come into force (roughly :smile: ).

|U03J7BXV8KA|:
Sorry, we can't comment on any "when" questions.

|U03J7BXV8KA|:
&gt; obtain older versions of macOS, virtualise them, and use an older Xcode version?
This is a reasonable approach, yes.

|U03HL553PNG|:
<@U03JEEUJPMJ> rest assured that when we do require a newer Xcode/SDK, we will announce that through the regular channels.  I believe there's usually an email from Developer Relations when those changes happen.

|U03HL553PNG|:
Another suggestion - in App Store Connect, you can also see your usage by OS version which is a good way to make that decision about when to cut off support for those devices.  You can't exactly tell (at least that I could figure out) what device they're on, but you can tell their OS version and since they will be capped at a certain OS, you'll know when it's safe to move your deployment target up.

|U03HL553PNG|:
You can also <https://support.apple.com/en-us/HT208891|dual boot multiple macOS versions> on the same computer without virtualization.

|U03JVMGR3RT|:
Thanks for clarifying - the potential date is important for us as ware are currently working on a project that will launch end of Q1 2023.  If we were fairly certain older devices would not be supported by this point we could reduce the amount of QA required for the project.

|U03HL553PNG|:
Yeah, I understand that's a tough call to make. :slightly_smiling_face:   You can also try reaching out to <http://developer.apple.com/contact> to see if they have any more guidance but I suspect they'll probably say the same thing.

|U03JVMGR3RT|:
Thanks - I'll give them a shot but understand if it's not possible :smile:
(this info would also help us guide the client towards only supporting back to iOS13 instead of 12 ! )

|U03HL553PNG|:
I don't know about you, but I love data-driven decisions.  So if I was your client and you showed me a cool pie chart with usage by OS version, that would definitely help me make that call!

|U03JVMGR3RT|:
:laughing:
Believe me I have plenty of pie charts and infographics!
Every little bit of extra ammo helps :+1:

--- 
> ####  We've been having issues when adding new files to local Swift packages that's embedded into an Xcode project. Xcode crashes for the most part, and it's quite difficult to modify local Swift Packages this way. Was this issue addressed in Xcode 14?  We filed a Radar ticket and posted about the issue on the dev forum as well; please find it here: <https://developer.apple.com/forums/thread/704568?login=true|https://developer.apple.com/forums/thread/704568?login=true>  Thank you!


|U03HES8111T|:
It looks like this issue was reported against 13.3, and it should already be addressed in Xcode 13.4.  If you are still seeing it in 13.4 or later, we would very much appreciate it if you could add the crash report to the feedback ticket you filed and send back as not fixed, or to file a new feedback ticket if the feedback system doesn't let you add to the original report.

--- 
> ####  In Xcode 14 running on macOS Ventura, when I try to launch an app that uses CloudKit in Simulator, I get the following error:   ------------------ dyld[6296]: Library not loaded: /usr/lib/swift/libswiftCloudKit.dylib   Referenced from: &lt;18DE50F3-6EE7-3075-AB2F-BFADE7C3496E /Users/testuser/Library/Developer/CoreSimulator/Devices/33CB0140-D129-46AE-8D54-D3372393031B/data/Containers/Bundle/Application/3BD45E5E-19D4-4716-84A8-E0247CDCBF98/FSTestApp.app/FSTestApp   Reason: tried: '/Users/testuser/Library/Developer/Xcode/DerivedData/FSTestApp-drxbwyfqohpnyqfrmpspvebmsjkp/Build/Products/Debug-iphonesimulator/libswiftCloudKit.dylib' (no such file), '/Applications/Xcode-beta.app/Contents/Developer/Platforms/iPhoneOS.platform/Library/Developer/CoreSimulator/Profiles/Runtimes/iOS.simruntime/Contents/Resources/RuntimeRoot/usr/lib/system/introspection/libswiftCloudKit.dylib' (no such file), '/Applications/Xcode-beta.app/Contents/Developer/Platforms/iPhoneOS.platform/Library/Developer/CoreSimulator/Profiles/Runtimes/iOS.simruntime/Contents/Resources/RuntimeRoot/usr/lib/swift/libswiftCloudKit.dylib' (no such file), '/usr/lib/swift/libswiftCloudKit.dylib' (no such file, not in dyld cache), '/Applications/Xcode-beta.app/Contents/Developer/Platforms/iPhoneOS.platform/Library/Developer/CoreSimulator/Profiles/Runtimes/iOS.simruntime/Contents/Resources/RuntimeRoot/usr/lib/libswiftCloudKit.dylib' (no such file) ------------------  The app runs without any problem on the actual device running iOS 16. Could this be a known issue?


|U03H36PM1BR|:
Hi there! Thanks for the great question! I believe this issue is noted on the Xcode 14 Beta Release Notes page at <https://developer.apple.com/documentation/xcode-release-notes/xcode-14-release-notes>:
```
Apps won't launch in simulator and the following error occurs when viewing Xcode's Target Output: Library not loaded: /usr/lib/swift/libswiftCloudKit.dylib (94331191)
Workaround: Set your app's minimum deployment target to iOS 16, tvOS 16, or watchOS 9.
```
Hope that helps!!

--- 
> ####  Xcode question. First beta of Xcode 14 seems to include SDK and simulator only for iOS 16. It doesn’t seem to be possible to install and use previous SDK-s and simulators? Can we expect this to be added in a future seed?


|U03HL5ECHUL|:
Previously released simulators are available by clicking on the '+' button in the lower left corner of the Platforms Preference pane. This will bring up a menu of platforms to choose a previously released simulator to download and install.

While you can install previously released simulators, you cannot install older SDKs. Instead you should set the "Minimum Deployment" target for each of your app's platforms (in the General section of your app's target) to control the minimum OSes your app can run on and use availability macros to control usage of APIs that are only available on newer OS versions.

--- 
> ####  I have a workspace with two projects, and each project contains the same shared framework sub-project:  Workspace - Project: app A for macOS –– Project: shared code B - Project: app A for iOS –– Project: shared code B  What would you recommend for handling source control / git in this scenario; with shared frameworks? (git submodule for for the framework, no source control over the whole workspace etc?)  (We've blissfully-ignorantly not used source control properly so far. :skull:)


|U03HES8U8FP|:
The answer is… it depends! if `shared code B` is also used by a different project that lives in another git repository, you should consider having it in its own repository. Then you can use git submodules, or even better, Swift Package Manager to import that into the different projects that use it.
If `shared code B` is not used from a different repository, then having it in the same repository is probably easier.

|U03JEEUJPMJ|:
Just to check: _years ago_ I worked with a team on an Xcode-based project, and I remember a lot of headaches relating to source control (git) and merging _non-source-code_; `.xcodeproj` and `.xcworkspace` contents. More specifically, I have memories of having to merge Xcode-internal contents of files that …I didn’t understand.

Are there any obvious recommendations these days re source control &amp; these directories?

(and: looking inside each of those _now_, I notably see an `xcuserdata` directory containing files seemingly specific to _me_. Is it advisable to check in _all but_ this `xcuserdata` directory? – i.e. check in everything else in the `.xcodeproj` and `.xcworkspace` directories. Or does Xcode reference these elsewhere, and …not having them in source control is _bad_.)

|U03HES8U8FP|:
That’s right, `xcuserdata` is only specific to you and you probably don’t want to share that with your team. It contains information like your local schemes, breakpoints, etc. I recommend you to put `xcuserdata/` in your `.gitignore` :slightly_smiling_face:

|U03HES8U8FP|:
You can still share schemes, breakpoints and some other things if you want, most of these offer a checkbox to share them in their own editors

|U03JEEUJPMJ|:
A) *OH!* That’s what the `Shared` checkbox refers to? (in the schemes window) :ok_hand::skin-tone-3:

B) Ah, is the `xcshareddata` directory where those (shared schemes, breakpoints, etc) would live instead?

|U03HES8U8FP|:
Yes and yes :smiley:

--- 
> ####  My project has a dependency on an older framework I have imported. XCode builds the project locally just fine, however XCode Cloud fails at 'xcodebuild archive', issuing a 'Precompile bridging header' error, due to a .h file I'm referring to, that's located in the framework bundle. The framework bundle is present in the Git repo. Am I overlooking something? Would there be a workaround to solve such an issue? Thank you!


|U03HESABNTX|:
I recommend ensuring your project fully specifies the necessary dependencies between its targets, especially the target with the bridging header and the target with the header it failed to import. If you continue to experience this issue, please file a bug report including the full log of the failing build from clean so we can investigate further.

|U03J1U2G8PM|:
Thank you! When you say “fully specifies the necessary dependencies”, do you mean the Framework search paths in the build settings of the target? At the moment that setting specifies an absolute path (on my local machine).. I will try to poke around this and if I can’t resolve it I will file a FB.

|U03HESABNTX|:
You can view the dependencies of a target in the build phases tab of the target editor, under the "Dependencies" section. If your scheme has "Find Implicit Dependencies" enabled, Xcode will also discover dependencies for you based on the contents of the "Link Binary with Libraries" section, and the "OTHER_LD_FLAGS" build setting.

|U03J1U2G8PM|:
Indeed, the scheme has Find Implicit Dependencies enabled and the framework is under “Link Binary Wth Libraries” section of the target Build Phases… I should maybe try to add the framework explicitly in “Dependencies”?  Thanks again, much appreciated :slightly_smiling_face:

|U03J1U2G8PM|:
Quick update: I just solved the issue.. I removed, then added back again the framework to the project. Not 100% certain but I suspect a path issue… Anyway, Wohooo! My very first Xcode Cloud build works! :partying_face:

--- 
> ####  hi, I am working on a big project and the building and run function work well on Xcode 12, but 2 months ago, we moved toward Xcode 13 and the nightmare began across almost of all members in teams. Even a small change in source file will lead to Xcode to indexing and pre-build which always take 30G memory on our MBP. We can do nothing at all. Each time the indexing and pre-building would take 2 ~3 hour to finish. Such insane, so we all disabled the indexing function by `defaults write com.apple.dt.XCode IDEIndexDisable 1`. Eventually, the Xcode would not freeze Mac, we can write code and easily build and run. through we lost the support of auto-complete from Xcode.   Any advises to avoid consuming 30 G mem and freezing Mac and I still get auto-complete support meanwhile? Thanks.


|U03J7H72X5W|:
That memory usage definitely sounds like a bug. To help us determine the root cause of the issue, could you file a bugreport on <https://feedbackassistant.apple.com> with the following information
1. Let Xcode index for a while (~10 minutes), reproducing the issue you describe, gather a sysdiagnose using `sudo sysdiagnose` and attach it to the feedback. This should tell us which process is doing all the work
2. Could you manually trigger an index build using the following command and capture its output using the following command `xcrun xcindex-test -project &lt;path/to/your.xcodeproj or path/to/your.xcworkspace&gt; -derivedDataPath index-data -- create-build-description -destination &lt;run destination, eg. generic/platform=macOS&gt; -scheme &lt;scheme&gt; -- prepare -all-targets -- index-files -all-targets -- quit &amp;&gt; index.log` wait for it to run, which might take a while because as you described your index takes long, and also attach it to the bug report.
3. About 10 minutes into the `xcindex-test` run described above, could you capture another sysdiagnose (using the same procedure as in (1)) while taking a screen recording of `xcindex-test` output at the same time. This will allow us to correlate the work that `xcindex-test` is doing with entries in the log

You can also try the Xcode 14 beta, which includes improvements to indexing and see if your issue has already been fixed.

In your bug report, you can also mention that you brought this up in the Friday Slack lounge, so we can link the report to your question.

|U03J7H72X5W|:
Sorry, I made a typo above. Step (2) should have `2&gt;&amp;1 | tee index.log` instead of `&amp;&gt; index.log`

--- 
> ####  Hi, I saw this MXAppLaunchDiagnostic in the API diffs and was wondering if there was any more detail on what it's for? Does it give you a trace of app launches in production by including time spent for nodes within `callStackTree`?


|U03HHPU5NLS|:
Hi there, thanks for the question! The new app launch diagnostic will provide you a backtrace for launches that exceed a duration threshold we define as a "long launch" and for a random subset of "normal" launches. The backtraces they contain are weighted, and can be used to determine where the majority of time is spent during the launch. Note that this diagnostic is sub-sampled in your user population, so not all devices running MetricKit will report that diagnostic.

|U03JN34N5TL|:
I see, so long launch and normal launch information is merged? Is more weight given to long launches?

|U03HHPU5NLS|:
Indeed. Long launches will be the preference. Random launches will be sampled less frequently on enabled devices.

|U03JN34N5TL|:
Cool, good to know. Are there any WWDC talks that discuss this? Are there any other new sorts of production performance data that are available in iOS 16, aside from hang detection?

|U03HHPU5NLS|:
We don't have a specific talk on these diagnostics this year. For production performance data, we've also introduced an API for extended launch telemetry via MetricKit, which will let you inform the system when its expected that you're taking a bit longer to launch.

|U03JN34N5TL|:
I see, it looks like `+extendLaunchMeasurementForTaskID:error:`  is part of that, and that appears to include a task ID. Are there further docs around what an `MXLaunchTaskID` is?

|U03HHPU5NLS|:
Our documentation should have some information. <https://developer.apple.com/documentation/metrickit/mxmetricmanager/3979268-extendlaunchmeasurementfortaskid?language=objc>

I think we can probably do a better job explaining how to use it here though. If you can file a feedback, I can get someone looking at it :slightly_smiling_face:

|U03JN34N5TL|:
I guess I’m not sure what an extended launch task is, or how that affects the metrics Apple would report to me. Is it that I can manually tell the system how long my launch took, so that it will provide data for that whole time frame, if I consider my launch to be finished at a further point than Apple automatically would? E.g., I unblock the main thread early on but have to show a spinner as I’m loading some database from disk, so the app would only be really interactive once that loading is done.

|U03HHPU5NLS|:
That's about right. You're basically telling us "I have some extra work to do, can you collapse the system measured launch time with my extra work time?" The metrics that would be reported to you wouldn't be impacted as long as you don't have conflicting taskID's. We report a histogram of launch times where you explicitly called the extendedLaunch APIs properly.

|U03HHPU5NLS|:
And to be clear, an extended launch task is any work thats going to start before or during UISceneDelegate.scene(_:restoreInteractionStateWith:), or before UISceneDelegate.sceneDidBecomeActive(_:) is called on the first scene to connect.

|U03JN34N5TL|:
Cool, thanks :thumbsup:

|U03HHPU5NLS|:
And one quick example of defining a taskID for you:

&gt; static let loadingUpMainContent = MXLaunchTaskID("loadingUpMainContent")
And subsequent usage w/ the API:

&gt; try MXMetricManager.extendLaunchMeasurement(forTaskID: .loadingUpMainContent)
...
&gt; try MXMetricManager.finishExtendedLaunchMeasurement(forTaskID: .loadingUpMainContent)

|U03JN34N5TL|:
Cool, and would the call stack tree that Apple reports in MXAppLaunchDiagnostic include backtraces up until the last launch task was finished?

|U03HHPU5NLS|:
Great question. You actually won't (shouldn't) get launch diagnostics for launches you're telling the system are purposefully extended.

|U03JN34N5TL|:
Oh, OK. So it’d be for a case like, “I’m doing this one-time db migration that I know will be slow, please don’t pollute my startup time info with it”?

|U03HHPU5NLS|:
Correct - the logic here is that launch diagnostics are designed to help you catch the unexpectedly slow launches, not the ones you expect :slightly_smiling_face:

--- 
> ####  I got an unexpected error on Xcode 14. Anyone know the reason?  Stored properties cannot be marked potentially unavailable with '@available'


|U03HES8U8FP|:
That’s normal, the compiler needs to know how much space in memory a type needs, so stored properties cannot be marked with `@available`

|U03HVCK66P8|:
However, it is working on Xcode 13? Or just changed.

|U03HVCK66P8|:
I just got this error after Xcode 14. Just wondering it is a bug, or I should put it to ‘upper level’

|U03J22A0C4S|:
This was a bug in conjunction with `lazy` that was fixed in Swift 5.7.

|U03HVCK66P8|:
So this is no longer work?

|U03HVCK66P8|:
:smiling_face_with_tear:

|U03J22A0C4S|:
I don't believe it properly worked before.

|U03HWEGHRKR|:
yea this no longer works. this is a solution someone gave
```
final class Foo {
  private lazy var _myBackingVar: Any { ... }
  @available(iOS 15.0, *)
  var myConditionallyAvailableVar: RealType { _myBackingVar as! RealType }
}
```


|U03HVCK66P8|:
Thanks <@U03HWEGHRKR> <@U03J22A0C4S>
Will give it a try :slightly_smiling_face:

|U03HVCK66P8|:
Works again :tada:

--- 
> ####  The compiler doesn't seem to be looking for the correct specialization overload when there's at least one generic indirection: <https://github.com/apple/swift/issues/59333|https://github.com/apple/swift/issues/59333> Is this expected?


|U03HWD526QH|:
Hi Gonzalo, thank you for the question! That's the expected behavior. Overloaded class methods are statically dispatched and overload resolution happens at compile time. The compiler only knows that `T: A` so it can only call the "unspecialized" overload. You need dynamic dispatch for this to work, e.g. using protocol requirement methods. We will add some explanation in the GitHub bug.

|U03HMCR1CFR|:
/cc <@U03HVD36H54>

|U03HMCR1CFR|:
thanks Rintaro! without the intention of challenging your response :sweat_smile: but just for the sake of the conversation, we were reasoning about this potentially being solvable by the compiler tho.

• 1st is `baz` where, as `T` is a parameter of what `baz` identifies as `Self` , we would’ve expected that be statically dispatchable by having two `baz` implementations: `Foo.bar()` and `Foo.bar&lt;&gt;()` (as identified in sil, attached later).
• 2nd is `callBarFails` where same could happening as demonstrated by `callBarWorks` if the compiler detected that actually N versions of the function may exist depending (where N is n1 + n2 + n3 with each n being the number of overloads functions being called could have) on a yet-unknown but eventually-known type.
is this reasonable or there’s any other implication here we might not be seeing?

|U03HWD526QH|:
Ah sorry for the late reply. I'm not a type checker expert, but...
For both cases in this specific example, yes the compiler _can_ know `T` conforms to `B` for `Foo&lt;TypeB&gt;` . However, in case these functions are in a different module like this:
```
// Library module.

public protocol A {}
public protocol B: A {}

public class Foo&lt;T: A&gt; {
  public func bar() -&gt; Bool
  public func bar() -&gt; Bool where T: B
  public func baz() -&gt; Bool
}

public func callBarFails&lt;X: A&gt;(from foo: Foo&lt;X&gt;) -&gt; Bool```
And in the main module:
```// main module

import Library

struct TypeA: A {}
struct TypeB: B {}

_ = Foo&lt;TypeB&gt;().baz()
_ = callBarFails(from: Foo&lt;TypeB&gt;())
```
In this case, when the compiler compiles `Library.callBarFails(from:)` , it doesn't know how the function is called at all. Also, when compiling `main` module, the compiler doesn't know the implementation of the `callBarFails(from:)` . So it can't dispatch it to `Foo&lt;T&gt;.bar() where T: B` .

|U03HWD526QH|:
It'd be super confusable if we differentiate the behavior between `internal` decls and `public` decls.

|U03HMCR1CFR|:
ah, that’s absolutely fair!

|U03HMCR1CFR|:
thank you for answer and the follow up Rintaro :wave: nice talking to you.

|U03HWD526QH|:
Thank you! Have a good weekend!

--- 
> ####  Our project has multiple framework targets. Each one has a build phase that runs swiftlint on its source code. It's setup so it only runs if needed using   input file: .swiftlint.yml file  input file lists: xcfilelist with all swift files in the framework output files: ${DERIVED_FILE_DIR}/ran-swiftlint that is touched by the script.  This works correctly most of the time, but if the `.swiftlint.yml` is changed, it seems each target runs sequentially instead of in parallel. Any tips for what to look at that could be causing it?


|U03HESABNTX|:
It is a known issue that incremental builds may unnecessarily serialize build tasks of some projects when specific inputs are updated. If possible, please file a bug report including a result bundle from an incremental build that demonstrates the issue so we can investigate further. You can generate a result bundle by passing the `-resultBundlePath` argument when building with `xcodebuild` on the command line.

|U03J22A0C4S|:
I submitted FB10143230 asking for better build script integration. In @Craig's case it probably be fine to unconditionally run the swiftlint scripts at the start of an overall build or the build of the target, but we can't express that dependency right now.

--- 
> ####  I'm not sure the best place to ask this question, since using scripting of all kinds is so relevant to so many workflows. But, I am curious about the future of shells being included in macOS, specifically /bin/bash. In Catalina, it was announced that 3rd party scripting environments are being deprecated. The specific language was "Scripting language runtimes such as Python, Ruby, and Perl are included in macOS for compatibility with legacy software." That "such as" leaves some ambiguity in regards to shells like bash, which could be considered a 3rd party scripting language runtime, but it could also be argued that shells fall under a different category.  Can you share any insight on the future of bash in macOS? I don't expect it to be updated beyond version 3.2.57, but should we be preparing for a future in which /bin/bash does not exist at all on macOS?  As a follow up question, should there be any concern for the future or 1st party scripting environments like AppleScript and JavaScript for Automation (JXA) such as being deprecated or removed from macOS?


|U03HWD3Q7LH|:
We are committed to maintaining POSIX compatibility, including that of the POSIX shell (/bin/sh).  There is no guarantee that /bin/sh will be implemented by bash in the future (and in fact you can customize /bin/sh to be zsh as of a couple years ago).

|U03HWD3Q7LH|:
We have no plans to update the version of bash that we are shipping but do continue to update the version of zsh we are shipping.

|U03J22A0C4S|:
It's zsh by default (for new installs) as of Catalina.

|U03HZ4PT2ER|:
But many Apple tools default to bash/sh for their scripting environments (for historical reasons)

|U03HWD3Q7LH|:
No.  /bin/sh is still bash.  $SHELL is zsh

|U03J22A0C4S|:
Ah, a distinction, thanks.

|U03JQJC0N1L|:
Thanks for the reply! My main concern would be for scripts specifically using the `#!/bin/bash` hashbang if `/bin/bash` were to be removed. There has been some speculation that `zsh` could emulate `bash` (the way that `/bin/sh` could be emulated by `bash` or `zsh` or `dash`), but `zsh` doesn't currently support full and proper `bash` emulation to be able to do that without breaking many `bash` scripts.

|U03HWD3Q7LH|:
For maximal portability, I would recommend ensuring that your scripts use /bin/sh and don't use bashisms.

However, as you point out, many scripts do use /bin/bash, so that will be a tall lift for many folks.  We would certainly not remove bash without understanding that problem space and having a solution.

|U03JQJC0N1L|:
When the default interactive shell was switched from `bash` to `zsh` in Catalina when this scripting environment deprecated was also announced, it was unclear if that meant `bash` was being deprecated as a scripting environment as well or simply being deprecated as a suggested interactive shell to use. So, it hasn't even been super clear if bash usage in scripts has been in a deprecated state for the past years.

|U03JQJC0N1L|:
I really appreciate hearing that you all understand the gravity of removing `bash` and that it would not be done without a way forward. Can I assume that if this were ever to happen in the future that we would get some very explicit advance warning like was done with Python?

|U03HWD3Q7LH|:
Those two events were unrelated.

Yes, we would almost certainly give advanced warning for something as big as that.

|U03JQJC0N1L|:
Thank you so much for taking the time to clarify this information about `bash`!

Any chance for some insight on the future of AppleScript and JXA? I think the ObjC-bridge in JXA is an incredible tool that provides some capabilities in scripts that really can't currently be done any other way without compiled code. And obviously AppleScript and JXAs ability to present basic prompts and interact with other apps is extremely valuable in countless scenarios.

|U03JQJC0N1L|:
I'll choose to hope that silence on AppleScript and JXA is not ominous :rolling_on_the_floor_laughing:

Really appreciate the answers on `bash` and shell scripting!

|U03HWD3Q7LH|:
No, it's not ominous... I just don't have an answer for that part of your question because I don't know and the relevant teams are not represented in the staffers of this lab.  Given that it is Friday, I'd suggest making a forum post about that and post the URL here.

|U03HZ4PT2ER|:
<@U03JQJC0N1L> Consider scripting with Swift if you only share with other developers

|U03JQJC0N1L|:
Thank you! I will make a forum post.

|U03JQJC0N1L|:
<@U03HZ4PT2ER> more considering for scenarios that can run on vanilla installations. It would be very cool if uncompiled Swift scripting could be done out of the box without any dev tools needing to be installed though!

|U03J07WJLRK|:
I think that Swift changes too fast to ever be outside of devtools

|U03HZ4PT2ER|:
<@U03J07WJLRK> You can actually specify the version in the interpreter line, I think

--- 
> ####  With SPM time-slot and this year dub-dub coming to an end I'd like to take this opportunity to thank everyone involved in Swift Package Manager development!  I used SPM in my pet-projects since Xcode 11 and played around with CLI apps before that, and during the last year SPM made a tremendous impact for my client project allowing to do things unimaginable before and iterate on the project with lightning speed. Taking into account the fact that it is a huge 12-years old app, it is mind-blowing how well SPM fit into this puzzle. :blush:


|U03HES8111T|:
Thank you, that is really nice of you to say!  It's so rewarding to hear that all of the hard work is paying off.  We really appreciate the kind words.  Hope you have a great remainder of WWDC.

|U03JEEUJPMJ|:
(Oh 100%! – I used to *hate* dealing with external code in my projects, using third party dependency managers– I felt like I’d sneeze and a tower of cards would come falling. I’m sure a _ton_ of effort went into those third-party dependency managers, but: the first time I added an external package as a ‘first class citizen’ …in the Xcode UI …was *phenomenal*. :partying_face:)

|U03JPFQNX5K|:
This team really took a brave challenge to replace long-running stable parts of our projects like *CocoaPods* and *Carthage*, and I have to say – they took a great direction and after some newborn issues, I can hardly ever remember CocoaPods and its invasive integration process :smile: it's been a great move to contain something that obviously useful to Xcode tooling. Becoming better and better with each release! :star-struck: My only wish would be the possibility to mix ObjC and Swift in a single package due to some project, now it rather forces me to some separation, but nevertheless…great for having it here, along with the erudite guys taking care of it around! :raised_hands:

|U03J22AU6DQ|:
<@U03JPFQNX5K>, but it is totally possible to mix Swift and Objective-C in a single *package*! The limitation is only for targets. Does the approach of having SwiftTarget and ObjcTarget not work for you? If so, I’d really love to know about your setup since it is extremely easy to create targets with SPM. :thinking_face:

--- 
> ####  We are developing a brand new SwiftUI app. Among other things, this includes 3 Swift packages, which were integrated via "Add local...". When developing, this works great for all developers, because these packages are all in the same place in the filesystem (outside the actual project). The repositories of these 3 packages are private GitHub repos.  How do we need to set up both Xcode and Xcode Cloud with these 3 private repo packages to make it work? How can Xcode Cloud access these 3 packages if they were only added locally to the project?


|U03HES8111T|:
In order for Xcode Cloud to have access to the three packages, the Xcode project that uses them needs to have URL references the repositories in which those packages reside.  That will cause Xcode Cloud to check them out after checking out the main repository and before building.

You can still work with those packages locally by putting locally checked-out references to them in the same workspace as the main project in the local file system.  If the workspace contains a local checkout of a package, it will shadow a remote dependency of the same name.  In this way you can work with the three package dependencies locally, but have Xcode Cloud check them out from repositories.

When committing changes to the main project you will want to make sure to also push any required changes to the three packages as well, and if needed, to create new tags for the main project to pull.  You may find it easiest to use branch dependencies for the packages if they are always developed together with the main project and not used from other projects.

The article at <https://developer.apple.com/documentation/xcode/editing-a-package-dependency-as-a-local-package> has some more information about local editing workflows.

|U03K681G98A|:
Oh, thanks Anders! I wasn't aware that I could use both local and URL references to the packages. That sounds good. :+1::skin-tone-3:

|U03K681G98A|:
<@U03HES8111T> I guess the  references must be `<mailto:git@github.com|git@github.com>: ...` URLs, right?

|U03HES8111T|:
Yes, it's a bit subtle, but can be a very flexible way to work with dependencies.  The dependency URLs can be any remote dependencies that Swift Package Manager supports, either `https://` or `git@`.

|U03K681G98A|:
<@U03HES8111T> Well, if I add `<mailto:git@github.com|git@github.com>` URL references (without local ones), Xcode Cloud now says me:

```
Could not resolve package dependencies:
Failed to clone repository git@github.com:MY_COMPANY/MY_PROJECT.git:
Cloning into bare repository '/Volumes/workspace/DerivedData/SourcePackages/repositories/MY_PROJECT-23c66cda'.
fatal: could not read Username for '<http://github.com>':terminal prompts disabled
Failed to clone repository git@github.com:MY_COMPANY/MY_PROJECT.git:
```

|U03JH7BL15L|:
FWIW, I’m getting this same error.

I posted about it to the dev forums as well.

<https://developer.apple.com/forums/thread/707839>

|U03K681G98A|:
Finally I solved the problem and I would like to share my findings. It was very tricky! :slightly_smiling_face:
Have a look at <https://gist.github.com/phranck/4d7c00e4d534d1c4abddc51d70ced81b|this Gist>.

|U03HES8111T|:
Thank you very much for the write-up of the problem and solution here!  If you feel that there were user interface issues in Xcode that contributed to this (and it sounds is if there were!), please do file at ticket on that at <https://feedbackassistant.apple.com/>.  It sounds as if the workflow in Xcode could have been more helpful and saved time in setting this up.

|U03HES8111T|:
I'm glad that you did arrive at a setup that works for you in the end, however!

--- 
> ####  For a workspace setup with multiple projects, is there a way to define a remote Swift package at the workspace level rather than within each project? It seems possible for local packages (drag it to the workspace) but wonder if it’s also possible for remote ones too.   Thank you. 


|U03HES8111T|:
There isn't currently a way to define dependencies on remote packages at the workspace level.  Part of the reason for this is that projects can belong to multiple workspaces (or even no workspace), so when the project is opened on its own or as part of a different workspace, it wouldn't have access to those dependencies.

If the project is built in Xcode Cloud, for example, it needs to be able to check out its dependencies so it can be built.

That said, there is definitely a compelling reason to be able to share dependency declarations in some form.  If you have particular needs or suggestions for how this should be expressed, we would very much like to hear about it as feedback at <https://feedbackassistant.apple.com/>.

|U03J4D51VC4|:
Thank you for your reply Anders and for clarifying some of the rationale behind this. 

--- 
> ####  I know some of my XCTests are fast enough, but some are slow, and I could probably change that. I saw in one session there is a method for profiling individual tests but I wonder if there is something broader. Is there something akin to a table I could get at the end of running all my tests that listed each test with its run time, so I can sort by execution time and then know which individual tests to zero in on first?


|U03HES8U8FP|:
You can look at this in the test summary in the report navigator :slightly_smiling_face:

|U03J4D1FEP6|:
Wat.

|U03J4D1FEP6|:
This has been there all along?? 🫣🥹:laughing:

--- 
> ####  Question for a friend: They've been building toolchains from source for some custom work, but have noticed some source releases have slowed. Specifically LD64 hasn't seen a source release on Github in 15 months. Will we see more regular source drops for the open source components to Xcodes tooling?


|U03HL553PNG|:
Thanks for the note, Colin.  Those drops are indeed a bit behind.  The team is aware of that and working on it but I don't have an ETA for you.  You can also reach out to <mailto:opensource@apple.com|opensource@apple.com> to express your support and interest.

--- 
> ####  We are trying to build some libraries for iOS that will be used by other projects within our organization.  We are finding that you cannot `lipo` binaries built for the Intel x64 catalyst target together in a single package as contains an x64 iOS Simulator target.  Are there plans to be able to do that at some point in the future, or should we just build separate "iOS device/Simulator" binaries and "arm/intel catalyst" binaries?


|U03HHPDDRK5|:
`lipo` is an architecture-level tool, so as you’ve discovered you cannot use it to create a binary with slices for the same architecture but different platforms.

XCFrameworks fill this role. You can learn about using them at these links: <https://developer.apple.com/documentation/xcode/distributing-binary-frameworks-as-swift-packages> and <https://help.apple.com/xcode/mac/11.4/#/dev544efab96>

--- 
> ####  This is probably a feedback request, but is there a way to get testing comments in Xcode to avoiud having to go to the developer portal prior to releasing to testers?


|U03HB5P2UTY|:
There currently isn't a way to do that, but it's a great idea. We'd welcome <https://feedbackassistant.apple.com|feedback> from you requesting this feature!

|U03HT20UKFZ|:
Sweet done!

|U03HT20UKFZ|:
thanks

|U03HB5P2UTY|:
If you wouldn't mind, could you share the FB number with me?

|U03HT20UKFZ|:
FB10163784

--- 
> ####  Are the rules for how Xcode links swift package product dependencies documented?   For cases when package products don’t define a linkage type (i.e auto) it looks like Xcode favours static unless it encounters a dynamic target in the dependency graph of a target. That seems sensible but curious if this behaviour is stable and if it can be controlled?  Thank you :pray: 


|U03HES8111T|:
If a product does use automatic linkage, Xcode will built it dynamically depending on the situation, e.g. if it is used by the app and its test bundle — this is done to avoid duplicated symbols at runtime. In `swift build`, automatic products always build statically.

There isn't currently any way for a client of a library to control whether to build an automatic library as static or dynamic.

--- 
> ####  s it possible to define a key binding in Xcode to trigger a command plugin more conveniently? Same is possible for Xcode Extensions today.  Same question asked here: <https://developer.apple.com/forums/thread/707866|https://developer.apple.com/forums/thread/707866>


|U03HES8SVM3|:
Currently, there isn’t a way to do that. We’d welcome <https://feedbackassistant.apple.com/|feedback> from you requesting it!

--- 
> ####  Are there plans to add Behaviors in Xcode that switch to the Debug Gauges? (Or am I the only one using Behaviors?)


|U03HES8U8FP|:
Can you send us a feedback about this? We would like to see what’s your use case :slightly_smiling_face:

|U03HRR4JA31|:
Feedback was closed Aug 14, 2018.

|U03HRR4JA31|:
FB5713916

--- 
> ####  Is there a way to have some code in my project participate in the preview mechanism outside of SwiftUI. In particular, I'm creating drawing (and rendering) code - one produces images, another produces 3D meshes with textures (SceneKit - I'm slowing adding RealityKit support, but not there yet) - and I'd like to be able to preview the output of a bit of sample to enable the quick-iteration/verify loop for how the code I'm writing is working.  So far I've been doing this with images by dropping the image into a SwiftUI view that only shows an image, which mostly works - but it gets notably more complicated and prone to failure when I'm looking at the resulting 3D object (through a hosting view currently).


|U03HB5N9QBY|:
That sounds like a good use for SwiftUI previews and generally should work. What specific issues are you seeing? (Feel free to file <https://developer.apple.com/bug-reporting/|feedback> for any problems you’re encountering.)

Another suggestion for you would be to try an Xcode Playground. You can use a <https://developer.apple.com/documentation/playgroundsupport/playgroundpage/1964506-liveview|live view> to show your view while working on it.

|U03HVD5Q8DC|:
The biggest issue that I was hitting was breaking the preview mechanism (usually when I screw up and typo in source, breaking the compile) and having to manually restart the whole preview mechanism.

|U03HVD5Q8DC|:
I’ll continue to use a SwiftUI View to front my visualizations for this iteration use case - sounds like the preferred means of getting these these days.

--- 
> ####  I have been using the workaround from this thread [<https://forums.swift.org/t/how-to-link-a-swift-package-as-dynamic/32062|https://forums.swift.org/t/how-to-link-a-swift-package-as-dynamic/32062>] for a long time due to issues if a package is used by multiple frameworks/targets. tl;dr: Create a framework that links all SPM packages I use and then all of my other frameworks/extensions dynamically load that single framework. It seems like there have been some Xcode improvements such that it isn't as necessary anymore, but is there a downside to sticking with it? I think if I stopped most of those packages would have to be dynamic and thus couldn't be dead-code-stripped, right?


|U03HES8111T|:
The approach you are currently using seems like the best approach also in Xcode 14.  I believe you are correct that you would lose dead-code-stripping if you switched to using purely dynamic libraries.

--- 
> ####  When I previously used CocoaPods for my dependencies I would commit the local pods directory into my git repo. I realise there are varying views on this but I wanted to be able to roll my project back to a specific point in the past and know that I was able to build the exact same binary when I needed to. This also protects against an upstream author unpublishing their code etc.   With SPM I have given this up, but wonder if there is a way to achieve the same thing: is it possible to specify the local checkout location of a remote package, so I could have those files committed as part of my git repo again, if I chose to?


|U03HES8111T|:
There isn't currently a way to do this with Swift Packages in Xcode, but that is an interesting idea.  If you have particular idea for how such a feature should work in order to be useful to you, we would love to hear feedback at <https://feedbackassistant.apple.com/>.

An alternative would be to fork the repositories for the dependencies, though it would mean having to pull updates from the upstream repository into the fork at regular intervals.

|U03J4D1FEP6|:
Will definitely file a feedback. I think this is quite straightforward to implement, if Xcode was willing.

--- 
> ####  Is XCUITest supported on Xcode cloud?


|U03HES8U8FP|:
Yes! You can use XCUITest in Xcode Cloud

|U03J4EW62N8|:
Thanks!

--- 
> ####  Not a question, but wanted to pass my thanks :pray: for the scheme selector pop up (from Xcode 13?) when running tests :ok_hand:. It works wonders in large workspaces with several hundred schemes!


|U03HES8CXHB|:
Thank you for your great feedback, Kassem. We’re glad it has been helpful!

--- 
> ####  Is there a way with package plugins to make linters / formatters run automatically in some cases? Maybe by users setting up a behavior or when a file is saved etc?


|U03HES8111T|:
There isn't currently any support for that, but we would love to hear feedback about what kinds of cases would be useful to you.  You can file feedback at <https://feedbackassistant.apple.com/>.

|U03HVF9E2BG|:
sounds good

|U03HVF9E2BG|:
i think the clearest case is autoformatting on save if the user likes that workflow

|U03HES8111T|:
Thanks, that makes a lot of sense as one of the events to cause it to run.

|U03HVF9E2BG|:
i filed FB10164124

|U03HVF9E2BG|:
another idea someone had here is if we could hack this into build plugins somehow, although i haven't looked around at what's possible there. today we use a run script to automatically run linters when building and output in the right format so xcode shows it inline

|U03JETJ7UU9|:
I've been trying to think through this exact same problem <@U03HVF9E2BG>

|U03HN9FSVT9|:
@Keith Couldn’t you trigger the linting/formatter using a tool like fswatch (used for watching for changes) and the run your linter? Or would that be to cumbersome?

|U03JETJ7UU9|:
Would it be possible for a build plugin to use git to find changed files and then run linting on it?

|U03HN9FSVT9|:
Could actually be awesome if the plugin context could provide you with a filelist of changed files since previous build.

|U03HN9FSVT9|:
(But that is properly out of scope for the plugin API)

|U03HVF9E2BG|:
one of the examples did the git file detection for sure

|U03HVF9E2BG|:
I don't think a fswatch solution would work to also surface the issues in Xcode?

|U03JETJ7UU9|:
But yeah running a plugin when a file is saved would be awesome. I have codegen that generates .swift files for .graphql query files. It would be so cool to be able to save a .graphql query file and then SPM generates the .swift files instantly

|U03HES8111T|:
Build tool plugins could definitely be used to run the linting at the start of the build (this is what the example in the State of the Union video did).  The API provided for plugins currently provides the structure of the package or project, but not state information — the example used `git status` for that information.

|U03HN9FSVT9|:
Oh yeah <@U03HVF9E2BG> … you are right that wouldn’t do it for you.

|U03HES8111T|:
For code generation, it's usually a good idea to generate the code to intermediate locations, unless it is intended to become part of the checked-in code.

|U03HVF9E2BG|:
<@U03HES8111T> would mutating files with a formatter in the build tool plugin run cause issues? in the past someone recommended we shouldn't do that in run script phases. I suppose we could define all inputs and outputs, but if we just wanted to add "all files" there that might be cumbersome

|U03HES8111T|:
It probably would, and also the sandbox doesn't allow mutating the project / package files from a build tool plugin.

|U03HN9FSVT9|:
<@U03HES8111T> - but that would assume your project is stored in a git repo (which might not always be the case)?

|U03HES8111T|:
Yes, it would — that's a good point.

|U03HES8111T|:
There is a lot of room for extending the plugin support, and it would be great to have follow-on discussions in the Swift Packages section of <https://forums.swift.org> about the direction the plugin capabilities should take to make them more useful broadly.

|U03HN9FSVT9|:
Cool - I’ll have a peak at the forum and thanks to the entire SPM team for bringing the plugin API to SPM.

|U03JETJ7UU9|:
Yeah agreed. This plugin API might be my favorite thing announced this year. We have a ton of command line scripts that we can drop in favor of this approach

|U03J01RLBBR|:
This ^^^

|U03HN9FSVT9|:
Totally agree - best SPM feature

--- 
> ####  I’m currently migrating an “old” ObjC codebase from a Xcode Framework to SPM. As I understand it, I need an include folder in my target which includes all the public headers (in my case a subfolder MyFrameworkKit) and the umbrella header (if I need a special umbrella header). I can also provide a (public and private) module map (which I have to in my case).   I have difficulties providing a proper #include/#import  for all my private and internal headers when trying to create tests for those classes in my test target (as they are not part of the umbrella header which they rightfully shouldn’t). But how do I then access the internal/private classes in the framework from the test target?  Additionally it seems like the private module header isn’t removed from the release build - shouldn’t it be removed or?


|U03HES8111T|:
That's correct, for C targets in SwiftPM you do need a folder called `include` in the target (though the name can be customized in the package manifest).  You do not actually have to provide a module map — SwiftPM and Xcode will create one from standard header layouts.  But if there is anything custom, you can include a module map.

I believe you should be able to have a private module that includes the header that your tests need, and to include that by having them import the private module.  There is some more information here: <https://clang.llvm.org/docs/Modules.html#private-module-map-files> but I am unsure of the details of private module maps.

Regarding the build artifact, that does sound like a bug if it isn't being removed.  Would you mind filing a bug report at <https://feedbackassistant.apple.com/> about that.

If there isn't a way to do what you want to with private module maps, then it would be great to have a <https://forums.swift.org> discussion about that as well (since the SwiftPM support for C is entirely in the open source).

|U03HN9FSVT9|:
Thanks a lot for your answer <@U03HES8111T> - I’ll have to look more into the private module map (it is relative new to me). Would the private module then require an additional folder in the include folder (which I’m currently doing) or could that be avoided?

|U03HN9FSVT9|:
I’ll have to be 100% sure that I understand the private module map and how it affects the   produced artifact before I file a bug (and avoid you wasting  time chasing ghosts) :slightly_smiling_face:

--- 
> ####  To make apps, built in Xcode on Apple silicon, run on older versions of macOS; in this post (<https://developer.apple.com/forums/thread/673323)|https://developer.apple.com/forums/thread/673323)> 'eskimo' says to add `--digest-algorithm=sha1,sha256` to `codesign`  Is there somewhere in Xcode that I can add this, so that I can archive and export &amp; have `codesign` use these behind the scenes?


|U03HB5P2UTY|:
You can add these to your `OTHER_CODE_SIGN_FLAGS` build setting and the archive action will use them. However, there is no way to add flags to the `codesign` command when exporting. Please send us <https://feedbackassistant.apple.com|feedback> if this is something you'd like to see!

|U03JEEUJPMJ|:
Ah, so - just to clarify - I could archive, and then hit `Distribute App`, and:
• `Copy App`
• but not any of the other options that involve re-signing? (Developer ID / upload to App Store Connect etc)
&amp; so, to ‘Developer ID’ sign: I’d archive, export via `Copy App`, then need to use the `codesign` program?

--- 
> ####  For a build phase that modifies a file in derived data (`$(TARGET_BUILD_DIR)/$(INFOPLIST_PATH)`), what should the input and output files be set to so Xcode knows to run the phase correctly.  Currently we just set the input files to the plist path which causes it to run for all builds since there's no output file. It's not a big deal cause the scripts are fast, but it's just extra work that isn't needed. Setting the output file to the plist path as well causes warnigns and errors.  warning: unexpected mutating task ('PhaseScriptExecution ...') with no relation to prior mutator ('PhaseScriptExecution ...') error: invalid task ('PhaseScriptExecution ...') with mutable output but no other virtual output node


|U03HHPDDRK5|:
What is your use case for modifying Info.plist using a script phase? To ensure dependencies between tasks are tracked correctly in builds, run script build phases generally should not modify files in place.

Could you instead have your script generate the Info.plist file, and have `INFO_PLIST_FILE` point to the generated file?

|U03KH907MME|:
We do a couple things
• Update the build number using the build number from CI.
• Copy the universal link domains from the entitlement to they’re accessible at runtime.
• Remove pinned domains for development environments in release builds.

|U03KH907MME|:
That’s an interesting idea about changing `INFO_PLIST_FILE`. So the script build phase would need to be above `Compile Sources` and it’d have input of our plist file in `$SRCROOT` and output to the same path as `INFO_PLIST_FILE`.

I think part if our issue is we have multiple build phase changing the plist when they should probably be combined into one since only one can work on the file at a time.

--- 
> ####  Tried to build this code as Apple engineer in Xcode 14 beta 1. It seems not able to build, did i miss something?  ``` // this will be back deployable struct AnyP&lt;T {   var value: any P&lt;T } ```  And since it is able to backport but couldn't use a type without Swift 5.7 runtime.  So does it mean if we are using it as a type argument and run app on iOS 15 (no 5.7 runtime). We will have a runtime crash or do we get compiler warning?


|U03J7H72X5W|:
The `protocol P&lt;T&gt;` syntax is not back-deployable to 5.6, so you need to wrap those definitions in `#if swift(&gt;=5.7)`. <https://github.com/apple/swift-evolution/blob/main/proposals/0346-light-weight-same-type-syntax.md#annotate-regular-associatedtype-declarations-with-primary> has some more information about that.

But you should not receive any runtime crashes. The compiler or linker should throw errors for these issues.

|U03HZ3L66QM|:
Thanks for the clarifying the syntax part. I think my second question can be rephrased into :
Can I run this code on `iOS 15` if I build with `Swift 5.7`?
```
struct AnyP&lt;T&gt; {
  var value: any P&lt;T&gt;
}
let anyP: AnyP&lt;Int&gt; = ....```
and even this
```func foo&lt;T&gt;(_ bar: T) { ... }

foo(any Collection&lt;Int&gt;) // can I run this on iOS 15 with built on Swift 5.7? 
```


--- 
> ####  I have a SwiftPM package I'm using both in other SwiftPM packages &amp; in an iOS project. Unfortunately I've had to use CocoaPods to import it into my iOS app's Xcode project so far because the binary targets get super confused when using Xcode's SwiftPM integration.  Any way to debug SwiftPM packages that work fine from the commandline (Linux + macOS) but not when imported into an Xcode project?  An example of the sort of error I see: NSObjCRuntime.h:523:1: Expected identifier or '('  ^ happens while #including a header from the binary target &amp; says it can't build a completely separate module in my app.


|U03HESABNTX|:
It's possible that your package is being built with a different set of flags when using Xcode as opposed to the SwiftPM command line tool. I recommend comparing the compile commands in each build log to look for any differences between the two which may be relevant, especially flags like `-D` which impact conditional compilation. If you're unable to determine the source of the failure, please file a bug report including both the SwiftPM and Xcode build logs so we can investigate further.

|U03J1UX2CQK|:
Thank you! Will do.

--- 
> ####  Weird question - why did the defaults command get quietly (in the man page) deprecated in favor of plistbuddy and then silently un-deprecated later?


|U03HL553PNG|:
Hi Sara.  We don't think the `defaults` command should be deprecated either!  If you're still seeing that message in the man page, can you please <https://feedbackassistant.apple.com/|file a feedback> for us to take a look?

|U03JE7PBQPP|:
Early versions of Catalina deprecated it and I rewrote all the scripts for plistbuddy. But now it's apparently not deprecated and I'm debating re-writing the scripts again or walking quietly away :smile:

|U03HL553PNG|:
Interesting!  That's very possible but we just didn't hear about it here in Dev Tools.  I think asking that question through feedback should get your question to the right folks.

|U03HESHGASH|:
In general, the rule of thumb to follow is use `defaults` for user defaults, and use other tools (plistbuddy is fine) for non-defaults plists.

--- 
> ####  In deinit, the removal of an observer object reports an error for an actor.  Is there a better solution available?


|U03HWDCSCHX|:
Hi <@U03KC4LFL64>, could you provide some additional information to help us answer this question:
• What is the text of the error you're hitting?
• Is this error occurring at build time or at run time?

|U03KC4LFL64|:
Cannot access property ‘observer’ with non-sendable type Any? From non-isolated deinit

|U03KC4LFL64|:
The code is if let observer = observer {…} to remove the observer if it still exists.

|U03HWDCSCHX|:
Okay, so the big document that has all the details here is <https://github.com/apple/swift-evolution/blob/main/proposals/0327-actor-initializers.md>

But to offer a more targeted explanation of events: `deinit` is the end of the road for an actor or a class. It is called after the final reference to the actor has disappeared, which can occur at any time and on any thread. So `deinit` is _never_ actor-isolated. So the correct way to clean up is always to copy out any resources from the actor and clean up those copies. Swift is warning you that you're trying to clean up by copying a type that it does not know can be cleanly shared across isolated contexts.

|U03HWDCSCHX|:
If you can, please use a specific `Sendable` observer type. otherwise, you can try to schedule a set of `@Sendable` cleanup blocks in an isolated context and run those cleanup blocks in `deinit`.

|U03KC4LFL64|:
Ok… thank you for the ideas.  And thanks to everyone at Apple for another great WWDC!

|U03HWDCSCHX|:
Really, `deinit` isn't the best place to cleanup, especially if you need guaranteed resource destruction. I would create some kind of `invalidate` method that you call from `defer` or some other known point where the system has asked you to clean up resources.

|U03KC4LFL64|:
That is my plan I believe.  It was always just a fail safe in most cases.

--- 
> ####  Are there any plans to make SwiftUI code accessible from Unit Tests (like the 3rd party lib ViewInspector does)?


|U03H36U6JHM|:
Thanks for the question! We're always working to improve the tools our developers use. If there's something that isn't working for you, please let us know using the <http://feedbackassistant.apple.com|Feedback Assistant>.

--- 
> ####  I am working on a project where all functionality is distributed across SPM packages, including SwiftUI views. All works great, except I cannot figure out how to have ‘Preview Assets’ in packages, so that SwiftUI view previews in those packages could include resources like images for previews. Is there a way to use Preview Assets with SPM packages and SwiftUI previews?


|U03HL553PNG|:
It looks like this is not currently supported as best as we can tell.  Can you please <https://feedbackassistant.apple.com/|file a feedback> detailing your use case so we can consider that?  Thanks!

|U03J30J1S3D|:
This workaround was recently discussed in <#C03HX19UNCQ|>. It has served us well too for several months now. <https://dev.jeremygale.com/swiftui-how-to-use-custom-fonts-and-images-in-a-swift-package-cl0k9bv52013h6bnvhw76alid|https://dev.jeremygale.com/swiftui-how-to-use-custom-fonts-and-images-in-a-swift-package-cl0k9bv52013h6bnvhw76alid>

--- 
> ####  Not a question but just wanted to say thanks... I've been developing Mac applications since 2007 and iOS from day one... Xcode has continues to improve over the years and I am very excited about the updates to Xcode 14. Thank you!


|U03HL553PNG|:
Thank you so much for the nice note, Ryan!  We all work really hard on Xcode so glad to hear that you're as excited as we are. :slightly_smiling_face:  Thanks for brightening our day!

|U03J01RLBBR|:
The only other thing that made me happier was the addition (and improvements this year) of DocC. :rolling_on_the_floor_laughing:

--- 
> ####  What's the proper way to separate keywords when uploading an app? The help tooltip says to "separate keywords with an English comma, Chinese comma, or a mix of both" but can you please give an example of a properly delineated string of keywords?  "termA, termB, termC, … "   or "termA,termB,termC,…" with no spaces in between?  What about for foreign characters like "キーワードA、キーワードB、…"   Do we use the local comma character:  、 ?


|U03HB5P2UTY|:
Hi <@U03J07GGVDK>, spaces are optional when including keywords so both of the examples you provided would work. As indicated in the tooltip, Chinese commas are also supported so those commas (and the spaces after them) would be accepted. More information on keywords and other App Store metadata can be found here: <https://help.apple.com/app-store-connect/#/devf29afbb74>

--- 
> ####  Hi,  I know Swift is designed mostly to build apps on Apple ecosystem. I would like to ask if there is initiative to make it more algorithm problem solving language. What I mean is, as a being as an enthusiastic problem solver, many talented competitive programmer or skilled Leetcoder suggest that to not use Swift. Especially in String manipulation while solving problem, is not so straightforward as Java or Phyton and there is no big community support. Do you guys have any plan to make Swift more algorithm problem solver friendly in the future?   Or any plan to expand Swift Problem solver community?   Thanks.


|U03HESHGASH|:
hi! We have a few potential solutions here. In particular, you may find the Algorithms and Collections packages useful:

<https://github.com/apple/swift-algorithms>
<https://github.com/apple/swift-collections>

and the new Regular Expression features announced this week may help with String manipulation.

|U03KWLVQP96|:
Thank you for answer!

--- 
> ####  I wish Swift cross platform, not just a Apple language, Why not support Windows,Linux as well.  Dart lang, Rust lang, Golang can even support both platform, but currently Swift lang is just ugly to support with windows, linux. that's should not be ...


|U03H36U6JHM|:
Swift officially supports Ubuntu Linux! :partying_face: The community has also worked hard to add Windows support, and our CI is set up to automatically build every PR for both platforms. For more information on the platforms we support, see the <https://www.swift.org/platform-support/|Platform Support> page on <http://swift.org|swift.org>.

The Swift compiler, runtime, and standard library are open source, so if you notice an issue using Swift on Linux or Windows, a PR would be welcome! If you see a way to improve the Swift user experience, please let us know on the Swift forums.

|U03JELM0ZNV|:
If you want an IDE on the other platforms, I'm a huge fan of the Jetbrains products. I use AppCode on Mac as my main editing environment, although XCode in recent years has closed the gap a lot.

<https://plugins.jetbrains.com/plugin/8240-swift>

|U03JELM0ZNV|:
I have a massive body of Swift to port to Android starting later this year. I'm hoping to keep most of it still working, rather than back-porting to C++.

|U03H36U6JHM|:
I know some folks in the community have been working to make Swift happy on Android. :slightly_smiling_face: Hopefully you'll be able to take advantage of that effort!

|U03JRQHF7T2|:
I’ve used Swift for some linux utility (command-line) projects very successfully. Gets built via Github Action on a linux node, then packaged up into a ubuntu-based docker container.

So while there is probably room for improvements, I can honestly say it works well for me.

|U03H36U6JHM|:
There's _always_ room for improvement! If there's something specific you think can be improved, the Swift forums and bug tracker are great resources.

|U03JRQHF7T2|:
Swift is now my favorite tool for scripting (has replaced bash!) and creating CLIs

|U03JELM0ZNV|:
This may be a bit cheeky, but my porting to Android is going to involve wrapping the Cocos2D-X game engine to expose a subset of SpriteKit APIs, so I can reuse my code. Once (if?) I get beyond a trivial proof-of-concept, I'll make that bit an open source project.

|U03H36U6JHM|:
I haven't tried to write any gaming graphics code since the mid-1990s, so I can't really speak to the effort there (although I appreciate a good sprite.) But it sounds like an interesting project. Please share it with us in the <https://forums.swift.org/c/community-showcase/|Community Showcase> ("didn't he mention the forums already?") when you post it! :smile:

|U03JELM0ZNV|:
I _started_ with a common C++ core using Cocos2D-X but once I moved from Objective-C++ to Swift, I dearly wanted to stay in that world. Also, as 99% of my app runs in iMessage as an extension, went conservative picking Apple tech over 3rd party as much as possible.

|U03JELM0ZNV|:
Note _massive body of Swift_ is about 20KLoC, which I reckon would be near double that in C++. It's enough to seriously give time to such wrapper projects rather than maintaining two codebases (solo founder).

|U03JSFUKL2U|:
Community Showcase is very great topic!

|U03JRP6568Y|:
We use Swift for some of our utility libraries - and actually sometimes do our CI inside of Linux Docker images. We sometimes manually test on Windows as well. The command line parser library is also a big help. Swift Packages work pretty well on Linux and Windows. Would love to see interop with Java on Android but thats a whole different deal...

|U03H36U6JHM|:
If that'd be useful to it, raise it on the Swift forums! I am sure the folks working on Android support would be interested in discussing it.

--- 
> ####  When building SwiftPM Library for iPhone Simulator there are now issue, but when trying to preview a view in the target I see the following issue: error build: multiple configured targets of 'MyPackageName_MyTargetName' are being created for watchOS Simulator  What might be the reason of such a behaviour?


|U03HL553PNG|:
The rest of the team is off in 1:1 labs right now so they asked me to pass along that this doesn't look intended.  Would you mind <https://feedbackassistant.apple.com/|filing a feedback report> for that issue so we can take a look?  You may also want to post on the <http://developer.apple.com/forums/|Developer Forums> to see if someone else is having the same issue.

--- 
> ####  I maintain a popular project that integrates with a vendor swift framework. We'd like to call some and debug some swift functions in the framework. Unfortunately there is no .swiftinterface for it. Is there any tools or improvements in this Xcode release to go from stable ABI - .swiftinerface?


|U03HWDCSCHX|:
There's no official tool for this. It would be very fun to build it yourself - it's essentially class dump for Swift which you can accomplish by reading existing runtime metadata embedded in the binary <https://twitter.com/CodaFi_/status/1113576538446991360?s=20&amp;t=6I1HWyERO40FYAgfLdamuQ>

|U03HWDCSCHX|:
Also note that swiftinterface files are entirely human readable, so you can write one yourself too!

--- 
> ####  We have an iOS app that has some SPM dependencies which in turn have SPM subdependencies. Some of these are shared: ie  App - FrameworkA - FrameworkB, FrameworkC          -FrameworkD - FrameworkB, FrameworkC  The app builds find locally and on CI but during archive and submission we see errors where the bundle has duplicates of FrameworkB and FrameworkC. We had to add a Post Build Action on release like so: ``` #!/bin/bash  # Source: <https://forums.swift.org/t/swift-packages-in-multiple-targets-results-in-this-will-result-in-duplication-of-library-code-errors/34892/67|https://forums.swift.org/t/swift-packages-in-multiple-targets-results-in-this-will-result-in-duplication-of-library-code-errors/34892/67>  APP_PATH="$ARCHIVE_PATH/Products/Applications/$<http://PRODUCT_NAME.app|PRODUCT_NAME.app>"  # Nested Frameworks in Frameworks cd "${APP_PATH}/Frameworks/" for framework in *; do     if [ -d "$framework" ]; then         if [ -d "${framework}/Frameworks" ]; then             echo "Delete nested frameworks from ${framework}/Frameworks/"             rm -rf "${framework}/Frameworks"         fi     fi done ```


|U03J7H4LJ5N|:
Could you please clarify if you are still seeing this issue in Xcode version 13.4 or newer (Xcode 13.4 did include a fix for this type of issue)? If yes, filing a bug report at <https://feedbackassistant.apple.com> would be greatly appreciated.

|U03JETJ7UU9|:
Hey <@U03J7H4LJ5N>! (We met at the event on Monday :smile:). Let me try again with Xcode 13.4 and see. When we put this fix in place it was months back and we haven't really revisited it since.

|U03JETJ7UU9|:
Hmm that's weird... I just tried with 13.3.1 and I don't see any of our in house frameworks in the `Framework` folder after archiving? Maybe they were all statically linked? Downloading 13.2 to try again

--- 
> ####  I had a great lab yesterday (thank you, Apple!) for trying to resolve an issue in Xcode 14 beta 1 that my project gets when trying to build; `Undefined symbols for architecture arm64:`.  I went back and confirmed that I can build with no issue on Xcode 13.4.1, so I'm wondering if there are any docs outside of the release notes that might help me in figuring out why I'm getting such an error only in Xcode 14.


|U03J7BXV8KA|:
That sounds like one of your dependencies might be a static library which is missing an arm64 slice. Is this for watchOS, or a different platform?
