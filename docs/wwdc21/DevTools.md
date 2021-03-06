# DevTools Lounge QAs
#### by [paul](https://twitter.com/squarefrog)
---

## 🗓 Tuesday

---

> #### Hi, thank you for organizing this! I have a question about CI, in regards to Xcode Cloud. I remember having troubles (no "std output" was printed anywhere) running certain scripts in Pre-build and post-build actions in Scheme editor when I was using Xcode Server & Bots back in 2015. The question is shall we rely on pre-build action for, say, running things like `pod install` and other pre-build setup w/o which the project cannot be run?

>>Xcode Cloud supports custom CI-specific build scripts that are distinct from pre-build action scripts in your Xcode scheme. The logging from these scripts is captured and made available in your workflow results. See some documentation about this here:
https://developer.apple.com/documentation/xcode/writing-custom-build-scripts

---

> #### Will XCode Cloud allow you to work with a locally hosted (on my personal network) git repo?

>>Xcode Cloud has first-class support and deep integration with both cloud-based source control providers — GitHub, Bitbucket, and GitLab — and on-premise source code providers; GitHub Enterprise, Bitbucket Server, and GitLab Self-managed.
https://developer.apple.com/documentation/xcode/source-code-management-setup


---

> #### First thanks for creating Xcode Cloud it seems so promising!
I watched all the videos so far, I was curious to know if there would be a way to retry particular failed test or not?

>>Xcode Cloud will allow you to re-run a failed job. Which tests get run will depend on how you configured your test plan, and if you enabled a test repetition mode (New in Xcode 13!) in your test plans. Xcode Cloud always respects your test plan/scheme and workflow configuration


---

> #### Would it be possible to get the dSYM file from XCode Cloud builds? Download dSYM option is not available on App Store Connect if we build without bitcode enabled. It would be nice to have Testflight crash reports be symbolicated, and still have access to the dSYM file to use with other tools.

>>What I found is that as long as Xcode outputs a dSYM, it will be included in the artifacts.  For more advanced workflows, you could use Xcode Cloud’s environment variables with a custom build script to extract symbols from an archive and upload them to a place of your choosing.
More specifically, take a look at `CI_ARCHIVE_PATH` and `ci_post_xcodebuild.sh`.

---

> #### Will Xcode Cloud be available to teams enrolled in the Apple Developer Enterprise Program?

>>No. Xcode Cloud is initially supported for Apple Developer Program teams. You can read more about requirements on this page:
https://developer.apple.com/documentation/xcode/requirements-for-using-xcode-cloud

---

> #### Will there be webhooks available with XCode Cloud for build, test, and distribute status updates?

>>You can find documentation about Xcode Cloud’s available webhooks below:
https://developer.apple.com/documentation/xcode/configuring-webhooks-in-xcode-cloud


---

> #### I have a question around testing - I was trying to figure out a way to test scenarios around IAP family sharing, either in the simulator using App Store Config files, or even in Sandbox on device.  I can't seem to find a way to simulate something like having a purchase made available via a family purchase made by someone else, or to simulate having a purchase revoked via the head of family disabling family sharing (or someone leaving a family).  Would testing in sandbox on two devices that were set up with family sharing be able to test these scenarios?  Was also wondering about generating server to server notifications for those events as well, but even just being able to test in Xcode would be helpful.

>>Have you been using StoreKit to test this? This documentation seems related to what you're asking for: https://developer.apple.com/documentation/storekit/original_api_for_in-app_purchase/supporting_family_sharing_in_your_app

There's also a great session from WWDC2020 around StoreKit Testing with Xcode: https://developer.apple.com/videos/play/wwdc2020/10659/

> Thanks, I've develop my code to handle the scenarios outlined in that document, I'm more wondering how I can trigger those in simulator testing...  for example, if I'm running in the Xcode simulator, is there any StoreKit testing feature that would trigger the ` paymentQueue(_:didRevokeEntitlementsForProductIdentifiers:)` method of `SKPaymentTransactionObserver`?  I'd like to be able to test my code to make sure my handling of that method works correctly.  That document also mentions a REVOKE server to server notification, when testing in Sandbox is there any way to generate that event?

Thanks for the followup.  For the server to server notification, unfortunately not.  Please file a feedback ticket for us to add that functionality (and add a followup here with the FB# if you have time to do so now).   For the first case, yes!  After a purchase is made, open the StoreKit Transaction Manager (Debug > StoreKit > Manage Transactions), you can select a transaction and refund it.  This will cause `paymentQueue(_:didRevokeEntitlementsForProductIdentifiers:)` to be called within your app.


---

> #### Hi! Congrats for your work! Demo looks awesome can't wait to test it myself.
Will Xcode Cloud allow us to use classic open source tools in the Swift ecosystem (like swiftlint or sourcery) in our workflows ? How will version dependency be managed ?

>>Yes, Xcode Cloud has ways for you to use open source tools in your workflows, so you can do things like lint or statically generate code in the cloud if you wanted to.

Check out this documentation on writing custom scripts for Xcode Cloud to see how to get started!

As for how your versioned dependencies will be managed, that would be handled by whatever dependency manager your project uses. So if you use Swift Package Manager, then your build step would resolve the dependencies and versions according to how you wrote your Package.swift  file.

---

> #### Will Xcode Cloud support building projects with third party dependencies and if so which dependency managers will be supported (Cocoapods, Carthage and Swift Package Manager)?

>>I believe you can use dependency managers using custom build scripts. Here is some documentation for you! https://developer.apple.com/documentation/xcode/writing-custom-build-scripts

See also:
https://developer.apple.com/documentation/xcode/making-dependencies-available-to-xcode-cloud


---

> #### Could Xcode Cloud be used as a platform to eventually add distributed build/cache systems? We're very interested in the ability to do use this to speed up developer builds. Build systems like Bazel have powerful capabilities here, but with poor Xcode integration and increased complexity for developers. We would love to see this problem solved natively using Xcode and is a major pain point.

>>Thank you for the feedback, please feel free to file this with Feedback Assistant as well.

---

> #### Can Xcode Cloud run UI-tests in parallel?

>>Xcode Cloud can parallelize tests among all the test destinations configured in your workflow, so yes, it can run UI tests in parallel

---

> #### Will it be possible to quickly download all the screenshots from the UI tests? This can be useful for submitting screenshots to the App Store listing. Perhaps this can even be automated right from the flow.

>>There are several ways to accomplish that: You can download the result bundle from Xcode Cloud and open the result bundle locally, or open it directly in Xcode.
1. From the familiar outline mode you can export the images you want by selecting them and clicking on the paperclip icon in that activity.
2. In gallery mode you can easily select multiple images and drag them to Finder or elsewhere.
3. You can automate this using xcresulttool. For an overview of xcresulttool see https://help.apple.com/xcode/mac/current/#/devc38fc7392


---

> #### Will Xcode Cloud have the concept of permissions? I.e Restrictions on who can create, edit & trigger workflows.

>>Depending on a user’s role in App Store Connect, they may be able to put restrictions on Workflows. You can find the documentation about that here: https://developer.apple.com/documentation/xcode/developing-a-workflow-strategy-for-xcode-cloud

Here is some information about App Store Connect role permissions: https://help.apple.com/app-store-connect/#/deve5f9a89d7


---

> #### When I'm working directly on a Swift package without a project file, creating a new unit test case file always uses the ObjC template, and it appears this is still true in Xcode 13. Is this something I've done wrong, or should I file a feedback?

>>Please do file a feedback about this issue on https://feedbackassistant.apple.com. Thank you for bringing this to our attention!

One workaround is to create new test case files from the Test Navigator. Use the “+” button at the bottom-left corner of the Test Navigator, select “New Unit Test Class…“, and then select Swift from the Language selector dropdown.

Another workaround would be to create the file in the Tests directory using the basic “Swift File” template, and then add the code for the new XCTestCase subclass yourself.


---

> #### Will Xcode Cloud integrate with Gitlab?

>>Xcode Cloud has first-class support and deep integration with both cloud-based source control providers — GitHub, Bitbucket, and GitLab — and on-premise source code providers; GitHub Enterprise, Bitbucket Server, and GitLab Self-managed.
https://developer.apple.com/documentation/xcode/source-code-management-setup


---

> #### First of all, Xcode Cloud looks very promising! Thanks for all the great work that has been done around it.
I have one question that I couldn't resolve by going through the docs though (sorry if it's already answered there). Basically, I'd like to know if workflows can be triggered on demand through an API the same way it can be done with some App Store Connect actions.

>>Yes, Xcode Cloud will be fully integrated into the App Store Connect public REST API. You will be able to start new builds programmatically.
The public REST API integration is not immediately available today, but will be released during the summer.

For context on one typical use case: apps that have not only a frontend component but also have backend server components require integration testing when the backend systems change. When the backend system has passed server side tests, you can set up programmatic triggers using the API to invoke Xcode Cloud builds and tests from an upstream pipeline using a specified staging server.

> Will a workflow be always tied to an App Store listing? Can a project be built (using REST API) without its app ID being registered in App Store Connect?

Yes a workflow is always associated with an App listing in App Store Connect. Creating a workflow in Xcode streamlines the creation of App Store Connect records. If the App Store Connect record already exists, then these will be associated automatically, based on the bundle ID in your project.


---

> #### For XCode Cloud and releasing to TestFlight, how do you capture "What to Test" for what's released to the user?

>>Once Xcode Cloud has successfully uploaded your build to TestFlight, you can go to the TestFlight tab in App Store Connect to enter that information


---


>Xcode 12.5 has a serious bug. When I run app via cmd+r, connecting debugger to a simulator takes about 1-2 minutes and debugserver process use 100% CPU. Do you plan to update Xcode 12.5 to 12.5.1 or 12.6 with fixing this issue? Xcode 13 doesn’t have this issue but I am unable to use Xcode 13.

>>Xcode 13 Beta should address this performance problem seen when running on recent versions of macOS Big Sur.


---

> #### My question is this: could you please share some of the best practices for CI/CD at Xcode projects? Im sure Xcode Cloud will be my best friend in a future, but if we imagine we don’t have Xcode cloud : )

>>For general, self maintained CI/CD xcodebuild is your best friend. The "Testing in Xcode" session from WWDC19 hits on some best practices that may be useful to you: https://developer.apple.com/videos/play/wwdc2019/413/
Some points I'd like to highlight from the session:
- The testing pyramid, to help strike the right balance between unit, integration, and UI tests
- Using test plans efficiently for the variables you want to change between test runs
- What configuration works best for your needs when planning CI/CD set up. At times it is useful to have a separate builder machine from test runner, in which case you'll break your executions into build-for-testing and test-without-building, but it really depends on your use case


---

> #### Will Xcode Cloud support some sort of secret management? i.e third-Party tokens, AWS creds

>>Yes it does! See the Custom Environment Variables section on this page:
https://developer.apple.com/documentation/xcode/xcode-cloud-workflow-reference

You can add secret environment variables to a workflow. They are "write-only", meaning that once set, secret environment variables cannot be read again from the workflow editor. Secret environment variables are encrypted in transit and at rest, and used only on the ephemeral build environment. As such they are suitable for AWS keys, passwords, and API tokens.  You can also add "non-secret" environment variables, which can be read and edited from the workflow editor.

---

> #### Does Xcode Cloud support custom build tools? for instance, to use Xcode Cloud workflow to run Bazel build of iOS project?

>>That’s not currently supported, but you may want to give it a try to see if you could get it to work. That’s great feedback though so please file a report on feedbackassistant.apple.com!


---

> #### How long will builds take on Xcode Cloud compared to building locally, for example using an M1 Mac Mini?

>>Build times will depend on a number of factors, like the size of your project, build settings, workflow settings, custom build scripts, and much more. I would recommend reading these articles:

- https://developer.apple.com/documentation/xcode/developing-a-workflow-strategy-for-xcode-cloud
- https://developer.apple.com/documentation/xcode/xcode-cloud-workflow-reference

> What type of machines will be available to run builds on? Will there be tiers in terms of performance?

There are currently no performance based tiers.


---

> #### How to join Xcode Cloud beta program now? we can try it ASAP :-)

>>You can join the waitlist here:
https://developer.apple.com/xcode-cloud/beta/


---

> #### Is there any tools within Xcode and Xcode Cloud that would allow for an easy monorepo setup? My team has two internal libraries that are used in a tvOS and two iOS applications so we are looking to move them all into an easy repo for ease of maintenance but we are unsure if there is anything available to help ease the transition.

>>I don’t believe Xcode nor Xcode Cloud have a way to merge multiple repos into a single repo, but there are a number of tools available to make dependencies between your projects easier to work with. I would highly recommend this talk from [WWDC 2019 on Swift Packages](https://developer.apple.com/videos/play/wwdc2019/408/) and also this guide on [managing dependencies in Xcode Cloud](https://developer.apple.com/documentation/xcode/making-dependencies-available-to-xcode-cloud).


---

> #### xcodebuild often returns error 65 without clear explanation. Sometimes it seems that it could be the simulator failing to boot, sometimes the test results fail to be collected, but the error messages are always a source of frustration as it's very hard to understand the problem. Furthermore, these errors are often sporadic and often happen on CI, where it's even harder to fix. What can we do to tackle this issues? Are there plans to improve xcodebuild error reporting?

>>If you encounter xcodebuild errors that are unclear, or indicate that the failure is a system error rather than a problem with your own project, please file feedback on https://feedbackassistant.apple.com and attach the .xcresult bundle from the test run for investigation. In many cases, additional helpful diagnostics can be captured by running xcrun simctl diagnose while the simulator you were targeting is still booted.
We are always looking for ways to make the errors produced by xcodebuild clearer and more helpful, and feedback from developers is a very important part of that process!


---

> #### Will there be a separate cost for Xcode Cloud? It seems like there is a lot of resources available. I already pay $140/mo for my own cloud-based build server.

>>There is no cost to join the beta program. You can sign up for that here https://developer.apple.com/xcode-cloud/beta/. More details on pricing and availability of Xcode Cloud will be announced this fall


---

> #### I see that Xcode Cloud supports custom scripts for bootstrapping the temporary environment, but is/will there be a mechanism for caching some of the dependencies? For example a Ruby version installed via rbenv, and bundler dependencies. These are things we typically would never check into Git but could do with caching somewhere like we can on other platforms

>>Caching build dependencies is not currently supported, but you might be able to do that yourself in a custom build script. You may want to give it a try to see if you could get it to work.  That’s great feedback though so please file a report on feedbackassistant.apple.com


---

> #### Does Xcode cloud require the new Macos 12? Or will it also work with code 13 and macoa 11?

>>Xcode Cloud is part of Xcode 13, which will be compatible with macOS 11.3 😀


---

> #### Will we be able to use an http API to upload/connect a repository to start a workflow, i.e. not using Xcode at all?

>>No. You must use Xcode Cloud to configure your initial workflow and connect your source code repositories.


---

> #### Will Xcode Cloud support pipelines that don't target the app store or testflight? Would love to be able to build .apps or even .pkg for my macOS apps.

>>Yes! You can use a custom build script to upload artifacts to your own destinations.  Please see the `ci_post_xcodebuild.sh` document in that linked article for information about copying artifacts outside of Xcode Cloud.


---

> #### xcodebuild has a long-standing difference (FB5531332)  from Xcode's testing behaviour: it ignores the LocationScenarioReference (Location Simulation in the Xcode Scheme Run Options) in a scheme.  This limits the kind of integration testing we can do on CI and requires workarounds to mock Core Location which is crucial to our app's functionality.  Has location simulation been considered for UI testing with Xcode Cloud?

>>Thank you for the report and the feedback reference number! This is a bug with xcodebuild that we will bring to the attention of the engineering team.


---

> #### Are the Xcode Cloud configurations stored in the Xcode project file, or is there a manifest that can be built up outside of the project file? Many other CI platforms have yml configurations that are easily human-readable. (I can't wait to try out Xcode Cloud; thanks to all involved!)

>>Xcode Cloud workflow configuration is not stored in the Xcode project file; the configuration is persisted only in the cloud services that support the service.
Being centralized in the cloud, the configuration can be accessed through the Xcode UI itself, the App Store Connect web UI, and also programmatically through the App Store Connect public REST API (available this summer). The API uses JSON format.

Whereas power users are comfortable with editing YAML, integrating the configuration into an intuitive user interface makes the service more broadly accessible initially.

> Does it mean that we can edit/create/delete Xcode Cloud workflows via App Store Connect REST API with JSON objects?

Yes, that’s correct.


---

> #### Will testflight be available without Xcode cloud?

>>Yes, TestFlight will be available on Mac separate from Xcode Cloud 😀

This session can give you more information about it as well: https://developer.apple.com/videos/play/wwdc2021/10170/


---

> #### Will Xcode Cloud support golden/snapshot testing? (take a screenshot of a view or scene so we can know how a view should look like, and then this screenshot can be used to compare to following runs and see if the UI was modified. I currently use for this snapshotpointfreeco/swift-snapshot-testing from github)

>>Xcode Cloud will run any tests authored using XCTest, which can be combined with that library to write XCTest-based snapshot tests.


---

> #### Sorry if i missed it, is it possible to use Xcode Cloud as a remote build server, so the artefacts are redownloaded to my local machine and i can run it on a sim locally? Cheers

>>Yes, you can download the artifacts with a “ci_post_xcodebuild.sh” script. See the docs here: https://developer.apple.com/documentation/xcode/writing-custom-build-scripts


---

> #### Will there be support for fetching dependencies from other dependency managers that are not Swift Package Manager? I have currently a big legacy macOS project that uses git submodules, cocoapods and carthage.

>>Yes, you can use Custom Build Scripts in Xcode Cloud to fetch dependencies from other systems which xcodebuild doesn’t natively support such as those. See
https://developer.apple.com/documentation/xcode/writing-custom-build-scripts

There is additional documentation specifically about CocoaPods and Carthage available at
https://developer.apple.com/documentation/xcode/making-dependencies-available-to-xcode-cloud


---

> #### Will Xcode Cloud support third-party static code analysis tools, like SonarQube? https://www.sonarqube.org

>>You can integrate an Xcode Cloud workflow with services that are addressable through a custom script. Check out this documentation for specifics and how-to!


---

> #### I have several groups in TestFlight. Will Xcode Cloud have an option to automatically add specified Groups to a build or will it only add the 'AppStore Connect User' group?

>>Yes, you can configure a workflow to select one or more TestFlight groups.


---

> #### Our macOS app targets pro users in the music industry, which isn't known for rapid adoption of OS updates…
Will TestFlight for macOS be made available to testers on prior releases of macOS? Our internal QA tests all the way back to macOS 10.12, so it would be nice to (finally!) maintain the same kind of workflow that we have for our iPad and iPhone testing on prior releases.

>>Mac TestFlight support begins with macOS 12.0, it cannot be used on older releases

> Are there any API requirements/limitations on the apps that are _submitted_ to TestFlight?
> For example, it would make sense (given your response) that I would be required to build w/ Xcode 13 + macOS 12 SDKs to integrate with TestFlight for macOS, but will I be OK to continue using a deployment target of macOS 10.12 for my app binary?

Yes, you can continue to use a deployment target older than macOS 12.0! The requirements are:
- You must build and submit from Xcode 13 and use a provisioning profile
- The TestFlight client supports macOS 12 and later, so builds aren't installable on older macOS versions.


---

> #### If we have our own build/test farm which use AWS mac instance, does Xcode Cloud support to use our own dedicated cloud infra?

>>No, this is not supported.


---

> #### I'm concerned about the Homebrew aspect. Is there a chance that some dependency scripts could build and link with parts of Homebrew? Some of these open source libraries build with things like CMake and Ninja and can be very difficult to control include paths. Not having Homebrew installed, I don't have to worry about that, but I would with Xcode cloud.

>>We include homebrew by default on the VMs in Xcode Cloud. You can read more about how dependencies can be made available to a workflow here.
That said, if you're concerned about a supply-side attack, or dependencies in your application being bad actors, you can conceivably audit the environment in a post-clone script after Xcode Cloud clones your source code into its environment. I haven't tried removing homebrew from the VM, but theoretically you could remove it or prevent it from phoning home.


---

> #### Is TestFlight for Mac limited to just apps destined for the Mac App Store or does it also work with notarised apps outside of the store?

>>Apps must be uploaded for distribution to App Store Connect to become eligible for Mac TestFlight.


---

> #### I'm incredibly excited to use async/await and actors. What OSes will those be limited to?

>>On Apple platforms, we currently support iOS 15, macOS Monterey, etc.  This is tied to runtime integration with improvements to Dispatch in those OS releases for better performance.  We are exploring the possibility of backward deployment.  Concurrency works on Linux.  Still a WIP on Windows.



---

> #### On performance. I have a 4500-lines enum for emojis in Unicode, so I can order them and move them around. In the functions I declared (such as one providing the group, or subgroup), I'm damned either way: if I don't put a `default: fatalError()` at then end of it, I have a compiler too slow error. If I put it, I have a Default will never be executed. Best of all, I cannot disable the warning in the package, so I'm left with 4 warnings at all times. Anything I can do?

>>Sorry you have to deal with that. When the compiler analyzes a switch statement for exhaustiveness, it has some heuristics for very large enums that are intended to keep compiler performance good even in the face of complex patterns with very large sets of enum cases, but it can lead to false-negative cases when that goes wrong. If you have a moment to try to extract some of your code into a test case, and submit an issue to bugs.swift.org, we might be able to use it to further refine these heuristics. Thanks for letting us know about the issue.

I'm wondering if this enum might be better-represented as a struct, perhaps conforming to RawRepresentable if you need an underlying value?


---

> #### Will Swift Concurrency be available on iOS 14? I saw the following text in the Known Issues of Xcode 13 Release Notes — "Swift Concurrency requires a deployment target of macOS 12, iOS 15, tvOS 15, and watchOS 8 or newer. (70738378)"


>>This question was answered earlier, but I'll answer it again.
Right now the supported deployment target is iOS 15, macOS Monterey, etc., or later.  This is because of enhancements to Dispatch to support Swift concurrency, of which the concurrency runtime uses.  We are actively exploring the possibility of backward deployment, but are mindful of not compromising the future of concurrency performance by doing so.


---

> #### How lightweight are Actors? What should be considered when declaring an Actor in regard of overhead or cost?

>>Creating an instance of an actor type is no more costly than creating an instance of a class type. It's only during the access of an actor's protected state that a task suspension may happen, to switch to that actor's executor. The implementation of that switching is lightweight, and is optimized by both the compiler and runtime system. You can find out more about how this works in Thursday's session *Swift concurrency: Behind the scenes*: https://developer.apple.com/wwdc21/10254


---

> #### How to run async function from sync code on main thread? Do I need to use `detached` or there is something else?

>>Yeah, you can launch either of the forms of unstructured task from sync code. In Xcode beta 1, there is `async { }`, which will start a new async task that runs on the same actor as the launching context (which can be important if that's the MainActor, for main-thread-bound UI work), and `asyncDetached { }`, which will start a completely independent task. If you've been following Swift open source, though, you may know that these names have been revised due to feedback from the open source community; they're known as `Task` and `Task.detached` currently.


---

> #### Are we likely to see Swift Argument Parser reach 1.0 in the near future? It's a really great package and I'm keen to use it more widely.

>>As an open source project, probably the best place to ask this question is on the "Related Projects" section of the Swift Forums where you'll connect with the main authors of that package:
https://forums.swift.org/c/related-projects/argumentparser/60


---

> #### How async/await works under the hood? Could you give us some links?


>>The "Swift concurrency: Behind the scenes" session on Thursday will provide a lot of details about this.


---

> #### What are recommended sessions for leveraging async/await?

>>The main session is "Meet async/await in Swift", but you may also find tomorrow's "Swift concurrency: Update a sample app" session to be very helpful, since we go through an existing app and use async/await , and more, so it can leverage Swift Concurrency.


---

> #### How will async/await and SwiftNIO work together? If I understood it correctly, async/await works very similar to SwiftNIO’s EventLoopFutures.

>>Support for async/await is in the `main` branch for SwiftNIO, and will be available officially after 5.5 is released.


---

> #### How long do you expect things to live in standard library previewing before being considered fully baked? SE-0270 has lived there for quite some time and wants its turn in the limelight

>>Based on my conversation with the proposal authors, some issues have turned up with the design during the preview period that we need to address before it graduates to the standard library. This will likely require a follow up review.


---

> #### For the long enum question, I created a github. https://github.com/Misoservices/MisoEmojiUnicode
Will do the bugs asap. Ty for the answer!

>>Thanks for the follow-up! This link is very helpful. I took a quick look and I think making this a `struct` would probably simplify things and eliminate the problems you're having with large `switch` statements. I'd recommend you sign up for one of our Swift labs later this week so you can get one-on-one support from our team.


---

> #### Will there be (or are there already) some „guidelines“ on when to use Publishers (combine) and when to use async/await? E.g. for a network request, URLSession currently provides support for both but what’s better/recommended?

>>For URLSession the async/await API is preferred for single values and the AsyncSequence API is preferred for reading chunk-by-chunk.  The language level support for structured concurrency is a great way to integrate URLSession downloads into your app.


---

> #### Will there be tools to help visualize task hierarchies in the future? Or "cancellation reason"s for debugging?

>>Thanks for the suggestion, I think it's a good idea!


---

> #### Really like what I'm seeing on DocC - I've heard its possible to output HTML pages from a generated docset - will there be some kind of simplified mechanism to apply styling to those pages? Thinking about how an organization might want to apply their own brand coloring and such to published docs on web.

>>Today there isn’t any integrated affordance for theming the generated site. This is certainly something we can see as valuable, though. Also a great topic to bring up when we make the project open source later this year.


---

> #### The "Host and Automate" presentation mentions: "DocC comes with a built-in clean design". Will it be possible to theme DocC builds?

>>Ah, already a popular request. Today, there is no integrated support for theming the generated site, but would love for you to file a feedback request so we can track your interest.


---

> #### Is there a detailed spec for the `.doccarchive` output, and/or alternative output formats?

>>You can get details about how you can host a `.doccarchive` on your website in the following session:  https://developer.apple.com/wwdc21/10236.  We’ll have more details about the structure of the `.doccarchive` when we open source.
You can post a followup question with details if you have something specific you’re looking to do.

> A member of my team is building a tool to create unified output from several disparate docs generators that we use across different language-specific SDKs. (Currently Jazzy, Javadoc, JSDoc, etc.) We're trying to unify the reference manual experience for devs who use our docs without disrupting the workflows of our SDK teams, so we're trying to integrate with existing docs generators.
> I'd love to use DocC for the in-Xcode documentation, if I could also then feed DocC's output into our custom tool to unify reference docs on our docs site for our users. Having a spec for DocC's output, or being able to theme it, would go a long way toward enabling that unified experience. I'll keep my eye out for more details about the structure of the `.doccarchive` when you open source. Thanks for this - it looks awesome!

Interesting details, thanks for that!  Yes, keep an eye out for the open source details.
As you explore using DocC, if there is something that would help or that you can't accomplish, please do file Feedback Assistant requests with details.


---

> #### Are there any plans for DocC to support Objective-C documentation builds? My org currently uses a docs generator to build both Swift and Objective-C doc sets for our SDKs, and we would likely have to continue using that if DocC can't build Objective-C docs sets.

>>We’ve definitely heard about the importance of Objective-C, and feel it ourselves. It is indeed a priority. Thank you for the feedback!


---

> #### What’s the best way (via CLI/CI) to generate a documentation archive from a library that’s just `Package.swift`, no wrapping Xcode project?

>>`xcodebuild docbuild` works with a `Package.swift` file. You'll need to add a `-scheme` argument to tell `xcodebuild` which target to build; you can run `xcodebuild -list` to see the list. For example, you can run `xcodebuild docbuild -scheme MyPackage` to build a DocC Archive for the MyPackage scheme. The archive bundle will appear in your Derived Data directory (for example, `~/Library/Developer/Xcode/DerivedData/MyPackage-[hash]/Build/Products/Debug/MyPackage.doccarchive`).


---

> #### Is there built-in integration with Xcode Cloud workflows? Or would we need to implement it via a script?

>>You’re correct, the best way to support documentation builds on Xcode Cloud would be via a script that invokes `xcodebuild docbuild`.
There’s a session here that is a great reference for a use case like that: https://developer.apple.com/videos/play/wwdc2021/10236/


---

> #### Does the hosted webapp have a way to download the archive so devs can add the docs to Xcode on their local machines?

>>The primary use case we’ve focused on is for other developers to import your package into their project as a dependency. That lets them use Build Documentation to build the docs locally and view the docs in Xcode’s Documentation window.

For other workflows, you can make a `.doccarchive` downloadable from your website.  For example, you could zip the archive and have it as a downloadable resource on your site.
If you have a different workflow in mind, post a followup and we can look at that specifically, or if you have other feedback we’d love to get your specific use case in a Feedback Assistant.


---

> #### Are there any versioning capabilities in DocC? For example, being able to view the documentation for older versions of a package for users that can't or don't want to update?

>>If a developer imports a specific version of your package into their project as a dependency and uses Build Documentation, they’ll be able to read the documentation for that version in Xcode’s documentation window.

If you have a different workflow in mind, please post a followup and we can look at that specifically, or if you have other feedback we’d love to get your specific use case in a Feedback Assistant.


---

> #### The "Host and Automate DocC Documentation" presentation mentions: "The main page groups important symbols into topics related to higher-level tasks. Each of those group related symbols into more specific topics."

Can you talk more about how those groupings happen? Is that something we dictate by the way we organize and present content in the framework, or can we manually specify groupings?

>>Organizing your pages into groups is a great way to improve your documentation! To do that, you create a Topics section with a second-level heading called "Topics", followed by a group title in a third-level heading, and then a bulleted list of links.

We describe the syntax in more detail here https://developer.apple.com/documentation/xcode/adding-structure-to-your-documentation-pages, under "Arrange Top-Level Symbols Using Topic Groups". We also have a session tomorrow that shows how to do this: https://developer.apple.com/videos/play/wwdc2021/10167/ along with some other ways you can improve your documentation.


---

> #### Usually a problem of documentation is that it becomes obsolete with time. Is there any way to prevent this by using docC? Is there any validation in the code that can be autocompleted?

>>We think so too! It's important to keep documentation up to date with the changes you make in your code. Like a code compiler, DocC can emit warnings and errors when it finds something that doesn't quite look right.

For example, DocC checks all of the links in your documentation you've written using double-backtick syntax, like ``MyType``. If you remove or change the name of an API in your code, Xcode will display a warning on the line containing the documentation letting you know that you should update your docs.

If you're interested in other ways to validate the documentation, we'd be interested to hear more via Feedback Assistant.


---

> #### Is there any way to know the documentation coverage of the public symbols of a framework? It would help a lot with adoption and finding blind spots on huge code bases.

>>Hi Mauro! DocC does not currently expose documentation coverage but this is great feedback. Could you please file a Feedback Assistant request with details about how you'd want this to work? These types of enhancement requests are super helpful.


---

> #### Wonderful work. Thank you! Thinking ahead, is there any limitation that would inhibit the ability to extract docs for symbols other than public or open? Seems like it would be useful for internal use, and as a way to collect data on undocumented symbols.

>>Thanks for the feedback! This is something that a lot of people have been interested in over the last few days :). We built the DocC integration in Xcode first and foremost to support public-facing docs. We know that this is important for teams working on a framework and will keep this in mind in the future—thanks again for the question!


---

> #### Will the DocC compiler run only over source code dependencies or will it also generate the docs for the public interfaces of consumed xcframeworks?

>>DocC integrates with the Swift compiler to extract information about the public symbols and their documentation during the build. The author of an xcframework can build and export that framework’s documentation and distribute it alongside the xcframework to enable consumers to read the framework's documentation in Xcode.

> Do you have any tips or references on how to best distribute them together with the xcframework so that they are detected by Xcode? Is there any SPM tooling around this or is your answer more about a context of a manual import? Thank you very much!

That's a great question and something that we want to explore more. For now, this can be accomplished if the framework author exports and uploads a separate zip file of the DocC Archive. Consumers of the xcframework can then download the documentation archive, and double-click it to import that documentation into Xcode.
If you have more questions or want to follow up, you can request a DocC Lab appointment.


---

> #### When building docs locally, does the archive get stored in derived data? i.e. are devs going to have to rebuild docs every time they clear derived data to fix weird issues?

>>When you use the Build Documentation menu in Xcode, the doccarchive does go into the derived data directory, so if you clear that, you would have to rebuild the docs.
If you want to keep a version of the built docs in the Documentation window, you can export the doccarchive, and then you can double click it which imports it back into the Documentation window.  When you import a `.doccarchive`, Xcode puts it into an “Imported Documentation” section of the Documentation window, and that doesn't get removed when cleaning the derived data.


---

> #### In some of our internal modules we have some utility methods in extensions of types that are part of another consumed frameworks (for example: Foundation). We're working towards removing the majority of them in favor of other solutions that avoid extensions. In the meantime though, I haven't seen them in our generated docs and would like to know if there's a way to include them so that they are discoverable while we deprecate/move them. Thanks!

>>DocC in Xcode 13 supports documentation for APIs defined in your module, but we know that documenting extension symbols is also very important. This is something that would also be a great topic for us to discuss when we open source later this year.


---

> #### I’m loving DocC, thanks for all the hard work!
Is there a way to omit some public symbols from the generated docs? For example, my UIView subclass overrides a series of methods like layoutSubviews that I don’t want to be documented but they currently show in the documentation archive. I had a scan through the docs but couldn’t seem to see a way to opt out

>>If you prefix a public symbol with an underscore, DocC will automatically hide it from the built documentation.
In your particular case, because you can't change the name of the method, that won't work. We're interested to hear more about this scenario and how it impacts your documentation via Feedback Assistant.


---

> #### I've checked the session in which you cover linking to types or methods. What I'd like to know is if we reference between documentations/symbols of 2 different frameworks. For example referencing in a class of package A a public type of package B in which A depends and uses as an input.

>>We built DocC with a focus on having a really great documentation experience for individual frameworks and we don’t currently support linking across different frameworks. We know this use case is especially important for developers who ship a collection of frameworks (maybe in a single repo) that work together.
It’s good to know that you’re interested in this and it’s (another) really great topic for discussion when we open source later this year.


---

> #### Is it possible to use DocC for local Swift packages? We're writing a lot of documentation for local modules (mostly for services and core API), and it would be really helpful to use DocC for them too.

>>Yes, DocC works with both local and remote Swift packages.
If you open the Swift package in Xcode, and use the Product > Build Documentation menu item, Xcode will build documentation for both that package and its dependencies (both local and remote).
Note: if you use a documentation catalog in your Swift package, make sure that the Package manifest’s swift-tools-version is set to 5.5
For more information about documenting a Swift package, see https://developer.apple.com/documentation/xcode/documenting-a-swift-framework-or-package


---

> #### I noticed that the website that gets generated in the .doccarchive doesn't have search functionality. Is there any plan to add this feature?

>>This is a great question, and a really good feature request. You’re correct- Today, search is supported when viewing documentation archives in Xcode’s documentation window and it would be great to have search available on the web as well.
This is something we’ll definitely be interested in discussing further with the community when we open source DocC later this year.

---

## 🗓 Wednesday

>Simulator Recordings were added in Xcode 12.5. Is it possible to record audio with Simulator recordings?

>>You cannot record audio with Simulator right now. If this is a feature you would like, please file a Feedback Request.


---

> #### `xcrun simctl` allows recording a video from the Simulator within the terminal. Can we progressively read out the recorded video while the recording is still running? E.g., start converting the video to a different video size?

>>Hi! You cannot do this with `simctl`. We begin recording right when the command is executed and when you press Control-C, that ends the recording. Throughout this process data is written to the file. We do not support “piping” the video or performing operations on it at the present time. If this is something you would like to have, please file a feature request using Feedback Assistant.


---

> #### Is it possible yet to run a native iOS app from Xcode on an M1 mac, instead of launching in a simulator?

>>Yes! You can learn more about how to run these apps from the WWDC 2020 session here: https://developer.apple.com/videos/play/wwdc2020/10114/


---

> #### Hi. Do you plan to release simulators for older iOS versions (13, 12) compatible with Apple Silicon?

>>The legacy iOS and tvOS simulator runtimes are compatible with Apple Silicon Macs via Rosetta.  Older watchOS simulator runtimes are i386 and are thus not compatible with Apple Silicon macs.


---

> #### I'm noticing when I try to store an item in the keychain on the iOS simulator with `whenPasscodeSetThisDeviceOnly` and `biometryCurrentSet` that it is failing. Is there a way to make this work on the simulators now? Is there somewhere I can learn more about changes here?

>>I believe Keychain requires the Secure Enclave which is a hardware feature on iOS devices and is not present in Simulators, so I don't believe Keychain testing on Simulator will work, sorry. Physical hardware will be necessary for testing Keychain features.


---

> #### In watchOS, when i try to activate the session it fails all the time, but succeeds in second attempt. (on device)

>>Thanks for the question! I think we'll need some more information from you to help triage this further. Could you please submit a bug report via http://feedbackassistant.apple.com so that we can investigate this. Also, please run `xcrun simctl diagnose` and attach the output to the feedback issue you report. Thank you!


---

> #### Is it possible to use `simctl` to connect & disconnect the hardware keyboard?

>>It is not possible to connect/disconnect the hardware keyboard using `simctl` currently.
If you could file a Feedback request with details about your use case that would be really helpful.


---

> #### Similar question to video,  Is it possible to record video with Simulator recordings?

>>Hi there! If you are referring to recording audio, we do not support that at the present time. Please use Feedback Assistant to file this request with us if this is important to you.

> I mean to record a video?

It is possible to record video both from the `simctl` command line tool and the Simulator app. Both features work the same way, but the `simctl` command line tool gives you a few more options.


---

> #### Is there a technical reason why Xcode may at times not be able to launch an app properly in the simulator? Quitting Xcode and the simulator and then trying again will resolve the issue. This has been a problem for many years.

>>Thanks for the question! I think we'll need some more information from you to help triage this further. Could you please submit a bug report via http://feedbackassistant.apple.com so that we can investigate this. It'd be really helpful to get some diagnostics, captured after you've reproduced the issue:

1. A sysdiagnose bundle from your host Mac (run `sudo sysdiagnose` and attach the resulting archive)
2. A diagnostics bundle from the simulator. Please run `xcrun simctl diagnose`

---

> #### With the new announcement of WebAuthn and security key improvements in ASAuthorization, will the Simulator with Xcode 13 support hardware security keys (so developers won't need to build and debug their app to on physical devices)?

>>Hardware security keys are not supported in the Simulator on iOS 15. It is important to test features on-device because they take advantage of functionality that's built into iPhone, but which is not always possible to simulate.
If this is important for your workflow, I recommend you let us know via the Feedback Assistant at https://feedbackassistant.apple.com.


---

> #### Is there a way to test apps using GroupActivities framework on Simulators? What would be the easiest approach to test such apps?

>>The Group Activities APIs (for SharePlay) requires FaceTime.  Unfortunately, there is no FaceTime support in the simulator, so you will need to test this aspect of your app on device.


---

> #### Enterprise apps from multiple companies don't run in iOS 15 (but are fine on 14) and claim they need to be updated - can you shed some light on what's actually out of date here and what needs to be changed?

```
-[IXSErrorPresenter presentErrorForBundleIDs:code:underlyingError:errorSource:]: Asked to present alert for error 17 source MobileInstallation underlying error Error Domain=MIInstallerErrorDomain Code=13 "Failed to verify code signature of /var/installd/Library/Caches/com.apple.mobile.installd.staging/temp.0Ti39V/extracted/Payload/App.app : 0xe8008029 (The code signature version is no longer supported.)"
```

>>These applications use an older code signature type that is no longer supported. Apps from the App Store have been automatically re-signed, however enterprise applications cannot be automatically signed by Apple. Please contact the developers for these apps; they will need to re-sign their apps with an up-to-date certificate and profile. For more information, I recommend you visit the Security lab on Friday.


---

> #### What is the currently fastest recommend setup to deploy a debug build onto a watch device? Especially with regards to the file transfer time. iOS and watchOS device connected via BLE and both in the same 5 GHz hotspot? Mac (Xcode device) in the same wifi? Other influences like power connected?

>>At present, the most reliable way to test on watch is to ensure you have a wired connection to your phone and to ensure that your phone and watch are in close proximity to eachother on the same wireless network.  It is also best to limit interference from other wireless devices and ensure that you have a wireless access point close enough to maintain a constant strong signal with both devices.


---

> #### I have a set of test simulators that I frequently use. As part of their initial setup I will often install a network profile, and for another project I require to login to iCloud for CloudKit testing. I notice between Xcode upgrades these changes are always wiped. Is there a way to keep them in place between iOS/Xcode upgrades, or is their a quick way to script provisioning them so I don't have to login to iCloud or install a profile for each simulator?

>>Thanks for the great question! I think this might be an excellent opportunity to request a lab appointment with one of our Simulator engineers. It would be good to have time to talk through what your use case is, what you're seeing go wrong and when, and what options we can come up with together to make this work better for you. Please visit https://developer.apple.com/wwdc21/labs , search for Simulator in the "Search WWDC21 labs..." field, and request an appointment. Thank you!!


---

> #### Why is the Xcode wireless connection to a device so unreliable? I basically have no idea when I will be able to use wireless debugging.

>>We hear your concerns about the reliability of wireless debugging, especially around needing to maintain a persistent connection to the device for a debug session.  While we don't have significant improvements in this area with Xcode 13, it is an issue that we are actively investigating as we look to improve the wireless development experience.


---

> #### I noticed that compiling to a simulator in M1 still uses `x86_64` arch even in Xcode 13, is there any reason for that? why the simulators arent arm64? Or maybe I am missing some config, I tried with latest iOS 15 deployment target

>>Hi, the architectures that get built depend on the simulator runtime that you are targeting. Which runtime are you targeting? Do you see the same behavior when targeting a newer simulator runtime?

> I am targeting iOS 15 in this case, xcrun simctl list gives me:
>  == Runtimes ==
> iOS 15.0 (15.0 - 19A5261u) - com.apple.CoreSimulator.SimRuntime.iOS-15-0
> And this is the flag that the swift compiler is using: -sdk
> /Users/omarzunigalagunas/Downloads/Xcode-beta.app/Contents/Developer/Platforms/iPhoneSimulator.platform/Developer/SDKs/iPhoneSimulator15.0.sdk
> I tested with an empty project and it compiles to arm64 but in our project it doesn't

Can you check whether your project's configuration is forcing x86 output by overriding the ARCHS or EXCLUDED_ARCHS build settings?


---

> #### Is there a way to hide the top bar on a simulator (that says "iPad 8th generation etc") so you only get the content, kind of like the QuickTime Player is "all window"?

>>You can change the appearance of the Simulator window by opening the Window menu and selecting Show Device Bezels.

> Thank you, but what I'd like is no bezel and no top bar. Is that possible?

There is no configuration option like that right now. If you're trying to record or screenshot content, you can crop your recording area in the Screenshot app before capturing the content. If that's not sufficient for your use case, I'd encourage you to let us know via the Feedback Assistant at https://feedbackassistant.apple.com.


---

> #### Is it possible to write to the app's Documents directory on the Simulator, outside of the app?
For example if we want to set up a specific state, by writing data on the documents folder, before running UI Tests.

>>You can get the path to an application’s data container for a given device with simctl:
```
xcrun simctl get_app_container <device-id> <app-bundle-id> data
```
The data container will contain the Documents directory. If you only have one simulator booted, you can use the device alias "booted" instead of specifying the device's ID.


---

> #### Are there any Xcode 12.x versions that support device debugging when a device is running iOS 14.6?

>>Hi Jared! Xcode 12.5 supports devices running iOS 14.6.


---

> #### For App Store screenshots, I would like to use the simulator tool to take a picture with the device bezels embedded with the image. But when I do this with 12.5, I only get an screenshot with a 'cutout' of the device shape. How should I enable it to capture both the screen and the device bezel together?

>>Currently we do not support this. The screenshot tool in Simulator can create images which are suitable for a variety of media. If you are trying to capture screenshots of your app for marketing materials, I suggest that you speak to the fine people over at the App Store for advice on how to take screenshots of your app for the store.


---

> #### Is there any way to view the XML file of Info.plist in Xcode 13? Because it's no longer a separate file, and inside the project target instead.

>>Hi Andrew! You should still be able to view your Info.plist file by opening it in the project navigator on the left-hand side of the Xcode window. If you want to view the Info.plist file as it appears after building your app, select your .app bundle, Control-click or right-click it, and select Show Package Contents. The Info.plist file should be in there. If this is a traditional macOS app, it may be under the Contents folder.

> Thanks! But when you create a new project in Xcode 13, for example in the SwiftUI App template, there's no more Info.plist in the project navigator.

This is an intentional design change in Xcode 13. I know the Xcode team would love to hear what you think. Please let them know by using the Feedback Assistant at https://feedbackassistant.apple.com.


---

> #### Is there a recommended way to control simulator setup when running UI tests? For example, for ui testing I need to disable the hardware keyboard but for regular development, it’s not practical to do that.
It’s a bit disruptive having to enable/disable it each time so I’m wondering if i can control this kind of thing somehow?

>>Thanks for the great question! We have a helpful command line tool for Simulators that you can run via `xcrun simctl` that can do a lot of useful automation and setup. Here's a video from a previous WWDC session for more information: https://developer.apple.com/videos/play/wwdc2019/418 . If you find functionality missing from simctl, please do submit feedback through Feedback Assistant!! Also, one thing that you might try if you're doing this often... You can run all of your setup steps on a Simulator device, shut it down, then make a clone of that device with `xcrun simctl clone`, and then use that cloned device for testing. This might also be a good question to discuss with one of our Simulator engineers in our labs, so if that sounds like it would be helpful to you, please visit https://developer.apple.com/wwdc21/labs , search for Simulator in the "Search WWDC21 labs..." field, and request an appointment. Thank you!!


---

> #### I'll try again - sorry for not being clear:
In Xcode 13, will we be able to launch our iPhone/iOS apps natively on our M1 Macs, rather than selecting a simulator?  I do not believe this is currently possible.  I am asking if Xcode 13 will support it.  Thank you!

>>iOS and iPadOS apps can run natively on macOS 11.0 and later running on Apple Silicon.  To run such an app from within Xcode 12.0 or later, select the scheme for your iOS or iPadOS app and choose the 'My Mac (Designed for iPad)' run destination.  This run destination is suitable for all iOS and iPadOS apps.


---

> #### Is it possible to run the iOS 14/15 simulators under Rosetta while running Xcode natively on the M1 Mac?

### A
Hi Michael! I'm sorry, but we don't support this configuration. If you need this for your workflow, please let us know via the Feedback Assistant at https://feedbackassistant.apple.com.

---

> #### Any recommendations on using simulator or writing tests for AR apps? I couldn't find a way to effectively test my app using simulators. We have both 2D UI written in SwiftUI and UIKit, as well as 3D UI in Scenekit. Would be nice to be able to still iterate on the 2D UI using simulators! thanks :)

>>AR requires the use of specialized camera hardware that is not present in the simulator, so AR cannot be simulated. You will need to test your AR-based UI on-device.


---

> #### When we changed our CI to GitHub Actions we started having issues with some simulators while running tests. Apparently sometimes (it seemed at random) when launching the simulator this would try to do some kind of "synchronisation", and it would stay idle for a few minutes, this, before running tests.
Is there a way to avoid this 'synchronisation' to happen?
PS: apologies in advance if you think this is related to the CI provider and not the simulator itself.

>>Hi there and thanks for the question! I think we'll need some more information from you to help triage this further. Could you please submit a bug report via http://feedbackassistant.apple.com so that we can investigate this. Please run `xcrun simctl diagnose` and attach the output to the feedback issue you report. Also, I think this might be an excellent opportunity to request a lab appointment with one of our Simulator engineers. It would be good to have time to go through what you're seeing go wrong and when, and what options we can come up with together to make this work better for you. Please visit https://developer.apple.com/wwdc21/labs , search for Simulator in the "Search WWDC21 labs..." field, and request an appointment. It would be really helpful if you mention the feedback number that you filed in the notes for the lab request. Thank you!!

> I believe issues like this occur in CI systems where "fresh" hosts are instantiated for each job.
> I've observed performance issues due to `update_dyld_sim_shared_cache` running after booting a simulator for the first time — which on CI tends to coincide when attempting to run a test suite.

Thanks for the follow-up! I think it might still be helpful to request a lab appointment with one of our Simulator engineers to talk through the issues here. It’s difficult to recommend a good solution without having a better understanding of all the issues. :) If the bottleneck you’re seeing is resource constraints because the dyld shared cache is being built at the same time as your tests are running, you could try adding to your CI scripting to build the dyld shared cache first, allow that to complete, and then run your UI tests after. You can do this by running `xcrun simctl runtime update_dyld_shared_cache`. Please note that this command is not currently documented externally and is subject to change. If this command is helpful to you, please file a Feedback Report and ask for this functionality to be made more broadly available. If it would be helpful to have a flag for `xcrun simctl boot` which changed the behavior to wait until the shared cache is generated before booting the device, please file a Feedback Report for that as well. Again, it’s hard to make a good recommendation without a full understanding of the problem you’re trying to solve, and the best way to do that would be to request a lab appointment as mentioned above. Hope this helps!!


---

> #### The Simulator menu offers an entry to set a Custom Location (Features > Location > Custom Location…)
Is there an equivalent for this in `simctl`?

>>Unfortunately, there is not a way to do this via simctl at this time.  Please file a feedback assistant report via http://bugreport.apple.com as this would be a great addition to make in the future.


---

> #### I remember from some past WWDC session that the Simulator uses macOS's version of Core Animation, not the same CA stack that runs on the device. Is this still true, and if so, are there any documented differences we should be aware of?

>>All recent versions (definitely iOS 5+ but possibly back to iOS 4) of the iOS simulator runtime have provided a version of CoreAnimation built from the same sources as the corresponding version for devices.  Prior to the introduction of Metal support in the simulator, CoreAnimation went down a software rendering codepath.  Since the introduction of Metal in the Simulator, CoreAnimation now uses the Metal rendering codepath for improved performance.  Any difference in behavior would be considered a bug, so if you notice anything unexpected, please file a report via Feedback Assistant at http://bugreport.apple.com


---

> #### I started playing with an M1 recently, I haven't try it but is it possible to compile the app for a device (arm64) and use that build to run it in a simulator (arm64)? This could give us some advantages as being able to distribute the app to devs iphones and at the same time run the tests in a simulator by building only once

>>Thanks for the question! The answer is no; this isn’t possible. Even though in your case both the device and the simulator have the same architecture, they are different platforms because Simulator runs in macOS. As a result the builds are not mutually compatible.


---

> #### Now that we have Apple Silicon, are the iOS binaries running on the Simulator (e.g. launchd, Springboard) the exact same binaries as on the device?

>>No, applications built for the Simulator are built against the Simulator SDK, which does not support exactly the same set of features and functionality as our devices support. So apps built for Simulator can't run on a device, nor the other way around.


---

> #### I often get this error "Could not find module 'RealmSwift' for target 'x86_64-apple-ios-simulator'; found: arm64, arm64-apple-ios-simulator" whenever I use Cocoapods. I'm on an M1 mac.
I've tried solutions like this one: (https://stackoverflow.com/a/58192079/14351818), but they sometimes work and sometimes don't. I've added `arm64` under Excluded Architectures too, and added a script to the Podfile from here (https://stackoverflow.com/a/63955114/14351818).
Any tips?

>>It is likely the pods you're trying to use are not configured to work with the simulator you want to use, or the CocoaPods technology itself is not simulator-aware. I would recommend reaching out to the developers of the specific pods you want to use.


---

> #### Do we have any simulators for macOS testing? (to cover scenarios like touchID or clamshell for automation)

>>No, the Simulator does not support simulating macOS or Mac hardware features. You may find that virtualization software from third parties can help you with your testing.

> So Xcode cloud also will use actual devices where we would run our CI?

Xcode Cloud uses the same macOS build worker to run your tests as the build worker that builds your code


---

> #### Is Xcode Cloud built on the "configuration as code" principle? Meaning: Will we be able to have different configurations in different branches, akin to GitHub actions or Azure DevOps YAML-based approaches?

>>Hi there - All of your workflow configuration is stored on the server and not checked into your repository. That being said, you can create workflows that will only start builds for specific branches or branch patterns and use specific settings (macOS & Xcode version, environment variables, etc.).


---

> #### What Xcode cloud uses, actual devices/simulator/virtual machines?

>>Great question!
Xcode Cloud runs your iOS, tvOS, and watchOS tests on simulators, and your macOS tests on the same build worker used to build your code.


---

> #### Hi, nice presentations all!  Can you confirm that Xcode Cloud will ONLY support Bitbucket, GitHub, and GitLab, both in cloud and on prem, but nothing else?  Specifically, I have a bare git repo on my local network that I would like to use as my source code repo for Xcode Cloud.  Is this possible?

>>Yes - Xcode Cloud only supports cloud and on premise instances of GitHub, Bitbucket, and GitLab. It doesn’t support bare git repos on local networks.


---

> #### Hello! In our current CI we check what changes the pr is doing and analyze our dependecy tree to find out which test bundles should run, then we use -only-testing args in xcodebuild cmd to filter them, is it possible to do so with Xcode Cloud?

>>Hi! You can probably get something like that working by using a combination of a post-clone script, which does the analysis of the changes in the git commit, and then generate a scheme/test plan that dynamically has only the tests you want to run


---

> #### Hey! Yesterday on a previous question I asked about API availability, Richie mentioned that we’ll have a REST API during this summer :partying_face:
I have a follow up question about this though, will we be able to trigger a certain workflow from an API call while this workflow doesn’t run on any git event (pr, tag change, commit, scheduled event, etc.). Basically, we want to control part of our release process from our custom Jenkins orchestration (that we have today) and trigger some of the workflow tasks on demand from there.
Thanks!

>>While we can't commit to the shape of the API right now, I think it will be a pretty common use case and a valid expectation to have w.r.t Xcode Cloud's public API.


---

> #### Can Xcode Cloud provide any support for Developer ID workflows? Either complete products or re-signable ones?

>>Hi! Xcode Cloud will code sign macOS builds with Developer ID, and for the Mac App Store


---

> #### When can we expect invites to Xcode Cloud after submitting a request?

>>Thanks for the question Edward! We’re all excited about Xcode Cloud and we want to make using it the best possible experience. We'll be gradually adding users throughout the summer and fall. Account holders will be notified by email when your team is accepted.


---

> #### Beyond the obvious fact that Apple is handling the server setup and maintenance, are there any features of Xcode Cloud not available on Xcode Server?

>>Hi! Xcode Cloud is a totally different product from Xcode Server, with many differences and improvements over Xcode Server. I'd recommend looking at the documentation if you're interested in specific features.


---

> #### Does Xcode cloud also support dependencies from private repositories? I.e. the project having private Swift framework dependencies that are hosted on GitHub privately or even on a local GitHub server.

>>Definitely! This is covered by today's session "Customize your advanced Xcode Cloud workflows":  https://developer.apple.com/videos/play/wwdc2021/10269/


---

> #### In our current CI we need to use internal metrics endpoints behind a vpn so our secops team exposed en external endpoint and ip whitelisted it so we can access it, is Xcode Cloud infra will have a fixed public ip range so we can do the same?

>>Hello! Yes, there will be a fixed IP range for all egress from Xcode Cloud. Over the summer details will be published in the documentation.


---

> #### If your git repo is in GitLab self hosted, because you want the source code to be secure and not hosted in the cloud, what happens to the source code during the Xcode Cloud CI/CD a process - is it transmitted to your servers encrypted?  How long is the source kept on your servers after compilation and deployment?

>>Xcode Cloud will securely clone your repo from your GitLab instance. Your source code is kept on the build worker for the duration of the build, then the build worker (and your source code) is immediately discarded


---

> #### Hi! I love what I have seen so far. Thanks for the hard work. In our company we rely heavily on Artifatory for our mobile builds. Some of the interactions we have require authentication via .netrc file configuration. Is this something that we will be able to achieve in Xcode Cloud ?

>>Thank you! Glad you like it! I'd recommend looking into the custom scripting and environment variables documentation (and the Advanced Skywagon session). You could generate a .netrc file in a script, populate an access token from a secure environment variable.


---

> #### We have precompiled binaries (XCFrameworks) that swift package manager downloads from an private server. The package manager authenticates using a .netrc file. Would I be able to *securely* provide a .netrc file with the proper credentials to Xcode Cloud so it can fetch these packages?

>>Xcode Cloud will no generate .netrc files for you, but you can create your own as part of a custom build script. For example, you could create the file in a `ci_postclone.sh` file and use a secret environment variable to store the token you want to use.
Both custom scripts and (secret) environment variables are covered in today's session: https://developer.apple.com/videos/play/wwdc2021/10269/


---

> #### Will Xcode cloud require that we push the app including the framework dependencies added by swift package manager or cocoapods?

>>Hi! Xcode Cloud requires the source of you app, and any non-binary dependencies to be available in the build environment.


---

> #### Xcode cloud is great for CI, would be eventually be able to avoid building locally and get the built products from the build servers?

>>Xcode Cloud is built specifically with CI in mind. Having said that, you can download all artifacts produced from each build.


---

> #### We are currently building in our CI using Macstadium + ORKA, which adds a virtualisation layer. We can deploy VMs with different CPU configurations depending how time critical the task we need to perform is (e.g. PRs have faster machines for better developer feedback, release builds work in slower VMs because we are not in a rush). Will something similar be possible with Xcode Cloud?

>>Great question!
Xcode Cloud was designed so that developers no longer have to maintain a CI build environment configuration


---

> #### Is it possible to configure a workflow to skip certain commits (e.g. we use [NO-CI] prefix in the commit message to have our Jenkins ignore a it)

>>That's not something that's supported right now but that's a great idea! Can you please file a feedback about this?


---

> #### We will able to select the stack? For example M1 over Intel computers, we noticed M1 has a huge impact in compilation time

>>Xcode Cloud was designed so that developers no longer have to maintain a CI build environment configuration.


---

> #### In our CI we do commits (e.g. to bump the version and build number of the app) and push it to the repo. Will we be able to commit and push from Xcode Cloud from the scripts?

>>Xcode Cloud will actually manage the build number (CFBundleVersion) automatically for you - so likely you won't need to do those kinds of pushes. However if you do need to, you can add a custom script that calls various git commands - which will then trigger another workflow

> I have a follow up question for this one. Will Xcode Cloud provide an option to somehow format the build number?

That's not something that's supported right now but that's a great idea! Can you please file a feedback about this?

---

> #### Does Xcode Cloud run tests on devices or simulators?

>>Thanks for your question. Xcode Cloud runs tests on simulators.


---

> #### Can we set baseline metrics for performance tests from Xcode cloud?

>>You’re not able to set baseline metrics for performance test, but please file a request through feedback assistant!


---

> #### Would Xcode Cloud support beta versions of Xcode ?

>>Totally! You can configure a workflow to use a specific version of Xcode (including betas), or always use the latest version available (including betas).


---

> #### Does Xcode Cloud work for Enterprise Accounts?

>>To use Xcode Cloud, you need to be enrolled in the Apple Developer Program.
https://developer.apple.com/documentation/xcode/requirements-for-using-xcode-cloud


---

> #### Would Xcode cloud support build systems like bazel?

>>Xcode Cloud supports custom build scripts, so you can run bazel commands if you need to build additional tools as part of your build process.


---

> #### Currently we create alpha builds for QA on a regularly basis using an Enterprise certificate. We distribute the alpha builds using Bitrise. On every alpha release we notify QA via a slack channel and they get a short description of the release, a download url as text and QR code (to ease installation from device). Would I be able to have the same flow using Xcode Cloud? And would I be able to distribute enterprise builds via TestFlight?

>>Hi! You can definitely achieve this workflow with Xcode Cloud - in fact we expect it would be significantly easier.
You can set up a workflow that deploys to an internal TestFlight group representing your QA team (and also notifies in slack). They can then install the build through the TestFlight app.
You don't need to worry about Enterprise signing to do this


---

> #### Will app extensions be supported in any capacity when building apps on iPad?

>>Swift Playgrounds 4 supports making apps for iPhone and iPad, in addition to the playgrounds and playground books already supported by Swift Playgrounds.

> Thanks! Specifically, I'm curious about whether any app extensions, action or intent extensions for example, might be available to include in the initial version of the new project type, or if it will be limited to just ‘standard’ apps without these kind of features for now? :slightly_smiling_face:

Those are some awesome ideas! We don't have any specific plans to support that this year but we encourage you to file a Feedback to state your support for those features.


---

> #### When building on iPad, will it be possible to install apps to the iPad homescreen locally? Or might this require a baseline of submitting through Testflight?

>>Great question! If you want to install the app you're making in Swift Playgrounds to your device, you can sign into your developer account and submit the app to TestFlight and then install it on your iPad or other devices using the TestFlight app.


---

> #### Is Swift Playgrounds expected to only support SwiftUI, or will it be open to other frameworks like UIKit and 3rd party frameworks?

>>Swift Playgrounds supports both UIKit and SwiftUI. SwiftUI Previews requires a SwiftUI app, but you can always use `UIViewRepresentable` to show UIKit content in SwiftUI.

> Thank you! To follow up on this, what is the planned support for 3rd party frameworks? Will it be possible to use say, Snapkit?

Playgrounds supports working with Swift Packages. SwiftUI Previews (which is in Swift Playground 4) allows you to preview SwiftUI content. To use something like SnapKit you'd have to use it with UIKit and use `UIViewRepresentable`


---

> #### I'm very excited for the upcoming features to enable building apps on iPad, but details in the main Keynote were very light. Is there anywhere I can learn more, and are you able to answer questions on it?

>>At this moment, the available details are available at the State of The Union video https://developer.apple.com/wwdc21/102 at 0:33


---

> #### I’m sure you’re getting this question a lot - any word on when a Swift Playgrounds 4 beta will be available? :smile: So excited to try it out!

>>Swift Playgrounds 4 will be available later this year.


---

> #### Is it possible to import SPM packages into Swift Playgrounds?  That would be really helpful in using Playgrounds as a tool to build more richly featured apps without having to traverse over to Xcode.

>>Yes! The apps you build can depend on publicly-available Swift Packages.

> Hmm.. so I won't be able to use my internal (non-public) Swift packages when building on iPad?

At this time, we do not plan to support packages that require authentication


---

> #### Will iPhone and iPad be the only devices supported in the next version of Swift Playgrounds when building Apps, or will developers be able to build for watchOS and macOS as well?

>>Apps built using Swift Playgrounds are universal, so when they’re distributed using TestFlight or the App Store, they’ll run on iPhone, iPad, and Apple Silicon Macs.


---

> #### To build and deploy to the AppStore from iPadOS using Swift Playgrounds, do you need to be a subscriber of Xcode Cloud?

>>Nope! Anyone with a developer account will be able to submit their apps to TestFlight and the App Store.


---

> #### Hi! Will Swift Playgrounds 4 for iPad have the ability to add dependencies on third party Swift Packages?

>>Yes! The apps you build can depend on publicly-available Swift Packages.


---

> #### What does the workflow look like if you wanted to work from Mac to iPad or vice versa? Is it all iCloud synced or will it connect to Github/Gitlab/other Git syncing services?

>>Swift Playgrounds already works great with iCloud Drive (or any other file provider extension) and will continue to do so. We don't have anything to announce at this time about other mechanisms.


---

> #### Will I be able to see what my app looks like on different device sizes and orientations from within Swift Playgrounds?

>>Yes! You can take your app preview fullscreen, and put Swift Playgrounds into split-view multitasking to test a bunch of different configurations. You can also write custom Preview Providers to try even more layouts and configurations.


---

> #### Will folks be able to open existing Xcode projects in Swift Playgrounds 4, or will a project have to be migrated to some sort of `.playgroundbook`?

>>Swift Playgrounds 4 has a new project format based on the Swift Package format.

> Can you edit this format in Xcode without conversion? So you can start working on a app in Xcode and then continue on iPad and vice versa?

Yes! App projects can be moved seamlessly between Swift Playgrounds and Xcode, so you can work on whichever device you prefer.


---

> #### It was mentioned I think that Playgrounds apps destined for the app store could be brought over to Xcode, can it go round trip back to Playgrounds?  Especially wondering if you could use Xcode to define storyboards or data models to use, and bring those into Playgrounds.

>>Thanks for the great question! You will be able to open Swift Playgrounds projects in Xcode since they're based on the Swift package format!
Note that Xcode has more tools than Playgrounds will be able to support. If you end up editing the Playgrounds project in Xcode in such a way that won't be supported by Playgrounds, you'll see a warning in Xcode.


---

> #### Any plans for further playground books like Cipher? The existing content is brilliant, as were the sessions on Swan's Quest last year. The additions like Sensor Arcade and the accompanying templates are inspirational, especially in the classroom...

>>We’re so glad you like the content! We haven’t announced anything about that. Stay tuned for future updates!


---

> #### Will Swift Playgrounds have any kind of simulator, or are SwiftUI projects running as previews?

>>Howdy! Swift Playgrounds projects will allow you to make iOS apps with the SwiftUI lifecycle — you'll be able to see your app running in Playgrounds, and you'll be able to create preview providers to preview specific views.


---

> #### What frameworks are unavailable in Swift Playgrounds created apps?

>>Howdy! While we can't go into details about what frameworks Swift Playgrounds will or will not support — I'm curious about what you're hoping to achieve! Feel free to respond in a followup or please send us feedback for your hopes, dreams, and expectations!


---

> #### Is the MacOS version of the Playgrounds app actively being maintained? Should we expect it to be able to build apps as well?

>>For this year, we focused on bringing support for building apps to iPad.


---

> #### Does Swift Playgrounds 4 support multi-window/scenes? i.e. Can I have my editor as one full screen scene and use slide over for my preview? Additionally, does it have support for a secondary display (AirPlay/USB) to go along with multiple scenes/windows?

>>We're excited to hear about the ways you want to use Swift Playgrounds! We'd love to have folks send us their desires and expectations via feedback! :)


---

> #### What are the requirements for Swift Playgrounds? Can you build apps on a 2018 (A12) iPad Pro for example?

>>You'll be able to build apps in Swift Playgrounds on any iPad running iPadOS 15!


---

> #### Will it he possible for a xcode project to be opened in swift playgrounds? And for a swift playground in xcode?

>>Projects in Swift Playgrounds 4 are built on top of the Swift Package format, and they work great in Xcode.
If you make a change to your project in Xcode, and Xcode knows that it won’t be compatible with Swift Playgrounds, it will show a warning.


---

> #### Are there any common app capabilities that I could create using SwiftUI & Swift on Xcode that I would not be able to create in Swift Playgrounds? Or Apple Frameworks that I could not use in Swift Playgrounds?

>>Swift Playgrounds 4 will have wide support for Apple’s SDKs. We believe some truly powerful apps will be built with Swift Playgrounds 4, and it will be easy to bring your projects into Xcode when you need more power.


---

> #### The State of the Union discussed moving an app project from Swift Playgrounds 4 on iPad over to Xcode on the Mac. Will this be possible in the reverse? Can I edit the project in Xcode and then bring it back to Swift Playgrounds?

>>Hi Greg. Yes the new project format will allow for editing the project in Xcode and bringing it back into Swift Playgrounds!


---

> #### Can we release to the app store from playgrounds today? To adhoc distribution? Do we need to wait until something comes out of beta before this flow works? Thank you.

>>Building apps, including submitting them to the App Store, is new in Swift Playgrounds 4. Swift Playgrounds 4 will be available later this year.


---

> #### 
What are some neat benefits of writing and running full iOS/iPadOS apps on iPad that may not be immediately obvious without having experienced using the app?

>>This is an incredible question!
Making iPadOS apps right on the iPad is an incredible experience. It's easy to get a feel for how your app will behave as a part of the amazing iPadOS system experience like when in fullscreen, a multitasking split size, and in slide over.
You can test out how your app responds to a software keyboard present and how it changes when you attach a  hardware keyboard. You can check how your UI responds to hover effects with the trackpad, and you can test out great gestures right in the canvas with your finger(s)!
We're so excited to see what y'all come up with when working in Swift Playgrounds 4. If any of these things spark inspiration or excitement about Playgrounds, we'd love to hear your desires and expectations via feedback!


---

> #### Might external display support be considered for preview rendering at various device sizes, to reserve more screen space for code?

>>That's an awesome idea! Please file an enhancement request through Feedback Assistant: https://developer.apple.com/bug-reporting/


---

> #### Might the upcoming Playgrounds release include support for find & replace, or refactoring? Renaming variables, or indeed any declaration, can be quite painful without it. Or if you can't comment - are there any good workarounds for this currently?

>>These are some awesome feature ideas! While we can't comment on plans for the future, we highly encourage you to submit an enhancement request through the Feedback Assistant: https://developer.apple.com/bug-reporting/


---

> #### Will Swift Playgrounds support all the latest new technologies Swift 5.5 is introducing ? (Async/Away, Actors...etc)

>>Yes! Swift Playgrounds 4 will ship with the Swift 5.5 compiler, including support for structured concurrency.


---

> #### Does Swift Playgrounds support using packages with Swift Package Manager? For example, using a networking library when building a SwiftUI app on the iPad.

>>Yes! The apps you build can depend on any publicly-available Swift Package.

> You are specifically saying “publicly available”. So I assume there will be no support for signing into a GitHub account for example, or to provide SSH credentials for a non-public repository at the beginning, right?

That's correct—at this time, we do not plan to support packages that require authentication.


---

> #### What kind of apps can be created and submitted to the App Store? iPad and iPhone only? Or macOS apps as well, even if only through Catalyst?

>>Apps built using Swift Playgrounds are built universal against the iOS SDK, so when they’re distributed using TestFlight or the App Store, they’ll run on iPhone, iPad, and Apple Silicon Macs.

---

## 🗓 Thursday

>What are best practices for including and managing a lot of Swift Package dependencies in a project?  Adding them to the project file doesn't scale well after more than a handful.

>>I believe this WWDC talk is a great resource for Swift Package Management Best Practices: https://developer.apple.com/videos/play/wwdc2019/408/


---

> #### So excited about the Vim keybindings (I have waited more than a decade for it!).
Does the team have a plan for supporting a richer set (repeat command (.), replace command ®, macros, …)

>>Good morning, and thanks for taking the time to ask this question! If there are suggestions or features your would like please submit them using the feedback assistant on https://developer.apple.com/bug-reporting/


---

> #### I am seeing that sometimes Xcode 13 cant opt+Click on symbols can't find quick help. It works on some frameworks methods, but not on class properties and methods, and frameworks from cocoapods most of the time. The same click worked in Xcode 12. I can't attach a screenshot to the question haha.

>>That sounds like an issue we'd like to investigate further. If you can reproduce it, would you mind filing a bug report using feedback assistant?
If you can run Xcode with some logging enabled, it would help us understand it better. Try running Xcode from Terminal like this, `env SOURCEKIT_LOGGING=3 /path/to/Xcode.app/Contents/MacOS/Xcode`, and then reproduce the problem by opt+clicking the symbol. If you can also provide a screen recording demonstrating the problem, it would help.


---

> #### Are you taking xcodebuild questions? :)
If I try to create an xcframework (`xcodebuild -create-xcframework ...`) that contains both Intel and ARM simulator frameworks I get the error `Both ios-arm64-simulator and ios-x86_64-simulator represent two equivalent library definitions.`
Has this been fixed in Xcode 13?

>>It sounds like you have two separate thin (single-architecture) binaries, one for each arch, but both for the simulator. XCFrameworks don't support that; there needs to be only a single multi-arch binary. You can use the lipo tool (which is what Xcode uses) to combine your binaries if you're not using Xcode to build the product already.


---

> #### Does the new Xcode PR review feature supports a strategy with forking? So far I can only create a PR that points my fork's `main` branch, not the original repo of my fork.

>>Xcode’s pull request feature doesn’t work with forked repositories. It doesn't support submitting pull requests to an upstream repository.


---

> #### When you create a new project using the iOS App template, and select SwiftUI for the interface, there is no more Info.plist file. Instead, I can find it inside Project -> Targets -> Info. However, how can I access the file's XML source code?

>>You should still be able to find the Info.plist file in the project directory under: `<Project Root Directory>/<Project Name>/Info.plist`. From there, it can be edited with your favorite editor, including Xcode. It can also be opened quickly in Xcode by using Cmd + Shift + O and searching for Info.plist.


---

> #### Our repo doesn't commit in the .xcodeproj but generates it locally on the fly. Does Xcode Cloud support a case when a project is generated or modified in the post-clone script?

>>Yes. Bear in mind that creating your first workflow with Xcode Cloud happens from Xcode. It analyzes your project locally, suggests a configuration to quickly create you first workflow.
In the scenario where you do not check in your Xcode project file, then these are the adjusted setup steps:

1. You need to generate the Xcode project file locally.
2. To exclude it from your repo, you could add a `.gitignore` rule.
3. Add the add Xcode project generation to post-clone script.
4. Create first workflow from Xcode.

Create first workflow from Xcode: https://developer.apple.com/documentation/xcode/configuring-your-first-xcode-cloud-workflow
Post clone script: https://developer.apple.com/documentation/xcode/writing-custom-build-scripts


---

> #### Is there a way to quickly mark a Swift (SPM) Package for editing in Xcode 13? (Similar to how you can `swift package edit` at the command-line), or is it still required that I separately clone the package, and drag it into my project?

>>The workflow for this remains unchanged from previous versions of Xcode.


---

> #### How did the team prioritize which new features to build vs which features to work on later?

>>One of the many considerations we take into account would be what we receive from Developers via the Feedback application. Please send us your suggestions —you can send those with the Feedback Assistant. https://developer.apple.com/bug-reporting/


---

> #### Creating a new project with Xcode 13 doesn't show up the Products directory in the Project Navigator, is it possible to add it back per project or globally?

>>The Products directory will no longer be shown by default in newly created projects, as you noticed. You can still get to the directory with your build products by using the new `Product -> Show Build Folder in Finder` menu item.
If there is a reason why you would want to always show the products directory in the project navigator, we'd love for you to file a Feedback via Feedback Assistant to explain your use case.
https://developer.apple.com/bug-reporting/


---

> #### What powers Xcode's new Swift syntax highlighting? It looks very similar to the semantic highlighting recently merged into sourcekit-lsp!
If sourcekit-lsp is being used, will future improvements be available in Xcode too, and will Xcode's features (like live issues) be available in sourcekit-lsp at some point?

>>Xcode 13 has a brand new syntax highlighting implementation for Swift, which is optimized for performance. This is unrelated to sourcekit-lsp. If you see any issues, please report feedback:
https://developer.apple.com/bug-reporting/


---

> #### What is recommended option of adding SPM dependency from private GitHub repo? Am I right, that for Xcode it is https:// url accompanied by Xcode GitHub account? Question: if I need to connect two private repos from GitHub, and one uses account1, another uses account2. How to specify, what account to use with what repository?

>>You're correct that when using privately hosted packages in Xcode you'll want to be signed into your source control account(s) (in Preferences → Accounts). Authentication should be handled automatically and should choose the correct account for each package.
If you run in to any issues, we'd really love to hear about it via Feedback Assistant. :bow:


---

> #### What is the simplest way to view a diff of changes to the documentation? Sometimes I'll find on Twitter from an Apple engineer or writer that particular docs have been updated, but I'm curious if there's a more consolidated and comprehensive way to find docs updates.

>>I checked in with the documentation team on this – while it's possible to view a diff to see what changed for individual APIs, there is not currently an overarching view that would show all the diffs between two versions. If that's something you'd find useful, please do submit that feedback via feedbackassistant.apple.com for the documentation team to look into!


---

> #### Given that the legacy build system is deprecated, we're still noticing that the new build system is slower to run a build than the old one. We're very sensitive to build performance -- currently a roughly no-op build with the legacy build system is roughly 60s on a recent Mac Pro with our codebase and considerably less than that on legacy. Any optimization tips would be greatly appreciated!
Also - a lot of this is spent in the build planning step -- are there plans to cache or reduce build graph evaluation in the future? Let me know if filing a radar/feedback is useful.

>>Task planning should only re-run if something in the project structure has changed between builds; when doing (for example) an immediate no-op rebuild, this shouldn't happen.
Please file a bug report. If you can attach a sample project which reproduces the problem, that would help, as I suspect there's something in the project structure which is triggering this.


---

> #### Do you have any general guidance for writing useful Feedback Assistant reports for Xcode? For example, is it a good idea to make bug reports isolated and minimal in an Xcode playground?

>>We have some great tips for filing bug reports using Feedback Assistant here: 
https://developer.apple.com/news/?id=vvrgkboh

Yes, it is an excellent idea to create a minimal scenario that shows your issue. Screenshots and a sysdiagnose also help us investigate the issue.

We really do read every one of your bug reports too so as one of the folks that reads those, I just want to say thanks for asking this question!  We really appreciate those reproducible, actionable bug reports!


---

> #### How is your experience using Xcode to work on Xcode?

>>It’s definitely a very “meta” experience to write code for new functionality in the Source Editor … in the Source Editor. Or to debug Xcode using Xcode. Honestly, you sometimes loose track of which one you are working in and which one your are debugging.

We can also profile Instruments with Instruments by the way.

So it’s definitely fun to build the tools you are working with every day. It’s not as fun to break the tools you are working with every day. Luckily, we’ve been using Xcode Cloud with the Xcode project to make this less likely!


---

> #### Why doesn't the new HTTP Traffic Instrument work with the simulator. Is this a limitation that might be fixed in a future beta?

>>Thank you for your feedback; it's a known issue in Beta-1 that we are investigating, as the feature is is intended to work in simulator.


---

> #### How does the team decide if a particular subsystem of Xcode should be open source for community contributions? Also, do you have any tips for getting started contributing to aspects to tooling on Apple's GitHub (perhaps low-hanging fruit)?

>>For the Swift project, we have some bugs tagged as "starter bugs". Suitable for new contributors, some even if you have no experience with compilers!
https://bugs.swift.org/browse/TF-67?jql=status%20%3D%20Open%20AND%20labels%20%3D%20StarterBug%20AND%20assignee%20in%20(EMPTY)


---

> #### Is there any way to cache Swift Package Manager dependency builds? This is one of the longest steps in our current build, especially on CI. We cache the checkouts for them, but being able to cache SPM builds would save a lot of time!

>>Adding `clonedSourcePackagesDirPath` to your xcodebuild flags should help you point CI systems to already downloaded packages. There are Swift Package Manager Office Hours starting today at 3pm and it might be a better forum for this question.


---

> #### What does `ALLOW_TARGET_PLATFORM_SPECIALIZATION` + `SUPPORTED_PLATFORMS` actually do? Can I build XCFramework fro macOS/iOS/tvOS with one xcodebuild call?

>>`SUPPORTED_PLATFORMS` defines the platforms a target supports building for. For example a framework target which can be built for either iOS or watchOS.
`ALLOW_TARGET_PLATFORM_SPECIALIZATION` allows a target to build for multiple platforms in a single build. For example, building an iOS app with an included watchOS app, both of which depend on that framework, so the framework can be built for both platforms.
But XCFrameworks still need to be built with a separate xcodebuild invocation.


---

> #### Is there a way to create our own file templates for adding a new file to a Swift Package Manager module? The existing templates don't include substitution templates, so they require modification (i.e. replacing `SwiftUIView` in both the View and the PreviewProvider) before they're as useful as their standard Xcode New File equivalent templates.

>>There's currently no way to customize new file templates (but, as ever, if you'd like to see this happen please file Feedback!).
Generally, creating a new file in a package from within Xcode (e.g., ⌘n while your package is selected in the Project Navigator) should bring up the same New File template pane as you would see for a project. If you're seeing something else or if this doesn't meet your expectations, we'd love to hear about it.


---

> #### Is there some way to download Xcode using the App Store Connect API and the keys that it generates ? This would be super useful in automated workflows to setup CI machines with Xcode.

>>While there are solutions like Configurator and Remote Desktop that allow you to install apps from the Mac App Store, including Xcode, there is not a way to install Xcode through the App Store Connect API. It sounds like the best solution for your situation would likely be to utilize Xcode available as a download in XIP format from developer.apple.com/download


---

> #### How does Xcode Cloud work for projects that make use of multiple developer accounts? For example, we have one enterprise account for development and beta builds, and then a separate developer account for our App Store builds. Would we be able to share configuration between these two accounts or will they have to be separate?

>>You cannot share data between different Developer Program accounts, all accounts are private and secure as you would expect. Also Apple Developer Enterprise Program accounts are not supported in Xcode Cloud at this point in time.


---

> #### In ObjC projects where headers haven't changed, how can we debug unexpected full/near-full rebuilds for Debug?

>>If you think this could be a bug and you can attach a reproducible case, please file a bug report.
That said, you can set a user default:
`defaults write com.apple.dt.XCBuild EnableBuildDebugging -bool YES`
which will dump Intermediates.noindex/XCBuildData/BuildDebugging-<timestamp> directories containing trace files (which are text files despite the file extension) which have the low-level dependency analysis that the build performs. There is not tooling for processing these files, but they might provide some insight into your situation.
Important: You should delete this user default when you've collected the data you want, as enabling build debugging can affect build times. It's not intended to be left enabled all the time.


---

> #### What was the reasoning behind having the HTTP Request monitoring in Instruments vs the Activity Monitor in Xcode? It would've been cool to not have to intentionally run and setup Instruments to have that working.

>>I assume you are referring to the debug gauges in the Debug navigator.
We do already show the “Network” activity. However, the HTTP Traffic instrument shows very detailed information that wouldn’t all fit into the the existing debug gauges. There are many different ways to visualizes network activity and even the HTTP Traffic instruments provides several ways to visualize your activity.
However, if a specific summary view about HTTP traffic would be really helpful to you in the Debug Navigator within Xcode, please file a Feedback to describe what you would like to see there.
https://developer.apple.com/bug-reporting/


---

> #### Has the team ever considered allowing Xcode to send macOS notifications - for example when tests pass / fail ?

>>After tests finish, if Xcode is not the frontmost application, it posts a macOS notification describing whether testing succeeded or failed.
Beyond testing, Xcode includes a flexible feature called Behaviors which allows you to opt-in to receiving notifications when various events occur during your development workflow. For more details, see
https://developer.apple.com/library/archive/documentation/ToolsLanguages/Conceptual/Xcode_Overview/CustomizingYourWorkflow.html


---

> #### MacOS build worked on xcode cloud, are these Apple silicon devices or Intel based? or can we select?

>>Xcode Cloud builds universal binaries, just as Xcode does on your local Mac, that are compatible with all Macs. For more information about Universal Binaries, please check the developer documentation here: https://developer.apple.com/documentation/apple-silicon/building-a-universal-macos-binary


---

> #### My team produces a number of software components, and we've recently switched from packaging them as static frameworks to XCFrameworks. Xcode doesn't have a target type for producing XCFrameworks (please tell me that I'm wrong!), so that packaging happens in a post-archive script. When we were just producing static frameworks, we could drop several of our projects into a common workspace and it was easy to debug right into each of those frameworks -- Xcode figures out that since the Foo project builds the Foo framework, other projects in the workspace should link against that. We'd really like to do the same thing with our XCFrameworks, but they don't seem to be supported as well. Is there something we can do to debug into our XCFrameworks when their respective projects are all in a workspace together?

>>Thanks for reaching out! XCFrameworks are not intended to be used during development if you're a team that produces them. From your perspective, they should be used only for distribution. For development, you should use dynamic framework targets and then link them into your executable that you'd like to debug. For more information on multi-platform frameworks, we suggest to watch "Explore advanced project configuration in Xcode" WWDC Session, which is set to be released on Friday: https://developer.apple.com/wwdc21/10210 Please let us know if you have any follow up questions or thoughts!


---

> #### With Vim mode, can you support “V” to visual select lines? FB9145839

>>It looks like you already submitted the request via feedback assistant. Thanks for doing that, it's the best way to let us know of any enhancements you'd like to see.


---

> #### When are we going to see TestFlight beta user crash report with the user information? It helps a lot when we receive a bug report from the user and find the stacktrace of the crash.

>>In Xcode 13, the Crashes Organizer will show you feedback from TestFlight users of your app, including user information like name and email address if the user chose to provide it. In addition, you can view feedback and download the crash log directly from App Store Connect.


---

> #### Xcode Cloud looks great. Has the team (or others in-channel) tested it with Fastlane-based CI workflows to date ?

>>Xcode Cloud is designed to automatically build, test, and prepare your app for TestFlight and the App Store. And should you need to install additional third-party tools to accomplish other tasks, then this can be done using Custom Build Scripts.
You can learn more about extending Xcode Cloud here:
https://developer.apple.com/documentation/xcode/writing-custom-build-scripts


---

> #### We built SDK for our clients, not an app and have several semi-independent components.
I want to understand what would be ideal way to distribute our SDK as XCFramework where it depends on our two other internal static libraries. But out of one we need to distribute one dependency as dynamic framework mainly as XCFramework moving forward.
SDK is mix of Objc and Swift.
Existing Structure

- SDK - Distribution as Dynamic FAT Framework.
- SDK has dependency on StaticLibrary1 and resources which are brought using cocoapods - whose public API needs to be available as part of SDK public interface.
- SDK contains StaticLibrary2 - whose public API needs to be available as part of SDK public interface.

End goal is to distribute SDK as XCFramework. Should we distribute other dependency as XCFramework as well and integrator needs to bring all those three in order to utilize our SDK

>>An XCFramework can be used to distribute a framework for multiple platforms in a single bundle, but XCFrameworks don’t provide any support for distributing an SDK; you still need to distribute a single XCFramework for each framework you’re distributing. Xcode doesn’t provide support for constructing SDKs like you’re distributing; you would need to provide guidance to your users to use the frameworks you're providing to them.


---

> #### Xcode Cloud: can it produce signed Developer ID Mac apps? Two answers in this channel conflict (one said only App Store, the other said yes Developer ID)

>>Great question! Xcode Cloud does not support Developer ID-signed Mac apps. It supports Development and Mac App Store signing. I encourage you to submit a feedback request for any enhancements you'd like to see in the future!


---

> #### Will it be possible to be notified by email when a user submits a feedback(with a crash)?

>>That is great feedback! Please submit an enhancement request with details about what you'd like to see via Feedback Assistant.


---

> #### In Xcode Organizer, is there anyway to:

1. See watchdog crashes
2. Search by the stack trace content of a crash
3. See a version's all crashes and not only the top ones

>>There is not a way to see individual watchdog crashes in the Xcode Organizer. However the new Terminations metric in Xcode 13 will show you aggregated data about all types of terminations that customers are experiencing in your app.
Searching by the content of a stack trace is not available in the Organizer, but this would be a great enhancement to request via Feedback Assistant!
In Xcode 13, you can see up to 1000 of your app's crashes for the past year, so you can view and investigate far more than the top 25 crashes.


---

> #### Do you think Xcode Cloud has value for individual developers, or just teams?

>>Xcode Cloud can greatly add value, even for individual developers. It can easily help you validate the code you write by starting integration jobs that will also run your tests on multiple run destinations (i.e. iOS/iPadOS simulator, Mac Catalyst) in addition to building. This way, you will feel more confident about your work when shipping updates of your App (it also handles code-signing for you).
With Xcode Cloud, your project is ready to accept outside contributions without needing to maintain a CI infrastructure.


---

> #### What are some of your favorite tips and tricks for triaging crashes?

>>When I am looking at a new crash in an area I am not familiar with, one of the first things I do is try to understand _why_ it is crashing. Asking yourself questions can also help you get a quick understanding of what kind of crash it is. "Is it crashing because I made an assumption I shouldn't be?" "Is this framework I am using expecting something different than what I am giving it?" "Does this only happen sometimes?"
Answering those questions can get you started on understand the crash and then working towards a fix!

Using the Organizer's inspector allows me to see how big of a problem it is. It can tell me all about how many devices this crash is happening on, what versions of the OS, and new this year,: where is it crashing (App Store or TestFlight) and which app versions?


---

> #### Our project has multiple build configurations, not just release and debug. Currently with Cocoapods we can have different dependencies for each build configuration. Last time we checked this was not possible with SPM. Has this changed? If not, do you expect this to change in the future?

>>There is a proposal for conditional target dependencies (see https://github.com/apple/swift-evolution/blob/main/proposals/0273-swiftpm-conditional-target-dependencies.md) but this has not yet been fully implemented.


---

> #### Is there a way to only update a single package? If I have multiple packages specifying "Up to next major", Xcode will try to update all of them when I run "Update to Latest Package Versions" but sometimes I might only want to update a specific one.

>>Not at the moment, but it is something we are looking to make available on both the CLI and Xcode.


---

> #### Hi, SPM is incredible! Does the presence of Playgrounds for iPad mean that we can replace xcodeproj-based projects with SPM-based projects even for non-console apps? Thanks!

>>We're glad you like it! Because projects in Swift Playgrounds are built on top of the Swift Package format, they'll also work great in Xcode when making apps for iPadOS.


---

> #### What's the best method for simultaneously develop an app and its dependent Swift packages? (local development with submodule?)

>>Check out https://developer.apple.com/documentation/swift_packages/editing_a_package_dependency_as_a_local_package


---

> #### If I've got private dependencies from, let's say GitHub via ssh. If they require different credentials, I can use `~/.ssh/config` to tie ssh keys to specific configurations. SPM uses that perfectly. But if I put same Package.swift file into Xcode, it does not. Is it possible to make Xcode behave like `swift package` commands in this case? Thanks!

>>You can configure Xcode to use the system git instead of the builtin one, which is what SwiftPM uses on the command line. See "Use Your System’s Git Tooling" in https://developer.apple.com/documentation/swift_packages/building_swift_packages_or_apps_that_use_them_in_continuous_integration_workflows


---

> #### Have there been improvements to checkout times in Xcode 13? We've had to fork many of our dependencies and remove the `git` history to allow for reasonable checkout times.

>>Starting Swift 5.4, Swift Package Manager caches package dependencies on a per-user basis, which reduces the amount of network traffic and increases performance of dependency resolution for subsequent uses of the same package. This means that while the initial clone may still be slow depending on the repository size, subsequent clones are much faster. This can also be used in CI by configuring the cache location. See https://swift.org/blog/swift-5-4-released/


---

> #### Is there a way to check to see if there are updates available without installing them, equivalent to `pod outdated`?

>>There's not a convenient command to do this yet. As a workaround, you could perform an update, then revert your `Package.resolved` file to its prior state with your source control system to go back to your old dependency versions.

---

## 🗓 Friday

>What is the recommended practice to run a test case only on the specific OS version?
Adding `@available(iOS 14, *)` directive on the top of class declarations seems not working somehow.

>>The recommended way for this is to use one of the XCTest APIs to skip the test if the required APIs are not available.  There is more information at https://developer.apple.com/documentation/xctest/methods_for_skipping_tests

---

> #### Maybe this has been asked, but with the new TestFlight crash logging in Xcode Organizer, will there be a way to integrate this with issue trackers like Jira?

>>Sorry, we don't have the answer to your question in this lab, but, if you haven't already seen it, I would recommend checking out the Triage TestFlight crashes in Xcode Organizer video at https://developer.apple.com/wwdc21/10203

---

> #### Are there any recommended workflows for using run scripts inside of a Package.swift file? There have definitely been a few times where I've wanted to have my Package.swift file generate things like compiled protobuf models. Thanks!

>>[SE-0303](https://github.com/apple/swift-evolution/blob/main/proposals/0303-swiftpm-extensible-build-tools.md) defines new functionality in SwiftPM enabling the creation of structured plugins for code generation.

---

> #### Can Static Analyzer in Xcode 13 find dead code like unused variables and methods?

>>Xcode provides compiler warnings for unused local variables and unused static functions and unused C++ private methods. There are no warnings for unused Objective-C methods because in Objective-C it's a fairly hard technical problem as it's a very dynamic language. The static analyzer provides additional warnings for problems that require deeper analysis such as unused initializations or assignments that get immediately overwritten ("dead stores") and redundant if-clauses and operands in expressions (new in this release! - off by default, see the "Unused Code" section in build settings).

> Do Static Analyzer works with Swift?

The static analyzer can detect bugs in C/C++ and ObjC code. It doesn’t detect bugs in Swift code. However, you can use the analyzer on mixed Swift-ObjC projects. We recommend running the analyzer on your project if it has C/C++ or ObjC code as some issues the analyzer can detect on ObjC code can manifest as hard-to-find bugs when used from Swift.

---

> #### How big would be the time impact on building with Xcode if I turn ON the “Analyze During build flag?”

>>Usually we expect 2-5x slowdown. You can set mode for analysis to "Shallow", which is generally faster (2-3x slowdown), but finds fewer bugs.

---

> #### Does the Static Analyzer, analyzes the full source code of my project including third party libraries and frameworks? If so, can I configure which frameworks to exclude?

>>This is a great question! The static analyzer will analyze all third party code as long as it's getting compiled when you're building your app (i.e., binary frameworks aren't analyzed) and it's written in a language supported by the static analyzer (i.e. C/C++/Objective-C but not Swift). There is no way to exclude individual frameworks but given that the source code is available, you may have a chance to edit it to suppress unwanted warnings.

---

> #### Is there any point in running on the static analyzer on pure Swift code?

>>If your app has only Swift code, the analyzer wouldn’t find any issues. However, if your project has ObjC code as well, we recommend running the analyzer.

---
