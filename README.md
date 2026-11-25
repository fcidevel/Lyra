# Lyra

<div align="center">

_A company website with a built-in blog, driven entirely by a headless CMS._

[![Live Demo](https://img.shields.io/badge/demo-live-brightgreen)](https://lyra.jwd-apps.com)
![Next.js 16](https://img.shields.io/badge/Next.js-16-black)
![React 19](https://img.shields.io/badge/React-19-149eca)
![Sanity Studio v6](https://img.shields.io/badge/Sanity_Studio-v6-f03e2f)
![TypeScript strict](https://img.shields.io/badge/TypeScript-strict-3178c6)
[![Secret scan](https://github.com/fcidevel/Lyra/actions/workflows/gitleaks.yml/badge.svg?branch=main)](https://github.com/fcidevel/Lyra/actions/workflows/gitleaks.yml)

</div>

---

## Description

Lyra is a modern company website with a built-in blog. Every page, menu, and
setting is authored in [Sanity](https://www.sanity.io/) and rendered by a fast,
SEO-friendly [Next.js](https://nextjs.org/) frontend, so publishing new content
never needs a code change.

The repository is a two-package monorepo:

| Package | What it is |
| --- | --- |
| [`web/`](web) | The public website - Next.js 16, React 19 Server Components, Sass |
| [`studio/`](studio) | The content studio - Sanity Studio v6, where editors manage everything |

### Key Features

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

---

## Tech Stack

### Frontend

| Technology | Purpose |
|------------|---------|
| [Next.js 16](https://nextjs.org/) (App Router) | Server-rendered pages, route handlers, metadata, sitemap and robots |
| [React 19](https://react.dev/) Server Components | UI rendering |
| [TypeScript](https://www.typescriptlang.org/) (strict) | Type safety in both packages |
| [Sass](https://sass-lang.com/) | Styling |
| [Zod](https://zod.dev/) | Runtime validation of every CMS response before it renders |
| [Portable Text](https://portabletext.org/) | Rich text rendering |

### Backend & Services

| Technology | Purpose |
|------------|---------|
| [Sanity](https://www.sanity.io/) (GROQ) | Content storage and queries |
| [Sanity Studio v6](https://www.sanity.io/studio) | Editorial interface: 77 schema types, custom desk structure, document actions |
| [Vercel](https://vercel.com/) | Hosting for the website |
| [reCAPTCHA v3](https://developers.google.com/recaptcha) | Bot protection for custom forms, verified server-side |
| Google Tag Manager, HubSpot | Analytics and forms, loaded according to cookie consent |

### Tooling

| Technology | Purpose |
|------------|---------|
| [ESLint](https://eslint.org/) | Linting in both packages |
| [Knip](https://knip.dev/) | Unused files, exports and dependencies in the website |
| [gitleaks](https://github.com/gitleaks/gitleaks) | Mandatory secret scanning (git hooks + CI) - see [Secret scanning](#secret-scanning-mandatory) |

---

## Project Structure

```
Lyra/
├── .githooks/                pre-commit and pre-push secret-scanning hooks
├── .github/workflows/
│   └── gitleaks.yml          "Secret scan" CI job
├── studio/                   Sanity Studio v6
│   ├── actions/              document actions: archiving, protected core pages
│   ├── consts/               shared constants and configuration
│   ├── schemas/              77 schema types: documents, sections, fields, objects
│   ├── structure/            desk structure
│   ├── utils/                validation and preview helpers
│   ├── sanity.config.ts
│   ├── sanity.cli.ts
│   └── LICENSE
├── web/                      Next.js 16 website
│   ├── app/                  App Router: pages, blog, API routes, metadata routes
│   ├── components/           layout, sections, blog, consent and analytics
│   ├── lib/                  data loaders, GROQ queries, Zod schemas, utilities
│   ├── styles/               Sass
│   ├── public/
│   ├── proxy.ts              per-request Content Security Policy with nonces
│   ├── next.config.mjs
│   └── LICENSE
├── .gitattributes
├── .gitleaks.toml            gitleaks rules: built-in plus project-specific
├── build-check.sh            Vercel ignored-build-step script
├── LICENSE
└── README.md
```

---

## Secret scanning (mandatory)

Every change is checked for leaked credentials with
[gitleaks](https://github.com/gitleaks/gitleaks), at three layers:

| Layer | What runs | What it blocks |
| --- | --- | --- |
| Commit | `.githooks/pre-commit` - scans the staged changes | the commit |
| Push | `.githooks/pre-push` - scans every commit being pushed | the push |
| GitHub | `Secret scan` job (`.github/workflows/gitleaks.yml`) - scans the full history on every push and pull request | updating `main` (required status check) |

- **No credentials in the repository.** Real values are never committed; both
  packages read them from their deployment environment.
- **Project-specific rule.** [`.gitleaks.toml`](.gitleaks.toml) keeps every
  built-in gitleaks rule and adds one for a format this app uses: Sanity API
  tokens, caught under any variable name.
- **Hooks on by default.** Installing either package's dependencies switches
  the hooks on for that clone.
- **Supply-chain hardening.** The CI job pins `actions/checkout` to a commit SHA
  and verifies the gitleaks release binary against its published checksum.
- **Leaked-secret policy.** A secret that ever reaches GitHub is treated as
  compromised: it is rotated first, then removed from the history.

---

## Security

### Application

- **Content Security Policy** with a per-request nonce on every page
  ([`web/proxy.ts`](web/proxy.ts)).
- **Validated content.** Every CMS response is checked against Zod schemas
  before it renders, so malformed content can't silently break a page.
- **Signed webhooks.** The cache-revalidation endpoint accepts only requests
  signed with the shared Sanity webhook secret, and refuses every request when
  no secret is configured.
- **Bot protection.** Custom form submissions are verified server-side with
  reCAPTCHA v3 and rejected below a score of 0.5.
- **Consent-first analytics.** Google Tag Manager and HubSpot load only
  according to the visitor's cookie choices.

### Repository protection

Only the repository owner can change this repository. Two GitHub rulesets
enforce it:

| Ruleset | Applies to | Rules | Bypass |
| --- | --- | --- | --- |
| Owner Only Rule | all branches | restrict creations, updates and deletions; block force pushes | repository admin (the owner) |
| Secret Scan Rule | default branch (`main`) | `Secret scan` must pass; block deletions and force pushes | none - applies to the owner too |

A commit can reach `main` only after `Secret scan` has passed on it, and
nobody can force-push or delete `main`.

---

## Author

Built with ❤️ by [FCI Devel](https://fcidevel.com)

---

## License

Source-available, not open source. The code is public so that prospective
clients, partners and employers can review how FCI Devel builds software: you
may read it, and clone and run it for that review, but you may not reuse it.

See [LICENSE](LICENSE) for the full terms.
