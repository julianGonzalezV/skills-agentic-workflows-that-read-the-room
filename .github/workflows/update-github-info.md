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
steps:
  - name: Check external source HTTP status
    shell: bash
    run: |
      urls=(
        https://github.blog/latest/
        https://github.blog/changelog/
        https://awesome-copilot.github.com/workflows/
        https://docs.github.com/en/release-notes
      )
      {
        printf '| URL | HTTP status | curl exit code |\n'
        printf '|---|---:|---:|\n'
        for url in "${urls[@]}"; do
          if status=$(curl -sS -L --fail --retry 5 --retry-all-errors --retry-delay 1 --max-time 20 -o /dev/null -w '%{http_code}' "$url"); then
            curl_exit=0
          else
            curl_exit=$?
          fi
          printf '| %s | %s | %s |\n' "$url" "${status:-000}" "$curl_exit"
        done
      } | tee -a "$GITHUB_STEP_SUMMARY"
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
`tools: web-fetch`. If a `web_fetch` call fails, retry that URL up to 5 times
after the initial attempt (at most 6 attempts total). Do not use shell commands
such as `curl` or `wget` to fetch web-page content. The runner's HTTP-status
check above is diagnostic only; use `web_fetch` for source content. If a fetch
still fails during a normal run after all attempts, do not use unverified
content or change the website. Create
`.github/aw-connection-reports/update-github-info-<run-id>.md`, replacing
`<run-id>` with the actual Actions run ID. Include a table titled
`External failed calls` with each affected URL, attempts made, HTTP status when
available, and detailed failure reason. Never invent a status code; write `not
reported` if the fetch result provides none. Open a draft pull request with
this report so the connection issues can be reviewed; do not call `noop` after
writing it. If there are no connection issues, only open a pull request for a
meaningful website update.

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