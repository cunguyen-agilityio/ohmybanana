# How to Restrict Your iOS App to iPhone Only

If you're developing an iOS app and want to make sure it only runs on iPhone, and not on iPad—even in iPad's compatibility mode—there are a few key steps you need to follow. Simply unchecking iPad support on App Store Connect is not always enough, as iPads can sometimes still install iPhone-only apps unless properly restricted.

Here’s how to ensure your app is truly iPhone-only:

## 1. Update the UIDeviceFamily in Info.plist

Edit your app's `Info.plist` file and set the `UIDeviceFamily` key to include only `1`, which corresponds to iPhone:

```xml
<key>UIDeviceFamily</key>
<array>
    <integer>1</integer>
</array>
```

If you see `<integer>2</integer>`, that means the app supports iPad. Make sure to remove it.

## 2. Check Device Targeting in Xcode

In Xcode, go to your target settings:

- Open your project, click on the app target.
- Under the General tab → find Deployment Info → set Devices to iPhone only (not "Universal").

## 3. Avoid Using iPad-specific UI Layouts

Make sure your UI doesn’t trigger iPad behavior:

- Don’t use size classes or UI components tailored for iPad.
- If using SwiftUI, avoid .navigationSplitViewStyle() or .popoverPresentationController, which are commonly iPad-focused.

## 4. Double Check App Store Connect Settings

On [App Store Connect](https://appstoreconnect.apple.com/):

- Go to your app > **General** > **App Information**.
- Under **Device Families**, make sure **only iPhone** is selected.

## 5. Understand TestFlight Limitations

Keep in mind:

- **TestFlight may still allow installation on iPads** for testing purposes—even if the app is not intended to support them.
- This does not reflect what will happen when the app is released on the App Store.

To fully verify:

- Upload a new build with updated device settings.
- Remove the app from iPad and try installing again via TestFlight.
- Confirm that installation is blocked.
