# BIRD-corpus

The semantic corpus that [governed-bi](https://github.com/Minhao-Zhang/governed-bi) retrieves at
serving time. One directory per database in
[BIRD-Obfuscation](https://github.com/Minhao-Zhang/BIRD-Obfuscation), holding the hand-authored
assets the engine grounds its SQL in:

- `<database>.yaml` — what the schema is, how its tables relate, and which of its columns are not
  dependable for analysis.
- `tables/`, `terms/`, `metrics/` — per-table notes, glossary entries for wording the schema does
  not spell out, and metric definitions.
- `joins/` — the join paths worth taking, so the engine is not left to infer them from column names.
- `few-shots/` — question-and-SQL pairs, each recording where it came from.

**The corpus lives in its own repository because the serving agent reads it and cannot write to
it.** Each evaluation run in `governed-bi` records the content hash of the corpus it served, so a
result can be traced back to the assets behind it.

The few-shots come from BIRD's gold SQL, and CI in `governed-bi` enforces that they stay on the
training side of the split: `tools/check_train_only.py`, plus
`tools/check_no_benchmark_discriminators.py` for the leakage that question wording alone would not
reveal.

## License

CC BY-SA 4.0, matching the BIRD-derived data it describes.
