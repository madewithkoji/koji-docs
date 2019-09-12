# Developing for KaiOS

Koji is the easiest way to create apps for KaiOS devices. This document will help you take existing Koji apps and transform them to be KaiOS-ready. This includes:
  - Configuring your bundler to target the KaiOS browser
  - Using Koji live preview to test on-device
  - Modifying user input methods to target KaiOS D-Pad navigation
  - Enabling the KaiOS Plugin to serve a manifest for your app
  - Submitting your app to the KaiOS app store

This document is a work in progress. If you have additional questions or find yourself stuck, please join our [Discord Chat](https://discord.gg/eQuMJF6) for live help. If you would like to contribute to this guide, message @seant on Discord, or submit a pull request to this document's source on [Github](https://github.com/madewithkoji/koji-docs).

## What is KaiOS?

[KaiOS](https://www.kaiostech.com) is a web-based mobile operating system that enables a new category of smart feature phones.

KaiOS brings support of 4G/LTE, GPS, and Wi-Fi, as well as HTML5-based apps and longer battery life, to non-touch devices. It has an optimized user interface for smart feature phones, needs little memory, and consumes less energy than other operating systems. It also comes with the KaiStore, which enables users to download applications in categories like social media, games, navigation, and streaming entertainment.

KaiOS powers millions of low-powered devices, like the Reliance JioPhone. It is the [second most popular operating system in India](https://www.androidauthority.com/india-kaios-market-share-885349/), after Android, and its users are often using it to experience the Internet for the first time. Developing and publishing KaiOS apps brings with it the potential to reach a large segment of the Next Billion Users in the developing world, whose needs and constraints might be radically different than those identified using the "default," developed-world framework we typically apply when thinking about creating mobile apps.

Koji introduces a new dimension to this opportunity. Not only does Koji make it incredibly easy to rapidly develop, test, and deploy KaiOS apps, Koji allows app developers to turn their apps into templates. These templates can then be remixed and redeployed by anyone, regardless of whether or not they know how to code.

We believe that the creation of an ecosystem where developers from around the world create app templates that are then remixed, localized, and deployed by anyone will empower a new class of "citizen developers" and local entrepreneurs within these newly-connected communities to create apps, tools, and games that are hyper-relevant to their community and solve the problems to which they are most proximate. The best person to identify an opportunity in a newly-connected local economy in India or Africa and use technology to take advantage of that opportuniry is not someone sitting in an office in Silicon Valley; it is someone who themselves is a member of that community.

## General technical overview

KaiOS apps are simply web apps. They come in two flavors: hosted apps, which are web apps that run remotely and serve a manifest file, and privileged apps, which run on device and have access to more secure sensors like camera and GPS.

In this overview, we will be focused on creating a hosted KaiOS app. If you need access to these additional sensors, please see the bottom of this document for additional instructions on how to take your hosted app and export it as a privileged app.

Developing a KaiOS app is not much different than developing a standard browser-based web app. The sections below explain some of the additional things you need to configure in order to get your app working on KaiOS. You can learn more by visiting the [KaiOS developer portal](https://developer.kaiostech.com/).

### Configuring your bundler to target the KaiOS browser

KaiOS is based on Mozilla OS, and the closest target browser that you can install on your computer to test compatibility is Firefox 59. You can install Firefox 59 from Mozilla's FTP server [here](https://ftp.mozilla.org/pub/firefox/releases/59.0/). If your app runs in Firefox 59 without any render-blocking exceptions, there is a good chance it will run correctly on the KaiOS device.

The easiest way to configure your bundler to target KaiOS is through the users of [Browserslist](https://github.com/browserslist/browserslist). Browserslist is a configuration format that helps tools like Babel and Parcel transpile code. In your apps frontend root (the folder where your `package.json` is located), create a new file called `.browserslistrc`. In that file, paste the following contents:
```
last 1 version
kaios
```

This tells your bundler to target the most recent version of each major browser, as well as the KaiOS browser. If your project is bundled using [Parcel](https://parceljs.org/transforms.html)  (look at the start commands inside your `package.json` to see), browserslistrc is picked up automatically. All you need to do is Ctrl-C in the frontend terminal to kill the process and re-run `npm start` to start your project's development server with the new settings. If your project is bundled used Babel, you must include `@babel/preset-env` in order for Babel to recogznize the Browserslist instructions. You can read more about configuring Browserslist [here](https://github.com/browserslist/browserslist-example).

### Using Koji live preview to test on-device

**If you do not have access to a KaiOS device, you can follow a modified version of these instructions using Firefox 59.** If you would like to use a KaiOS device for testing, [this one](https://www.amazon.com/gp/product/B07FSCTVCF/ref=ppx_yo_dt_b_search_asin_title?ie=UTF8&psc=1) is available in the US from Amazon.

Once your project is running in Koji and targeting the KaiOS browser, you can click the "Remote Preview" button in the Koji editor's right-side preview UI and scan the QR code using the device to open the development preview. If you cannot scan the QR code, you can click "Copy URL" or "Open in new tab" to get the raw URL.

Once you have loaded the staging URL on the device, you will see the device appear in the list below the QR code. In development mode, Koji streams `console` logs from remote devices back into the editor. Simply click on the device to view logs as you are developing. Depending on how your bundler is configured, you should be able to make changes to the code and see them hot-reload in real-time on the device.

### Modifying user input methods to target KaiOS D-Pad navigation

KaiOS devices do not use a touch screen. Most apps and games on Koji include keyboard controls in addition to touch controls -- in most cases, modifying these should simply be a matter of identifying the keycodes in the app and replacing their values with more appropriate keycodes for KaiOS devices (D-Pad + number pad). You can read more about KaiOS keycodes [here](https://developer.kaiostech.com/core-developer-topics/softkeys).

You should also be aware of how your app handles accessibility in response to screen size, as KaiOS devices run at a small screen size.

It is a good idea to detect the app's user agent in order to provide an intuitive experience across device classes. For example, if an app is loaded in KaiOS, the key bindings should reflect the KaiOS device. However, if the same app is loaded on desktop or Android/iOS, it should use desktop keybindings or touch inputs. Building in this way allows your app to run on every class of device, not just KaiOS.

### Enabling the KaiOS Plugin to serve a manifest for your app

After you have deployed your app using Koji, you can use the Koji editor to navigate to "Tools" -> "Plugins" and enable the "KaiOS Manifest" file. Use the configuration screen to specify data to be included in the manifest. This manifest is what KaiOS uses in order to understand how to render your app on the device and list it in the app store.

If you use KaiAds in your app, you can also enable the KaiAds plugin to serve ads on the device.

### Submitting your app to the KaiOS App Store

Follow instructions [here](https://developer.kaiostech.com/submit-to-kaistore).

### Exporting your compiled bundle to submit as a privileged app

If your app uses privileged APIs, you will need to submit a bundle to the KaiStore. To find your compiled bundle, you must first navigate to "Tools" -> "Domains" in the Koji editor. Copy your frontend domain and use it to create the following path: `https://y-frontend-domain-id.withkoji.com/.well-known/koji/archive.zip`. This link will download a ZIP archive of your app's frontend that you can submit to KaiStore. If your app uses a backend, that bundle will reference the Koji-hosted backend.
