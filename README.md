# BIRD-corpus

The semantic layer (corpus) for the **BIRD-Obfuscation** benchmark of the
[governed-bi](https://github.com/Minhao-Zhang/governed-bi) engine. This repo holds
data, not code: the assets the engine serves from.

Kept as its own repository per governed-bi **decision D13** (semantic layer as its
own repo). The engine points at a local checkout through the
`GOVERNED_BI_CORPUS_DIR` environment variable; a relative value resolves against the
engine's repo root, so a sibling checkout is reachable as:

```
GOVERNED_BI_CORPUS_DIR=../BIRD-corpus
```

## What is in here, as of 2026-08-07

The gold semantic layer generated 2026-08-04 from governed-bi `v2 @ 9b02c95`.
`GOLD_LAYER_MANIFEST.json` is its provenance and is the file to read first:

| | |
|---|---|
| Schemas | 57 (the BIRD-Obfuscation databases with an evaluated build) |
| Assets | 13 975 declared / **13 981 loaded**, 0 validation problems |
| Scope | **TRAIN ONLY** — `test_final.jsonl` held out, so the eval arm is fair |
| Structural grounding | live introspection of the rename+decoy BIRD Postgres |
| Decoy columns marked suspect | 2 282 |

**This replaced a different generation.** Until 2026-08-07 this repo held 69 schemas
of bare-minimum facts (`tables/` only, 731 files). That tree is not gone — it is the
commit before this one — but it is not what the engine serves and its numbers are not
comparable to this one's.

## Layout

One subtree per database, matching the loader's asset-type directories:

```
GOLD_LAYER_MANIFEST.json        provenance for the whole tree
<db_id>/
  <db_id>.yaml                  the namespace asset
  tables/   joins/   metrics/   terms/   few-shots/
```

`<db_id>` is a BIRD-Obfuscation database (e.g. `beer_factory`). Typed YAML assets, in
the schema defined by governed-bi's `corpus` package.

Earlier revisions of this file also listed `rules/`, `negatives/` and `skills/`. No
tree here has ever had them: `skill` was retired in favour of notes on assets, and
the other two are asset *types* that live inside the directories above when present.

## Git is the source of truth

Every store the engine builds (graph / vector / BM25) is a rebuildable projection of
this repo. For the SME-growth benchmark (**D14**), this repo's git history is also the
checkpoint record: each growth round is a commit, and a benchmark point estimate is
pinned to the commit SHA that produced it.

That is the whole reason this repo exists, and until 2026-08-07 the engine could not
honour it — the corpus it served was an untracked directory, so
`corpus_content_hash` named bytes that no reader could obtain. A number is now
resolvable to a commit here.

**Do not put a generated store in this tree.** `corpus_content_hash` hashes every file
in the subtrees it is given, so anything that lands beside the assets becomes part of
the corpus's identity. The engine ignores a VCS's own bookkeeping (`.git/`,
`__pycache__/`) and nothing else — a `README.md` at the root *does* enter the digest
when no schema manifest is passed, which is intended: this file describes the corpus,
so changing it changes what the corpus claims to be.

Empty is valid: with no `<db>/` subtree the engine loads an empty corpus, which is the
`facts-only` / cold starting point before any curation.
