# GlobalStack API docs

Source for https://docs.globalstack.io, built with [Mintlify](https://mintlify.com).

## Run locally

```bash
npm i -g mint
mint dev
```

Then open http://localhost:3000. Mintlify doesn't support Node 25+, so use Node 24 or lower.

## Publish

Merging to `main` deploys to production automatically.

## Contributing

Read [AGENTS.md](AGENTS.md) before editing. It covers where pages live, the terminology to use,
content rules (including never naming upstream providers), and why `api-reference/openapi.json`
is edited in the `sentinel` repo, not here.
