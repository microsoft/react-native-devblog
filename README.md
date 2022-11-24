# React Native devblog

This **private** repository serves as the backbone of the official [React Native DevBlog](https://devblogs.microsoft.com/react-native/).

The editorial team is currently composed by [Ryan Haning](https://github.com/rhaning), [Steven Moyes](https://github.com/stmoy), [Lorenzo Sciandra](https://github.com/kelset), [Adam Foxman](https://github.com/afoxman).

## Writing guide

The general flow for contributing content to the devblog is, briefly:

    1. Idea gets pitched
    2. Idea gets approved
    3. Draft gets made
    4. Draft gets approved
    5. Draft gets published

Let's dive a bit more into each step:

### 1. Pitching an idea for content for the devblog

We want to empower everyone at Microsoft to be able to create content for the React Native DevBlog; in order to do so, we have prepared an issue template for [pitching ideas and proposing content](https://github.com/microsoft/react-native-devblog/issues/new?assignees=&labels=pitch&template=pitch-idea.yml&title=%5BPitch%5D%3A+) that the author would like to work on.

First time authors **need** to pitch their idea via this method. Recurring authors can jump directly to step #3 if they feel their proposal is solid, but we'd suggest to still go first via an issue for pitching.

### 2. Idea gets approved

Once the editorial team has been able to review the idea and approve it, the author will be added to the [RNDevBlog Writers](https://github.com/orgs/microsoft/teams/rndevblog-writers) GitHub team. This is needed to get write access to the repo: since it is a private repo we can't allow for forks, authors will be expected to create branches to submit PRs.

The author will also be asked to join the [Wordpress Instance](https://devblogs.microsoft.com/react-native/wp-admin/users.php) and setup their author profile over there; the username in Wordpress is needed for the author metadata field (see in the next section).

### 3. Draft gets made

Once the author has got write access to the repository, they will git clone it and create a branch similar to `<author-alias>/<blogpost-title>`.

In it, they will make a new markdown file named something like `<tentative-date-YY-MM-DD>-<title>.md` and copy over this template:

```md
---
post_title: "post title"
author1: the-author-wordpress-username
post_slug: same-as-filename
post_date: same-as-filename-YY-MM-DD 12:00:00
categories: react-native // don't change the category
tags: // add one or more tags, try reusing existing ones
summary: // one of two phrases that will preview the content
---

// the actual content goes here

---

You can also follow us on Twitter [@ReactNativeMSFT](https://twitter.com/reactnativemsft) to keep up to date on news, feature roadmaps, and more.
```

They can then work on the markdown file, adding their content and modifying the metadata accordingly.

If they need to add assets like images, put them in the `assets` folder; if there's more than one piece of media, please make a dedicated subfolder under `assets/<blogpost-file-title>`.

If needed, a custom featured image can be added under `assets/featured-images/<blogpost-file-title>`. More indications around featured images will be added ASAP.

### 4. Draft gets approved

Once the author is ready to have their content reviewed, they can push up the branch and open a PR. At this point, the editorial team will review it and decide a publishing date with the author (so that the markdown file can have the right timestamp where needed).

Once it's approved, congrats! It's almost ready to be published: the PR will get merged, and with the help of the editorial team the tool syncing the GH->WP content will be triggered (it might require a one-commit-one-file follow up commit in main branch).

### 5. Draft gets published

Once the draft reaches WordPress, it will only put it as a draft; the editorial team will need to go into the WP dashboard, "all posts" sections, select the new post (it will have a "- DRAFT" appended to the title), click on Edit, set the right date, then hit publish.

After publication, the content will be broadcasted accordingly on whichever social medias have been agreeded (if any).

### More information and gotchas

Read more of how to use Markdown to deploy to the WordPress based blog [in this documentation](https://dev.azure.com/devdiv/DevDiv/_wiki/wikis/DevDiv.wiki/10339/Drafting-in-GitHub?anchor=template-to-add-at-the-top-of-a-github-file).

The tool used for this GH -> WP synchronization has a few limitations, so please keep the following in mind:

- any edits to the post in WordPress side will not be synced back to GitHub. They will need to be manually replicated.
- remember: author username need to be the WordPress ones, not the GitHub ID
- on main, you need to do a one-commit-per-one-blog strategy (you can checkout the history for examples): the GH->WP tool doesn't work when you push multiple files in the same commit

For any further questions reach out to the editorial team.

## Code of Conduct

This project has adopted the [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/).
For more information see the [Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/) or
contact [opencode@microsoft.com](mailto:opencode@microsoft.com) with any additional questions or comments.

## Trademarks

This project may contain trademarks or logos for projects, products, or services. Authorized use of Microsoft
trademarks or logos is subject to and must follow
[Microsoft's Trademark & Brand Guidelines](https://www.microsoft.com/en-us/legal/intellectualproperty/trademarks/usage/general).
Use of Microsoft trademarks or logos in modified versions of this project must not cause confusion or imply Microsoft sponsorship.
Any use of third-party trademarks or logos are subject to those third-party's policies.
