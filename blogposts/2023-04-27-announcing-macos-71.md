---
post_title: "Introducing React Native macOS 0.71"
author1: sanajmi
post_slug: 2023-04-27-announcing-macos-71
post_date: 2023-04-27 12:00:00
categories: react-native
tags: react-native, announcement, launch, react-native-macos, release
summary: "A new phase for react-native-macos: introducing version 0.71 and experimental support for the new architecture!"
---

Hello everyone,

We're thrilled to announce the latest release of React Native macOS: 0.71. There’s a lot to talk about with this new minor version, so let’s get right into it!

### Catching up to core

This is the first time that react-native-macos is caught up with its [iOS, Android](https://github.com/facebook/react-native) and [Windows](https://github.com/microsoft/react-native-windows) counterparts, and our goal is to keep it that way going forward. We have already started working on 0.72 to ensure that developers targeting all the platforms can swiftly upgrade across the board once the new version comes out.

To reach this goal, as some of you might have noticed, we had to completely skip some minor releases (such as 0.69 and 0.70). We had to take this tradeoff to accommodate our internal challenges, motivated by the fact that it is the same code that we use in our products (very much like how react-native core is used by Meta). For this reason, it will get more stable as we use it more internally: we push a new patch release on every commit so you can expect in the future new patches to be published as we quickly catch and fix bugs.

### A successful collaboration

A lot of work went into making React Native macOS better for this release, and a special thanks must be given to the Meta and Messenger Desktop engineers that regularly submitted fixes and improvements. In particular, we wanted to shout out [Shawn Dempsey](https://github.com/shwanton) and [Christoph Purrer](https://github.com/christophpurrer) for their great work on the many component (such as TextInput and ScrollView) and adding support for Fabric, among many other things!

### Bringing the new architecture from iOS to macOS

Talking about Fabric: the reason why react-native-macos is a fork and not a fully independent implementation, is that a lot of the existing iOS code can be easily massaged into working for macOS. While this introduces extra complexity in its regular maintenance, it allows to quickly get access to new features -- including the new architecture.

With this release, we are excited to introduce an experimental preview of Fabric for macOS!

It uses the same flags as iOS, and you can test it out the full new architecture on any project you like - by passing the ` RCT_NEW_ARCH_ENABLED=1` flag when running the pod install.

### What does this release bring?

This release builds on top of the React Native core - and we encourage you to check out the blogpost highlighting the main changes for the versions between 0.68 and 0.71:

- https://reactnative.dev/blog/2022/06/21/version-069

- https://reactnative.dev/blog/2022/09/05/version-070

- https://reactnative.dev/blog/2023/01/12/version-071

It's worth noting that the TypeScript types are not fully ready yet - only the core ones are included in this release.

We have also spent a lot of effort on cleaning up the divergence points between React Native and our fork to make future releases smoother: this includes removing (thousands!) of stale files, upstreaming various bug fixes, and cleaning up RNTester-macOS. In doing so, we were also able to spot bugs and improvements that we sent back upstream, for the benefit of everyone using react-native on any platform.

For a complete list of all the changes, please refer to the [0.71 GitHub Release](https://github.com/microsoft/react-native-macos/releases/tag/v0.71.2) – it contains the full changelog. For a smoother upgrade experience, check out our tools including [align-deps](https://microsoft.github.io/rnx-kit/docs/guides/dependency-management) and the website [upgrade-helper](https://react-native-community.github.io/upgrade-helper/?package=react-native-macos).

We hope that you will find this new release useful and continue to support us as we work to make React Native macOS better than ever!

Happy coding!

---

You can also follow us on Twitter [@ReactNativeMSFT](https://twitter.com/reactnativemsft) to keep up to date on news, feature roadmaps, and more.
