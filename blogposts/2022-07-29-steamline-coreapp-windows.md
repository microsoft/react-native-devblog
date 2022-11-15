---
post_title: "Streamlining app creation with React Native for Windows CoreApp"
author1: asklar
post_slug: steamline-coreapp-windows
post_date: 2022-07-29 12:00:00
# categories: existingcategory1, existingcategory2
tags: windows, uwp, react-native, react-native-windows
# featured_image: https://dev.azure.com/devdiv/DevDiv/_wiki/wikis/DevDiv.wiki/10339/Drafting-in-GitHub?anchor=images
summary: We are introducing an API called `CoreApp` to help improve the developer experience for Windows developers.
---

React Native builds on top of the native platform for every OS it runs on; on Windows, this translates to producing a Universal Windows Platform (UWP) app.

We heard from some of our customers that build times for these types of apps can be pretty long - especially so when building the React Native for Windows framework from source. To mitigate this problem we offer pre-built binary distributions of the framework in the form of NuGet packages. This works pretty well but has a few shortcomings: even when not building the framework itself, building a UWP app requires several build steps behind the scenes (the steps below are what happens for C++/WinRT apps):

- the MIDL compiler is run to turn IDL files into Windows Metadata - this is required to produce information used by the XAML compiler
- the XAML compiler is invoked to convert XAML markup files into C++ code
- the MDMerge tool runs to merge all Windows Metadata files from the application
- C++/WinRT is invoked to turn Windows Metadata into C++ projection headers as well as component sources
- Only then, the C++ compiler and linker run on the app

We set out to address this since a lot of applications won't need to customize their native code (e.g. by having XAML UI that isn't rendered by RNW); these _greenfield_ applications consist of the bare-bones native UWP app, and all of their app logic is confined to their JavaScript (and whatever native modules they require).

## Enter `CoreApp`

To solve this issue, we came up with a very lean, yet flexible, API that enables a UWP Core Application (the kind that doesn't require XAML, or any interface definition) to host these _greenfield_ apps. We call this API `CoreApp`.
When using the `CoreApp` API, the React Native for Windows framework provides the required `XamlApplication` object as well as a simple XAML `Page` object, which contains a React root view.

This is a new, _experimental_ C API that comes in two flavors:

### `RNCoreAppStart`

```cpp
void RNCoreAppStart(RNCoreAppCallback launched, void *data)
```

This API starts a `CoreApp`, and upon launch, it will call your app back and pass some optional custom data to it as a parameter. The callback enables you to customize a number of parameters, like your bundle name, app name, etc. See CoreApp Schema below for more info.

### `RNCoreAppStartFromConfigJson`

```cpp
void RNCoreAppStartFromConfigJson(wchar_t const *configJson,
      RNCoreAppCallback launched,
      void *data)
```

This API is similar to the previous one but it takes a path to a JSON configuration file. The callback is optional in this case, since more often than not, the configuration file will have all the necessary information.

The `launched` callback above gives you an output `RNCoreApp` structure, where you can set a number of parameters, as well as the `data` parameter that you passed in when you called the `CoreApp` API.

The contents of the `RNCoreApp` structure can be fully set via the JSON configuration file, so next we'll take a look at what is in this JSON configuration file.

## `CoreApp` schema

Below are the properties you can set in your app.config.json, with their default values.
These properties correspond to properties on the ReactInstanceSettings type.

```json
{
  "jsBundleFile": "index.windows",
  "bundleRootPath": "ms-appx:///Bundle/",
  "componentName": null, // Required, this is your App's component name
  "useWebDebugger": true,
  "useFastRefresh": true,
  "useDeveloperSupport": true,
  "useDirectDebugger": false,
  "requestInlineSourceMap": true,
  "enableDefaultCrashHandler": false,
  "debuggerPort": 9229,
  "sourceBundlePort": 8081,
  "sourceBundleHost": "localhost",
  "jsEngine": "chakra", // possible values: "chakra", "hermes"
  "viewName": null, // adds an optional title to the window
  "nativeModules": [
    {
      // this corresponds to the DLL that hosts the native module,
      // or the name of the app's exe (or null) if the module is locally defined
      "moduleContainer": null,

      // the name of the factory function to call to produce a ReactPackageProvider for
      // the module; see Using native modules below.
      "factory": null
    }
  ],
  "properties": {
    // these are all optional. They correspond to properties that will get set in the
    // ReactPropertyBag for the instance. Some examples:
    "someString": "string value",
    "someNumber": 42.5,
    "someBoolean": true,
    "namespace1.namespace2.foo": 22
  }
}
```

For building in Debug mode, your app will usually only need to set a few properties: `componentName`, and the native modules it uses.

## Using native modules

There are a couple of ways that a `CoreApp` can load native modules.

The simplest way is using the `RNCoreAppStartFromConfigJson` API. As we saw above, this API allows us to pass an optional DLL name to load and a plain C function to call in that DLL, to produce the `ReactPackageProvider` for the module.
Here's what this function would look like:

```cpp
extern "C" __declspec(dllexport) void *__cdecl MySpecialPackageProvider() {
  auto provider = winrt::make<MyModulePackageProvider>();
  void *abi{nullptr};
  winrt::copy_to_abi(provider, abi);
  return abi;
}
```

This snippet will declare a plain C function (this is important so that C++ name mangling mechanism doesn't come into play, which is required to be able to use the function name in the JSON file), and export it from the DLL (or the application's EXE). It takes no parameters, and simply return a pointer to the package provider for the module.

If using `RNCoreAppStart`, you can create each the `ReactPackageProvider` for each native module you use, and pass it in the `packageProvidersAbi` and `packageProvidersAbiCount` members of the `RNCoreApp` structure:

```cpp
app->packageProvidersAbiCount = 1;
app->packageProvidersAbi =
    reinterpret_cast<void **>(CoTaskMemAlloc(sizeof(void *) * app->packageProvidersAbiCount));
app->packageProvidersAbi[0] = MySpecialPackageProvider();
```

where `MySpecialPackageProvider` is the same as above, only that the `extern "C" __declspec(dllexport)` part is not required since we don't care about the name, or exporting the function.

## Get Started

For a sample, take a look at the Calculator CoreApp NuGet project in [/samples/CalculatorCoreAppNuGet](https://github.com/microsoft/react-native-windows-samples/tree/main/samples/CalculatorCoreAppNuGet).

## Wrapping up

With the `RNCoreApp` approach - combined with NuGet distribution - we are seeing extremely fast native builds; builds that used to take 15-30 minutes now are down to 10-15 seconds. Because your application actually has very little native code to build, and all of the native logic is encapsulated for you in the React Native for Windows framework, you can get back to being productive in JavaScript, avoid hard-to-diagnose native code errors, and ship your app more quickly!

Compare the calculator sample app (C++/WinRT, not using NuGet, not using CoreApp), takes almost 15 minutes to build:
![Calculator build time from the pipeline](assets/2022-07-29-coreapp/Calculator.png)

The same app built using NuGet (but not CoreApp), slashes this to 3.5 minutes:
![Calculator NuGet build time from the pipeline](assets/2022-07-29-coreapp/CalculatorNuGet.png)

Meanwhile the CoreApp calculator sample clocks in at a speedy 1:49!
![Calculator CoreApp NuGet build time from the pipeline](assets/2022-07-29-coreapp/CalculatorCoreAppNuGet.png)

Look for this new API to make its debut in our 0.70 release, or try it today in our canary builds.

If you have any feedback, please don't hesitate to start a conversation on the [GitHub repo](https://github.com/microsoft/react-native-windows)!

If you’re interested in getting started with React Native for Windows, check out our website at [aka.ms/reactnative](https://aka.ms/reactnative).

You can also follow us on Twitter [@ReactNativeMSFT](https://twitter.com/reactnativemsft) to keep up to date on news, feature roadmaps, and more.
