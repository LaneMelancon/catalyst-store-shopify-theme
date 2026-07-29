# NeuroSolution ATX Supplement Store — Shopify Theme

<<<<<<< HEAD
**Maintained By:** Lane Melancon — Onn Grid, LLC **Client:** Dr. Brandon Crawford — NeuroSolution Center of Austin **Last Updated:** 2026-07-15 **GitHub Repo:** `https://github.com/LaneMelancon/nsatx-store-shopify-theme`
=======
**Maintained By:** Lane Melancon — Onn Grid, LLC **Client:** Dr. Brandon Crawford — NeuroSolution Center of Austin **Last Updated:** 2026-07-28 **GitHub Repo:** `https://github.com/LaneMelancon/nsatx-store-shopify-theme`
>>>>>>> dev

---

## Project Direction

This is an exported default theme from Shopify called 'Tinker.' It serves as the boilerplate skeleton for building the Supplement Store using the Design System (`nsatx-store/design-system/CLAUDE.md`).

<<<<<<< HEAD
This repo is kept independent (as a submodule of `nsatx-store`) specifically to support Shopify's GitHub integration — the Shopify GitHub app requires a standalone repo to sync theme code directly to a store without manual imports.
=======
This repo is kept independent (as a submodule of `nsatx-store`) specifically to support Shopify's GitHub integration — the Shopify GitHub app requires a standalone repo to sync theme code directly to a store without manual imports. This repo contains two branches 'main' and 'dev'. All changes, updated, and additions are only comitted and pushed to the 'dev' branch and reviewed before squash-merging into the 'main' branch. The 'main' branch is connected to the Shopify store via the Github connection and is published as the live theme (changes to the 'main' branch automatically get published to the live site). The 'dev' branch is connected to the Shopify store as a draft theme where changes are made locally or within the Shopify theme editor.
>>>>>>> dev

**Design-system token bridge:** complete (see `TRANSITION.md` for the full plan and Changelog below). Tinker's font/color/spacing CSS custom properties are overridden at `:root` by `assets/design-tokens.css` + `snippets/brand-fonts.liquid` + `snippets/token-bridge.liquid`, rendered last in `layout/theme.liquid`'s `<head>`. No section/block/component file was changed — Tinker's own CSS consumes the bridged values automatically.

## Shopify GitHub Integration

- **Dev store:** `atx-prod-test-01.myshopify.com` — connected via Shopify GitHub app and **live**: `nsatx-store-shopify-theme/main` is the store's published theme. Pushing to `main` deploys to this store immediately — always confirm with the project owner before pushing.
- **Production store:** `neurosolution-shop.myshopify.com` — connect after dev store is confirmed
- Once connected, pushing to `main` will sync theme changes to the store in real time

## Shopify Theme Development Guidelines

Read the `AGENTS.md` file only when directed to make changes, updates, or modifcations to the theme code. The sections within the file provide guidelines, frontend best practices, and conventions for Claude Code when working on the Shopify theme project. Follow these guidelines and rules unless project owner/maintainer explicitly overrides them.

### Shopify CLI

- `shopify theme dev` — starts a local dev server that mirrors file changes to the dev store in real time. Use this before committing to preview changes in a real browser.
- `shopify theme check` — lints the theme for errors and warnings. Run before every push.
- `shopify theme push` — redundant in this project; the GitHub integration handles deployment to the dev store automatically on push to `main`.

---

## Project Rules

- Keep this markdown file up to date with changes or additions related to the Shopify Theme
<<<<<<< HEAD
- Always develop against the dev store (`atx-prod-test-01`) before pushing to production
- Reference `nsatx-store/design-system/CLAUDE.md` for all design tokens, components, and styling decisions
=======
- Always develop and implement changes against the 'dev' branch.
- Never commit changes to the 'main' branch, Lane will take care of merges from 'dev' to 'main'.
- Reference `nsatx-store/design-system/CLAUDE.md` for all design and styling decisions
>>>>>>> dev
- Read `TRANSITION.md` only when specifically instructed to work on or perform the design-system to Shopify theme transition

### Testing & Quality

- Run `shopify theme check` before every commit and resolve all errors. Treat warnings as errors for new code.
- Validate all schema JSON — malformed schema silently breaks the Theme Editor.
- Test on real Shopify preview URLs, not just `localhost` — cart, redirects, and some Liquid objects behave differently locally.
- Test across: Chrome, Firefox, Safari (desktop + iOS), and Chrome Android.
- Test with a screen reader (VoiceOver on macOS/iOS, NVDA on Windows) for accessibility-critical flows.
- Test Theme Editor functionality: every section must be addable, removable, and reorderable without JS errors.

### Documentation

- Every section must have a comment block at the top describing its purpose and any non-obvious schema settings.
- Add inline `{% comment %}` blocks for non-obvious Liquid workarounds explaining the reason.

---

## Changelog

| Date | Changes |
| --- | --- |
<<<<<<< HEAD
=======
| 2026-07-28 | Expanded the `Style: Custom` button panel from three colors into four groups — Appearance, Borders, Typography, Padding — across all five blocks sharing `snippets/button-custom-styles.liquid`, with new ids prefixed `custom_button_*` so they don't collide with `email-signup`'s own `border_*`/`padding-*` container settings. Rebuilt `blocks/popup-link.liquid`'s schema (deleted wholesale by Shopify sync `326c730`) and wired it up for the first time — the trigger had only ever rendered `spacing-style`, so its typography and appearance settings had never done anything. Also extracted the fluid `clamp()` font-size math into `snippets/fluid-font-size.liquid`; note that button padding is fed through the `--button-padding-*` variables rather than physical properties, which by design leaves the Padding group inert on email-signup buttons so their arrow/integrated layouts survive. |
>>>>>>> dev
| 2026-07-15 | Implemented the design-system → theme token bridge per `TRANSITION.md`. Self-hosted Objective + Trade Gothic LT (14 `.woff2` files + `snippets/brand-fonts.liquid`, replacing Shopify's font-picker CDN). Added `assets/design-tokens.css` (verbatim copy of the design system's `tokens.css`) and `snippets/token-bridge.liquid`, which overrides ~25 Tinker `--color-*`/`--font-*`/`--padding-*`/`--margin-*`/`--gap-*` variables at `:root`. Replaced the theme's 6-key `color_palette` with a 13-key design-system ramp in `settings_schema.json`/`settings_data.json` and remapped the 6 settings that referenced the old `color1`–`color4` keys. Hid (via `visible_if`, not deletion) the 4 now-vestigial font-picker settings. Realigned `assets/base.css`'s local hover/transition variables to the design system's duration/easing tokens. `shopify theme check` passes clean. Discovered during verification: `badge_sale_text_color` and the newly-remapped `badge_sale_background_color` both resolve to the `foreground` palette key, making the Sale badge text invisible — needs a manual fix in the Theme Editor (point `badge_sale_text_color` at `background` instead). Confirmed the dev store's GitHub integration is already live (not "pending" as previously documented) — `nsatx-store-shopify-theme/main` is the published theme on `atx-prod-test-01.myshopify.com`. |
