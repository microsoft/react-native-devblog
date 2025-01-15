---
post_title: "A look into the new architecture on RNW 0.76 and 0.77!"
author1: tatianakapos
post_slug: 2025-22-25-new-architecture-on-0.76-0.77
post_date: 2025-22-25 12:00:00
categories: react-native
tags: react-native, releases, react-native-windows
summary: We've recently released React Native Windows 0.76 and 0.77- marking the first time we invite developers to create RNW experiences on the new architecture
---

React Native Windows has exciting updates with versions 0.76 and 0.77! While React Native made the New Architecture the default starting in 0.76 —introducing improvements like the advanced Fabric rendering system— React Native Windows is taking a more gradual approach. Version 0.76 offers our first preview of the New Architecture, and 0.77 builds on it with stabilization and new features. Rest assured, we’re continuing to maintain reliability and full support for the default Paper architecture as we look ahead. Let's dive in and see what’s coming!

# New Architecture

React Native for Windows’ New Architecture is here—but with some caveats. It’s best suited for early adopters ready to explore a work-in-progress experience with limited documentation. If you're up for it, this is your chance to preview the future of React Native Windows development! 🚀

In the Old Architecture (Paper), we relied on the Universal Windows Platform (UWP). With the New Architecture (Fabric), we’ve transitioned to the modern Windows App SDK. This upgrade allows us to leverage React Native’s cross-platform rendering logic while better integrating Windows-specific features.

All New Architecture apps will now be Win32-based and built on the Windows App SDK, aligning with Microsoft’s latest app development recommendations. Meanwhile, Old Architecture apps will remain UWP-based, and it’s still possible to create and maintain them using legacy templates.

While there are no immediate plans to deprecate Old Architecture, future investments are focused on the New Architecture. As React Native moves away from Old Architecture, React Native for Windows will follow, with clear migration guidance provided when the time comes.

Have more questions? Read our [FAQ in our new architecture landing page](https://microsoft.github.io/react-native-windows/docs/new-architecture#faq)!

## Steps on creating on new architecture application

Let's dive into creating a new architecture application. You'll find the steps below are almost exactly the same as creating a old architecture application - with the only change being is specifiying new architecture template in step 4.

1. Create a new React Native Project

npx --yes @react-native-community/cli@latest init <projectName> --version "latest"

2. Navigate into this newly created directory

cd <projectName>

3. Add React Native Windows to your project's dependencies

yarn add react-native-windows@^0.77.0

4. Initialize the React Native Windows native code and projects with the [new architecture templates](https://microsoft.github.io/react-native-windows/docs/init-windows-cli#templates)

yarn react-native init-windows --template cpp-app --overwrite

5. Run your RNW app as normal

npx react-native run-windows

You have now successfully created a RNW app on the new architecture!

// PLACEHOLDER: Insert clip of newly made Fabric application

## New Architecture changes in 0.76 and 0.77

### New Templates

With the preview release of the new architecture, we're introducing brand-new templates! These templates are built to use the precompiled Microsoft.ReactNative NuGet packages by default, enabling faster build times. In the previous architecture, this experience was experimental and had limited support. However, in the new architecture, it has been the primary approach from day one.

We now offer two new templates:

cpp-app: For React Native Windows Applications (New Architecture, C++, Win32, Hermes)
cpp-lib: For React Native Windows Turbo Modules (New Architecture, C++)

In version 0.77, we further refined these templates, updating CLI commands like `autolink-windows` and `init-windows` to adapt their behavior based on the template used to create the application.

Check out the full list of supported templates [here](https://microsoft.github.io/react-native-windows/docs/init-windows-cli#templates).

### Introducing Modal

In the old UWP-based implementation, we had to work around some frustrating limitations—Modal wasn’t an option, so developers had to use Windows-specific components like Flyout and Popup instead. This created extra work for those transitioning iOS or Android apps, as they needed to rework their code specifically for Windows. But good news—those limitations are a thing of the past! With the New Architecture, those roadblocks are gone, and we’re thrilled to introduce the new *(for RNW)* [Modal component](https://reactnative.dev/docs/modal) in 0.77. We hope customers using Flyout and Popup will find it easy to switch over to Modal and enjoy a more seamless cross-platform experience!

// PLACEHOLDER: Insert clip of Modal in rntester or app

As a brand-new component, Modal is still a work in progress, and we’re actively enhancing its functionality. Currently, it supports the `visible`, `onShow`, and `onDismiss` properties/events. We're hard at work adding support for `transparent`, `backdropColor`, and exciting Windows-specific features like setting a title bar and moving the modal outside of the application boundaries. Stay tuned for more updates!

### New Features for Accessibility, TextInput, and Custom Components

The 0.77 release of the new architecture brings exciting updates to explore! You can now enable custom controls to implement the `expand`/`collapse` action, support a wider range of accessibility features, and take advantage of new properties like `autocapitalize` and `cursor` in TextInput. Check out all the details in our [release notes for 0.77](PLACEHOLDER: link release notes) under the section `New Architecture-Specific Changes`.

## Commitment to Reliability in the Old Architecture

While we’re focused on rolling out new features for the new architecture, we’re still fully committed to supporting the old architecture. Although no new features are being added, we've made several key reliability improvements. These include resolving Flyout/Alert crashes on Windows 10, optimizing our Text component, ensuring consistent button sizes when pressed, and much more. Additionally, we've added support for TurboModule event emitters, enhanced new project namespace/cleanup, and increased the method parameter limit from 7 to 10.

For a deeper dive into these reliability improvements, check out our [0.77 release notes](PLACEHOLDER: link release notes) and  our [0.76 release notes](https://github.com/microsoft/react-native-windows/releases/tag/react-native-windows_v0.76.0)!

---

If you’re interested in getting started with React Native for Windows, check out our website at [aka.ms/reactnative](https://microsoft.github.io/react-native-windows/).

You can also follow us on X [@ReactNativeMSFT](https://twitter.com/reactnativemsft) to keep up to date on news, feature roadmaps, and more.
