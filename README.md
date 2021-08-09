# Replace pattern in PR body/title with a text

## Motivation

I needed to generate URLs in our PR body that contained the actual PR number. The rest of the URL was static, co I just needed to put some `substring` in it and then replace it programmatically.

## Usage

```yaml
name: PR body substitution

on:
  pull_request:
    types:
      - opened

jobs:
  fill-pr-body:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v2
      - uses: stefanluptak/replace-in-pr-body@v1
        with:
          pattern: "macOS artifact url: _build in progress..._"
          replacement: "https://your-artifact-url"
```

## Notes

It's needed to have the `actions/checkout@v2` step before this action, otherwise the `hub` command inside the action won't know on what git repository to operate.

If you're using Dependabot, you might want to consider putting `if: startsWith(github.head_ref, 'dependabot') != true` on top of your job definition to prevent this action running on PRs made by Dependabot.