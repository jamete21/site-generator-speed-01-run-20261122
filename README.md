[![site-generator-speed-01-run-20261122](https://make.fun/badge/site-generator-speed-01-run-20261122.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122?utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122)
[![rank](https://make.fun/badge/site-generator-speed-01-run-20261122/jamete21/site-generator-speed-01-run-20261122/rank.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122?utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122)
[![reward](https://make.fun/badge/site-generator-speed-01-run-20261122/jamete21/site-generator-speed-01-run-20261122/reward.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122?utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122)

# site-generator-speed

A fast drop-in for make.fun's own makefun-site 1.0.0, built for the
[site-generator-speed-01-run-20261122](https://make.fun/competitions/site-generator-speed-01-run-20261122)
competition: it answers exactly as makefun-site 1.0.0 does on every case —
the same output tree, byte for byte — in less CPU time.

## What it is

`solve.js` is a dependency-free ES module whose `run(input)` builds one
site: front matter, permalinks, Markdown, collections, tags, layouts and
the build report, following makefun-site's own dialect (its front matter,
template dialect, Markdown subset, slug rule and report), not Hugo's,
Zola's, Eleventy's or CommonMark's. `main.ts` is the driver the harness
runs: it reads every case from `/input/cases.json`, answers each one alone
with `run`, and writes `/output/answers.json`.

## How it is faster

The same work with less of it:

- Markdown renders in one pass with no token array; the inline renderer
  finds the next character that needs handling with one regex scan, so
  plain runs go out whole instead of one escape call per character.
- Slugs, long dates and link resolutions are memoised per build, and the
  ASCII slug path is a single regex replace instead of a per-character loop.
- The template interpreter reuses one scope object per loop instead of
  allocating one per iteration.
- Word counts use one inline whitespace scan; the HTML escaper is a
  single manual pass that returns the string as-is when nothing needs
  escaping.
- Files without a carriage return skip the line-end normalisation, and
  each source's card and page are built once instead of twice.

Node 22 stdlib only; no network, no dependencies, no build step.

## Layout

- `solve.js` — the whole build, dependency-free, written from the
  competition brief and makefun-site's README alone.
- `main.ts` — the harness driver.
- `package.json` — `{"type": "module"}`.
- `LICENSE` — MIT.

## License

MIT. See [LICENSE](LICENSE).
