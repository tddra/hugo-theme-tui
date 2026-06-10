# hugo-theme-tui

A terminal-UI Hugo theme. Box-drawn frame around the page, lazygit-style
row hovers on lists, restrained Nord palette, monospace typography, and a
dot-matrix glyph marker on the frame border.

Built for personal sites — bio + projects + a blog.

```
╭── whoami.md ──────────────────────────────────── • • • • ▏──╮
│                                                              │
│  user@example.com ~/                  [whoami]  projects  …  │
│  ────────────────────────────────────────────────────────    │
│  Your Name                                                   │
│  one-line tagline                                            │
│                                                              │
│  ── about ──────────────────────────────────────────────     │
│   · bullet one                                               │
│   · bullet two                                               │
│   · bullet three                                             │
│                                                              │
╰──────────────────────────────────────────────────────────────╯
```

To try it locally: clone the repo, then `hugo server --source exampleSite
--themesDir ../..`. The rest of this README documents only what is
specific to this theme — see `exampleSite/hugo.toml` for a full working
example.

## Theme-specific parameters

| Param                     | Type    | Default          | Purpose                                                       |
| ------------------------- | ------- | ---------------- | ------------------------------------------------------------- |
| `brandUser`               | string  | `"user"`         | Left side of the `user@host` brand line in the header.        |
| `brandHost`               | string  | `"example.com"`  | Right side of the brand line.                                 |
| `brandLabel`              | string  | `"whoami"`       | Label of the first nav link (always points to `/`).           |
| `about.title`             | string  | site title       | H1 on the homepage.                                           |
| `about.description`       | string  | —                | Subtitle under the homepage H1.                               |
| `marker.home`             | []string| `["Д","О","М"]` | Dot-matrix word on the homepage.                              |
| `marker.projects`         | []string| `["К","О","Д"]` | Dot-matrix word on the projects page.                         |
| `marker.posts_list`       | []string| `["П","О","С","Т"]` | Dot-matrix word on the posts list.                        |
| `marker.posts_single`     | []string| `["С","Т","А","Т"]` | Dot-matrix word on a single post.                         |
| `marker.fallback`         | []string| `["С","Т","А","Т"]` | Dot-matrix word on any other page.                        |
| `socialLinks`             | array   | —                | Tiles in the homepage socials grid (see below).               |

## The dot-matrix marker

The bracketed dots on the frame's top border are the theme's signature.
Each "word" is a list of single characters; each character is rendered
from a 5×5 bitmap in `layouts/partials/cyrillic-svg.html`.

The bitmap font ships with **eight Cyrillic glyphs**: `А Д К М О П С Т`.
Anything outside that set renders nothing — so if you want to use
different letters, add their bitmaps to the `$font` dict in that partial.
Each entry is a list of five 5-char strings of `1`/`0`. E.g. to add
Cyrillic `Р`:

```go-html-template
"Р" (slice "11110" "10001" "11110" "10000" "10000")
```

## Nav

The first nav link is always the home link; its label comes from
`brandLabel`. The remaining links are read from `[[menu.main]]` — add or
remove entries to change the nav. Active state is auto-detected from the
current page URL (matches the menu entry's URL or any descendant of it).

```toml
[[menu.main]]
identifier = "projects"
name = "projects"
url = "/projects/"
weight = 10
```

## Socials grid

The homepage renders a tile grid driven by `params.socialLinks`. Each
entry needs four fields:

```toml
[[params.socialLinks]]
key   = "email"                   # small-caps label inside the tile
value = "hello@example.com"       # value shown below the key
url   = "mailto:hello@example.com"
icon  = "email"                   # see icon list below
```

Built-in icons: `email`, `linkedin`, `github`, `git`, `rss`, `pgp`,
`coffee`. To add more, edit `layouts/partials/icon.html` and add another
`{{ else if eq $name "your-name" }}<svg …></svg>` branch — the SVGs
inherit the current text color via `stroke="currentColor"`.

## Projects page

The projects page reads its entries from **page params**, not from child
pages. Create `content/projects.md` with `type = "projects"` and a
`[[projects]]` array:

```toml
+++
title = "projects"
type = "projects"
layout = "projects"

[[projects]]
title = "my-project"
url   = "https://github.com/…"   # optional — omit for a plain row
lang  = "rust"                    # shown in the left column, bracketed
desc  = "one-line description"
+++
```

The `lang` column is rendered as `[rust]` and is purely visual — it can
be any short string.

## Posts

Standard Hugo `content/posts/*.md`. Theme-specific behaviours:

- Posts are grouped by year on the posts list (`GroupByDate "2006"`).
- The frame title on a single post is `── <basename>.md ──`.
- A table of contents auto-renders **above** the article when the post
  has any `##`/`###` headings. Hugo's TOC settings apply (`markup.tableOfContents`).
- `tags` in front matter render as `[#tag]` chips in the post-list row.

## Customising

Theme structure:

```
layouts/
  _default/   baseof, list, single, projects
  partials/   head, header, footer, socials, icon, cyrillic-svg
  posts/      list, single
  index.html  homepage
static/css/   shared, tui, pattern, custom
```

Override any partial or stylesheet by creating a same-named file at the
same path inside **your site's** `layouts/` or `static/` directory.
Example: to add an analytics script in `<head>` without forking the
theme, create `layouts/partials/head.html` in your site and Hugo will use
that one instead.

The CSS is four small files (~700 LOC total):

| File          | Holds                                                      |
| ------------- | ---------------------------------------------------------- |
| `shared.css`  | Nord palette CSS variables, font import, body reset        |
| `tui.css`     | All component styles (frame, header, sections, rows, …)    |
| `pattern.css` | Border-anchored marker positioning                          |
| `custom.css`  | Adjustments for Hugo-rendered markdown (TOC, images, …)    |

## Known limitations

- The marker bitmap font ships with 8 Cyrillic glyphs only — extend it
  in `cyrillic-svg.html` to use other characters.
- Only a dark theme (Nord). No light-mode toggle.
- The projects page reads from a page-param array, not from child pages.
  If you want one-page-per-project, you'll need to add a layout.

## License

[MIT](./LICENSE) — Fedor Vinogradov, 2026.
