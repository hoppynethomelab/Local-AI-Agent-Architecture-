# Development Workflow

## Branches and working directories

- `main` is the stable branch. The production working directory at
  `/opt/local_ai_agent_prod` should check out this branch.
- `dev` is the development branch based on `main`. The development working
  directory at `/opt/local_ai_agent_dev` should check out this branch.
- Treat “master” as an informal reference to `main`; do not create a separate
  `master` branch.

## Change and release flow

1. Make and test changes on `dev`.
2. Push `dev` to the configured Git remote.
3. Open a pull request from `dev` into `main`.
4. Require the CI checks to pass before merging.
5. After the merge, update the production working directory from `main`.

The CI workflow currently runs the repository's documentation smoke tests on
pushes to `dev` and `main`, and on pull requests targeting `main`. As application
code and tests are added, extend the checks to run the relevant linters, builds,
and test suites.

No Git remote is configured yet, so pushing, pull requests, and hosted CI will
become available after a remote is added. Branch protection and production
deployment also need to be configured at the hosting provider.
