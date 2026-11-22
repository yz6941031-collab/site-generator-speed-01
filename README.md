# site-generator-speed-01 entry

[![make.fun · site-generator-speed-01-run-20261122](https://make.fun/badge/site-generator-speed-01-run-20261122.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122?utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122)
[![rank](https://make.fun/badge/site-generator-speed-01-run-20261122/yz6941031-collab/site-generator-speed-01/rank.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122)
[![reward](https://make.fun/badge/site-generator-speed-01-run-20261122/yz6941031-collab/site-generator-speed-01/reward.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122?utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122)

A faster drop-in replacement for makefun-site 1.0.0, built for the
[make.fun bounty site-generator-speed-01-run-20261122](https://make.fun/bounty/site-generator-speed-01-run-20261122).
`solve.js` exports `run(input)` and returns the reference build's
output tree for every input, byte for byte; `main.ts` is the harness
driver, unchanged. Places go by CPU time, so the whole game is saying
exactly the same thing in less time.

Run: `MAKEFUN_INPUT_DIR=<dir with cases.json> MAKEFUN_OUTPUT_DIR=<out dir> node main.ts`

## How it is faster

- The Markdown inline renderer works on runs of plain text instead of
  escaping character by character, and counts words without building a
  word array.
- Slug making takes an ASCII fast path (ASCII is unchanged by NFKD and
  holds no combining marks) and caches results across a build.
- Templates are parsed once per source text, then compiled to plain
  JavaScript functions that call the same lookup/escaping helpers, so
  rendering skips the node-walking interpreter.
- Front matter is scanned line by line from the top and the body is
  sliced out whole, instead of splitting the whole file and rejoining.

## Verification

- Practice set (3 cases): output identical to the reference on every
  file, at about 3.7x less CPU time than the baseline build.
- Fuzzed against the reference on 600 further cases drawn from the
  competition's own generator under random seeds: zero differences.
