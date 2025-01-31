---
post_title: "A look into the new architecture on RNW 0.76 and 0.77!"
author1: tatianakapos
post_slug: 2025-01-29-new-architecture-on-0.76-0.77
post_date: 2025-01-31 12:00:00
categories: react-native
tags: react-native, releases, react-native-windows
summary: We've recently released React Native Windows 0.76 and 0.77- marking the first time we invite developers to create RNW experiences on the new architecture.
---

React Native Windows has exciting updates in versions 0.76 and 0.77! While React Native made the New Architecture the default starting in 0.76 —introducing improvements like the advanced Fabric rendering system— React Native Windows is taking a more gradual approach. We’re still actively refining the New Architecture to ensure it meets the high standards our developers expect, so it’s not the default just yet. With version 0.76, we’re offering our first preview of the New Architecture, and 0.77 builds on this foundation with key stabilization and new features. At the same time, we remain fully committed to maintaining reliability and robust support for the default Paper architecture. Let’s dive in and explore what’s new!"

# 🌟 New Architecture (Preview)

React Native for Windows’ New Architecture is here — with some caveats. It’s best suited for early adopters ready to explore a work-in-progress experience with limited documentation. If you're up for it, this is your chance to preview the future of React Native Windows development! 🚀

In the Old Architecture (Paper), we relied on the Universal Windows Platform ([UWP](https://learn.microsoft.com/en-us/windows/uwp/)). With the New Architecture (Fabric), we’ve transitioned to the modern [Windows App SDK](https://learn.microsoft.com/en-us/windows/apps/windows-app-sdk/). This upgrade allows us to leverage React Native’s cross-platform rendering logic while better integrating Windows-specific features.

All New Architecture apps will now be Win32-based and built on the Windows App SDK, aligning with Microsoft’s latest app development recommendations. Meanwhile, Old Architecture apps will remain UWP-based, and it’s still possible to create and maintain them using legacy templates.

While there are no immediate plans to deprecate Old Architecture, future investments are focused on the New Architecture. 

Have more questions? Check out the [New Architecture FAQ](https://microsoft.github.io/react-native-windows/docs/new-architecture#faq)!

## 🛠️ Steps on creating on New Architecture application on 0.77

Let's dive into creating a new architecture application. These steps are tailored for version 0.77, but you can visit our [Getting Started page](https://microsoft.github.io/react-native-windows/docs/getting-started) for guidance on other versions. You'll find the steps below are almost exactly the same as creating an old architecture application - with the only change being is specifying a new architecture template in step 4.

1. Create a new React Native Project

`npx --yes @react-native-community/cli@latest init <projectName> --version "latest"`

2. Navigate into this newly created directory

`cd <projectName>`

3. Add React Native Windows to your project's dependencies

`yarn add react-native-windows@^0.77.0`

4. Initialize the React Native Windows native code and projects with the [new architecture templates](https://microsoft.github.io/react-native-windows/docs/init-windows-cli#templates)

`yarn react-native init-windows --template cpp-app --overwrite`

5. Run your RNW app

`npx react-native run-windows`

Congrats, you’ve just created an RNW app using the new architecture! 🎉

[new-fabric-app-2025](assets/2025-01-24-2025-new-architecture-on-76-and-77/new-fabric-app.gif)

## 🚧 New Architecture Changes in 0.76 and 0.77

### 📑 New Templates

The new architecture has been built from day one using the precompiled Microsoft.ReactNative NuGet packages, enabling significantly faster build times. The preview release of the new architecture has brand new templates that take advantage of the precompiled NuGet packages.

We now offer two new templates:

cpp-app: For React Native Windows Applications (New Architecture, C++, Win32, Hermes)
cpp-lib: For React Native Windows Turbo Modules (New Architecture, C++)

These templates can be easily used with the react-native init-windows CLI command: `yarn react-native init-windows --template <TemplateName> --overwrite` 

In version 0.77, we further refined these templates, updating CLI commands like `autolink-windows` and `init-windows` to adapt their behavior based on the template used to create the application.

Check out the full list of supported templates [here](https://microsoft.github.io/react-native-windows/docs/init-windows-cli#templates).

### 🪟 Introducing Modal

The old UWP-based implementation suffered from frustrating limitations in the underlying Windows platform including lack of `Modal` support. Although Windows developers could use Windows-specific components like `Flyout` and `Popup`, this created extra work for those transitioning iOS or Android apps, as they needed to rework their code specifically for Windows. 

Good news! Those limitations are a thing of the past! With the New Architecture, we’re thrilled to introduce the new *(for RNW)* [Modal component](https://reactnative.dev/docs/modal) in 0.77. We hope customers using `Flyout` and `Popup` will find it easy to switch over to `Modal` and enjoy a more seamless cross-platform experience. 

[modal-playground-composition-2025](assets/2025-01-24-2025-new-architecture-on-76-and-77/modal-playground.gif)
[*View Source Code*](https://github.com/microsoft/react-native-windows/tree/0.77-stable/packages/%40react-native-windows/tester/src/js/examples/Modal)

As a brand-new component, Modal is still a work in progress, and we’re actively enhancing its functionality. Currently, Modal supports the `visible`, `onShow`, and `onDismiss` properties/events. We're hard at work adding support for `transparent`, `backdropColor`, and exciting Windows-specific features like setting a title bar and moving the modal outside of the application boundaries. Stay tuned in our New Architecture [tracking issue](https://github.com/microsoft/react-native-windows/issues/12042) for more updates!

### 👩‍💻 New Features for Accessibility, TextInput, and Custom Components

The 0.77 release of the new architecture brings exciting updates to explore! You can now enable custom controls to implement the `expand`/`collapse` action, support a wider range of accessibility features, and take advantage of new properties like `autocapitalize` and `cursor` in TextInput. Check out all the details in our [release notes for 0.77](https://github.com/microsoft/react-native-windows/releases/tag/react-native-windows_v0.77.0) under the section `New Architecture-Specific Changes`.

## 🛡️ Commitment to Reliability in the Old Architecture

While we’re focused on rolling out new features for the new architecture, we recognize the need to support the many folks that are still on the old architecture. Although no new features are being added to the old architecture, we recently made several key reliability improvements including resolving Flyout/Alert crashes on Windows 10, optimizing the Text component, ensuring consistent button sizes when pressed, and more. Additionally, we've added support for TurboModule event emitters, enhanced new project namespace/cleanup, and increased the method parameter limit from 7 to 10.

For a deeper dive into these reliability improvements, check out our [0.77 release notes](https://github.com/microsoft/react-native-windows/releases/tag/react-native-windows_v0.77.0) and  our [0.76 release notes](https://github.com/microsoft/react-native-windows/releases/tag/react-native-windows_v0.76.0)!

---

If you’re interested in getting started with React Native for Windows, check out our website at [aka.ms/reactnative](https://microsoft.github.io/react-native-windows/).

You can also follow us on X [@ReactNativeMSFT](https://twitter.com/reactnativemsft) to keep up to date on news, feature roadmaps, and more.
