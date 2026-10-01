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

Fetch the Awesome Copilot workflows page and include relevant workflows as
sources when they fit Mona's editorial angle. Cite the source in the content.
Fetch the GitHub Docs release notes and include relevant updates when they fit
Mona's editorial angle. Cite the source in the content.

Update `site/content/github-info.md` with concise,
practical updates for readers and include source context when content comes
from the GitHub Blog, GitHub Changelog, Awesome Copilot workflows, or GitHub Docs
release notes.

Open a pull request for Mona to review. 
Use a pull request title that mentions Mona or GitHub Info. 
Do not write directly to `main`;
rely on `safe-outputs` with `create-pull-request`.