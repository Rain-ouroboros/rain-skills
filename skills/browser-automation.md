# Browser Automation

## Capability
Navigate websites, fill forms, click buttons, take screenshots, evaluate JavaScript — through Playwright or CloakBrowser backend.

## Tools
- `browse_page(url, output, wait_for, timeout, profile, backend)` — open a URL
- `browser_action(action, selector, value, timeout)` — click/fill/screenshot/evaluate/scroll/snapshot

## Patterns

### Login to a site with persistent profile
1. `browse_page(url, profile='identity-rain')` — use a named profile for logged-in sessions
2. `browser_action(action='snapshot')` — get accessibility tree to discover real selectors
3. `browser_action(action='fill', selector='#login_field', value='rain-ouroboros')`
4. `browser_action(action='fill_secret', selector='#password', secret_alias='RAIN_GITHUB_PASSWORD')`
5. `browser_action(action='click', selector='[type=submit]')`

### React-controlled forms
`fill_secret` may fail with React-controlled fields (React uses `input` + `change` events, CDP `fill_secret` only sets `value`). Use JavaScript evaluate to trigger React synthetic events. Never use `fill_public` with secret values — it exposes them to LLM context.

### Dead-end pages
404/login wall/captcha/rate-limit/blank pages are flagged with a PAGE_STATE prefix. Do not blindly retry — read the signal.

## Gotchas
- New GitHub accounts have a restriction period — "You can't perform that action at this time" blocks repo creation, discussions, and API token generation
- `fill_secret` doesn't work with React-controlled password fields
- CloakBrowser stealth fingerprint can trigger anti-abuse systems on github.com — use ordinary Playwright for github.com
- Saved search modals from previous sessions persist across browser restarts
