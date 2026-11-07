# Contensis GitLab CI workflows

Include reusable workflows in your own `.gitlab-ci.yml` allowing you to perform CI actions with Contensis

- [Push block](./docs/push-block.README.md)

- [Release block](./docs/release-block.README.md)

## Versions

Include the templates through jsDelivr at the `@1` version range, for example
`https://cdn.jsdelivr.net/gh/contensis/gitlab-ci@1/push-block.yml`.
`@1` resolves to the newest `v1.x.y` tag. Breaking changes ship as `v2.0.0`, which you reach with `@2`.
To pin to one exact version, use `@1.x.y` instead.

Don't include from raw.githubusercontent.com. GitLab fetches includes from a single IP, and GitHub rate-limits anonymous raw downloads.

### Releasing

1. Merge to `main`. In the same PR, point the `cli-base.yml` include in `push-block.yml` and `release-block.yml` at the new version (`@1.x.y`).
2. Tag the merge commit `v1.x.y`. Never move or reuse a tag: jsDelivr caches tagged files permanently.
3. Purge the range so it switches over at once rather than within 12 hours:
   `for f in push-block.yml release-block.yml cli-base.yml; do curl -s https://purge.jsdelivr.net/gh/contensis/gitlab-ci@1/$f; done`

To roll back, release a new patch version that reverts the change.

These templates used to live at `gitlab.zengenti.com/ops/contensis-ci`, which is being retired.
