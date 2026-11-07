# Contensis block release GitLab CI

Include this pipeline in your own `.gitlab-ci.yml` allowing you to release a block deployed to Contensis.

The pipeline will be triggered only when a tag has been added to your git repo with the prefix `release-*` e.g. `release-v1.0.1`

> **Note**
> It is possible to "auto release" with every block push by adding `release: 'true'` to the variables in your `push-block` jobs

## Inputs

Uses the same variables as the equivalent GitHub Action (except variables here use underscores instead of dashes) - docs: https://github.com/contensis/block-release

## Example

Your `.gitlab-ci.yml` file may look something like this

```yml
image: gitlab.zengenti.com:4567/ops/zengenti-dind:latest

# Include the release-block pipeline
include:
  - "https://raw.githubusercontent.com/contensis/gitlab-ci/v1/push-block.yml"
  - "https://raw.githubusercontent.com/contensis/gitlab-ci/v1/release-block.yml"

# Add a stage called release-block to run after your project build
stages:
  - latest-build
  - push-block
  - release-block

variables:
  APP_IMAGE: "$CI_REGISTRY_IMAGE/$CI_COMMIT_REF_NAME/app"

# Your project build stage job(s)
build-latest-code:
  stage: latest-build
  script:
    - echo RUN BUILD SCRIPTS

# Pushing your build to contensis
push-to-contensis:
  stage: push-block
  only:
    - master
  variables:
    alias: example-dev
    project_id: website
    block_id: name-of-block
    image_uri: $APP_IMAGE:latest

# Releasing the block (with a `release-*` tag)
release-contensis-block:
  stage: release-block
  variables:
    alias: example-dev
    project_id: website
    block_id: name-of-block
```
