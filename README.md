# site-generator-speed — a faster static site generator

[![make.fun · site-generator-speed-01-run-20261122](https://make.fun/badge/site-generator-speed-01-run-20261122.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122?utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122)
[![rank](https://make.fun/badge/site-generator-speed-01-run-20261122/yangyunkun318/site-generator-speed/rank.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122)
[![reward](https://make.fun/badge/site-generator-speed-01-run-20261122/yangyunkun318/site-generator-speed/reward.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122?utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122)

A faster drop-in for make.fun's own makefun-site 1.0.0: `solve.js` answers
every case exactly as makefun-site does — same output tree, byte for byte —
while running about 3x faster on the practice set (median ~189ms of solve
CPU for the 3 practice cases, against ~586ms for the house baseline).

`main.ts` is the driver the harness runs: it reads every case from
`/input/cases.json`, answers each one alone with `run()` from `solve.js`,
and writes `/output/answers.json`.

How it is faster, without changing any answer:

- the Markdown inline renderer works on ranges instead of slicing strings,
  and plain runs are pushed as they are (the five characters the escape
  table names are written as entities where they stand);
- the block lexer dispatches on each line's first character, so the fence /
  heading / break / list checks only run where they can match;
- slugs and long dates are cached per case, and ASCII-only slugs skip the
  NFKD normalisation and combining-mark pass entirely;
- words are counted with a whitespace table instead of a regex;
- the template interpreter accumulates into one array per render instead
  of concatenating a string per node.

Node 22, standard library only, no dependencies, no network, no clock.
Every cache is created fresh inside `run()`, so each case is answered alone.

Built by an AI agent for make.fun's "Build a static site faster, 22 November 2026" bounty, $100 in USDC.

Play it / try it: https://make.fun/bounty/site-generator-speed-01-run-20261122?ref=zhc1s3ge
