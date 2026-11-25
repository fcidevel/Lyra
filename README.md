# Lyra

A modern company website with a built-in blog, fully driven by a headless CMS.
Every page, menu, and setting is authored in [Sanity](https://www.sanity.io/)
and rendered by a fast, SEO-friendly [Next.js](https://nextjs.org/) frontend -
no code changes needed to publish new content.

## What's inside

| Package | What it is |
| --- | --- |
| [`web/`](web) | The public website - Next.js 16, React 19 Server Components, Sass |
| [`studio/`](studio) | The content studio - Sanity Studio v6, where editors manage everything |

## Features

- **Composable pages** - editors build pages from a library of 18 reusable
  content sections (heroes, feature grids, steppers, comparisons, forms,
  maps, team overviews, and more) that can be shared across pages
- **Full blog** - posts with authors, categories, and tags, plus filtering
  and pagination out of the box
- **Editor-friendly studio** - organized desk structure, singleton settings,
  soft archiving, and protected core pages that can't be accidentally deleted
- **SEO built in** - per-page metadata, Open Graph and Twitter cards, JSON-LD
  structured data, sitemap, robots, and web manifest
- **Privacy-aware analytics** - Google Tag Manager and HubSpot load only
  according to the visitor's cookie consent choices
- **Lead capture** - custom forms with reCAPTCHA protection, or embedded
  HubSpot forms
- **Fast by design** - server-rendered pages with tag-based caching and
  instant cache invalidation when content changes in the CMS

## Technology

- **Frontend**: Next.js 16 (App Router), React 19, TypeScript, Sass
- **CMS**: Sanity Studio v6 with GROQ queries
- **Validation**: Zod runtime schemas guard every piece of CMS content before
  it renders
- **Security**: Content Security Policy with per-request nonces, and mandatory
  [secret scanning](#secret-scanning-mandatory) with gitleaks on every commit,
  push, and pull request
- **Quality**: ESLint, strict TypeScript, unused-code audits

## Running locally

Each package installs and runs independently. Both need a `.env.local` created
from their `.env.example` template, pointing at your own Sanity project.

You also need [gitleaks](https://github.com/gitleaks/gitleaks) installed
(`winget install Gitleaks.Gitleaks` or `brew install gitleaks`). Once the git
hooks are enabled (`npm install` does it), commits and pushes are blocked
without it - see [Secret scanning](#secret-scanning-mandatory).

```bash
# Content studio
cd studio
npm install
npm run dev          # http://localhost:3333

# Website - in a second shell
cd web
npm install
npm run dev          # http://localhost:3000
```

The website renders once the studio's core content (site settings, header,
footer, and a home page) has been created and published.

## Secret scanning (mandatory)

Every change is checked for leaked credentials with
[gitleaks](https://github.com/gitleaks/gitleaks). The checks are required, not
optional:

| Where | What runs | What it blocks |
| --- | --- | --- |
| `git commit` | `.githooks/pre-commit` - scans the staged changes | the commit |
| `git push` | `.githooks/pre-push` - scans every commit being pushed | the push |
| GitHub | `Secret scan` workflow - scans the full history on every push and pull request | merging into `main` |

- **Setup:** `npm install` in `web/` or `studio/` enables the hooks
  automatically by setting `git config core.hooksPath .githooks`. Run that
  command yourself if you haven't installed dependencies yet.
- **Credentials** live only in each package's `.env.local`, which is gitignored.
  `.env.example` lists variable names with empty values.
- **Never bypass** the hooks with `--no-verify`, and never force-add ignored
  files.
- **False positives:** after review, mark the line with a `gitleaks:allow`
  comment or add the finding's fingerprint to `.gitleaksignore`.
- **Leaked secrets:** if a secret ever reaches GitHub, treat it as compromised.
  Rotate it first, then clean the history.

Rules are configured in [`.gitleaks.toml`](.gitleaks.toml): gitleaks' built-in
rules plus a Sanity API token rule.

---

## Author

Built with ❤️ by [FCI Devel](https://fcidevel.com)

---

## License

This source code is published for reference and evaluation purposes only.

> No permission is granted to use, copy, modify, merge, publish, distribute, sublicense, or sell copies of this software, in whole or in part.

See [LICENSE](LICENSE) for the full terms.
