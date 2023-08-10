# block-package-dependency
Block a specific package being introduced using dependency review

The example [GitHub Actions workflow](.github/workflows/dependency-review.yml) blocks a package using dependency review

we can deny a specific package [pac-resolver version 4.0.0](https://www.npmjs.com/package/pac-resolver/v/4.0.0) as per [the documentation for the dependency review action](https://github.com/actions/dependency-review-action/blob/main/README.md#:~:text=false-,deny%2Dpackages,-Any%20number%20of)

```yaml
name: 'Dependency Review'
on: [pull_request]
permissions:
  contents: read
jobs:
  dependency-review:
    runs-on: ubuntu-latest
    steps:
      - name: 'Checkout Repository'
        uses: actions/checkout@v3
      - name: Dependency Review
        uses: actions/dependency-review-action@v3
        with:
          fail-on-severity: moderate
          # we can deny a specific package as per https://github.com/actions/dependency-review-action/blob/main/README.md#:~:text=false-,deny%2Dpackages,-Any%20number%20of
          deny-packages: pkg:npm/pac-resolver@4.0.0

          # Use comma-separated names to pass list arguments:
          #deny-licenses: LGPL-2.0, BSD-2-Clause
```

The example PR is blocked with a check as shown below
<img width="950" alt="image" src="https://github.com/gitstua-labs/block-package-dependency/assets/25424433/d09de565-ff03-4529-b4df-10dfe8f71072">

The detailed error in the action
<img width="776" alt="image" src="https://github.com/gitstua-labs/block-package-dependency/assets/25424433/67670d41-d6e2-475b-93dc-c6c14155bb05">

## Making this a required workflow for the whole organization
To make this a required workflow for the whole organization see the [documentation](https://docs.github.com/en/organizations/managing-organization-settings/disabling-or-limiting-github-actions-for-your-organization#configuring-a-required-workflow-for-your-organization)
