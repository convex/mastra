# Developing guide

## Testing

```sh
npm ci
npm run build
npm run check
npm run test:e2e
```

## Releases

Mastra is currently on the `0.0.1-alpha` line. Prepare prerelease version
bumps deliberately in a PR; the automated Changesets version PR is deferred
until the first stable-version plan is settled. For another alpha, update
`package.json` and `package-lock.json` to the next prerelease version, add its
release notes to `CHANGELOG.md`, and merge after CI passes. For example:

```sh
npm version prerelease --preid alpha --no-git-tag-version
```

The release workflow stages the package from the version-bump commit on `main`
under the `alpha` npm tag and then creates its source tag. A maintainer reviews
and approves the staged package with 2FA. Configure its npm trusted publisher
before the first CI release:

```sh
npm trust github @convex-dev/mastra --repo convex/mastra --file release.yml --allow-stage-publish
```

Do not merge a stable version bump until the intended first stable version and
changelog are reviewed. For recovery, dispatch **Release** with the exact
version-bump commit SHA only after checking whether it is already staged.
