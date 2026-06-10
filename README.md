# hugo-theme-tui

A terminal-UI Hugo theme: a box-drawn frame around the page, lazygit-style
row hovers on lists, a restrained Nord palette, a monospace font ladder,
and a dot-matrix glyph marker on the frame border.

Built for personal sites — bio + projects + a blog.

```
╭── whoami.md ──────────────────────────────────── • • • • ▏──╮
│                                                              │
│  user@example.com ~/                  [whoami]  projects  …  │
│  ────────────────────────────────────────────────────────    │
│  Fedor Vinogradov                                            │
│  one-line description here                                   │
│                                                              │
│  ── about ──────────────────────────────────────────────     │
│   · recently graduated …                                     │
│   · working on …                                             │
│   · passionate about open source                             │
│                                                              │
╰──────────────────────────────────────────────────────────────╯
```

## Install

```bash
# from your Hugo site root
git submodule add https://github.com/tddra/hugo-theme-tui themes/hugo-theme-tui
```

Then in your `hugo.toml`:

```toml
theme = "hugo-theme-tui"
```

Or clone directly without submodules:

```bash
git clone https://github.com/tddra/hugo-theme-tui themes/hugo-theme-tui
```

## Try the demo site

```bash
git clone https://github.com/tddra/hugo-theme-tui
cd hugo-theme-tui
hugo server --source exampleSite --themesDir ../..
```

Then open `http://localhost:1313`.

## Configure

All configuration lives in `hugo.toml`. See `exampleSite/hugo.toml` for a
complete working example. Minimum to look right:

```toml
[params]
brandUser = "fedor"           # left side of the user@host brand line
brandHost = "fedorvin.com"    # right side

[params.about]
title       = "Your Name"
description = "one-line tagline shown under the title"
```

### The dot-matrix marker

The bracketed dot-matrix glyphs sitting on the frame's top border are the
theme's signature element. The word changes per page kind, and is
configurable:

```toml
[params.marker]
home         = ["Д", "О", "М"]
projects     = ["К", "О", "Д"]
posts_list   = ["П", "О", "С", "Т"]
posts_single = ["С", "Т", "А", "Т"]
fallback     = ["С", "Т", "А", "Т"]
```

Each entry is a list of single characters. The bitmap font ships with
**eight Cyrillic glyphs**: `А Д К М О П С Т`. To use different
characters, add 5×5 bitmap entries to the `$font` dict in
`layouts/partials/cyrillic-svg.html`. (Each glyph is a list of five
strings of five `1`/`0` digits — see the existing entries for examples.)

### Socials

The home page renders a socials grid driven by `params.socialLinks`:

```toml
[[params.socialLinks]]
key   = "email"
value = "hello@example.com"
url   = "mailto:hello@example.com"
icon  = "email"
```

Built-in icons: `email`, `linkedin`, `github`, `git`, `rss`, `pgp`,
`coffee`. Add more by extending `layouts/partials/icon.html`.

### Projects list

A "projects" page renders a `proj-list` from page params, not from child
pages. See `exampleSite/content/projects.md` for the structure:

```toml
[[projects]]
title = "my-project"
url   = "https://github.com/…"
lang  = "rust"
desc  = "one-line description"
```

## Customizing

The whole theme is four CSS files under `static/css/`:

| File         | Purpose                                                   |
| ------------ | --------------------------------------------------------- |
| `shared.css` | Nord palette variables, base typography, body reset        |
| `tui.css`    | All component styles (frame, header, sections, rows, etc.) |
| `pattern.css`| Marker positioning (border-anchored)                       |
| `custom.css` | Hugo-rendered-markdown adjustments (TOC, images, etc.)     |

Override anything by creating a same-named partial or stylesheet in your
site's own `layouts/` or `static/` directory — Hugo's lookup order picks
your site's files over the theme's.

## License

[MIT](./LICENSE) — Fedor Vinogradov, 2026.
