# lunula-soft-launch

Original soft launch site built on Shopify Dawn v10.0.0 (Online Store 2.0).

## Changelog

- v1.0.0 — originally built on Shopify
- v1.0.1 — make the whole product card a clickable link
- v1.0.2 — add deployment docs to README

## Deployment

This theme deploys directly to Shopify via the Shopify CLI. There is no build step — files are served directly from Shopify's CDN.

### Prerequisites

- Shopify CLI v3+ — install via `npm install -g @shopify/cli @shopify/theme`
- One-time auth: `shopify auth login`

### Development workflow

```bash
# Start local dev server with hot reload (does NOT affect the live store)
shopify theme dev
```

This opens a browser preview at a `*.myshopify.com` URL that reflects local changes in real time. Use this for all day-to-day development.

### Pushing changes

```bash
# Push to a development theme (safe — doesn't touch the live store)
shopify theme push --development

# Push to the live (published) theme — use with caution
shopify theme push
```

When running `shopify theme push` without `--development`, Shopify will prompt you to select which theme to push to. Always double-check before confirming a push to the published/live theme.

### Pulling the live theme

```bash
# Sync local files from the live theme (overwrites local changes)
shopify theme pull
```

Run this before starting work if you suspect the Shopify admin was used to make changes directly.

### Linting

```bash
shopify theme check
```

## Architecture notes

- `templates/*.json` — page structure (managed via Shopify admin; avoid hand-editing)
- `sections/*.liquid` — page-building blocks with embedded `{% schema %}` settings
- `snippets/*.liquid` — reusable partials (must have variables passed explicitly)
- `layout/theme.liquid` — global HTML shell
- `assets/` — flat directory of JS/CSS; no bundler
- `config/settings_data.json` — live theme settings (managed by Shopify admin; treat as read-only)

## Known issues

1. **Dark mode FOUC** — the dark mode script runs on `DOMContentLoaded` causing a flash. Fix: move to an inline `<script>` in `<head>` before CSS loads.
2. **`spacing_sections: 0`** — all section spacing is zeroed in `config/settings_data.json`; verify intentional.
3. **`show_rating: false`** on product template — Judge.me star ratings may not surface on PDPs; verify intent.
