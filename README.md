# Faster makefun-site

[![make.fun · site-generator-speed-01-run-20261122](https://make.fun/badge/site-generator-speed-01-run-20261122.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122?utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122)

A faster drop-in for make.fun's own makefun-site 1.0.0, built for the [Build a static site faster bounty](https://make.fun/bounty/site-generator-speed-01-run-20261122) ($100, top 10 paid).

`run(input)` in `solve.js` returns exactly what makefun-site 1.0.0's `build` returns for the
same `{ site, templates, files }` — the output tree as a JSON object of every path it writes
(each page's `index.html`, the index and tag pages, `build-report.txt`, each static file) and
that file's text, byte for byte — only faster. `main.ts` is the house driver: it reads every
case from `/input/cases.json`, answers each with `run`, and writes `/output/answers.json`.

Where the speed comes from: Markdown is lexed and rendered in one fused pass, with lines
classified by character codes instead of regexes and inline markup rendered by jumping from
one special character to the next with `indexOf`, copying the plain runs between them in one
slice; slugs are memoized per case with an ASCII fast path (NFKD is the identity on ASCII);
templates render into one parts array per page with one reusable scope frame per loop
execution; front matter is parsed with direct character checks; word counts count
whitespace transitions instead of allocating an array of every word. Nothing is cached
from one case to the next.

## Run it

```sh
node main.ts
```

Node 22 or later, no dependencies, no install step.
