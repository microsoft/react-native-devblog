# React Native devblog

This **private** repository serves as the backbone of the official React Native DevBlog (still to be made public).

Authors are expected to submit their drafts here are PRs so that they can be reviewed by the editorial team (Ryan Haning, Steven Moyes, Lorenzo Sciandra, Adam Foxman).

Read more of how to use Markdown to deploy to the WordPress based blog [in this documentation](https://dev.azure.com/devdiv/DevDiv/_wiki/wikis/DevDiv.wiki/10339/Drafting-in-GitHub?anchor=template-to-add-at-the-top-of-a-github-file). A couple of things to keep in mind:

- merge to main will always only create a draft to review in WordPress.
- any edits to the post in DevBlogs will not be synced back to GitHub.
- author username as seen in WordPress, not GitHub ID
- on main, you need to do a one-commit-per-one-blog strategy (you can checkout the history for examples): the GH->WP tool doesn't work when you push multiple files in the same commit
- when publishing something for the first time, the GH->WP tool will only put it as a draft on the WP side; so you need to go into the WP dashboard, all posts sections, select the new post (it will have a "- DRAFT" appended to the title), click on Edit, set the right date, then hit publish.

For any further questions reach out to the editorial team.

## Contributing

This project has adopted the [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/).
For more information see the [Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/) or
contact [opencode@microsoft.com](mailto:opencode@microsoft.com) with any additional questions or comments.

## Trademarks

This project may contain trademarks or logos for projects, products, or services. Authorized use of Microsoft
trademarks or logos is subject to and must follow
[Microsoft's Trademark & Brand Guidelines](https://www.microsoft.com/en-us/legal/intellectualproperty/trademarks/usage/general).
Use of Microsoft trademarks or logos in modified versions of this project must not cause confusion or imply Microsoft sponsorship.
Any use of third-party trademarks or logos are subject to those third-party's policies.
