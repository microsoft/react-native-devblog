---
post_title: "Adding Jest Snapshot Testing to React Native Windows Apps"
author1: chiaramooney
post_slug: 2023-02-10-adding-snapshot-testing-to-rnw-apps
post_date: 2023-02-10 12:00:00
categories: react-native
tags: jest, react-native, testing, react-native-windows, tutorial
summary: I added Jest Snapshot testing to a React Native Windows app with community modules. Here's a guide on how I did it!
---

[React Native Gallery](https://github.com/microsoft/react-native-gallery) is a React Native Windows app which showcases core and community module component samples. One function of the app is to validate whether a release has caused UI changes to our component set. In the past, this process was done manually by upgrading the app and walking through all sample pages to confirm that components were being rendered as expected, following the version upgrade. This process was time consuming and lacked accuracy. To automate more of this process, we've begun to add UI testing to React Native Gallery. In this document, I'll walk through how I added Jest Snapshot testing to React Native Gallery. 

One of the most common testing frameworks to use with React Native apps is [Jest](https://jestjs.io/). Jest is a JavaScript testing framework, which is included by default within React Native apps that were created after v0.38. You can confirm that your app has Jest setup by checking the `package.json` file at the root of your app. It should contain the following code:
```json
{
  "scripts": {
    "test": "jest"
  },
  "jest": {
    "preset": "react-native"
  }
}
```

[Snapshot tests](https://jestjs.io/docs/snapshot-testing) help to capture UI and verify that no unexpected UI changes have been made. A snapshot test case will include rendering a UI component, taking a snapshot, and then comparing the snapshot to a reference file. If the two do not match, the test will fail. This is will occur in one of two cases. Either the UI change is unexpected, or the change is expected, and the snapshot reference needs to be updated.

To begin, I followed Jest's [Getting Started](https://jestjs.io/docs/tutorial-react-native) guide for adding testing to React Native apps. This gave me a good overview of potential Jest configurations/customizations, how to write a simple snapshot test, and information on snapshot reference files.

Writing snapshot tests in Jest is fairly simple. Once you have your `App-test.js` file, you'll want to import the desired component that you want to snapshot and write a test case. Here is an example snippet from React Native Gallery's testing of its Button component page:
```js
import {ButtonExamplePage} from '../src/examples/ButtonExamplePage';

test('Button Example Page', () => {
  const tree = create(<ButtonExamplePage />).toJSON();
  expect(tree).toMatchSnapshot();
});
```

As I began to develop, I realized there was some additional configuration I needed to add in order to get my tests to run correctly. First, I had several modules that needed to be added to the `transformIgnorePatterns` Jest customization. This customization is used to specify which files should be transformed by Babel. By default, Jest only processes the app's and react-native's source code. You'll know if a module needs to be transformed, if the app throws an error saying "Jest encountered an unexpected token". In this case, the module should be listed in the `transformIgnorePatterns` customization within your app's `package.json`. See Jest's [transformIgnorePatterns](https://jestjs.io/docs/tutorial-react-native#transformignorepatterns-customization) documentation for syntax info.

Next, I had several modules which required [mocking](https://jestjs.io/docs/mock-function-api/). Mock functions are often needed for modules which implement API's instead of UI components. Examples within React Native Gallery include the `react-native-device-info` module or the `react-navigation` module. If a function needs to be mocked by Jest, you'll commonly receive an error that the method is undefined, when trying to render a component which makes use of the method. Here's an example error for a `react-native-device-info` method that needs mocking: "`@react-native-community/react-native-device-info`: `NativeModule.RNDeviceInfo` is null". To add mocks to your Jest configuration, you'll want to create a `jest-setup.js` file at the root of your repo. Then you'll want to edit your Jest configuration in your app's `package.json` to identify that you've created a setup file. See the `package.json` source snippet below for syntax. Now you'll want to add contents to your `jest-setup.js` file. Some community modules already have `mock.js` files implemented in their source. If this is the case, you'll just want to reference this file in your setup file.

Here is an example of the code added for the `react-native-permissions` module:
```js
jest.mock('react-native-permissions', () =>
require('react-native-permissions/mock'),
);
```
If the community module does not have a mock.js file, you'll have to mock the undefined function yourself. There are a couple ways to sleuth out how to correctly mock: 
	1. Check the community module source. If they have an example app with Jest testing, what do they do in their `jest-setup.js` file?
	2. You can also check postings from the community on how they mocked particular methods for a given module.
	3. React Native Gallery has a wide set of community modules it has tests for. Check out our [`jest-setup.js`](https://github.com/microsoft/react-native-gallery/blob/main/jest-setup.js) to see how we mocked needed modules. 

Here is React Native Gallery's Jest customization within [`package.json`](https://github.com/microsoft/react-native-gallery/blob/main/package.json):
```json
"jest": {
    "preset": "react-native",
    "modulePathIgnorePatterns": [
      "<rootDir>/lib/"
    ],
    "transformIgnorePatterns": [
      "node_modules/(?!(react-native|react-native-xaml|@react-native|react-native-windows|react-native-config|@react-native-community|react-native-print|react-native-webview|react-native-windows-hello|react-native-permissions|react-native-tts|react-native-gesture-handler)/)"
    ],
    "setupFiles": [
      "<rootDir>/jest-setup.js"
    ]
  },
```

Once you have all your tests running and passing, you might see that the Jest run is still exiting with exit code 1. This occurred in React Native Gallery. After some searching, I discovered it was because some of the components our tests were rendering contained asynchronous calls and updates. This results in Jest throwing errors because it cannot ensure that all tasks such as renders, user events, or data fetching were completed and applied before the snapshot match assertion was made. To resolve this error, you'll need to wrap the render of the component you wish to snapshot in React's `act()` function. Then, following the `act()` call, make your snapshot assertion. 

Here's a code snippet of the test for React Native Gallery's `DateTimePicker` page:
```js
test('TimePicker Example Page', async () => {
  let tree;
  await act(() => {
    tree = create(<TimePickerExamplePage />).toJSON();
  });
  expect(tree).toMatchSnapshot();
});
```

A couple of recommendations:
	1. The code for writing the tests is fairly straightforward; getting Jest configured correctly for your app is where the challenge kicks in. If you're working with an app with a number of community modules, tackle adding tests which use each community module one at a time.
	2. Develop incrementally. Start with a test case which simply renders a `<View/>` control. Make sure your base case runs successfully. Then, test as you go. Jest errors aren't always clear, so the more granular you can make your changes between runs of the tests, the easier you'll be able to diagnose what's wrong.

Additional Troubleshooting:
	1. Attempting to run tests on the `react-native-permissions` module causes Jest to hang. As of now, we haven't solved this issue, so the test is disabled.
	2. Some modules usage of the `UIManager.getViewManagerConfig()` API does not work well with Jest. `UIManager` returns undefined, even though the component is a part of core and thus should have already been transformed by Jest. As of now, we've had to disable tests for `TrackPlayer` and `SketchCanvas`.

---

You can also follow us on Twitter [@ReactNativeMSFT](https://twitter.com/reactnativemsft) to keep up to date on news, feature roadmaps, and more.