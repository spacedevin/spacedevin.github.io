# spacedevin.github.io

The index page at [spacedevin.github.io](https://spacedevin.github.io/) — a list of pinned projects,
generated at build time.

Written in **[Tish](https://tishlang.com)**. `src/build.tish` runs on the Tish runtime rather than
being compiled to JS, because the build needs both `fetch` and a filesystem and `tish:fs` is only
available under `tish run`.

```bash
npm install
GITHUB_TOKEN=$(gh auth token) npm run build   # -> dist/
npm run serve                                 # build + preview
```

## Where the list comes from

**[github.com/spacedevin/spacedevin](https://github.com/spacedevin/spacedevin)** — the profile
README. GitHub caps pinned repos at six; that list has no cap and keeps its own order. `build.tish`
parses its `- [label](https://github.com/owner/repo)` lines and bakes the result into static HTML, so
the page ships as one file with no client-side JavaScript and nothing to rate-limit.

**Only selection and order are manual.** Descriptions, homepages and languages are read from the
repos themselves at build time, so the list doesn't go stale — unless a line adds `— an override`,
which wins.

Editing that README dispatches a rebuild here (`repository_dispatch: profile-updated`). Deploys also
run weekly, which is what picks up a description edited in one of the listed repos.

A line pointing at a repo that is missing or private is skipped with a warning rather than failing
the build.

A token is required even though the data is public: GitHub's GraphQL API rejects anonymous requests.
CI uses the default `GITHUB_TOKEN`.

## Layout

| Path | |
|------|--|
| `src/build.tish` | fetch → HTML. The whole generator |
| `src/style.css` | the theme, copied verbatim into `dist/` |
| `.github/workflows/pages.yml` | build + deploy to Pages |

Design follows [payitforwardlicense.com](https://payitforwardlicense.com/) — Catppuccin, monospace,
terminal prompt markers.

## Note

This is a **user page**, so it serves the root. It does not affect project pages, which keep serving
under their own path — [spacedevin.github.io/deck/](https://spacedevin.github.io/deck/) and friends
are unchanged.
