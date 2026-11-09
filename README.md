# h-slang-docs

GitHub Pages hosting for the experimental h-slang research project. h-slang is
not part of the official Slang specification.

- **Website:** <https://saipraveenb25.github.io/h-slang-docs/>
- **Documentation sources:** [shader-slang/slang, branch h-slang](https://github.com/shader-slang/slang/tree/h-slang/docs/heterogeneity)
- **Deployment workflow:** [Deploy h-slang docs](https://github.com/saipraveenb25/h-slang-docs/actions/workflows/deploy.yml)

## Publish an update

Open the deployment workflow, select **Run workflow**, leave the workflow branch
as `main`, and use `h-slang` as `source_ref`. To publish a specific revision or
roll back, enter a full commit SHA from `shader-slang/slang` instead.

Changes pushed to this repository's `main` branch also deploy the latest
`h-slang` source. Changes to `shader-slang/slang` do not yet trigger deployments
automatically.

The workflow checks out the public source repository with Git LFS enabled and
builds with the prebuilt `squidfunk/mkdocs-material:9.6.23` image. It runs the
documentation tests and a strict MkDocs build, using the checked-in
`javascript/docs-runtime` snapshot for WASM, precompiled modules, Monaco, and
workers. It does not install dependencies or compile browser packages. The docs
hook checks the runtime manifest and asset hashes before publication. Refresh
browser packages locally in the source repository when needed.

The site uses its GitHub Pages base URL, including `/h-slang-docs/`. Links to
source files point at the exact source commit used for the deployment. The
workflow summary and the site's [`deployment.json`](https://saipraveenb25.github.io/h-slang-docs/deployment.json)
record that revision.

## Repository settings

- **Settings → Pages → Build and deployment → Source:** GitHub Actions.
- **Settings → Environments → github-pages:** allow deployment from `main`.
- GitHub Actions must be enabled. The workflow grants read access to the build
  job and grants Pages write and OIDC permissions only to the deployment job.

Manual deployments need no repository secrets. The source is public, and Pages
deployment uses this repository's `GITHUB_TOKEN`.

## Optional automatic source updates

The workflow also accepts a `repository_dispatch` event named `deploy-docs`,
with an optional `client_payload.source_ref` (defaults to `h-slang`). A workflow
in the source repository can send this event after a source push, using the
source commit SHA as `source_ref`.

That integration is not configured here. It requires a GitHub App token or
fine-grained personal access token scoped to this hosting repository with
**Contents: write** permission, stored as a secret in the source repository.
The source repository's ordinary `GITHUB_TOKEN` cannot dispatch to this separate
repository. See [GitHub's repository dispatch API](https://docs.github.com/en/rest/repos/repos#create-a-repository-dispatch-event).
