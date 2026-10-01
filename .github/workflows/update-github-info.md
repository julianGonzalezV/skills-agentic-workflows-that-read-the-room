---
name: update-github-info
description: Draft website updates for Mona's GitHub Info site from official GitHub sources.
model: gpt-4.1
on:
  workflow_dispatch:
  schedule:
    - cron: '17 9 * * *'
safe-outputs:
  create-pull-request:
    title-prefix: "[mona] "
    draft: true
    fallback-as-issue: false
tools:
  edit:
  web-fetch:
network:
  allowed:
    - github.com
    - github.blog
    - awesome-copilot.github.com
    - docs.github.com
---

# Update Mona's GitHub Info website

Read `notes/mona-notes.md` before making changes.

Use these sources:
- `notes/mona-notes.md`
- GitHub Blog: https://github.blog/latest/
- GitHub Changelog: https://github.blog/changelog/
- Awesome Copilot workflows: https://awesome-copilot.github.com/workflows/
- GitHub Docs release notes: https://docs.github.com/en/release-notes

Before editing, use the `web-fetch` tool to fetch each web source listed above.
Do not use shell commands such as `curl` or `wget` to fetch pages. If a fetch
fails, do not retry it with a shell command or use unverified information from
that source; continue only with sources fetched successfully.

Include relevant Awesome Copilot workflows and GitHub Docs release notes when
they fit Mona's editorial angle, and cite each source used in the content.

Update `site/content/github-info.md` with concise,
practical updates for readers and include source context when content comes
from the GitHub Blog, GitHub Changelog, Awesome Copilot workflows, or GitHub Docs
release notes.

For new content, open a draft pull request for Mona
to review with a title that mentions Mona or GitHub Info.
Do not write directly to `main`;
rely on `safe-outputs` with `create-pull-request`.