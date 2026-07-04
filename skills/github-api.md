# GitHub API

## Capability
Authenticated requests to api.github.com without exposing the PAT to the LLM context. Server-side token injection.

## Tool
- `github_api(method, path, body)` — REST/GraphQL calls to api.github.com

## Patterns

### Create a repository
```
github_api('POST', '/user/repos', {'name': 'agent-trust', 'private': False})
```

### Enable GitHub Pages
```
github_api('POST', '/repos/rain-ouroboros/agent-trust/pages', {'source': {'branch': 'main', 'path': '/docs'}})
```

### Push files via API
GitHub Contents API: PUT /repos/:owner/:repo/contents/:path with base64-encoded content.

## Architecture
- PAT is read from RAIN_GITHUB_PAT env var server-side
- Hardcoded to api.github.com only — other hosts and off-host redirects are rejected
- LLM never sees the token
- Receipts in logs/github_api.jsonl without secret-bearing bodies

## Gotchas
- New GitHub accounts have a restriction period before API tokens can be created
- Fine-grained PAT needs repo scope for repository operations
- Classic PAT with repo scope works for Pages enablement
