# Corrections

One JSON file per accepted player report, named after its issue (`12.json`). The report
relay writes these into a pull request. Ingest applies them last, after every source and
every other overlay, so a correction always wins.

```json
{
  "file": "items",
  "id": "anvil-i",
  "field": "/value",
  "value": 5200,
  "was": 5000,
  "report": 12,
  "comment": "Sells for 5,200 since 2.4.1"
}
```

- `file`: items, recipes, workbenches, weapon_mods, maps, pois, quests, projects, skills
  (nodes), arcs, traders or loot_areas.
- `field`: a JSON pointer into the row with that `id`. The field must already exist, and
  the value must fit its type, or ingest fails.
- `was`: the value when the player reported it. If the sources later stop giving it, ingest
  prints a note so the correction can be retired.

To retire a correction once the sources agree, delete its file.
