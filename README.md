# CocoaPods-Test

This project is intended to illustrate an issue I'm having with CocoaPods.

To observe the issue, do the following:

- Open `CocoaPods-Test.xcworkspace` in Xcode.
- Note that the `CocoaPods-Test` target builds and runs successfully, importing `PromiseKit` and `TwilioChatClient` pods.
- Navigate to `Playground.playground` within the workspace. 
- Note that the playground executes fine while importing `PromiseKit` but if `TwilioChatClient` is imported, playground execution fails with "no such module 'TwilioChatClient'".

After reading the following resources:

- [https://github.com/CocoaPods/CocoaPods/issues/5334](https://github.com/CocoaPods/CocoaPods/issues/5334)
- [https://github.com/CocoaPods/CocoaPods/issues/5215](https://github.com/CocoaPods/CocoaPods/issues/5215)
- [https://github.com/CocoaPods/CocoaPods/issues/5563](https://github.com/CocoaPods/CocoaPods/issues/5563)
- [https://github.com/CocoaPods/swift/issues/3](https://github.com/CocoaPods/swift/issues/3)
- [https://github.com/CocoaPods/CocoaPods/issues/4135](https://github.com/CocoaPods/CocoaPods/issues/4135)
- [https://github.com/CocoaPods/CocoaPods/issues/2240](https://github.com/CocoaPods/CocoaPods/issues/2240)
- [https://github.com/CocoaPods/CocoaPods/issues/6669#issuecomment-300188519](https://github.com/CocoaPods/CocoaPods/issues/6669#issuecomment-300188519)
- [https://guides.cocoapods.org/using/troubleshooting.html](https://guides.cocoapods.org/using/troubleshooting.html)
- [https://www.objc.io/issues/6-build-tools/cocoapods-under-the-hood/](https://www.objc.io/issues/6-build-tools/cocoapods-under-the-hood/)

I think the issue is probably related to the fact that `TwilioChatClient` is a "vendored framework" (see its [podspec](https://github.com/twilio/cocoapod-specs/blob/master/TwilioChatClient/1.0.0/TwilioChatClient.podspec)), which means a pod target is not created for it. After reading the above resources, I feel like a solution is within reach, but I can't quite figure it out.

While the sample project here illustrates what I think is the underlying issue, the issue that prompted me to create this example project is just a small bit more complicated. 

In my project, I create a framework target containing all my app's code (so it can be imported into my app and also into my playgrounds using app resources). This framework then has pod dependencies, including `PromiseKit` and `TwilioChatClient`. The execution error in the playground is different ("Couldn't lookup symbols" instead of "no such module"), as I am not importing the CocoaPods module directly but my framework which uses the pod framework.

I suspect if I can solve the "no such module" issue, it will help me solve my "couldn't lookup symbols" issue.