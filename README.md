# Auto Cancel Redundant Workflow

---

**Please note:** This repository is no longer maintained.  
Please use the [concurrency](https://docs.github.com/en/actions/reference/workflow-syntax-for-github-actions#concurrency) provided by the officials instead.

### Example of migration

```yaml
on: push
jobs:
  test:
    steps:
      - uses: technote-space/auto-cancel-redundant-workflow@v1
      - run: echo step1
      - run: echo step2
      # ...
```

↓

```yaml
on: push
concurrency:
  group: ${{ github.workflow }}-${{ github.ref }}
#  cancel-in-progress: true
jobs:
  test:
    steps:
      - run: echo step1
      - run: echo step2
      # ...
```

---

[![CI Status](https://github.com/technote-space/auto-cancel-redundant-workflow/workflows/CI/badge.svg)](https://github.com/technote-space/auto-cancel-redundant-workflow/actions)
[![codecov](https://codecov.io/gh/technote-space/auto-cancel-redundant-workflow/branch/master/graph/badge.svg)](https://codecov.io/gh/technote-space/auto-cancel-redundant-workflow)
[![CodeFactor](https://www.codefactor.io/repository/github/technote-space/auto-cancel-redundant-workflow/badge)](https://www.codefactor.io/repository/github/technote-space/auto-cancel-redundant-workflow)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://github.com/technote-space/auto-cancel-redundant-workflow/blob/master/LICENSE)

*Read this in other languages: [English](README.md), [日本語](README.ja.md).*

GitHub Actions to automatically cancel redundant workflow.

## Table of Contents

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
<details>
<summary>Details</summary>

- [Usage](#usage)
- [Screenshots](#screenshots)
  - [Cancelling jobs](#cancelling-jobs)
  - [Cancelled workflow](#cancelled-workflow)
  - [Result](#result)
- [Outputs](#outputs)
- [Author](#author)

</details>
<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## Usage
```yaml
on:
  repository_dispatch:
    types: [test]
  # push:
  # ...

name: Example
jobs:
  firstJob:
    name: First Job
    runs-on: ubuntu-latest
    steps:
        # Use this GitHub Actions at least once in this workflow as soon as possible.
      - uses: technote-space/auto-cancel-redundant-workflow@v1
        # Run any steps
      - name: any steps
        run: echo test
      # ...

  # Run any jobs
  secondJob:
    name: Second Job
    needs: firstJob
    runs-on: ubuntu-latest
    steps:
      - run: echo test

  # ...
```

e.g. 
- [ci.yml](https://github.com/technote-space/toc-generator/blob/master/.github/workflows/ci.yml)
- [update-dependencies.yml](https://github.com/technote-space/toc-generator/blob/master/.github/workflows/update-dependencies.yml)

## Screenshots
### Cancelling jobs
![cancelling](https://raw.githubusercontent.com/technote-space/auto-cancel-redundant-workflow/images/cancelling.png)

### Cancelled workflow
![cancelled](https://raw.githubusercontent.com/technote-space/auto-cancel-redundant-workflow/images/cancelled.png)

### Result
![result](https://raw.githubusercontent.com/technote-space/auto-cancel-redundant-workflow/images/result.png)

## Outputs
| name | description | e.g. |
|:---:|:---|:---:|
|ids|Run IDs of the cancelled workflow.|`1234,2345`|

## Author
[GitHub (Technote)](https://github.com/technote-space)  
[Blog](https://technote.space)
