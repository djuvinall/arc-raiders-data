# ARC Raiders Data

The open dataset behind [ARC Raiders Companion](https://github.com/djuvinall/arc-raiders-companion):
items, recipes, quests, projects, workbenches, skills, ARCs, traders, maps and map markers
for ARC Raiders, as plain JSON.

Unofficial. ARC Raiders is a trademark of Embark Studios. This repository holds no game assets.

## Layout

| Path | What it is |
|---|---|
| `data/*.json` | The dataset. The app embeds it at build time and downloads newer copies from here at runtime. |
| `data/schema/` | JSON Schema for each file. |
| `fixtures/<source>/<date>.json` | Raw snapshots of each source, which the import builds `data/` from. |
| `overlays/` | Hand-entered corrections and additions, and `precedence.toml`, which says which source wins for each field. |
| `DATA_AUDIT.md` | The latest cross-source check, with what to verify in game. |
| `DATA_SOURCES.md` | Where each field comes from. |

## Updating

The import tool lives in the app repository, which carries this one as a submodule at `dataset/`:

```
git clone --recurse-submodules https://github.com/djuvinall/arc-raiders-companion
cd arc-raiders-companion
cargo run -p arc-ingest -- probe          # snapshot every source into dataset/fixtures
cargo run -p arc-ingest -- sync           # show what would change
cargo run -p arc-ingest -- sync --write   # write dataset/data
cargo test -p arc-core -p arc-ingest      # validate
```

Commit and push from `dataset/`. The **refresh-data** workflow here does the same on demand.
Corrections belong in `overlays/`, not in `data/`, so the next import doesn't overwrite them.

`data/version.json`'s `dataset_version` follows the data: `stamp.json` keeps a hash of the
last written data, and whenever `sync --write` gives different data, for any reason, the
version moves to that day (`2026-11-09`, then `2026-11-09.02` for a second change that day).
Apps download data whose version sorts after theirs, so commit `stamp.json` with `data/`.
CI fails when the data no longer matches the stamp.

## License

The data is licensed under [CC BY-SA 4.0](LICENSE). Credit the sources in
[ATTRIBUTION.md](data/ATTRIBUTION.md) when you reuse it, and share changes under the same terms.
MetaForge's terms also ask for a link back, and for contact before any monetized use.
