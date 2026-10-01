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

For every external source above, use the `web_fetch` tool enabled by
`tools: web-fetch`. Do not use shell commands such as `curl` or `wget` to fetch
web pages. If a fetch fails during a normal run, do not use unverified content;
call `noop` and explain which source could not be fetched. Additionally, create in log a table style 
witn url that failed, detailed reason so that it is easy to traige, title the table as External failed calls

Update `site/content/github-info.md` with concise,
practical updates for readers and include source context when content comes
from the GitHub Blog or GitHub Changelog.

Open a pull request for Mona to review. 
Use a pull request title that mentions Mona or GitHub Info. 
Do not write directly to `main`;
rely on `safe-outputs` with `create-pull-request`.

## Manual PR Smoke Test

When the caller context starts with `TEST_PR:`, leave the website content
unchanged. Create `.github/aw-test-results/update-github-info-<run-id>.md`,
replacing `<run-id>` with the actual Actions run ID. Include the run ID, the
external URLs fetched successfully, and any fetch failures. This disposable
report creates a real change for the configured draft
pull request, even when a web fetch fails. Title the pull request so it is
clearly marked as a workflow smoke test. Do not use `noop` after writing the
report; outside this explicit test mode, only create a pull request for a
meaningful website update.