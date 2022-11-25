---
post_title: "Announcing the evolution of dep-check: align-deps"
author1: tonguye
author2: lsciandra
post_slug: 2022-11-30-announcing-align-deps
post_date: 2022-11-30 12:00:00
categories: react-native
tags: announcement, launch, developer-tools, react-native, dependencies
summary: Today, we are announcing a huge upgrade to our tool for dependencies alignment - formerly known as dep-check, its 2.0 version brings big changes and a rename; let's all welcome align-deps!
---

Let's get the obvious out of the way first: yes, we moved the rnx-kit blog - welcome to the official React Native's Microsoft DevBlog!

In this new place, you will be able to find all the content Microsoft has been putting out over the years around React Native, to streamline your ability to learn about the impact and work we have been doing. Bookmark this website or subscribe to the [RSS feed](https://devblogs.microsoft.com/react-native/feed/) to keep up-to-date with all the new blogposts and announcements.

In itself, that's already pretty exciting, but today we are even more excited to introduce you the next iteration of our dependency aligner tool: formerly known as dep-check, please welcome `align-deps`!

In case you are in a rush and can't be bothered to read the whole thing, here's a quick **tl;dr**: `align-deps` is the 2.0 iteration of `dep-check`. It allows you to keep your dependencies on the right version based on requirements, by leveraging presets of rules. It also has a CLI, so you can wire it up to your CI and ensure that noone in your repo (or monorepo!) will inadvertently introduce incompatible versions of packages and break the devloop. Migrating is to the new version is easy as pie, and we have big plans for its future!

For more details, read below or go to [the dedicated documentation](https://microsoft.github.io/rnx-kit/docs/guides/dependency-management). Did I mention that it's [open source too](https://github.com/microsoft/rnx-kit/tree/main/packages/align-deps)?

## How do I start using it?

If you are a new user, you can get started by heading over to [the our Basics documentation](https://microsoft.github.io/rnx-kit/docs/dependencies) to set up the configuration; then come back here, and go straight for the conclusions to get an idea of what the future looks like!

If you are already using `dep-check`, we made sure to make the transition to this new stage as smooth as we can: first off, upgrading from dep-check 1.x to align-deps 2.x literally only requires you to change the dependency in your package.json to `@rnx-kit/align-deps` (click here to see the [latest version available](https://github.com/microsoft/rnx-kit/releases)).

That's because `align-deps` will support the old `dep-check` style configuration structure for some time still, to help you migrate at your own pace - but please consider it **deprecated**.

To help you with the migration of the configuration (learn more in the next sections), we have even made a dedicated command that will automagically migrate your existing dep-check v1 config: once you have installed `align-deps` in your dependendencies, run `yarn rnx-align-deps --migrate-config` and the tool will take care of modifying your package.json accordingly to the new V2 configuration.

## Why the rename?

One glaring difference between 1.x and 2.x is indeed the name: from `dep-check` to `align-deps`.

While there are multiple reasons for this, the one that influenced us the most is name-clashing: there is a different tool used in monorepo scenarios called [`depcheck`](https://github.com/depcheck/depcheck) (with an entirely different, yet still great, set of features) and we only realized when, internally, we ended up having both running in the same codebase. As you can imagine, having `depcheck` and `dep-check` in the same repo can lead to confusion.

It wasn't just that though: the new name is actually better reflective of the purpose of the tool - it's not only **checking** what goes on with your dependencies, but actively **aligning** them for you via the CLI.

...and yes, the new name doesn't have [any clashes](https://www.npmjs.com/search?q=align-deps).

## What changes did we make?

Besides the new name, the most obvious change you'll notice when upgrading to `align-deps` is the config schema. We've gotten requests from multiple teams wanting to use the tool in more scenarios outside of React Native. The configuration schema in `dep-check` is centered around React Native. This is evident in the example below:

```json
// 1.0
{
  "rnx-kit": {
    "reactNativeVersion": "0.68 || 0.69 || 0.70",
    "reactNativeDevVersion": "0.68",
    "customProfiles": "/path/to/custom-profiles",
    "capabilities": [...]
  }
}
```

To support more scenarios, we have to generalize this to support arbitrary packages. Starting with `align-deps`, the equivalent configuration will look like below:

```json
// 2.0
{
  "rnx-kit": {
    "alignDeps": {
      "presets": [
        "microsoft/react-native",
        "/path/to/custom-profiles"
      ],
      "requirements": {
        "development": ["react-native@0.68"],
        // └── equivalent to `reactNativeDevVersion`

        "production": ["react-native@0.68 || 0.69 || 0.70"]
        // └── equivalent to `reactNativeVersion`
      },
      "capabilities": [...]
    }
  }
}
```

Let's break down the changes:

- **Config has moved into `rnx-kit.alignDeps`** — this keeps things more consistent with the rest of the [tools](https://microsoft.github.io/rnx-kit/docs/tools/overview) we have built, and avoids potential clashes with other tools we might build in the future.

- **`reactNativeVersion` and `reactNativeDevVersion` have been replaced by `requirements`** — under `requirements`, you can now require arbitrary packages. We've used `react-native` here, but you could require `react@18` or `jest@29`. If you have different requirements for development and production, you can declare them separately as seen in the example above.

- **`customProfiles` is now `presets`** — previously, you could only specify a single preset to augment the default one. With the new config schema, you can now list any number of presets, and even exclude the built-in one! This opens up for completely custom presets, and for built-in community presets (like this preset that [only includes new-arch compatible libraries](https://github.com/microsoft/rnx-kit/pull/1877)). As before, this flag is entirely _optional_. If you omit it, the default built-in preset, `microsoft/react-native`, will be used.

Note that `align-deps` will automatically migrate your existing configs if you add `--migrate-config` when you run it. The old configuration will still work, but it is officially deprecated.

Further, to keep things consistent, we also had to make equivalent changes to a couple of command line flags:

- **`--vigilant` is replaced with `--requirements`** — to support requiring arbitrary packages, you will have to spell out `react-native` (or other package names). You should be able to replace `--vigilant <versions>` with `--requirements react-native@<versions>`.

- **`--custom-profiles` is replaced with `--presets`** — if you were specifying custom profiles on the command line, you should be able to replace `--custom-profiles <path>` with `--presets microsoft/react-native,<path>`.

Other command line flags have not been changed in any significant ways.

We've also improved error messages in a number of places to make it more obvious what went wrong where. These can always be improved so feel free to file issues if things are still unclear.

If you're curious about how the new changes work, have a read through the [RFC](https://github.com/microsoft/rnx-kit/blob/rfcs/text/0001-dep-check-v2.md#summary).

##

- [LORENZO] conclusions
  - align-deps is a great new step in our effort of making RN's DX better for everyone
    - remember, <call to action to upgrade again>
  - we are already seeing great usage of it across Microsoft
  - we are commited in making it a foundatational stone for the community, remember that you can be a part of it in our discussion section (also mention to use the issue section of the repo)
- [LORENZO] happy coding!

---

Remember that you can also follow us on Twitter [@ReactNativeMSFT](https://twitter.com/reactnativemsft) to keep up to date on news, feature roadmaps, and more.
