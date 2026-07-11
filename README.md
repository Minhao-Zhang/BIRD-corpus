# BIRD-corpus

The semantic layer (corpus) for the **BIRD-Obfuscation** benchmark of the
[governed-bi](https://github.com/Minhao-Zhang/governed-bi) engine. This repo holds
data, not code: the curator-built, human-audited assets the engine serves from.

Kept as its own repository per governed-bi **decision D13** (semantic layer as its
own repo). The engine points at a local checkout through the `GOVERNED_BI_CORPUS`
environment variable; a relative value resolves against the engine's repo root, so
a sibling checkout is reachable as:

```
GOVERNED_BI_CORPUS=../BIRD-corpus
```

## Layout

One subtree per database, matching the loader's asset-type directories:

```
<db_id>/
  tables/      joins/      metrics/     terms/
  rules/       few-shots/  negatives/   skills/
```

`<db_id>` is a BIRD-Obfuscation database (e.g. `beer_factory`). YAML typed assets
plus Markdown skills, in the schema defined by governed-bi's `corpus` package.

## Git is the source of truth

Every store the engine builds (graph / vector / BM25) is a rebuildable projection
of this repo. For the SME-growth benchmark (**D14**), this repo's git history is
also the checkpoint record: each growth round is a commit, and a benchmark point
estimate is pinned to the commit SHA that produced it.

Empty is valid: with no `<db>/` subtree the engine loads an empty corpus, which is
the `facts-only` / cold starting point before any curation.
