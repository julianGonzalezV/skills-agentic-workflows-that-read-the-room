---
name: update-github-info
on:
  schedule: daily
  workflow_dispatch:
permissions:
  contents: read
  pull-requests: read
engine: copilot
tools:
  github:
    toolsets: [repos, pull_requests]
  web-fetch:
  edit:
network:
  allowed:
    - github.blog
    - github.com
safe-outputs:
  create-pull-request:
    draft: true
    title-prefix: "[github-info] "
---

# Update GitHub Info

Keep Mona's GitHub Info website current with concise, practical updates backed by official GitHub sources.

## Research

1. Read `notes/mona-notes.md` before making any changes.
2. Use the GitHub repository API tools to read the repository guidance and reference files you need, including the current `site/content/github-info.md`.
3. Use the `web-fetch` tool to fetch `https://github.blog/latest/`.
4. Use the `web-fetch` tool to fetch `https://github.blog/changelog/`.
5. Prefer recent, useful items that fit Mona's editorial angle. Cite the relevant GitHub Blog or Changelog source in the content.

## Update

Edit only `site/content/github-info.md`. Keep summaries short and practical, preserve the existing structure, avoid duplicating existing entries, and leave unrelated work untouched. If the sources do not support a worthwhile update, do not invent one and do not create a pull request.

After editing, review the diff for accuracy, source links, formatting, and accidental unrelated changes. When there is a meaningful update, use the `create-pull-request` safe output to open a draft pull request for Mona to review. Describe the sources consulted and summarize the changes in the pull request body. Never write directly to the default branch.
