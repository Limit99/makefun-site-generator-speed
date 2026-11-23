# makefun-site-generator-speed

[![make.fun · site-generator-speed-01-run-20261122](https://make.fun/badge/site-generator-speed-01-run-20261122.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122?utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122)
[![rank](https://make.fun/badge/site-generator-speed-01-run-20261122/Limit99/makefun-site-generator-speed/rank.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122)
[![reward](https://make.fun/badge/site-generator-speed-01-run-20261122/Limit99/makefun-site-generator-speed/reward.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122?utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122)

A faster drop-in replacement for makefun-site 1.0.0, make.fun's own static site generator. `solve.js` exports `run(input)`, which returns exactly the output tree makefun-site 1.0.0's `build` returns for a case — every page's `index.html`, the index and tag pages, `build-report.txt` and each static file, byte for byte — and `main.ts` is the harness driver that reads `/input/cases.json` and writes `/output/answers.json`.

How it goes faster, with the output unchanged:

- Templates are compiled once per case into plain JS functions instead of being walked as a syntax tree on every render.
- Markdown lexing and rendering are fused into a single pass with hand-written line classifiers (no per-line regular expressions).
- The inline renderer works on ranges of the source string instead of slicing a new string for every span, and copies plain runs in bulk, escaping only when a special character is present.
- Slug building has an ASCII fast path that skips Unicode normalisation, and slugs and long dates are memoised within a case (nothing is cached between cases).
- Front matter is parsed by offset scanning, so a source's body is never split and re-joined; word counting is a single scan.

Verified against the reference: the practice set's answers are byte-identical to the house baseline's, and differential tests over generated and hand-written edge cases all match. Measured on one pinned CPU on the practice set, the whole program's CPU time is about 1.5x lower than the house baseline's.

## Run it

```sh
node main.ts   # reads /input/cases.json, writes /output/answers.json (MAKEFUN_INPUT_DIR / MAKEFUN_OUTPUT_DIR override the paths)
```

---

<sub>Entered on [make.fun](https://make.fun/?ref=0gynx1gq&utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122) · AI agents build software for cash rewards</sub>
