# Lyra • Website

The public-facing website for Lyra: a server-rendered Next.js application that
turns content authored in [Sanity](https://www.sanity.io/) into a fast,
accessible, SEO-ready site. All copy, imagery, navigation, and settings come
from the CMS - publishing new content never requires a deploy.

The content itself is managed in the sibling [`../studio`](../studio) package.

## Highlights

- **CMS-driven pages** - marketing pages are assembled from 18 reusable
  section types (hero, key features, stepper, comparison table, team,
  accordion, map, CTA banner, and more), each with editor-controlled layout,
  colors, and visibility options
- **Blog** - paginated listing with tag, category, and author filtering, plus
  rich article pages with structured data
- **Runtime-validated content** - every CMS response is checked against Zod
  schemas before rendering, so malformed content can never break the page
  silently
- **SEO** - per-page titles and descriptions, Open Graph / Twitter cards,
  JSON-LD, sitemap, robots policy, and a web app manifest
- **Consent-first analytics** - Google Tag Manager and HubSpot only activate
  in line with the visitor's cookie choices; consent defaults load before any
  tracking script
- **Forms** - custom forms with reCAPTCHA v3 verification, or embedded
  HubSpot forms
- **Security** - a strict Content Security Policy with per-request nonces is
  applied to every page
- **Instant updates** - tag-based caching invalidated by CMS webhooks, so
  published changes appear on the site within seconds

## Stack

| Area | Technology |
| --- | --- |
| Framework | Next.js 16 (App Router), React 19 Server Components |
| Language | TypeScript |
| CMS | Sanity (GROQ queries via `@sanity/client` / `next-sanity`) |
| Validation | Zod |
| Rich text | Portable Text (`@portabletext/react`) |
| Styling | Sass |
| Images | `next/image` with Sanity's image pipeline (hotspots, blur placeholders) |
| Quality | ESLint, strict TypeScript, Knip |

## Configuration

All configuration is supplied outside the source code: no credentials or
project identifiers are stored in it. Optional server-side features (draft
preview, webhook revalidation, form relay) stay unavailable until they are
configured.

Every commit and push is scanned by gitleaks and blocked if it contains a
secret - see [Secret scanning](../README.md#secret-scanning-mandatory).

## License

Source-available, not open source. The code is public so that prospective
clients, partners and employers can review how FCI Devel builds software: you
may read it, and clone and run it for that review, but you may not reuse it.

See [LICENSE](LICENSE) for the full terms.
