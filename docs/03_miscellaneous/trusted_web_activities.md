# Trusted Web Activities

The Koji [**Trusted Web Activities**](https://developers.google.com/web/updates/2019/02/using-twa) plugin lets you easily generate an Android APK from your Koji app and submit it to the Google Play store.

## General overview

On Android and [Android Go](https://www.android.com/versions/go-edition/), Koji apps already run great inside the browser. By configuring the "PWA Manifest" plugin, you can make your app installable to the Home Screen on Android devices. Trusted Web Activities take this a step further, allowing you to generate an APK from your PWA that lets your Android App launch a full screen browser tab without any browser UI.

This APK can then be listed on the Google Play Store alongside native apps for both Android and Android Go.

**Koji makes it trivially easy to make your app TWA-Ready.** The instructions below will help you generate an APK that launches your Koji app as a Trusted Web Activity.

## Prerequisites

- Download and install [Android Studio](https://developer.android.com/studio/install) on your computer.
- [Configure your Android device for USB debugging](https://developer.android.com/studio/debug/dev-options.html#enable)
- Make sure your Android device is running Chrome version 72 or later

## Creating your APK

- [Download or clone the Google Chrome sample TWA repository](https://github.com/GoogleChromeLabs/svgomg-twa)
- Open the repository in Android Studio
- Open `app/build.gradle` and find the `twaManifest` section.
- Publish your Koji app and navigate to the Tools section of the editor. Open the "Domains" page and copy your project's `frontend` domain. Paste this domain in the `hostName` section of the manifest in Android Studio. Give your Android app a name, and a unique Application ID. Save the file and press "Run" to build the app and load it onto your connected device.
- If you open the app, you should see your Koji app. However, notice that it renders with a browser toolbar at the top. The next steps, enabling TWA support, remove that bar and make your app behave as if it were a native Android app.

## Enabling Digital Asset Links

- Download the [Google Asset Link Tool](https://play.google.com/store/apps/details?id=dev.conn.assetlinkstool) to your Android Device. Open it and find the ID for your application. Copy the "Package Signature" value
- Navigate to the "Plugins" page in the Koji editor. Click to enable the "TWA Digital Asset Links" plugins. Click again to configure the plugin. Enter the ID of the your application and paste the signature you copied from the Asset Link tool.
- Save the plugin and re-open the app on your device. The top bar will be removed and your app will behave as a native Android app.

Congratulations! You have successfully packaged your Progressive Web App as a native Android APK and are ready to submit it to the Google Play store. To learn more about how to submit an app to the Google Play store, see the following [guide](https://support.google.com/googleplay/android-developer/answer/113469?hl=en).
