---
post_title: "Improved Gallery, Road to Fabric, RNW 0.74"
author1: tatianakapos
post_slug: 2024-05-15-improved-gallery-fabric-0.74
post_date: 2024-05-15 12:00:00
categories: react-native // don't change the category
tags: react-native, react-native-windows, releases
summary: We've recently release React Native 0.74! Alongside all the fantastic features from React Native, we've been hard at work enhancing the Windows experience, refreshing our Gallery App, and setting the stage for Fabric support.
---

Embark on an exciting journey with our revamped Gallery app, now aligned with the WinUI3 gallery to showcase a modern RNW application. We've spruced up multiple accessibility features and expanded example matrix. Plus, get a sneak peek at Fabric, our latest rendering system for React Native, designed for efficiency in C++. And guess what? React Native Windows 0.74 is now live, bringing you exciting updates, from empowered view managers to streamlined template support and enhanced Pressable component functionality. Explore these changes and more, and don't forget to follow us on Twitter @ReactNativeMSFT for the latest news and updates! 

# ✨ Improved Gallery App

We've been hard at work revitalizing our Gallery app, aligning it with the [WinUI3 gallery](https://www.microsoft.com/store/productId/9P3JFPWWDZRC?ocid=pdpshare) to showcase a more sleek and modern RNW application. In addition, we've elevated the accessibility features and broadened our array of examples to encompass clipboard functionality, straightforward fetch/post operations, as well as demonstrations of radio buttons and linear gradients.

Get React Native Gallery from the [Microsoft store]((https://www.microsoft.com/store/productId/9NPG0B292H4R?ocid=pdpshare)) or get the latest changes from our (Github repository)[https://github.com/microsoft/react-native-gallery]!

![rn-gallery-gif-2024](D:\react-native-devblog\blogposts\assets\gallery-demo-2024.gif)

# 🗺️ Roadmap to Fabric

Introducing [Fabric](https://reactnative.dev/architecture/fabric-renderer), the latest rendering system for React Native. Designed for efficiency across platforms in C++, RNW Fabric seamlessly targets Composition while allowing hosting XAML islands within. Fabric-powered apps default to Win32 architecture, aligning with WinAppSDK and WinUI3 standards. Stay tuned for migration guidance for current UWP RNW apps. Follow our journey to Fabric [here](https://github.com/microsoft/react-native-windows/issues/12042).

*I think it would be super slay to have a demo of an app here. Some choices include Gallery on Fabric (hopefully updated to Chris's latest changes), Playground RNTester, ChatAI App*

## Increasing Core Functionality

We have been hard at work getting component parity up to speed with the older Paper rendering system. Several components, including Views, TextInput, and ActivityIndicator, already support a large set of properties and methods, with more components in the work. Here's a current snapshot of our progress:

| Priority | Component | Available | Properties |
| --- | --- | --- | --- | 
| 0 | View| ✅ | 90% | 
| 0 | Text| ✅ | 85% | 
| 0 | Image| ✅ | 89% | 
| 0 | TextInput| ✅ | 80% | 
| 1 | ScrollView| ✅ | 84% | 
| 1 | Modal| 🟥 | 64% | 
| 2 | ActivityIndicator| ✅ | 86% | 
| 2 | Switch| ✅ | 90% | 
| 2 | RefreshControl| 🟥 | 28% | 
*As of February 2024*

## Getting Community Modules Onboard

*Do we have a plan for this?*

## Current Timeline

*Copied mostly from the Github issue, need to know what the current expected timeline is*

At this time, Fabric on Windows isn't quite ready for widespread adoption. While we're actively working to enhance both functionality and the developer experience, there are still significant gaps to address. If you're an early adopter who doesn't mind diving into an evolving landscape, you can explore Fabric [here](https://github.com/microsoft/react-native-windows/wiki/Using-the-new-architecture-templates). Keep in mind that documentation may be limited, and we recommend reviewing the provided lists of yet-to-be-implemented features before submitting any bug reports. Rest assured, as we continue our journey, support for Fabric will steadily expand.


# 🎉 React Native Windows 0.74 Release Now Available! 🎉

We're thrilled to announce the release of React Native Windows version 0.74, freshly rolled out on 4/29/2024! Alongside the enhancements brought by React Native 0.74, we've been hard at work implementing some exciting Windows-specific updates.

## New Features

### View Managers Empowered to Capture Pointers 🖱️ https://github.com/microsoft/react-native-windows/pull/8969

Now, view managers can control of the pointer with the new 'AllowUncaptured' flag. This feature enables continued receipt of events, such as PointerMoved, even after the initial press. Say hello to more sophisticated interactions, like dragging elements beyond their conventional boundaries. [Learn more...](https://github.com/microsoft/react-native-windows/pull/8969)

### Streamlined C++ Template Support for create-react-native-library 

Introducing a fresh template, seamlessly integrating with the 'npx create-react-native-library' command. Say goodbye to complexities; now adding Windows support for turbo modules is a breeze. [Learn more...](https://github.com/microsoft/react-native-windows/pull/12481)

### React Native Windows Upgraded to C++20  

With React Native Windows now adopting the latest C++20 standard, we're bringing our codebase up to speed with modern development practices. [Learn more...](https://github.com/microsoft/react-native-windows/pull/12656)

### Enable custom timer factory implementation 

Introducing `Microsoft.ReactNative.ReactCoreInjection.SetTimerFactory`, a game-changer for custom timer implementations in React Native for Windows. Ideal for environments lacking a `Microsoft.UI.Dispatching.DispatcherQueue`, this feature ensures seamless operation, even under diverse circumstances. [Learn more...](https://github.com/microsoft/react-native-windows/pull/12856)


### Enhanced Pressable Component with Disabled Property Support

In response to community feedback, we've bolstered the Pressable component's functionality. This update introduces native support for the `disabled` property, while also ensuring that the onKeyDown event handler behaves appropriately when the component is disabled. [Learn more...](https://github.com/microsoft/react-native-windows/pull/12799)

## Deprecations

### Direct Debugging as the Default

To streamline our debugging process, we're transitioning away from web debugging to align with core practices. For all engines except Hermes, we recommend utilizing direct debugging. While Hermes currently experiences challenges with direct debugging, rest assured, we're actively working on a solution for an upcoming patch release. Check for updates surrounding Hermes [here](https://github.com/microsoft/react-native-windows/issues/12982)

### CoreApp APIs and prototypes

CoreApp was created as a testing ground to explore potential directions for Fabric. However, as we pivoted towards a different trajectory, we've made the decision to deprecate these APIs.

## How can I upgrade my App to get all this good stuff??

The best way to upgrade to the latest **0.74** release is to follow our [upgrade documentation](https://microsoft.github.io/react-native-windows/docs/upgrade-app) where you can either upgrade with CLI commands or manually update your application using the Upgrade Helper. 

To get the full list of release details, including breaking changes, check out our [release notes over on our repo.](https://github.com/microsoft/react-native-windows/releases/tag/react-native-windows_v0.74.0)

---

If you’re interested in getting started with React Native for Windows, check out our website at [aka.ms/reactnative](https://aka.ms/reactnative).

You can also follow us on Twitter [@ReactNativeMSFT](https://twitter.com/reactnativemsft) to keep up to date on news, feature roadmaps, and more.
