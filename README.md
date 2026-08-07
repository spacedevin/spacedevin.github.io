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

## How the list stays current

The repo list is **not in this repo**. `build.tish` queries GitHub's GraphQL API for the profile's
pinned repositories and bakes the result into static HTML — so the page ships as one file with no
client-side JavaScript, nothing to rate-limit, and no list to keep in sync by hand.

Re-pin a repo on the GitHub profile and the next build picks it up. Deploys run on push and weekly,
so a re-pin lands without a commit.

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
