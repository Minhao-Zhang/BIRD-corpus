# BIRD-corpus

The semantic corpus that [governed-bi](https://github.com/Minhao-Zhang/governed-bi) retrieves at
serving time. One directory per database in
[BIRD-Obfuscation](https://github.com/Minhao-Zhang/BIRD-Obfuscation), holding the hand-authored
assets the engine grounds its SQL in:

Each directory holds one file, `<database>/<database>.yaml`: an
[Apache Ossie](https://github.com/apache/ossie) `0.2.0.dev0` model of that database. Its datasets
and fields are the tables and columns, its relationships the joins Ossie can express, and its
metrics the metric definitions. What Ossie has no field for (glossary terms, few-shot
question-and-SQL pairs, other joins, which columns are not dependable, and where each definition
came from) is in the JSON of a `GOVERNED_BI` extension. The placement is in governed-bi's
`docs/corpus-format.md`.

So that each diff hunk names the dataset or field it is in, run once per clone:

```bash
git config diff.ossie.xfuncname "^(- name: .*|  - name: .*)$"
```

**The corpus lives in its own repository because the serving agent reads it and cannot write to
it.** Each evaluation run in `governed-bi` records the content hash of the corpus it served, so a
result can be traced back to the assets behind it.

The few-shots come from BIRD's gold SQL, and CI in `governed-bi` enforces that they stay on the
training side of the split: `tools/check_train_only.py`, plus
`tools/check_no_benchmark_discriminators.py` for the leakage that question wording alone would not
reveal.

## License

CC BY-SA 4.0, matching the BIRD-derived data it describes.
