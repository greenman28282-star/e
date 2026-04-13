# UMT Factory Guide — Ultimate Mining Tycoon

An interactive web guide for factory layouts in [Ultimate Mining Tycoon](https://www.roblox.com/games/) (UMT) on Roblox.

## Features

- **Live Ore Calculator** — Select ore type or enter custom values; toggle prestige machines. All values update live across every stage.
- **Stage-by-Stage Factory Layouts** — 10 stages from beginner ($6K) to full end-game ($11.5M+), each with flow diagrams, interactive SVG factory layouts, building tags, and per-ore totals.
- **Prestige Integration** — Ore Upgrader, Philosopher's Stone, Nano Sifter, and Duplicator toggles recalculate all stage values and factory outputs in real time.
- **End-Game Power Core** — Full factory layout and recipe breakdown for the best $/ore product in the game.
- **Product Comparison Table** — Ranked table comparing all products by $/ore including QA, gem EV, and brick byproducts.

## Supplementary Docs

Reference guides are in the `docs/` folder:
- `building-reference.html` — Searchable table of all 45+ buildings sorted by price
- `machine-logic.html` — Machine interactions, tagging rules, and obsolescence
- `gem-strategy.html` — Prospector stacking math and gem optimization
- `byproduct-guide.html` — Stone/brick/concrete processing flows
- `transport-guide.html` — Conveyor, splitter, and elevator layouts
- `pro-tips.html` — Factory design tips and common mistakes

## Usage

Open `index.html` in any modern browser. No build step, no dependencies — single self-contained HTML file with embedded CSS and JavaScript.

## Data Sources

All game data sourced from the [UMT Wiki](https://umt.miraheze.org/wiki/Ultimate_Mining_Tycoon_Wiki). Calculation logic is in the `recalc()` function in the `<script>` section of `index.html`.

## Contributing

If you find incorrect values:
1. Check the [UMT Wiki](https://umt.miraheze.org/wiki/Ultimate_Mining_Tycoon_Wiki) for latest data
2. Open an issue or PR with the correction
3. Calculation logic is in `recalc()` in `index.html`
