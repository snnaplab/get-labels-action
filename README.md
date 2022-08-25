# Get Labels Action

[![CI](https://github.com/snnaplab/get-labels-action//actions/workflows/ci.yml/badge.svg)](https://github.com/snnaplab/get-labels-action//actions/workflows/ci.yml)

Get pull request or issue labels.

## Why?

Usually the pull request or issue labels are available from the `github` context, like `${{ github.event.pull_request.labels }}`, `${{ github.event.issue.labels }}`.
However, the `github` context is the one when the pull request or issue was created, so it does not include the label given during the CI test after creating the pull request.

This Action gets labels in real time.

## Inputs

| Name           | Description                                                                                                                                   | Default | Required |
|----------------|-----------------------------------------------------------------------------------------------------------------------------------------------| --- |-------|
| `number`       | Pull request or issue number. Specify when changing from the default. | `${{ github.event.pull_request.number }}` or `${{ github.event.issue.number }}` | false |
| `repository`   | Target repository name with owner, eg `snnaplab/get-labels-action`. Specify when accessing labels in a repository different from the workflow. | `${{ github.repository }}` | false |
| `github-token` | Specify a personal access token (PAT) when targeting a repository different from the workflow.                                                | `${{ github.token }}` | false |

## Outputs

| Name | Description                                                                     |
| --- |---------------------------------------------------------------------------------|
| `labels` | Labels for pull requests or issues, in JSON format, eg `["bug","help wanted"]`. |

The result is also stored in the environment variable `LABELS`.

## Example

### Run a step only if it contains a specific label

```yaml
- uses: snnaplab/get-labels-action@v1
- if: contains(fromJSON(env.LABELS), 'bug')
  run: ...
```

### Get label in bash

```yaml
- uses: snnaplab/get-labels-action@v1
- run: |
    for i in $(seq 0 $(($(echo '${{ env.LABELS }}' | jq length) - 1)))
    do
      label=$(echo '${{ env.LABELS }}' | jq -r .[$i])
      ...
    done
```

## License

[MIT](LICENSE)
