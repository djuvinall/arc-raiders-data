# Data audit, 2026-11-03

A check of the whole dataset against every source, run on fresh snapshots of MetaForge,
the ARC Raiders Wiki, RaidTheory and Steam taken on 2026-11-03. The import was rerun
from those snapshots. This file records what was checked, what was fixed, and what only
a look in game can settle.

## What was checked

- Quests: names, traders, XP, rewards, prerequisites and loops, across MetaForge, the
  wiki and RaidTheory.
- Items: value, weight and rarity against the wiki; missing fields; duplicate names.
- Recipes against the wiki's crafting table.
- Every reference the import makes (quest rewards, project costs, recycle outputs).
- ARCs, traders, projects and weapon mods for missing data.

## Result

The data matches its sources. A rebuild from the new snapshots changed 4 things against
the previous data. All 123 quests MetaForge lists are present. The graph has no broken
prerequisites and no loops. Four quests have no prerequisite: Picking Up The Pieces,
Nothing Ventured, The First Nomads and When Everything Falls.

## Fixed in this pass

- **Quest rewards.** MetaForge leaves some rewards out, so the wiki's reward list is now
  merged in wherever it names a known item. This added 14 missing rewards, such as the
  Rascal on Boarding the Frigate, the Aphelion on Furtive Meetings and the charms and
  attachments on several Tian Wen quests. Earlier, wiki rewards were only used when
  MetaForge listed none.
- **Name matching.** The sources punctuate names differently ("Combat Mk 3 Aggressive"
  vs "Combat Mk. 3 (Aggressive)"). The import now matches names regardless of
  punctuation, which resolved several references that had been dropped.
- **Tian Wen.** MetaForge spells the trader "TianWen". It is now "Tian Wen" everywhere,
  as in the game.
- **Duplicate cosmetics.** MetaForge lists a few cosmetics twice, once under a
  placeholder id. Those duplicates are removed.
- **ARC abilities.** Some carried leftover wiki markup ("Cluster Bombs<br>"). They are
  now clean lists, e.g. "Gas Grenades, Flashbangs, Energy Shield, Summons ARC".

## Sources disagree: check in game

The import keeps MetaForge's value in each of these. If the game shows otherwise, add a
correction to an overlay.

| What | MetaForge | Wiki |
|---|---|---|
| Quest name | When Everything Falls | Where Everything Falls |
| Boarding the Frigate, trader | Shani | Celeste |
| Clean Hands, Taking Stock, A Path Not Yours, Divided: trader | Cassio | Nomadic Envoys |
| In My Image, reward | Looting Mk. 2 ×1 | ×2 |
| Bronze Statuette, weight | 6 kg | 0.5 kg |
| Burned ARC Circuitry, weight | 1 kg | 0.25 kg |
| Extended Light Mag I, weight | 0.25 kg | 0.5 kg |
| Muzzle Brake III, weight | 0.75 kg | 0.5 kg |
| Power Rod, weight | 2 kg | 1 kg |
| Riven Tides Hotel Keycards No. 102 and 205, weight | 0.5 kg | 0.25 kg |
| Rope, value | 300 | 500 |

Colorful Shoes also differ, because MetaForge has three rarities (rare, epic,
legendary) and the wiki has one.

## Gaps

- **ARCs with no health on any source:** ARC Assessor, Bully, ARC Frigate, Matriarch,
  Queen, Skulker, ARC Turbine. Their bestiary pages show no threat tier until a source
  lists health.
- **Projects with no stage data:** Breaking New Ground, Forgotten Relics, Last Resort,
  Miniature Voyages, Phantom Targets (Event), Shared Watch. Neither source lists their
  costs yet.
- **Items only the wiki has**, which may have left the game: Anvil Splitter, Piano,
  Riven Tides Crane Secure Storage Keycard. They have no icon.
- **References that match no item:** cosmetic costs on Avian Alarm, Converging Paths and
  Flickering Flames (Bird House, Fist In Air, Gel Patches, the Colorful Shoes variants,
  Red Black, Merit, Snowglobe Charm), and "Gun Parts" as the Bantam's recycle output,
  which may be Light, Medium or Heavy Gun Parts in game.
- **Missing fields on gameplay items:** most are blueprints, keys and currencies, which
  have no weight or value in game. Value is missing on 34, rarity on 49 and weight on
  129.
- **Wiki rewards still unmatched** (26): outfit colours, emotes and currencies. They aren't
  items in this dataset, so they're skipped on purpose.

## Source freshness

RaidTheory has not updated since 2026-08-19, before the 2.0 update. It lists 100 quests
against MetaForge's 123. It is now used only to fill gaps. The wiki wins on recipes, and
5 recycle recipes come only from RaidTheory: Anvil I and IV, and Canto II, III and IV.
Rascal II's recipe needs "Rascal I" on the wiki and "Rascal" here, which is the same
item under two names.

## How to repeat this

From the app repository, with this repository checked out at `dataset/`:

```
cargo run -p arc-ingest -- probe
cargo run -p arc-ingest -- sync
```

`probe` snapshots every source into `fixtures/`, and `sync` lists what would change.
