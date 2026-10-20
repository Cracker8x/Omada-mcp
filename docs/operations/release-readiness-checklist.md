# Release Readiness Checklist

- [ ] `npm ci` completes from a clean checkout
- [ ] `npm run check` passes
- [ ] `npm run build` passes
- [ ] `npm run test:coverage` passes
- [ ] new tools have matching tests and README entries
- [ ] docs reflect the current capability model and supported transport
- [ ] no unsupported HTTP or compatibility feature appears in the secure quick start
- [ ] all major feature additions reference official Omada API docs or are explicitly marked compatibility-only

## npm trusted publishing

The `publish-npm` job uses GitHub OIDC with `id-token: write` on a GitHub-hosted
runner. It requires npm 11.5.1 or newer and does not use `NPM_TOKEN`.

For `safe-omada-mcp`, configure npm's GitHub Actions trusted publisher with:

- Organization or user: `gaspareduard`
- Repository: `Omada-mcp`
- Workflow filename: `publish.yml`
- Environment name: empty (the publishing job does not use an environment)
- Allowed actions: `npm publish`; separate `npm dist-tag` access is unnecessary

New trust connections require a first successful publish within two days or they
expire. Complete the initial OIDC publish before removing fallback credentials or
disabling token publishing. A dry run does not verify registry authentication.

After a successful OIDC publish, remove the unused GitHub `NPM_TOKEN` secret,
revoke the obsolete npm automation token, and select npm's publishing option that
requires 2FA and disallows bypass tokens.

Reference: [npm trusted publishing](https://docs.npmjs.com/trusted-publishers/).
