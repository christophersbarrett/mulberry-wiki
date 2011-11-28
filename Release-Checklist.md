## Preparing the Release

- Run the tests and ensure they all pass: `rake`
- Ensure you can test the kitchensink demo: `mulberry test demos/kitchensink/`
- Review the diff since the last commit: `git diff <last_tag_name>` (you'll need to `git pull --tags` to get the remote tags)
- Write release notes and add a link to them to the repo wiki's home page

## Creating the Release
- Bump the version in `mulberry/mulberry.rb` and commit
- Tag the release by running `git tag -a`; add a link to the release notes in the commit message
- Push the tag: `git push --tags`
- Edit the downloads page on the Mulberry site to point to the new release (currently at `source/download/index.markdown` -- just edit the `download_url` property); regenerate and deploy the site (see the site repo's README for details).

## Announcing the Release
- Post a message to the [Google Group](https://groups.google.com/forum/#!forum/toura-mulberry) with a link to the release notes, and provide any other pertinent information.
- Announce the release on @touradev.
- Update the topic in the IRC channel: `/topic Toura Mulberry - http://mulberry.toura.com - Latest Release: 0.X.X`
- For major releases, write a blog post.