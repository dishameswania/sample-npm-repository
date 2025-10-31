# Sample-npm-repository

Sample containing NPM application.

## Publishing to Fly via GitHub Actions

This repo is configured to publish to the Fly npm registry when a version tag is pushed (e.g., `v1.0.0`). Authentication is automatic via GitHub OIDC using JFrog CLI—no secrets required.

1) In GitHub, add repository variables (Settings → Secrets and variables → Actions → Variables):
   - `FLY_URL`: your Fly instance URL (e.g., `https://<fly-host>/`)
   - `FLY_NPM_REPO`: the target npm repository name in Fly (e.g., `npm-virtual`)

2) Trigger a release:

```bash
git tag v1.0.0
git push origin v1.0.0
```

The workflow at `.github/workflows/release.yml` will install, test, build, and publish using OIDC and these variables.

### Docker image publishing to Fly (optional)

On tag pushes (`v*`), if a `Dockerfile` exists, `.github/workflows/docker-release.yml` will build and push a Docker image to Fly using OIDC via JFrog CLI.

Set repository variables:
 - `FLY_URL`: your Fly base URL (e.g., `https://<fly-host>/`)
 - `FLY_DOCKER_REPO`: target Docker repo in Fly (e.g., `docker-virtual`)
 - `IMAGE_NAME` (optional): override image name; defaults to `<owner>/<repo>`