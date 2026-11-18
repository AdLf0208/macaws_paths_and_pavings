# Macaw's Paths and Pavings — a Horizon port

**Macaw's Paths and Pavings** is a Minecraft mod. This repository is a **port of it to [Horizon](https://github.com/horizon-rust/horizon)**, the Rust engine for Minecraft Java 26.3. It carries the complete source of the port plus the content files it ships, and installs as a single `.hmod` package.

Upstream: [sketchmacaw/Paths-Pavings](https://github.com/sketchmacaw/Paths-Pavings) — licence **MIT**.

| | |
|---|---|
| Upstream mod | Macaw's Paths and Pavings ([source](https://github.com/sketchmacaw/Paths-Pavings)) |
| Ported to | Horizon 0b14f8a, Minecraft Java 26.3, mod API `0.2` |
| Package | `mcwpaths.hmod` (install it with the Horizon launcher) |
| Licence | **MIT** — the upstream mod's licence; see [Licence and credit](#licence-and-credit) |

## What this port implements

### What works

- All 315 blocks: 130 thin path/paving blocks, 24 facing paths, 52 engraved pavings, 5
  flattened soil paths, 52 slabs and 52 stairs — 4,812 block states, each with the mod's
  own model and texture.
- The thin paving shape (`Block.box(0, 0.01, 0, 16, 0.99, 16)` in block units, i.e.
  0.000625–0.061875 normalized) and the engraved/flattened 15/16 shape are declared,
  not inherited from the template, so the collision matches upstream.
- 315 recipes (304 stonecutting, 11 shapeless), 315 loot tables, 11 tags, 25
  advancements and 7 language files.
- A creative tab (`itemGroup.mcwpaths`) listing all 315 blocks.

## Not yet ported

- **Engraving and flattening are logic-tier behaviour and are not carried.**
- **Wooden path burn time.** Upstream's `FuelItemBlock` burns 50 ticks; a
- **Tooltips.** `mcwpaths.engraved.desc` / `mcwpaths.flattened.desc` are
- **`forceSolidOn()`** (the 154 `PathBlock`/`FacingPathBlock` blocks) has no
- **Facing-path placement rule.** The content block takes its carved-pumpkin
- **Numbers come from the 26.3 tables, not a NeoForge oracle.** There is no
- **The jar's blockstates omit `waterlogged`** for slabs/stairs (their models
- **`pale_oak_planks_path`** exists in the upstream repository (1.21.8) but

## Installing

1. Install the [Horizon launcher](https://github.com/horizon-rust/horizon/releases) (0.2.70 or newer).
2. Add `mcwpaths.hmod` to the instance's mods.
3. The package is pinned to a specific Horizon build; the launcher checks that pin.

## Licence and credit

Macaw's Paths and Pavings was created by its upstream authors; the code, models and assets in the original mod
remain theirs and are used here under **MIT**. The Rust in this repository is a clean-room
rewrite for Horizon, not a translation of the original source.

- `LICENSE` — the upstream licence text.
- `LICENSE-NOTICE.md` — what this port carries and where it came from.
- The upstream project: https://github.com/sketchmacaw/Paths-Pavings

## Technical details

```
horizon-mod.toml   package manifest (id, version, api, [native] pin)
Cargo.toml         the crate; depends on `horizon-mod-api` from the fetched SDK
src/               lib.rs (the one `impl Hooks`), runtime.rs (pure logic + tests), glue
assets/ data/      the content the package ships
tools/             content generators and the e2e scenario
```

- Build: `cargo horizon-mod build` (packages; it does not compile against the game).
- Test: `cargo test`, `cargo clippy --all-targets -- -D warnings`.
- In-game: `ports/tools/launcher-game.sh run mcwpaths` in the campaign tree, or install the
  `.hmod` and play; the harness scenario is `tools/e2e.sh`.

This repository is generated from the campaign's port at `ports/mcwpaths/`; the port's own
`notes.md` is the full record (decisions, traps, evidence).
