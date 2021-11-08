# GitHub Action for Pull Request Labeling

**Name:** `cloudposse/github-action-pull-request-labeling`

This GitHub Action will label a pull request according to the statuses of its checks.

## Usage instructions

Create a workflow file (e.g. `.github/workflows/github-pr-labeling.yml`) that contains a step that `uses: cloudposse/github-action-pull-request-labeling@v1.0.0`.
Here's an example workflow file:

```yaml
name: status-check-labeling
on:
  status
jobs:
  status-check-labeling:
    name: status-check-labeling
    runs-on: ubuntu-latest
    steps:
      - name: status-check-summaries
        uses: cloudposse/github-action-pull-request-labeling@v1.0.0
        with:
          label: "no-plan-changes"
          name-contains: "spacelift"
          description-contains: "Plan contains no changes"
          github-token: ${{ secrets.GITHUB_TOKEN }}
```

## Inputs

- `label`: Label to apply to the pull request if its checks meet the desired conditions. (If it needs to be added and does not already exist, the label will be created.)
- `name-contains`: All checks whose names contain this string will be have their statuses tested to see if they contain the `description-contains` input.
- `description-contains`: All checks whose names contain the `name-contains` input will have their descriptions checked to see whether they contain this string.
- `github-token`: Secret token assigned by the GitHub Actions runner. Used for several GitHub API calls.


## Labeling Rules

A label named `label` will be applied to a pull request if all of the checks run against that pull request whose names contain the `name-contains` string also have descriptions that contain the `description-contains` string. If any of the checks on this pull request contain `name-contains` in their names but do not have `description-contains` in their descriptions, the label will not be applied. Also, if no checks are found whose names contain `name-contains`, the label will not be applied.

## Use-case

This event facilitates simpler triage of pull requests on repos that have many pull request checks. The motivating application at Cloud Posse is repos that contains multiple Terraform components and are integrated into clickops workflows with Spacelift. In such a repository, if a pull request could be definitely shown ahead of time to have no effect on any deployed infrastructure, that pull request could potentially be fast-tracked.
