# Lyra • Content Studio (CMS)

The editorial home of the Lyra website, built on
[Sanity Studio v6](https://www.sanity.io/studio). Everything the site shows -
pages, blog posts, navigation, footer, and site-wide settings - is created and
published here. The website in [`../web`](../web) picks up changes within
seconds of publishing.

## What editors can do

- **Build pages from reusable sections** - author a section once (a hero, a
  feature grid, a stepper, a comparison table…) and reuse it on any page.
  A dedicated view surfaces sections that are no longer used anywhere.
- **Write and organize the blog** - posts with authors, categories, and tags,
  rich text with images, code blocks, and tables
- **Control the site's look** - header (dropdown or mega-menu navigation),
  footer, brand colors, and per-section layout options
- **Manage site-wide settings** - general info, SEO defaults, reading
  settings, and analytics/tracking configuration, each in a single dedicated
  document
- **Archive instead of delete** - pages and posts support soft archiving with
  one-click restore
- **Stay safe** - core pages (home, blog) are protected from deletion,
  unpublishing, and slug changes

## Stack

- Sanity Studio v6 (React 19, TypeScript)
- Structured content: 77 typed schemas covering documents, sections, fields,
  and objects
- Plugins: media library, color input, code input, tables, and Vision for
  query exploration

## Configuration

All configuration is supplied outside the source code: no project identifiers
or credentials are stored in it.

Every commit and push is scanned by gitleaks and blocked if it contains a
secret - see [Secret scanning](../README.md#secret-scanning-mandatory).

## License

Source-available, not open source. The code is public so that prospective
clients, partners and employers can review how FCI Devel builds software: you
may read it, and clone and run it for that review, but you may not reuse it.

See [LICENSE](LICENSE) for the full terms.
