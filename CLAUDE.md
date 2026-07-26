# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project overview

"SOSHogar24" — a static landing page for home-repair/emergency services in Puebla (albañilería, eléctrica, pintura, techo, paneles solares, jardinería). No framework, no build step, no backend: `index.html` is the marketing page and `cotizador.html` is a separate internal quote-builder tool. Every conversion path is a `wa.me` WhatsApp deep link with a pre-filled message.

## Files

- `index.html` — the public landing page. Includes an inline Meta/Facebook Pixel snippet (`fbq('init', '1981359202511359')`) that fires `PageView` on load and `Contact`/`Lead`/`Purchase` (value 0, MXN) plus `trackCustom` (`WhatsAppClick`/`PhoneClick`) events on every `wa.me`/`tel:` link click — if you add a new CTA button, wire it into the same click-tracking handler (it queries `a[href*="wa.me"]` and phone links generically, so as long as a new link matches those selectors it should already be covered; verify rather than assuming).
- `cotizador.html` — **internal-only** tool (`<meta name="robots" content="noindex, nofollow">`), not linked from the public site. Lets a technician build an itemized quote (add/remove line items, quantities, totals) client-side, then generates a `wa.me` link with the formatted quote text to send to the customer. No persistence, no backend — everything lives in the page's in-memory state while it's open.
- `assets/` — service category photos (`albanileria-*`, `electrica-*`, `pintura-*`, `techo-*`) used directly by `index.html`.
- `MARKET_ANALYSIS_MX.md` — market-sizing research doc for the home-services sector in Mexico; business reference material, not something the code depends on.
- `Estrategia_Facebook_Ads_SOSHogar24.docx` — ad strategy doc; same, reference-only.
- `deploy.bat` — **do not treat this as the real deploy process.** It's a one-off script from a specific past change (hardcoded commit message about installing the Meta pixel, and it `cd`s into a OneDrive path that isn't this repo's actual working copy). To deploy, just commit/push normally — Vercel auto-deploys `main`.

## Working in this repo

- There's no dev server / build — open `index.html` (or `cotizador.html`) directly, or serve the folder with any static file server.
- Both pages are single self-contained files (inline `<style>`/`<script>`), matching the pattern used across this account's other static-site repos (e.g. `Auria`) — search within the file rather than expecting a `src/` structure.
