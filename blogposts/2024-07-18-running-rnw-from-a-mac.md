---
post_title: "Building React Native for Windows Apps from a MacOS Device"
author1: chiaramooney
post_slug: 2024-07-18-running-rnw-from-a-mac.md
post_date: 2024-07-18 12:00:00
categories: react-native // don't change the category
tags: talk, conference, chain-react-conf, react-native, windows, mac
summary: Owning a Windows device is not a requirement to build and ship Windows experiences. In this blog post, I'll walk through how to develop React Native for Windows applications from your MacOS device.
---

Last year, I gave a talk at the Chain React conference ["Building for Microsoft: Stories from React Native Windows Maintainers"](https://www.youtube.com/watch?v=kMJNEFHj8b8&t=13s&pp=ygUeY2hhaW4gcmVhY3QgMjAyMyBjaGlhcmEgbW9vbmV5) and spoke all about the recent work Microsoft was doing with the React Native for Desktop platform and how Microsoft is leveraging React Native within key products such as Office and the Windows OS.

I thoroughly enjoyed my experience last year as a speaker. I also loved the conversations I had with developers at the conference after my talk. It was so great to hear how many of you were excited by React Native for Desktop and that our message that Microsoft has bet on this platform has landed.

In all the conversations, one comment rang especially clearly: _*"React Native for Windows sounds great! I'd love to give it a try on my product…but I use a Mac as my development device."*_ Many of you were excited by React Native for Windows, but thought you were unable to give the platform a try because you use a Mac as your development machine and didn't have access to a Windows device.

The React Native for Windows team has heard the community feedback. While running on a Windows device will be the best experience for developing on Windows; we recognize that not everyone involved in a development team can have access to multiple devices to develop for different operating systems. We also see that Mac computers are used widely within the React Native developer community.

In today's blog post, I'm going to walk through a few options for how to develop React Native for Windows applications from your Mac computer because owning a Windows device is not a requirement to build and ship Windows experiences.

To build Windows experiences from a non-Windows device, we'll leverage virtual machine software. There are several options you have as a developer to spin-up a Windows VM from a Mac computer including Parallels, VirtualBox, VMWare, and Microsoft Dev Box to name a few. In this post, I'll walk through my experience using Parallels and Microsoft Dev Box.

## Creating a Windows VM using Parallels

The first Windows VM option I tried was [Parallels](https://www.parallels.com/). Parallels is a software which can be locally installed on a developer's MacOS machine to be used to create virtual machines of a variety of operating systems.

After signing up for the [Parallels free trial](https://www.parallels.com/products/desktop/trial/), I was given a download of the Parallels Desktop software and installation instructions. After Parallels Desktop was installed and running, the software prompted me to install Windows 11. From there, I was able to run a Windows VM.

With my virtual machine running, I was able to use the VM as if I was on a Windows computer. From here, I could follow the ["System Requirements"](https://microsoft.github.io/react-native-windows/docs/rnw-dependencies) and ["Getting Started"](https://microsoft.github.io/react-native-windows/docs/getting-started) documentation for React Native for Windows. Parallels had access to my local files, so I did not need to redownload any source code to get started developing. When I was finished with my development session, I was able to close the virtual machine and the machine's state was saved for next time.

![](assets/2024-07-18-running-rnw-from-a-mac/Screenshot-Parallels.jpg)

## Takeaways about Parallels

In less than an hour, I was able to spin up a Windows VM on a Mac and use that environment to build a React Native for Windows app. Windows VM’s allowed me to quickly experiment with adding Windows support to my existing React Native app because of the low level of overhead to get a VM up and running.

## Creating a Windows VM using Microsoft Dev Box

The second Windows VM option I tried was [Microsoft Dev Box](https://azure.microsoft.com/en-us/products/dev-box/). Microsoft Dev Box is a [Microsoft Azure](https://azure.microsoft.com/en-us) product that allows developers to spin up a Windows VM from most modern devices including MacOS devices.

Microsoft Dev Box supports:

1. Cloud-based virtual desktop infrastructure.
1. Project-based dev box configurations which allow for code repositories and tools for your current task to be preinstalled.
1. Region-specific dev boxes to help ensure that dev team members have a high-fidelity experience anywhere in the world.
1. Secured cloud workstations via Microsoft Intune.

Before you get started creating a VM with Microsoft Dev Box, there are a couple troubleshooting callouts I want to mention:

1. You will need an Azure subscription that is not the ["free trial"](https://azure.microsoft.com/en-us/free/) subscription. You must upgrade to the [“Pay As You Go”](https://azure.microsoft.com/en-us/pricing/purchase-options/pay-as-you-go/) subscription to use Microsoft Dev Box, but you can use the $200 of free credits that you gained via the "free trial" subscription as initial payment within the “Pay As You Go” plan. If you try to create a Dev Box Definition with the "free trial" Azure subscription, you will received a "Quota has been reached" error.
1. You will also need a work or school MSA (Microsoft Account). Personal or guest MSA’s cannot be used with the Dev Box portal at this time.
1. Your machine will need to be managed with [Microsoft Intune](https://www.microsoft.com/en-us/security/business/Microsoft-Intune).

I followed Microsoft's documentation to create a dev box using Microsoft Dev Box. I found the documentation to be straightforward to follow; there are also several tutorial videos online from developers, if you prefer to see someone walk through the steps live. You'll need to complete two steps of documentation to get your dev box up and running: [Setting Up Your Dev Box Service](https://learn.microsoft.com/en-us/azure/dev-box/quickstart-configure-dev-box-service) and [Creating a Dev Box](https://learn.microsoft.com/en-us/azure/dev-box/quickstart-create-dev-box). These steps should take about an hour to complete.

Once your dev box is running, you will be able to use the virtual machine as if you are on a Windows computer.

From here, you can follow the ["System Requirements"](https://microsoft.github.io/react-native-windows/docs/rnw-dependencies) and ["Getting Started"](https://microsoft.github.io/react-native-windows/docs/getting-started) documentation for React Native for Windows. Dev box will not have access to your local files, so you will need to redownload any source code that you need to get started developing. When you are finished with your development session and have shut down the virtual machine in the Dev Box portal, your machine's state will be saved for next time.

![](assets/2024-07-18-running-rnw-from-a-mac/Screenshot-MicrosoftDevBox.jpg)

## Takeaways about Microsoft Dev Box

Microsoft Dev Box is a great tool when your team is ready for a larger level of investment in the Windows platform. Its feature offerings of cloud-based infrastructure, pre-configured dev boxes, and Intune secured workstations are great for enterprise-scale development.

Microsoft Dev Box does require several admin and configuration steps to get started, so if a developer is looking to quickly experiment with RNW within a VM, Parallels or another virtual machine offering may be a good option to get started with. Then, once the developer is ready to scale their Windows VM usage across many developers to build large production experiences, Microsoft Dev Box will become a better option.

## Overall Takeaways

In this post, I looked at two options for building React Native for Windows apps from Mac devices using Windows VMs. As I close out my content for today, I want to emphasize a few key points. Below is a table which captures the scenario comparisons between Microsoft Dev Box and Parallels.

| Feature                                          | Microsoft Dev Box | Parallels |
| ------------------------------------------------ | ----------------- | --------- |
| Locally installed virtual machine software       |                   | YES       |
| Cloud-based virtual machine                      | YES               |           |
| Easy to follow documentation                     | YES               | YES       |
| Great for quick experimentation with Windows     |                   | YES       |
| Great for enterprise development                 | YES               |           |
| Scale Windows VM usage across several developers | YES               |           |
| Pre-configured dev boxes                         | YES               |           |

As you reach the end of reading, I encourage you to give React Native for Windows and Windows VMs a try! To get started with React Native for Windows visit [aka.ms/reactnative](https://microsoft.github.io/react-native-windows/). If you have any feedback or questions for the React Native for Windows team, you can reach out to us on GitHub at [microsoft/react-native-windows](https://github.com/microsoft/react-native-windows). We want to hear about your developer experience!

---

You can also follow us on Twitter [@ReactNativeMSFT](https://twitter.com/reactnativemsft) to keep up to date on news, feature roadmaps, and more.
