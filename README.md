# Check Code Quality GitHub Actions Workflow
by Joseph Leedy

Check Code Quality provides an automated process for verifying that code follows
all required standards and does not introduce any potential bugs.

## Prerequisites

The following tools must installed as development dependencies in your project: 

### PHP
- [PHP Parallel Lint](https://github.com/php-parallel-lint/PHP-Parallel-Lint)
- [PHP_CodeSniffer](https://github.com/squizlabs/PHP_CodeSniffer)
  - [Magento Coding Standard](https://github.com/magento/magento-coding-standard)
- [PHPStan](https://github.com/phpstan/phpstan)

### JavaScript
- [ESLint](https://github.com/eslint/eslint)

## Usage

### Example Workflow
This workflow is intended to be used within another workflow using a
configuration similar to this:

```yaml
name: Check Code Quality

on:
  push:
    branches:
      - 'feature/**'

  workflow_dispatch:

jobs:
  check-code-quality:
    name: Check Code Quality
    uses: JosephLeedy/github-action-check-code-quality/.github/workflows/check-code-quality.yml@main
    with:
      php-version: 8.1
    secrets:
      access-token: ${{ secrets.GITHUB_TOKEN }}
```

### Accepted Inputs

The following parameters can be set by using the `with:` keyword in your calling
workflow:

| Name         | Default Value | Required | Description                                                     |
|--------------|---------------|----------|-----------------------------------------------------------------|
| fetch-depth  | 2             | No       | Number of code revisions to check out ("0" for all<sup>1</sup>) |
| base-branch  | develop       | No       | Base Git branch (i.e. "develop" or "main")                      |
| node-version | 16            | No       | Version of Node.js to use for running JavaScript checks         |
| php-version  | 7.4           | No       | Version of PHP to use for running PHP checks                    |

<small><sup>1</sup><strong>Note</strong>: setting a value of "0" for the
`fetch-depth` parameter is not recommended for large repositories as it could
adversely impact performance.</small>

### Accepted Secrets

The following secrets can be passed by in the `secrets:` section of your calling
workflow:

| Name               | Required | Description                      |
|--------------------|----------|----------------------------------|
| access-token       | Yes      | GitHub access token              |
| composer-auth-json | No       | Composer auth.json file contents |
