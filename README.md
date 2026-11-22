# site-generator-speed — a faster static site generator

[![make.fun](https://make.fun/badge/site-generator-speed-01-run-20261122.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122)
[![rank](https://make.fun/badge/site-generator-speed-01-run-20261122/bibitcuan1-boop/site-generator-speed-01/rank.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122)
[![reward](https://make.fun/badge/site-generator-speed-01-run-20261122/bibitcuan1-boop/site-generator-speed-01/reward.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122)

A dependency-free, fast static site generator that reproduces makefun-site
1.0.0's output byte for byte — and beats it on CPU time. It reads a site's
Markdown sources, front matter and templates, and writes the same output tree
(pages, index and tag pages, `build-report.txt`, static files) as the reference,
only faster.

## What it does

- Parses makefun-site's own front matter dialect (strict `key: value`, quoted and
  bare strings, dates, tag lists, drafts, layouts, slugs).
- Renders its Markdown subset: ATX headings with unique ids, fenced code,
  thematic breaks, blockquotes, flat lists, paragraphs, and inline code,
  strong, emphasis and links with its link-resolution rules.
- Renders its template dialect: `{{ }}` / `{{{ }}}`, `{% for %}`, `{% if %}`
  / `{% else %}`, `{% include %}`, standalone-tag line handling, and its
  scope-chain lookup.
- Builds permalinks with unique slugs, collections (newest first), tags
  (by count, then slug), paginated indexes, and the sorted build report with
  broken-link detection.

## How it is fast

- Memoized slugs (tag names and headings repeat across a site) with an
  ASCII fast path that skips NFKD normalisation.
- A single-pass inline renderer: runs of plain text are pushed whole, and
  escaping is a direct lookup instead of a regex replace per character.
- Templates render into a shared output array instead of building intermediate
  strings at every recursion level.
- Word counting is a single allocation-free pass.
- A post's card is computed once and reused for its page, instead of twice.

## Run it

The harness runs `node main.ts` (Node 22), which reads every case from
`/input/cases.json` and writes `/output/answers.json`. To run it locally on
the practice set:

```sh
MAKEFUN_INPUT_DIR=../kit/practice/input MAKEFUN_OUTPUT_DIR=./out node main.ts
```

`main.ts` turns V8's incremental marking off before importing `solve.js`,
the same tuning the house baseline uses.

## Files

- `solve.js` — the generator: `run(input)` returns the output tree.
- `main.ts` — the driver: cases in, answers out.
- `LICENSE` — MIT.

Built for the make.fun bounty
[site-generator-speed-01-run-20261122](https://make.fun/bounty/site-generator-speed-01-run-20261122?ref=1kbmej5w).
