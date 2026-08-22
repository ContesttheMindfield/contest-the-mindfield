# Contest the Mindfield

Contest the Mindfield is a bilingual Hugo site about Flesh and Blood. It uses the [Brewm theme](https://github.com/foxihd/hugo-brewm), with English at `/en/` and Brazilian Portuguese at `/pt-br/`.

## Requirements

- Hugo Extended 0.165.x
- Node.js 22.x
- Git with submodule support

## Local setup

```sh
git submodule update --init --recursive
npm ci
hugo server --disableFastRender
```

The Hugo development server is best for editing layouts and content. Pagefind is generated only after a production build, so use this command when testing search:

```sh
npm run build:search
npx pagefind --site public --serve
```

The second command serves the indexed `public` directory. The no-JavaScript search fallback uses DuckDuckGo.

## Create content

Articles are page bundles under `content/<language>/articles/<slug>/`. Select the language explicitly in the path:

```sh
# Standard article
hugo new content --contentDir content/en --kind article articles/my-article/index.md
hugo new content --contentDir content/pt-br --kind article articles/my-article/index.md

# Series article
hugo new content --contentDir content/en --kind series-article articles/my-series-article/index.md

# Media-rich article
hugo new content --contentDir content/en --kind media-article articles/my-media-article/index.md

# Author profile
hugo new content --contentDir content/en --kind author authors/author-slug/_index.md
hugo new content --contentDir content/pt-br --kind author authors/author-slug/_index.md
```

Every article must keep `type = "post"` and must list at least one author profile slug in the `authors` array. Categories are curated, tags are flexible, and series are optional. New articles default to `toc = true`; set it to `false` on an individual article when needed.

The media archetype includes guidance for optional covers and alt text, audio, math, syntax highlighting, and redaction history. Advanced Brewm shortcodes and external libraries should be enabled only by content that needs them.

## Translations

Translations are optional. When two pages are translations of one another:

1. Use the same technical article slug in both language directories.
2. Give both files the same non-empty `translationKey`.
3. Translate titles, descriptions, taxonomy terms, body content, cover alt text, and redaction notes.
4. Use the same author profile slug in both languages, with matching author-profile `translationKey` values.

Shared section and taxonomy slugs stay in English in both languages: `articles`, `authors`, `categories`, `tags`, and `series`.

## Site configuration

Shared Hugo and Brewm settings live in `config/_default/hugo.toml`. Language definitions and localized menus are separated into:

- `config/_default/languages.toml`
- `config/_default/menus.en.toml`
- `config/_default/menus.pt-br.toml`

Series is registered as a taxonomy but is intentionally absent from the main menu. Add it to both localized menu files when the first real series is published.

## Production and deployment

Build and index locally with:

```sh
npm ci
npm run build:search
```

GitHub Pages runs the same sequence: it checks out the pinned submodule, installs the lockfile dependencies, builds with Hugo 0.165 using garbage collection and minification, runs Pagefind, and uploads `public`.

## Update Brewm manually

Brewm is pinned as a Git submodule. Review an upstream commit before changing the pin:

```sh
git -C themes/hugo-brewm fetch origin
git -C themes/hugo-brewm checkout <reviewed-commit>
git add themes/hugo-brewm
npm ci
npm run build:search
```

Commit the updated submodule pointer only after the production build and visual checks pass. Keep site overrides in the repository root instead of editing Brewm or its `exampleSite` files. In particular, compare the upstream list template with `layouts/_default/list.html`, which removes Brewm's demo empty-state image, and retain the responsive wordmark rules in `assets/css/custom.css`.
