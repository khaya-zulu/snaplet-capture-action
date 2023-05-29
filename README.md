# Snaplet capture action.

> Capture a snapshot of your database in your own github actions, using Snaplet.

Snaplet makes it easy and safe to create snapshots of your database. By providing a typescript configuration for transforming, reducing (subsetting) and excluding tables.

This action allows you to run `snaplet capture` in your CI environment and optionally share those snapshots in Snaplet Cloud.
Self-hosting gives you the flexibility to run snaplet commands on your own infrastracture without the need to share any of your credentials.

## Usage

Create a GitHub Action Workflow file in your repository following one of these examples.

### Standalone

```yaml
# .github/workflows/preview.yml

name: Snaplet Capture

env:
  SNAPLET_TARGET_DATABASE_URL: ${{ secrets.SNAPLET_TARGET_DATABASE_URL }}

on:
  workflow_dispatch:
  schedule:
    - cron: "0 4 * * 1-5" # At 04:00 on every day-of-week from Monday through Friday.

jobs:
  capture:
    runs-on: ubuntu-latest
    steps:
      - name: Check out
        uses: actions/checkout@v3
      - uses: snaplet/capture-action@v3
        with: |
          destination-path: /tmp/my-snapshot
          skip-share: true # opt out of sharing snapshot.
```

## Documentation

### Environment variables

- SNAPLET_TARGET_DATABASE_URL **[required]**
- SNAPLET_ACCESS_TOKEN

### Inputs

```yaml
destination-path:
  description: The path to where the snapshots are stored
  required: true
  type: string
tags:
  description: Attach tags to the snapshot (seperate by comma)
  required: false
  type: string
skip-share:
  description: Skip the share step
  required: false
  type: boolean
  default: true
```
