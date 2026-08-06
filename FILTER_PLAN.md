# Pathway Filter Pill Component — Implementation Prompt

Before planning or writing any code, read `CLAUDE.md`, `AGENTS.md`, and `TRANSITION.md` in full. All work must be on the `dev` branch. Do not touch `main`.

---

## Objective

Build a custom Pathway filter pill component for Health Goal collection pages. A row of pill-shaped filter labels renders above the product grid, allowing visitors to filter products by Pathway without leaving the page.

---

## Critical Data Structure Facts — Verify Before Acting

### Collection metafield

Each Health Goal collection has a metafield `custom.ref_pathways` — a list type connected to the `supplement_pathway` metaobject. This is the data source for which pills to render. Storefront API access is enabled.

### Filter URL behavior

When a Pathway filter is applied, Shopify appends a parameter in this format:

```
?filter.p.m.custom.supplement_pathway=gid%3A%2F%2Fshopify%2FMetaobject%2F203627626808
```

The filter value is the **full URL-encoded metaobject GID** — not the entry handle. Pill links must use the GID, and active state detection must compare against it.

**Before writing any Liquid, verify against Shopify documentation or the Shopify Dev MCP:**

- That `pathway.id` returns the full GID string `gid://shopify/Metaobject/<id>`
- That `request.parameters` returns the URL-decoded GID so the active state comparison works
- The exact filter parameter key against a live filtered URL in the dev store — expected convention is `filter.p.m.custom.supplement_pathway` but confirm before assuming
- Correct field accessor syntax for metaobject references on list-type metafields

---

## Component Behavior

- **No filter active** — "All Pathways" pill is active, full product grid shows, URL is the base collection path
- **Filter active** — matching Pathway pill is active, "All Pathways" links back to base collection URL, product grid filters accordingly
- Pills render dynamically from `custom.ref_pathways` — never hardcoded
- "All Pathways" is always first
- Active pill links back to base collection URL to allow deselection
- Core filter behavior must work via Liquid and anchor tags alone — no JavaScript dependency for filtering to function Do not append `sort_by`, `grid`, or other UI state parameters to pill-constructed URLs.

---

## Template Scope

This component belongs on a **dedicated Health Goal collection template**, not the default collection template.

- Check whether a dedicated Health Goal template already exists before creating one
- Check whether Shopify's native filter UI renders on collection pages and whether it should be suppressed on this template — do not suppress it without first confirming what other filter types it exposes. Discuss the tradeoff before acting.

### Health Goal Collection Handles

The six Health Goal collections are Brain & Mood, Energy & Metabolism, Gut & Detox, Hormones & Vitality, Immune & Repair, and Strength & Mobility. Verify their exact Shopify handles in the dev store before building — do not assume the handle format. The dedicated template must be assigned to these six collections specifically and no others.

---

## Secondary Objective — Title & Description Swap

When a Pathway filter is active, swap the collection title and description for the active Pathway's name and description from the `supplement_pathway` metaobject's `name` and `description` fields.

- No filter active → render `collection.title` and `collection.description` as normal
- Filter active → render `pathway.name.value` and `pathway.description.value`
- If `pathway.description.value` is empty, fall back to `collection.description` Pathway descriptions are not yet populated in Admin — build the logic now so it works as soon as they are.

---

## Styling

Use design system tokens exclusively — no hardcoded hex values, pixel values, or font families:

- Shape: `--rounded-full`
- Typography: `--font-accent` role, `--text-accent` size, `--tracking-accent` letter spacing
- Spacing: `--space-*` tokens for padding and gap
- Transitions: `--duration-fast` and `--ease-out` for hover/active states
- Active vs inactive pill treatment: filled vs outlined, using brand tokens from `token-bridge.liquid` Check `assets/base.css` for any existing pill or badge styles to reuse before writing new CSS.

---

## Implementation Sequence

1. Verify Liquid behavior for `pathway.id`, `request.parameters`, and metaobject field accessors via Shopify Dev MCP or documentation
2. Confirm or create the dedicated Health Goal collection template
3. Build the pill component as a standalone reusable snippet
4. Render the snippet in the Health Goal template above the product grid
5. Implement the title/description swap logic
6. Apply design system token styling
7. Run `shopify theme check` — resolve all errors, treat warnings as errors for new code
8. Test on a real Shopify preview URL against the dev store
9. Verify across Chrome, Firefox, Safari desktop, Safari iOS, and Chrome Android

---

## Git Convention

Follow the Conventional Commits standard in `CLAUDE.md`. Suggested commits:

```
feat(collections): add pathway filter pill snippet for health goal pages
feat(collections): add health goal collection template with pathway pills
style(collections): apply design system tokens to pathway filter pills
feat(collections): swap collection title and description on active pathway filter
```

---

## Success Criteria

- Visiting a Health Goal collection page renders pills dynamically from `custom.ref_pathways` with "All Pathways" active by default
- Clicking a Pathway pill appends the correct GID-based filter parameter, activates that pill, and filters the product grid
- Clicking an active pill or "All Pathways" returns to the base collection URL
- Active Pathway name and description swap in when a filter is active, falling back gracefully if empty
- `shopify theme check` passes clean
- No hardcoded values anywhere in new code
- Component works across all required browsers
