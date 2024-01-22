# Setup Forklift GitHub Action

GitHub action to configure the [Forklift CLI](https://github.com/PlanktoScope/forklift) in your GitHub Actions workflow.

Forklift is an open source tool for distributing updating apps on embedded Linux.

## Basic Usage

Here we see a simple template that checks out the pallet, installs the specified version of the Forklift CLI, and then verifies the pallet.

```yml
name: Run Forklift checks
on: [push]
jobs:
  forklift-checks:
    runs-on: ubuntu-latest
    steps:
    - name: Check out repository code
      uses: actions/checkout@v4

    - name: Setup Forklift
      uses: PlanktoScope/setup-forklift@v0
      with:
        version: v0.5.0

    - name: Run Forklift checks
      run: |
        forklift dev plt cache-repo
        forklift dev plt check
        forklift dev plt plan
        forklift dev plt plan --parallel
```

## Choose Forklift Version

When Forklift is installed on the GitHub runner, you can select a the specific version of Forklift you wish to run.

```yml
steps:
  - name: Setup Forklift
    uses: PlanktoScope/setup-forklift@v0
    with:
      version: 0.5.0
```

Or, Forklift can be locked to a [SemVer range](https://www.npmjs.com/package/semver#ranges).

```yml
steps:
  - name: Setup Forklift
    uses: PlanktoScope/setup-forklift@v0
    with:
      version: 0.4.x
```

```yml
steps:
  - name: Setup Forklift
    uses: PlanktoScope/setup-forklift@v0
    with:
      version: 0.4
```

```yml
steps:
  - name: Setup Forklift
    uses: PlanktoScope/setup-forklift@v0
    with:
      version: <0.4
```

You can also choose to run your checks against multiple versions of Forklift.

```yml
strategy:
  matrix:
    forklift-version: [0.5.x, 0.4.x]
steps:
  - name: Setup Forklift
    uses: PlanktoScope/setup-forklift@v0
    with:
      version: ${{ matrix.forklift-version }}
```

## Inputs

The action supports the following inputs:

- `version`: Required. [SemVer ranges](https://www.npmjs.com/package/semver#ranges) are supported, so instead of a [full version](https://github.com/PlanktoScope/forklift/releases) string, you can use `0.4`. This enables you to automatically get the latest backward compatible changes in the v0.4 release.

## Outputs

This action does not set any direct outputs.

## Credits

This repository was copied and adapted from the Open Policy Agent project's [setup-opa](https://github.com/open-policy-agent/setup-opa) repository, published under the Apache 2.0 License; as a result, this repository is also released under the Apache 2.0 License. Thanks also to the folks over at [Infracost](https://github.com/infracost/infracost) who created the initial version of that repository.
