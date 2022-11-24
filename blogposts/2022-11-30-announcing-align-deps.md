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

##

- [TOMMY] ...ok, but what did actually change
  - why we did do this (to make it more generic, we want this tool to be able to be used even in non RN scenarios)
  - we moved the config into the more generic `rnx-kit.align-deps` section
    - why we did this (more consistent with the other rnx-kit tools, check them out here wink)
    - (put a code snippet before/after as in RFC)
  - (as you noticed) we changed the config schema:
    - we changed some config keys (replaced `reactNativeVersion` and `reactNativeDevVersion` with `requirements`)
  - custom profiles are now called presets and they are more powerful
    - and we also exposed our default preset as `"microsoft/react-native"` so that you can have more control over it
      - we want to make it easier for the community to come up with their own presets, ex. the preset for the new arch libraries (https://github.com/microsoft/rnx-kit/pull/1877)
  - same changes in CLI commands, the rest stays the same
  - we're also improving error messaging to make it easier to use

##

- [LORENZO] conclusions
  - align-deps is a great new step in our effort of making RN's DX better for everyone
    - remember, <call to action to upgrade again>
  - we are already seeing great usage of it across Microsoft
  - we are commited in making it a foundatational stone for the community, remember that you can be a part of it in our discussion section (also mention to use the issue section of the repo)
- [LORENZO] happy coding!

---

Remember that you can also follow us on Twitter [@ReactNativeMSFT](https://twitter.com/reactnativemsft) to keep up to date on news, feature roadmaps, and more.
