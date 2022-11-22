# Contensis block push GitLab CI

Include this pipeline in your own `.gitlab-ci.yml` allowing you to push a block to Contensis

## Inputs

Uses the same variables as the equivalent GitHub Action (except variables here use underscores instead of dashes) - docs: https://github.com/contensis/block-push

## Example

Your `.gitlab-ci.yml` file may look something like this

```yml
image: gitlab.zengenti.com:4567/ops/zengenti-dind:latest

include:
  - remote: "https://gitlab.zengenti.com/n.flatley/block-push-ci/raw/main/push-block.yml"

stages:
  - latest-build
  - push-block

variables:
  APP_IMAGE: "$CI_REGISTRY_IMAGE/$CI_COMMIT_REF_NAME/app"

build-latest-code:
  stage: latest-build
  script:
    - echo BUILD SCRIPTS

push-to-contensis:
  stage: push-block
  only:
    - master
  variables:
    block_id: name-of-block
    auto_release: "true"
    alias: example-dev
    project_id: website
    client_id: $CONTENSIS_CLIENT_ID
    shared_secret: $CONTENSIS_SHARED_SECRET
    image_uri: $APP_IMAGE:latest
    tag_repo: "true"
    git_token: $GITLAB_ACCESS_TOKEN
```
