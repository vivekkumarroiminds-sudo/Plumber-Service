# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A static, single-page lead-generation landing site for "ProConnect Plumbing" — it connects visitors with local plumbers via a click-to-call number and a lead form. There is no build system, no package manager, no tests, and no backend. It's plain HTML/CSS/JS served as static files.

To preview, open `index.html` directly in a browser or serve the folder (e.g. `python3 -m http.server`). The deployed site is GitHub Pages from the `Plumber-Service/` repo.

## Layout & the two copies

Three pages, fully self-contained (CSS in a `<style>` block, JS in a `<script>` at the bottom of each file — no external CSS/JS assets except Google Fonts and gtag.js):
- `index.html` — landing page (hero + lead form, services, reviews, FAQ accordion, CTAs)
- `privacy.html`, `terms.html` — legal pages

**Important:** the repo contains two near-identical copies of all three pages:
- The **root** (`./index.html` etc.) is the working/editing copy and is **not** under git.
- `Plumber-Service/` is the git repository (remote: `github.com/vivekkumarroiminds-sudo/Plumber-Service`) and is the version that actually deploys.

These can drift (currently the only difference is the `<title>` in `index.html`). When making changes that should go live, edit/sync the file inside `Plumber-Service/` and commit there — editing only the root copy will not deploy.

## Lead capture & conversion tracking (the parts that need real values)

The page is wired for Google Ads conversion tracking but ships with **placeholders that must be replaced** before tracking works:

- `AW-CONVERSION_ID` appears in the gtag.js `<head>` snippet and in the `CONVERSIONS` object near the bottom of `index.html`. Replace with the real Google Ads ID (`AW-XXXXXXXXX`).
- `CONVERSIONS.call` (`tel:` click conversion) and `CONVERSIONS.form` (lead-form submit conversion) hold `.../CALL_LABEL` and `.../FORM_LABEL` — replace with real conversion labels.
- `fireConversion()` is deliberately a no-op until the placeholders are filled (it checks for the literal `CONVERSION_ID` string and skips firing), so it's safe to leave unconfigured.

The lead form (`#leadForm`) currently only validates and shows an `alert()` — it does **not** send data anywhere. The `submit` handler in `index.html` has a `// TODO: send lead data to your CRM / MarketCall offer endpoint here.` Wire the real lead-post / MarketCall integration there (a commented `fetch(...)` example is in place).

The phone number `(800) 555-0199` / `tel:+18005550199` is a placeholder repeated across the header, hero, final CTA, and mobile bar — update all occurrences together.
