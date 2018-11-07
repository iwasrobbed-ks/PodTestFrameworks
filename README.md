## UI test target issue

### Repro

1. Clone this repo
1. Run `$ bundle`
1. Run `$ pod install` and open the workspace
1. Try to run the UI tests, notice it quietly fails
1. Check the build/test logs and see something like the output below

### Issue

UI test targets won't build when using static lib support in [CP 1.5](https://github.com/CocoaPods/CocoaPods/pull/6966) for universal frameworks.

>2018-11-07 12:36:29.056238-0500 AppTargetUITests-Runner[15372:2311552] The bundle “AppTargetUITests” couldn’t be loaded because it is damaged or missing necessary resources. Try reinstalling the bundle.
>2018-11-07 12:36:29.056371-0500 AppTargetUITests-Runner[15372:2311552] (dlopen_preflight(/Users/rob/Library/Developer/Xcode/DerivedData/PodFrameworksIssue-dgbhzpefmnrciobxoyaigxwsmbph/Build/Products/Debug-iphonesimulator/AppTargetUITests-Runner.app/PlugIns/AppTargetUITests.xctest/AppTargetUITests): Library not loaded: @rpath/ObjectiveRocks.framework/ObjectiveRocks
>  Referenced from: /Users/rob/Library/Developer/Xcode/DerivedData/PodFrameworksIssue-dgbhzpefmnrciobxoyaigxwsmbph/Build/Products/Debug-iphonesimulator/AppTargetUITests-Runner.app/PlugIns/AppTargetUITests.xctest/AppTargetUITests
>  Reason: image not found)

In this case, `ObjectiveRocks.framework` is a universal framework [built using this script](https://github.com/KeepSafe/ObjectiveRocks/blob/frameworks/build_universal_framework.sh).

### Workaround

For now, copying over the AppTarget's `[CP] Embed Pods Frameworks` run script to the UI test target links it properly.

#### App target

<img width="477" alt="screen shot 2018-11-07 at 12 37 07 pm" src="https://user-images.githubusercontent.com/30269720/48151105-71f64b00-e28e-11e8-9605-bb2fdc4ec11f.png">

#### UI test target

<img width="509" alt="screen shot 2018-11-07 at 12 37 13 pm" src="https://user-images.githubusercontent.com/30269720/48151146-876b7500-e28e-11e8-84e7-e2d2537f6ad9.png">


### However

You need to name the test target's run script something other than `[CP] Embed Pods Frameworks` or else running `pod install` will just delete the run script.
