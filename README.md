# python-action
[![Test](https://github.com/dciborow/pyaction/workflows/Test/badge.svg)](https://github.com/dciborow/pyaction/actions?query=workflow%3ATest)
[![reviewdog](https://github.com/dciborow/pyaction/workflows/reviewdog/badge.svg)](https://github.com/dciborow/pyaction/actions?query=workflow%3Areviewdog)
[![depup](https://github.com/dciborow/pyaction/workflows/depup/badge.svg)](https://github.com/dciborow/pyaction/actions?query=workflow%3Adepup)
[![release](https://github.com/dciborow/pyaction/workflows/release/badge.svg)](https://github.com/dciborow/pyaction/actions?query=workflow%3Arelease)
[![GitHub release (latest SemVer)](https://img.shields.io/github/v/release/dciborow/pyaction?logo=github&sort=semver)](https://github.com/dciborow/pyaction/releases)
[![action-bumpr supported](https://img.shields.io/badge/bumpr-supported-ff69b4?logo=github&link=https://github.com/haya14busa/action-bumpr)](https://github.com/haya14busa/action-bumpr)

This repo contains a action to run various Python tools including:
- [bandit](https://pypi.org/project/bandit)
- [black](https://pypi.org/project/black)
- [flake8](https://pypi.org/project/flake8)
- [pylint](https://pypi.org/project/pylint)
- [pyright](https://pypi.org/project/pyright)
- [pytest](https://pypi.org/project/pytest)

## Input

```yaml
inputs:
  black:
    description: |
      Run Black
      Default is false.
    default: false
  bandit:
    description: |
      Run Bandit
      Default is false.
    default: false
  pylint:
    description: |
      Run Pylint
      Default is false.
    default: false
  pyright:
    description: |
      Run Pyright
      Default is false.
    default: false
  flake8:
    description: |
      Run Flake8
      Default is false.
    default: false
  testing:
    description: |
      Run tests with PyTest
      Default is false.
    default: false
  publish:
    description: |
      Publish to PyPi
      Default is false
    default: false
  publish_url:
    description: |
      PyPi Target. Use this to point to private or test locations.      
      Default https://pypi.org
    defualt: 'https://pypi.org'
  github_token:
    description: 'GITHUB_TOKEN'
    default: '${{ github.token }}'
  workdir:
    description: 'Working directory relative to the root directory.'
    default: 'src'
  ### Flags for reviewdog ###
  level:
    description: 'Report level for reviewdog [info,warning,error]'
    default: 'error'
  reporter:
    description: 'Reporter of reviewdog command [github-pr-check,github-pr-review].'
    default: 'github-pr-check'
  filter_mode:
    description: |
      Filtering mode for the reviewdog command [added,diff_context,file,nofilter].
      Default is added.
    default: 'added'
  fail_on_error:
    description: |
      Exit code for reviewdog when errors are found [true,false]
      Default is `false`.
    default: 'false'
  reviewdog_flags:
    description: 'Additional reviewdog flags'
    default: ''
  toml:
    description: |
      pyproject.toml location.
      Default pyproject.toml
    default: 'pyproject.toml'
  pylint_rc:
    description: '.pylintrc configuration file'
    default: '.pylintrc'
```

## Usage

```yaml
name: Pull Request
on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]
  workflow_dispatch:

jobs:
  linting:
    runs-on: ubuntu-latest
    steps:
      - name: Black
        uses: dciborow/pyaction@0.0.13
        with:
          black: true

      - name: Bandit
        uses: dciborow/pyaction@0.0.13
        with:          
          bandit: true

      - name: Pylint
        uses: dciborow/pyaction@0.0.13
        with:
          pylint: true
          
      - name: Pyright
        uses: dciborow/pyaction@0.0.13
        with:          
          pyright: true
          
      - name: Flake8
        uses: dciborow/pyaction@0.0.13
        with:          
          flake8: true

  testing:
    runs-on: ubuntu-latest
    steps:    
      - name: Pytest
        uses: dciborow/pyaction@0.0.13
        with:          
          testing: true
```

## Development

### Release

#### [haya14busa/action-bumpr](https://github.com/haya14busa/action-bumpr)
You can bump version on merging Pull Requests with specific labels (bump:major,bump:minor,bump:patch).
Pushing tag manually by yourself also work.

#### [haya14busa/action-update-semver](https://github.com/haya14busa/action-update-semver)

This action updates major/minor release tags on a tag push. e.g. Update v1 and v1.2 tag when released v1.2.3.
ref: https://help.github.com/en/articles/about-actions#versioning-your-action
