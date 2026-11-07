# Contensis block push GitLab CI

Include this pipeline in your own `.gitlab-ci.yml` allowing you to push a block to Contensis

## Inputs

Uses the same variables as the equivalent GitHub Action (except variables here use underscores instead of dashes) - docs: https://github.com/contensis/block-push

## Example

Your `.gitlab-ci.yml` file may look something like this

```yml
image: gitlab.zengenti.com:4567/ops/zengenti-dind:latest

# Include the push-block pipeline
include:
  - remote: "https://cdn.jsdelivr.net/gh/contensis/gitlab-ci@1/push-block.yml"

# Add a stage called push-block to run after your project build
stages:
  - latest-build
  - push-block

variables:
  APP_IMAGE: "$CI_REGISTRY_IMAGE/$CI_COMMIT_REF_NAME/app"

# Your project build stage job(s)
build-latest-code:
  stage: latest-build
  script:
    - echo RUN BUILD SCRIPTS

# Add a job to the push-block stage setting the required variables
push-to-contensis:
  stage: push-block
  only:
    - master
  variables:
    alias: example
    project_id: website
    block_id: name-of-block
    image_uri: $APP_IMAGE:build-$CI_PIPELINE_IID
    release: "true"
    client_id: $CONTENSIS_CLIENT_ID # Create a CI/CD Variable called CONTENSIS_CLIENT_ID and paste your id
    shared_secret: $CONTENSIS_SHARED_SECRET # Create a CI/CD Variable called CONTENSIS_SHARED_SECRET and paste your secret
    tag_repo: "true" # git_token is required if this is 'true'
    # First create a Project Access Token (access: api, read_repository) and copy it
    # Then create a CI/CD Variable called GITLAB_ACCESS_TOKEN and paste the token
    git_token: $GITLAB_ACCESS_TOKEN
```

## Pushing to multiple environments

Ensure the `push-to-contensis` job is in place as above

Add all of the subsequent "push" jobs you require following this example

```yml
push-to-contensis-dev:
  extends: push-to-contensis
  stage: push-block
  variables:
    alias: example-dev
    project_id: website
    block_id: name-of-block
    image_uri: $APP_IMAGE:build-$CI_PIPELINE_IID
    release: "true"
```
