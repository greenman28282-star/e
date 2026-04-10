# Factory Optimization Guide — Ultimate Mining Tycoon

An interactive web guide for optimizing factory layouts in [Ultimate Mining Tycoon](https://www.roblox.com/games/) (UMT) on Roblox.

## Features

- **Live Calculator** — Select ore type or enter custom values; toggle prestige machines. All values update across every section in real time.
- **Stage-by-Stage Progression** — 8 stages from beginner ($8K) to full end-game ($9M+), each with flow diagrams, building tags, transport tips, and per-ore totals.
- **End-Game Product Comparison** — Ranked table comparing Power Core, Tablet, Superconductor, Engine, Laser, Amulet, Glider, Electromagnet, and Tempered Mech Parts by $/ore including gem EV and brick byproducts.
- **Machine Logic & Obsolescence** — Which machines to buy, which are traps, and when buildings become obsolete.
- **Prestige Guide** — Medal priority, "when to prestige" timing, and how prestige machines compound through the full processing chain.
- **Gem & Byproduct Optimization** — Expected value tables, prospector stacking math, concrete brick flows, and Sifter integration.
- **Full Building Reference** — Searchable table of all 45+ buildings sorted by price.

## Usage

Open `index.html` in any modern browser. No build step, no dependencies — it's a single self-contained HTML file with embedded CSS and JavaScript.

## Data Sources

All game data is sourced from the [UMT Wiki](https://umt.miraheze.org/wiki/Ultimate_Mining_Tycoon_Wiki). Key pages:
- [Machines](https://umt.miraheze.org/wiki/Machines)
- [Ores](https://umt.miraheze.org/wiki/Ores)
- [Gems](https://umt.miraheze.org/wiki/Gems)
- [Prestige](https://umt.miraheze.org/wiki/Prestige)

## Contributing

If you find incorrect values or have suggestions:
1. Check the [UMT Wiki](https://umt.miraheze.org/wiki/Ultimate_Mining_Tycoon_Wiki) for the latest data
2. Open an issue or PR with the correction
3. All building data is in the `buildings` array in the `<script>` section of `index.html`
4. Calculation logic is in the `recalc()` function
