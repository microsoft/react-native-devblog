---
post_title: "Announcing the evolution of dep-check: align-deps"
author1: tonguye
author1: lsciandra
post_slug: 2022-11-30-announcing-align-deps
post_date: 2022-11-30 12:00:00
categories: react-native
tags: announcement, launch, developer-tools, react-native, dependencies
summary: Today, we are announcing a huge upgrade to our tool for dependencies alignment - formerly known as dep-check, its 2.0 version brings big changes and a rename; let's all welcome align-deps!
---

##

- [LORENZO] small intro / tldr
  - call out the devblog, we're renaming dep-check and making it more better and more generic and more customizable
  - what is dep-check/align-deps all about (key features and/or just a link to the docs with clear explanation)

##

- [LORENZO] how can I use it today?
  - just change your package.json from dep-check 1.x to align-deps 2.x, and everything will just keep working!
  - old config will still work for a while, but it's deprecated
  - great news, we have made a new command to automagically migrate your existing dep-check v1 config! just run <command>

##

- [LORENZO] why did you rename it
  - depcheck so having dep-check and depcheck running in the same monorepo wasn't ideal
  - also, new name is better suited, more direct in explaining what the tool does - we don't only check deps but we literally also align them for you, and keep them aligned in your CI
  - (ofc, new name doesn't have any name clashes https://www.npmjs.com/search?q=align-deps)

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
