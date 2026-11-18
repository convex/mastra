# Developing guide

## Testing

```sh
npm ci
npm run build
npm run check
npm run test:e2e
```

## Releases

Mastra is currently on the `0.0.1-alpha` line. Prepare prerelease version bumps
deliberately in a PR; the automated Changesets version PR is deferred until the
first stable-version plan is settled. For another alpha, update `package.json`
and `package-lock.json` to the next prerelease version, add its release notes to
`CHANGELOG.md`, and merge after CI passes. For example:

```sh
npm version prerelease --preid alpha --no-git-tag-version
```

When that bump reaches `main`, CI tags the commit that set the new version as
`v<version>` and starts the [Release workflow](./.github/workflows/release.yml)
on that tag. It builds and checks that commit, then stages it under the `alpha`
npm tag. A maintainer reviews and approves the staged package with 2FA. To
release a different commit, tag it yourself:

```sh
gh api repos/convex/mastra/git/refs -f ref=refs/tags/v0.0.1-alpha.9 -f sha=<commit>
```

Do not merge a stable version bump until the intended first stable version and
changelog are reviewed.

If the workflow fails, check npm's staged packages first. If the version isn't
staged, rerun the failed jobs
(`gh run rerun <run-id> -R convex/mastra --failed`), or move the tag to a
corrected commit:

```sh
gh api -X PATCH repos/convex/mastra/git/refs/tags/v0.0.1-alpha.9 -f sha=<commit> -F force=true
```

If the tag exists but Release didn't start, run
`gh workflow run release.yml -R convex/mastra --ref <tag>`. A staged version
rejected with `npm stage reject <stage-id>` can be staged again; a published
version cannot be reused.
