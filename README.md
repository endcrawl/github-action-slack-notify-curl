## Github Action Slack Notifications via Curl

Notify Slack when a GitHub Action succeeds, fails, or is cancelled. Lightweight: only requires `curl`. Concise notification text.

![Example Slack Notification on Success](./example-slack-notification-success.png)

## Setup

1. Create a [custom slack app](https://api.slack.com/apps?new_app=1) and enable its webhooks.
2. Add the webhook url as a Github secret. Examples below assume it's named `SLACK_WEBHOOK`.
3. Incorporate the example below into your repo's `.github/workflows/` directory.

## Example Usage

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

      - name: Report Status to Slack
        if: always()
        uses: endcrawl/github-action-slack-notify-curl@master
        with:
          slack_webhook: ${{ secrets.SLACK_WEBHOOK }}
```

