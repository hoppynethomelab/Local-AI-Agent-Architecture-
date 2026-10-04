# Development Workflow

## Branches and working directories

- `main` is the stable branch. The production working directory at
  `/opt/local_ai_agent_prod` checks out this branch after a successful deployment.
- `dev` is the shared development branch based on `main`. The development
  working directory at `/opt/local_ai_agent_dev` should check out this branch.
- Create short-lived feature branches from `dev`, then open pull requests into
  `dev`. Merge tested feature work into `dev`; promote `dev` to `main` through
  a pull request when it is ready for release.
- Treat “master” as an informal reference to `main`; do not create a separate
  `master` branch.

## Continuous integration and merge requirements

1. Create a feature branch from `dev`, make changes, and run the relevant
   tests locally.
2. Push the feature branch and open a pull request into `dev`. Review and merge
   only after its CI checks pass.
3. When the development branch is release-ready, open a pull request from
   `dev` into `main`.
4. Require the `Repository checks` status from `.github/workflows/ci.yml` to
   pass before merging. Do not push directly to `main`.
5. Merge the pull request; the production deployment workflow then updates
   `/opt/local_ai_agent_prod` from that exact `main` revision.

The CI workflow currently checks that the project documentation exists and is
non-empty, checks patch whitespace, and compiles/runs pytest when Python source
and tests are present. When application code is introduced, add its required
dependencies, linters, build steps, and test suites to CI before treating the
checks as sufficient for that code.

## Production deployment setup

`.github/workflows/deploy-production.yml` deploys only after a push to `main`.
It installs Cloudflare's `cloudflared` client on the GitHub-hosted runner,
creates an Access-authenticated TCP connection to `git-hub.hoppynet.co.uk`,
and uses SSH to send a Git bundle and fast-forward the server checkout at
`/opt/local_ai_agent_prod`. The workflow refuses to overwrite a non-empty
directory that is not already a Git checkout, and refuses to deploy if the
production checkout is on another branch or cannot fast-forward.

Before the first deployment:

1. Confirm the Cloudflare Tunnel is healthy and its published application route
   sends `git-hub.hoppynet.co.uk` to the production server's SSH service on port
   22. Protect that hostname with a Cloudflare Access Self-hosted application
   and a Service Auth policy restricted to the service token used by GitHub
   Actions.
2. Create a GitHub Actions environment named `production` and configure these
   environment secrets:
   - `PROD_SSH_USER`: SSH account with permission to update the production
     directory.
   - `PROD_SSH_PRIVATE_KEY`: private key for that account.
   - `PROD_SSH_KNOWN_HOSTS`: verified SSH host-key entry for
     `git-hub.hoppynet.co.uk` (the SSH host key, not a Cloudflare Access key).
   - `PROD_ACCESS_CLIENT_ID`: Cloudflare Access service-token client ID.
   - `PROD_ACCESS_CLIENT_SECRET`: Cloudflare Access service-token client
     secret.
3. Ensure the tunnel connector can reach the server's SSH service and the SSH
   account can write to `/opt/local_ai_agent_prod`. Verify the server host-key
   fingerprint through a trusted channel; do not trust an unverified key scan.

Do not commit or paste any private key or service-token secret into the
repository or chat. Rotate the Cloudflare service token if its secret is lost.

## Repository settings to enable

Configure branch rules for `main` on GitHub to require pull requests and the
`Repository checks` status before merging, disallow force pushes and deletion,
and restrict updates to pull requests. Apply the same required status check to
`dev` so feature branches are tested before joining the shared development
branch. Restrict the release pull request's base to `main` and its source to
`dev` as a team convention. The CI workflow runs on pushes to `dev` and `main`
and on pull requests targeting either branch.

GitHub branch protection/rulesets are repository settings, not files in this
checkout, so they must be enabled in the repository's GitHub settings. At this
stage the repository contains documentation rather than application code;
the current CI smoke checks do not substitute for application tests.
