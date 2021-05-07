## Setup

1. Create a [custom slack app](https://api.slack.com/apps?new_app=1) and enable its webhooks
2. Add the webhook url as a Github secret. Examples below assume it's named `SLACK_WEBHOOK`.
3. Incorporate the examples below into your repo's `.github/workflows/` directory.

## Pull Request Example

```yaml
name: CI

on:
  pull_request:

permissions:
  actions: read

jobs:
  pr:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Run Tests
        run: make check
      - uses: endcrawl/github-action-slack-notify-curl@v0.1
        if: always()
        env:
          SLACK_WEBHOOK: ${{ secrets.SLACK_WEBHOOK }}
          STATUS: ${{ job.status }}
          PR_NUMBER: ${{ github.event.number }}
          COMMIT_SHA: ${{ github.event.after }}
```

## Merge Example

```yaml
name: CI

on:
  push:
    branches:
      - master

permissions:
  actions: read

jobs:
  merge:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Run Tests
        run: make check
      - uses: endcrawl/github-action-slack-notify-curl@v0.1
        if: always()
        env:
          SLACK_WEBHOOK: ${{ secrets.CHECKS_SLACK_WEBHOOK_URL }}
          STATUS: ${{ job.status }}
```
