# Comprehensive Guide Review — UMT Factory Optimization Guide

> **Date:** April 12, 2026  
> **Scope:** Data accuracy, calculation correctness, structural issues  
> **Source of Truth:** [UMT Wiki (umt.miraheze.org)](https://umt.miraheze.org/wiki/Ultimate_Mining_Tycoon_Wiki) as of April 2026 (v0.5.10)

---

## Table of Contents

1. [Critical Findings Summary](#1-critical-findings-summary)
2. [Building Price Discrepancies](#2-building-price-discrepancies)
3. [Machine Multiplier & Effect Errors](#3-machine-multiplier--effect-errors)
4. [Material Base Value Errors](#4-material-base-value-errors)
5. [Stone Byproduct Rate Error](#5-stone-byproduct-rate-error)
6. [Recipe / Input Errors](#6-recipe--input-errors)
7. [Gem & Prospector Data Errors](#7-gem--prospector-data-errors)
8. [Calculation Logic Errors in recalc()](#8-calculation-logic-errors-in-recalc)
9. [Stage Guide Errors & Missing Content](#9-stage-guide-errors--missing-content)
10. [End-Game Product Calculation Errors](#10-end-game-product-calculation-errors)
11. [Prestige System Issues](#11-prestige-system-issues)
12. [Structural / Bloat Issues](#12-structural--bloat-issues)
13. [Recommended Restructuring Plan](#13-recommended-restructuring-plan)

---

## 1. Critical Findings Summary

The guide contains **dozens of data errors** ranging from minor price discrepancies to **fundamental calculation-breaking mistakes**. The most impactful issues are:

| # | Issue | Impact |
|---|-------|--------|
| 1 | **Stone byproduct rate: guide assumes 1 stone per ore, wiki says 1 per 2 ores** | Breaks ALL byproduct/gem/brick calculations — halves all stone-derived income |
| 2 | **Dust base value: guide uses $25, wiki says $1** | All brick/cement/stone chain values are wrong |
| 3 | **Concrete brick: guide uses $300, wiki says $100** | Stage 5+ concrete brick values are ~3× inflated |
| 4 | **Power Core multiplier: guide uses ×3.5, wiki says ×2.5** | End-game Power Core value is ~40% inflated |
| 5 | **Engine Factory multiplier: guide uses ×1.2, wiki says ×2.5** | Engine values are drastically UNDERVALUED |
| 6 | **Laser Maker multiplier: guide uses ×2.75, wiki says ×1.75** | Laser values are ~57% inflated |
| 7 | **Bolt Machine: guide uses +$15, wiki says +$5** | All casing/frame calculations are inflated |
| 8 | **Brick base value: guide uses $30, wiki says $25** | All fired brick values are wrong |
| 9 | **Cement value: guide uses $50, wiki says $30** | Cement mixer and concrete chain values wrong |
| 10 | **Ring Maker recipe: guide says Dust+Coil, wiki says Gem+Coil** | Entire Amulet/Glider chain uses wrong inputs |
| 11 | **~20 building prices are wrong** | Stage cumulative costs are all wrong |
| 12 | **Prestige toggles not incorporated into stage calculations** | Stages don't update with prestige |

These errors compound through the calculation chain, meaning **virtually every calculated value in the guide is incorrect**.

---

## 2. Building Price Discrepancies

Prices compared: **Guide value** vs **Wiki value** (from umt.miraheze.org, confirmed April 2026).

| Building | Guide Price | Wiki Price | Difference | Source |
|----------|-----------|-----------|------------|--------|
| **Polisher** | $850 | **$250** | ❌ +$600 | [Wiki](https://umt.miraheze.org/wiki/Polisher) |
| **Ore Cleaner** | $1,000 | **$80** | ❌ +$920 | [Wiki](https://umt.miraheze.org/wiki/Ore_Cleaner) |
| **Ore Smelter** | $1,500 | **$380** | ❌ +$1,120 | [Wiki](https://umt.miraheze.org/wiki/Ore_Smelter) |
| Crusher | $1,750 | $1,750 | ✅ | |
| Coiler | $1,750 | $1,750 | ✅ | [Wiki](https://umt.miraheze.org/wiki/Coiler) |
| Topaz Prospector | $2,000 | $2,000 | ✅ | [Wiki](https://umt.miraheze.org/wiki/Prospectors) |
| Brick Mold | $2,000 | **$2,500** | ❌ -$500 | [Wiki](https://umt.miraheze.org/wiki/Brick_Mold) |
| Bolt Machine | $3,000 | **$2,800** | ❌ +$200 | |
| **Plate Stamper** | $3,500 | **$3,000** | ❌ +$500 | [Wiki](https://umt.miraheze.org/wiki/Plate_Stamper) |
| Sifter | $4,000 | $4,000 | ✅ | [Wiki](https://umt.miraheze.org/wiki/Sifter) |
| Pipe Maker | $4,000 | $4,000 | ✅ | [Wiki](https://umt.miraheze.org/wiki/Pipe_Maker) |
| Kiln | $4,750 | $4,750 | ✅ | [Wiki](https://umt.miraheze.org/wiki/Kiln) |
| **Emerald Prospector** | $6,000 | **$5,000** | ❌ +$1,000 | [Wiki](https://umt.miraheze.org/wiki/Prospectors) |
| **Mechanical Parts Maker** | $6,000 | **$8,000** | ❌ -$2,000 | [Wiki](https://umt.miraheze.org/wiki/Mechanical_Parts_Maker) |
| **Sapphire Prospector** | $8,000 | $8,000 | ✅ | [Wiki](https://umt.miraheze.org/wiki/Prospectors) |
| **Electronic Tuner** | $8,000 | **$8,500** | ❌ -$500 | [Wiki](https://umt.miraheze.org/wiki/Electronic_Tuner) |
| Cement Mixer | $10,000 | $10,000 | ✅ | [Wiki](https://umt.miraheze.org/wiki/Cement_Mixer) |
| Frame Maker | $10,000 | $10,000 | ✅ | [Wiki](https://umt.miraheze.org/wiki/Frame_Maker) |
| **Ruby Prospector** | $10,000 | **$15,000** | ❌ -$5,000 | [Wiki](https://umt.miraheze.org/wiki/Prospectors) |
| **Ring Maker** | $10,000 | **$15,000** | ❌ -$5,000 | [Wiki](https://umt.miraheze.org/wiki/Ring_Maker) |
| Blasting Powder Chamber | $15,000 | $15,000 | ✅ (assumed) | |
| Explosives Maker | $15,000 | $15,000 | ✅ (assumed) | |
| Circuit Maker | $20,000 | $20,000 | ✅ | [Wiki](https://umt.miraheze.org/wiki/Circuit_Maker) |
| Clay Mixer | $20,000 | $20,000 | ✅ | |
| Gem Cutter | $20,000 | $20,000 | ✅ | [Wiki](https://umt.miraheze.org/wiki/Gem_Cutter) |
| Blast Furnace | $25,000 | $25,000 | ✅ | |
| Ceramic Furnace | $30,000 | $30,000 | ✅ | |
| Diamond Prospector | $30,000 | $30,000 | ✅ | [Wiki](https://umt.miraheze.org/wiki/Prospectors) |
| Tempering Forge | $50,000 | $50,000 | ✅ | [Wiki](https://umt.miraheze.org/wiki/Tempering_Forge) |
| Casing Machine | $50,000 | $50,000 | ✅ | [Wiki](https://umt.miraheze.org/wiki/Casing_Machine) |
| Filigree Cutter | $50,000 | $50,000 | ✅ | |
| Lens Cutter | $75,000 | $75,000 | ✅ | |
| Prismatic Gem Crucible | $100,000 | $100,000 | ✅ | [Wiki](https://umt.miraheze.org/wiki/Prismatic_Gem_Crucible) |
| Alloy Furnace | $100,000 | $100,000 | ✅ | |
| **Magnetic Machine** | $100,000 | **$120,000** | ❌ -$20,000 | |
| Optics Machine | $300,000 | $300,000 | ✅ | [Wiki](https://umt.miraheze.org/wiki/Optics_Machine) |
| Glider | $500,000 | $500,000 | ✅ | |
| Engine Factory | $1,000,000 | $1,000,000 | ✅ | [Wiki](https://umt.miraheze.org/wiki/Engine_Factory) |
| Superconductor Constructor | $1,000,000 | $1,000,000 | ✅ | [Wiki](https://umt.miraheze.org/wiki/Superconductor_Constructor) |
| QA Machine | $2,000,000 | $2,000,000 | ✅ | [Wiki](https://umt.miraheze.org/wiki/Quality_Assurance_Machine) |
| Amulet Maker | $2,000,000 | $2,000,000 | ✅ | [Wiki](https://umt.miraheze.org/wiki/Amulet_Maker) |
| Tablet Factory | $2,500,000 | $2,500,000 | ✅ | [Wiki](https://umt.miraheze.org/wiki/Tablet_Factory) |
| Laser Maker | $3,500,000 | $3,500,000 | ✅ | [Wiki](https://umt.miraheze.org/wiki/Laser_Maker) |
| **Power Core Assembler** | $5,000,000 | **$4,500,000** | ❌ +$500,000 | [Wiki](https://umt.miraheze.org/wiki/Power_Core_Assembler) |

**Summary:** 15 out of ~45 buildings have wrong prices. Many early-game prices are wildly off (Polisher $850 vs $250, Ore Cleaner $1,000 vs $80, Ore Smelter $1,500 vs $380). This cascades to make every stage cumulative cost wrong.

---

## 3. Machine Multiplier & Effect Errors

| Machine | Guide Value | Wiki Value | Status | Source |
|---------|------------|-----------|--------|--------|
| Ore Smelter | ×1.2 (+20%) | ×1.2 (+20%) | ✅ | [Wiki](https://umt.miraheze.org/wiki/Ore_Smelter) |
| Blast Furnace | ×0.9 (-10%) | ×0.9 (-10%) | ✅ | |
| Tempering Forge | ×2.0 | ×2.0 | ✅ | [Wiki](https://umt.miraheze.org/wiki/Tempering_Forge) |
| Kiln (on bricks) | ×1.2 | ×1.2 | ✅ | [Wiki](https://umt.miraheze.org/wiki/Kiln) |
| Gem Cutter | ×1.4 | ×1.4 | ✅ | [Wiki](https://umt.miraheze.org/wiki/Gem_Cutter) |
| Frame Maker | ×1.25 | ×1.25 | ✅ | [Wiki](https://umt.miraheze.org/wiki/Frame_Maker) |
| Casing Machine | ×1.30 | ×1.30 | ✅ | [Wiki](https://umt.miraheze.org/wiki/Casing_Machine) |
| Circuit Maker | ×2.0 | ×2.0 | ✅ | [Wiki](https://umt.miraheze.org/wiki/Circuit_Maker) |
| Alloy Furnace | ×1.2 | ×1.2 | ✅ | |
| Magnetic Machine | ×1.5 | ×1.5 | ✅ | |
| Optics Machine | ×1.25 | ×1.25 | ✅ | [Wiki](https://umt.miraheze.org/wiki/Optics_Machine) |
| Tablet Factory | ×3.0 | ×3.0 | ✅ | [Wiki](https://umt.miraheze.org/wiki/Tablet_Factory) |
| Amulet Maker | ×2.0 | ×2.0 | ✅ | [Wiki](https://umt.miraheze.org/wiki/Amulet_Maker) |
| Glider | ×1.5 | ×1.5 | ✅ | |
| **Bolt Machine** | **+$15** | **+$5** | ❌ | [Source](https://ultimate-mining-tycoon.wiki/factory) |
| **Power Core Assembler** | **×3.5** | **×2.5** | ❌ CRITICAL | [Wiki](https://umt.miraheze.org/wiki/Power_Core_Assembler) |
| **Engine Factory** | **×1.2** | **×2.5** | ❌ CRITICAL | [Wiki](https://umt.miraheze.org/wiki/Engine_Factory) |
| **Laser Maker** | **×2.75** | **×1.75** | ❌ CRITICAL | [Wiki](https://umt.miraheze.org/wiki/Laser_Maker) |
| **Ring Maker** | **×1.75** | **×1.7** | ❌ Minor | [Wiki](https://umt.miraheze.org/wiki/Ring_Maker) |
| **Prismatic Gem Crucible** | **×1.25** | **×1.15** | ❌ | [Wiki](https://umt.miraheze.org/wiki/Prismatic_Gem_Crucible) |
| Polisher | +$10 | +$10 | ✅ | |
| Ore Cleaner | +$10 | +$10 | ✅ | |
| Coiler | +$20 | +$20 | ✅ | |
| Plate Stamper | +$20 | +$20 | ✅ | |
| Pipe Maker | +$20 | +$20 | ✅ | |
| Mech Parts Maker | +$30 | +$30 | ✅ | [Wiki](https://umt.miraheze.org/wiki/Mechanical_Parts_Maker) |
| Electronic Tuner | +$50 | +$50 | ✅ | |
| Lens Cutter | +$50 | +$50 | ✅ | |

**Critical impacts:**
- Power Core ×3.5 → ×2.5 means Power Core values are **~40% too high**
- Engine ×1.2 → ×2.5 means Engines are **massively undervalued** (this may change the entire product ranking)
- Laser ×2.75 → ×1.75 means Lasers are **~57% too high**
- Bolt +$15 → +$5 means every product using bolts (Frames, Casings, all end-game products) has inflated values

---

## 4. Material Base Value Errors

| Material | Guide Value | Wiki Value | Status | Source |
|----------|-----------|-----------|--------|--------|
| **Dust (from Crusher)** | **$25** | **$1** | ❌ CRITICAL | [Wiki](https://umt.miraheze.org/wiki/Crusher), [Materials](https://umt.miraheze.org/wiki/Materials) |
| **Brick (from Brick Mold, dust input)** | **$30** | **$25** | ❌ | [Wiki](https://umt.miraheze.org/wiki/Brick_Mold) |
| **Concrete Brick (from Brick Mold, cement input)** | **$300** | **$100** | ❌ CRITICAL | [Wiki](https://umt.miraheze.org/wiki/Brick_Mold) |
| **Cement (from Cement Mixer)** | **$50** | **$30** | ❌ | [Wiki](https://umt.miraheze.org/wiki/Cement_Mixer) |
| Glass (from Kiln, dust input) | $30 | $30 | ✅ | [Wiki](https://umt.miraheze.org/wiki/Kiln) |
| Clay (from Clay Mixer) | $50 | $50 | ✅ | |
| Ceramic Casing (from Ceramic Furnace) | $150 | $150 | ✅ | |
| Blasting Powder | $52 | $52 | ✅ (assumed) | |

**Cascading impact on brick chain (using Lead ore, $30):**

| Step | Guide Calculation | Correct Calculation |
|------|------------------|-------------------|
| Dust | $25 (wrong) | $1 |
| Polished Dust | $35 (25+10) | $11 (1+10) |
| Brick (from dust) | $30 (wrong) | $25 |
| Fired Brick (kilned) | $36 (30×1.2) | $30 (25×1.2) |
| Polished Fired Brick | $46 (36+10) | $40 (30+10) |
| QA Fired Brick | $55 (46×1.2) | $48 (40×1.2) |
| Cement | $50 (wrong) | $30 |
| Concrete Brick | $300 (wrong) | $100 |
| Kilned Concrete Brick | $360 (300×1.2) | $120 (100×1.2) |
| Polished Kilned Concrete | $370 (360+10) | $130 (120+10) |
| QA Concrete Brick | $444 (370×1.2) | $156 (130×1.2) |

**The guide's concrete brick value ($370/$444) is roughly 3× the actual value ($130/$156).** This is one of the most impactful errors since the guide presents Stage 5 (Cement Mixer) as "the biggest single upgrade in the game" based on $370 concrete bricks. With the correct $130 value, concrete bricks are still an upgrade over fired bricks but far less dramatic.

---

## 5. Stone Byproduct Rate Error

**Guide assumption:** Every ore smelted produces 1 stone (1:1 ratio)  
**Wiki fact:** The Ore Smelter produces 1 stone per 2 ores smelted (1:2 ratio)

Source: [Ore Smelter Wiki](https://umt.miraheze.org/wiki/Ore_Smelter) — confirmed across multiple searches

**Note:** The Blast Furnace DOES produce 1 stone per ore, but at -10% bar value.

### Impact

This halves all stone-derived income per ore:
- Gem prospector EV per ore is **halved** (only 0.5 stones per ore, not 1)
- Concrete brick income per ore is **halved**
- Fired brick income per ore is **halved**
- The guide's "total per ore" figures in every stage are wrong

**The guide's entire value proposition for stone processing is built on double the actual stone output.** This doesn't change the relative value of stone processing (it's still worth doing), but it halves the absolute contribution to $/ore figures.

### What the guide currently calculates:

```
Stage 1 total = Coil + (1.0 × gemEV_per_stone) + (1.0 × dust_value)
```

### What it should calculate:

```
Stage 1 total = Coil + (0.5 × gemEV_per_stone) + (0.5 × dust_value)
```

---

## 6. Recipe / Input Errors

### Ring Maker Recipe (CRITICAL)

| | Guide | Wiki |
|---|---|---|
| **Inputs** | Metal Dust + Coil | **Gem + Coil** |
| **Multiplier** | ×1.75 | ×1.7 |

Source: [Ring Maker Wiki](https://umt.miraheze.org/wiki/Ring_Maker)

The guide says: "Metal Dust + Coil → Ring (×1.75)" and the JS code uses `metalDust = 25` (the Crusher dust value).

The Wiki says: The Ring Maker combines a **Gem** and a **Coil** to produce a Ring. This fundamentally changes the Amulet recipe chain since rings consume gems, not dust.

**Impact on Amulet/Glider calculations:** The guide's Amulet calculation uses cheap metal dust as a Ring input. In reality, a ring requires a valuable gem, dramatically changing the economics:
- Guide Ring: ($25 dust + $80 coil) × 1.75 = $184
- Actual Ring (e.g., with polished+cut Diamond $2,114): ($2,114 + $80 coil) × 1.7 = $3,730

### Ore Smelter Stone Production

As covered in Section 5: 1 stone per 2 ores, not 1 per 1.

### Superconductor Constructor

The guide treats this as having a ×3.0 multiplier. Wiki sources are ambiguous on whether it's a direct value multiplier or primarily a crafting intermediary. Some sources say ×2.0 (+100%), others ×3.0. This needs in-game verification.

---

## 7. Gem & Prospector Data Errors

### Prospector Prices

| Prospector | Guide Price | Wiki Price | Status |
|-----------|-----------|-----------|--------|
| Topaz | $2,000 | $2,000 | ✅ |
| **Emerald** | **$6,000** | **$5,000** | ❌ |
| Sapphire | $8,000 | $8,000 | ✅ |
| **Ruby** | **$10,000** | **$15,000** | ❌ |
| Diamond | $30,000 | $30,000 | ✅ |

### Gem Base Values

| Gem | Guide | Wiki | Status |
|-----|-------|------|--------|
| Topaz | $75 | $75 | ✅ |
| Emerald | $200 | $200 | ✅ |
| Sapphire | $250 | $250 | ✅ |
| Ruby | $300 | $300 | ✅ |
| Diamond | $1,500 | $1,500 | ✅ |
| Poudretteite | $1,700 | $1,700 | ✅ |
| Zultanite | $2,300 | $2,300 | ✅ |
| Grandidierite | $4,500 | $4,500 | ✅ |
| Musgravite | $5,800 | $5,800 | ✅ |
| Painite | $12,000 | $12,000 | ✅ |

### Prospect Chance
- Guide: 5% per prospector — ✅ confirmed by wiki

### Gem Processing Chain
- Polish (+$10) then Cut (×1.4) — ✅ confirmed
- Cut gem values in the HTML table (e.g., Topaz $119) match: ($75+$10)×1.4 = $119 ✅

### Internal Inconsistency
The guide HTML text at line 2171 says Emerald Prospector costs $5K but the JS `buildings` array at line 2349 says $6,000. Similarly, Ruby Prospector is listed as $15K in some HTML text but $10,000 in the JS array.

---

## 8. Calculation Logic Errors in recalc()

### 8.1 Base Bar Calculation

```javascript
var bar = (effectiveV + 20) * 1.2;   // Clean(+$10) + Polish(+$10) then Smelt(×1.2)
```

**This is correct** — Clean adds $10, Polish adds $10, total +$20 before smelting. Smelt applies ×1.2 to the total. ✅

### 8.2 Dust Value (WRONG)

```javascript
var dustValue = 25;   // Guide claims Crusher produces $25 dust
```

**Wiki says dust is $1.** The Crusher strips all value and produces dust at $1. ✅ for the wiki, ❌ for the guide.

### 8.3 Brick Values (WRONG)

```javascript
var brickBaseValue = 30;             // Wiki says $25
var polishedBrickValue = 40;         // Should be $35 (25+10)
var firedBrickValue = 46;            // Should be $40 (25×1.2+10)
var firedBrickQA = 55;               // Should be $48 (40×1.2)
var concreteBrickPairValue = 370;    // Should be $130 (100×1.2+10)
var concreteBrickPairQA = 444;       // Should be $156 (130×1.2)
```

### 8.4 Bolt Value (WRONG)

In stage 7+ calculations:
```javascript
var bolt = temperedBar + 15;   // Should be temperedBar + 5
```

### 8.5 Stone Rate Not Accounted For

The guide assumes 1 stone per ore smelted. Every stage calculation multiplies gem EV and brick value by the per-stone survival rate but treats it as 1 stone per ore. With the correct 0.5 stones per ore, all byproduct values need to be halved.

### 8.6 Concrete Brick Stone Accounting

```javascript
// Stage 5: Per stone: survival_rate * (concreteBrickPairValue / 2)
var s5ConcretePerStone = gem4.survivalRate * (concreteBrickPairValue / 2);
```

This divides the pair value by 2 (since 2 stones make 1 concrete brick), but it already assumed 1 stone per ore. With 0.5 stones per ore, this should be:
```javascript
var s5ConcretePerStone = 0.5 * gem4.survivalRate * (concreteBrickPairValue / 2);
```

### 8.7 Comparison Table - Electromagnet Bricks (Line 2776)

```javascript
{ name: "Electromagnet + QA", ores: 5, ..., bricks: 1 * concreteBrickPairQA, ... }
```

This hardcodes `1 * concreteBrickPairQA` without proper surviving stone calculation. It should account for how many stones survive prospecting from 5 ores (at 0.5 stones per ore = 2.5 stones, ~1.94 surviving, barely enough for 1 concrete brick after subtracting any used for recipes).

### 8.8 Stage 4 Mech Parts Value

```javascript
var s4Mech = bar + 50;   // bar → Plate(+20) → Mech(+30) = +50 total
```

This is correct mechanically (Plate Stamper +$20 then Mech Parts +$30 = +$50 added to bar) ✅

### 8.9 Power Core Ore Count

Guide claims 11 ores for a Power Core:
- Casing: 4 ores (1 bar + 1 bolt + 1 plate → frame; frame + 1 bolt + 1 plate → casing... actually: 1 bar for frame, 1 bar for bolt in frame, then 1 more bolt, 1 plate. That's: bar=1, bolt-for-frame=1(bar), bolt-for-casing=1(bar), plate=1(bar) = 4 bars → 4 ores) ✅
- Superconductor: 2 ores (alloy = 2 bars) + 2 stones for ceramic (from those same ores or other ores)
- Electromagnet: 1 coil ore + 4 casing ores = 5 ores
- Total: 4 + 2 + 5 = 11 ores

**But:** The ceramic casing needs 2 dust (clay mixer), which needs 2 stones. At 0.5 stones/ore, those 2 alloy ores only produce 1 stone. You'd need additional ores specifically for stone production, or use stones from other ores in the chain. The guide's stone accounting for complex recipes is fragile and likely wrong when the stone rate is corrected.

---

## 9. Stage Guide Errors & Missing Content

### 9.1 Stage Cumulative Costs Are All Wrong

Due to incorrect building prices, every stage cost is wrong. Here are the corrected sums:

| Stage | Guide Claims | Corrected (wiki prices) | Difference |
|-------|-------------|------------------------|------------|
| 1 | $8,850 | $6,210 | -$2,640 |
| 2 | $24,350 | ~$17,710 | ~-$6,640 |
| 3 | $37,100 | ~$30,460 | ~-$6,640 |
| 4 | $53,100 | ~$53,460 | ~+$360 |
| 5 | $73,350 | ~$63,460 | ~-$9,890 |
| 6 | $83,850 | ~$113,460 | varies |
| 7+ | $254,100+ | varies significantly | |

*(Note: Exact Stage 2+ corrected costs depend on which buildings are included in each stage. The primary error sources are Polisher, Ore Cleaner, Ore Smelter, Emerald Prosp, Ruby Prosp.)*

### 9.2 Per-Ore Totals Are All Wrong

Due to compounding errors (dust value, brick value, concrete brick value, stone rate, bolt value, multipliers), every "Total per ore" figure is incorrect. These would need to be fully recalculated from scratch with wiki-correct values.

### 9.3 Prestige Toggles Not Integrated Into Stage Guides

The calculator has prestige toggles (Philosopher's Stone, Ore Upgrader, Nano Sifter, Duplicator), but:
- Stage descriptions and flow diagrams are static HTML — they don't update with prestige toggles
- Only the comparison table and a few summary values update
- Stages should show how prestige machines affect per-ore values at each stage

### 9.4 Missing Stages / Progression Gaps

- No mention of when to buy the Sifter (pre-prestige) vs Nano Sifter (post-prestige)
- Stage 5 introduces Cement Mixer but doesn't account for the Blast Furnace trade-off (more stones at -10% bar value)
- No guidance on how the corrected stone rate (1 per 2 ores) affects optimal factory layout
- No discussion of the Duplicator's impact on production chains

### 9.5 Factory Layout Diagrams

The SVG factory layout diagrams (lines 2984–4036) are hardcoded for each stage but:
- Don't account for 1 stone per 2 ores (layouts show 1:1 stone routing)
- Don't show prestige machine placement
- Missing late-game layouts for Laser, Amulet, Glider chains

---

## 10. End-Game Product Calculation Errors

Using Lead ($30) with no prestige as baseline:

### Current Guide Values vs Corrected Values

**Bar chain (Lead $30, no prestige):**
- Bar = ($30 + $20) × 1.2 = $60 ✅ (same)
- Tempered bar = $60 × 2 = $120 ✅ (same)

**With corrected bolt value (+$5 vs guide's +$15):**
- Bolt = $120 + 5 = $125 (guide: $135)
- Plate = $120 + 20 = $140 (same)
- Frame = ($120 + $125) × 1.25 = $306 (guide: $169 — wait, let me recheck)

Actually, the guide uses `bar` not `temperedBar` for the bar input in frames on some paths. Let me verify:

```javascript
// Line 2590-2593:
var bolt = temperedBar + 15;
var plate = temperedBar + 20;
var frame = (temperedBar + bolt) * 1.25;
var casing = (frame + bolt + plate) * 1.30;
```

With corrected bolt (+$5):
- bolt = $120 + $5 = $125
- plate = $120 + $20 = $140
- frame = ($120 + $125) × 1.25 = $306.25
- casing = ($306.25 + $125 + $140) × 1.30 = $742.63

Guide values (bolt +$15):
- bolt = $120 + $15 = $135
- frame = ($120 + $135) × 1.25 = $318.75
- casing = ($318.75 + $135 + $140) × 1.30 = $771.88

### Power Core (Corrected)

```
alloy = ($120 + $120) × 1.2 = $288
ceramic = $150
SC = ($288 + $150) × ??? (need verified SC multiplier)
coil = $120 + $20 = $140
EM = ($140 + $742.63) × 1.5 = $1,323.94
```

If SC multiplier is ×3.0 (guide's value):
```
SC = $438 × 3.0 = $1,314
PC = ($742.63 + $1,314 + $1,323.94) × 2.5 = $8,451.43  (guide uses ×3.5 = $11,832)
PC_QA = $8,451.43 × 1.2 = $10,141.71  (guide: ~$14,199)
```

If SC multiplier is ×2.0:
```
SC = $438 × 2.0 = $876
PC = ($742.63 + $876 + $1,323.94) × 2.5 = $7,356.43
PC_QA = $7,356.43 × 1.2 = $8,827.71
```

### Engine (Corrected - MAJOR!)

```
mech = $120 + $50 = $170
pipe = $120 + $40 = $160
engine = ($170 + $160 + $742.63) × 2.5 = $2,681.58  (guide uses ×1.2 = $1,286.72)
engine_QA = $2,681.58 × 1.2 = $3,217.89  (guide: ~$1,544)
```

**With the corrected ×2.5 multiplier, Engine is dramatically more valuable than the guide shows.** At 6 ores, Engine_QA/6 = $536/ore (guide showed $593/ore but with wrong numbers). The key point: Engine value roughly doubles from what the guide calculated.

### Tablet (Corrected)

```
glass = $30
coil = $140
circuit = ($30 + $140) × 2.0 = $340
tablet = ($742.63 + $30 + $340) × 3.0 = $3,337.89
tablet_tuned = $3,337.89 + $50 = $3,387.89
tablet_QA = $3,387.89 × 1.2 = $4,065.47  (guide: ~$4,504)
```

### Product Ranking Impact

The corrected Engine multiplier (×2.5 vs guide's ×1.2) and corrected Power Core multiplier (×2.5 vs guide's ×3.5) significantly change the product rankings. Engine becomes much more competitive, while Power Core's dominance is reduced.

---

## 11. Prestige System Issues

### 11.1 Prestige Data

| Machine | Guide Medals | Wiki Medals | Status |
|---------|-------------|-------------|--------|
| Philosopher's Stone | 3 | 3 | ✅ |
| Ore Upgrader | 3 | 3 | ✅ |
| Nano Sifter | 1 | 1 | ✅ |
| Duplicator | 8 | 8 | ✅ |
| Gem→Bar Transmuter | 4 | 4 | ✅ |
| Bar→Gem Transmuter | 4 | 4 | ✅ |

Effects:
- Philosopher's Stone ×1.25 ✅
- Ore Upgrader: next tier, max Mithril ✅
- Nano Sifter: 16.6% sift rate ✅
- Duplicator: half value ✅

### 11.2 Missing Prestige Integration

- **Duplicator** and **Transmuters** are toggled in the UI but have **no effect on calculations** — the JS code reads their checkbox values but doesn't use them anywhere in `recalc()`.
- The Nano Sifter toggle is read but only used for display, not factored into stage $/ore calculations.
- Stage guides should show how Philosopher's Stone and Ore Upgrader affect each stage's per-ore totals, but they only affect the comparison table summary.

### 11.3 Medal Progression Missing

The guide doesn't include the medal earning formula:
- 1st Medal: $20M
- 2nd Medal: $40M (cumulative $60M)
- 3rd Medal: $80M (cumulative $140M)
- 4th Medal: $160M (cumulative $300M)
- etc. (doubles each time)

This is important for knowing when to prestige and which machines to prioritize.

---

## 12. Structural / Bloat Issues

### 12.1 Single-File Monolith
The entire guide is in one 4,040-line `index.html` with embedded CSS (~530 lines), HTML content (~1,770 lines), and JavaScript (~1,740 lines). This makes it very hard to maintain.

### 12.2 Duplicated / Bloated Content
- The Building Reference table (Section 10) duplicates info from the Machine Logic section
- Gem Strategy section duplicates info from the Byproducts section
- Pro Tips section restates advice from stage descriptions
- Machine Logic section is very long and overlaps with stage progression

### 12.3 Inconsistent Data Sources
Because the HTML text and JS `buildings` array were edited at different times, there are internal inconsistencies:
- Emerald Prospector: $6,000 in JS, $5,000 in some HTML text
- Ruby Prospector: $10,000 in JS, $15,000 in some HTML text
- Various multipliers stated in text don't match JS calculations

### 12.4 Static Content That Should Be Dynamic
- Stage flow diagrams are hardcoded HTML — values don't update with calculator
- Byproduct EV tables are partially dynamic but reference hardcoded gem prices
- Machine Logic section has static claims about "$370 concrete bricks" that don't update

### 12.5 Factory Layout Visualization Overhead
The SVG factory layout code (lines 2984–4036, ~1,050 lines) is the single largest JS block. It's impressive but adds significant maintenance burden and doesn't adapt to corrected data.

---

## 13. Recommended Restructuring Plan

Based on the volume of errors and the user's feedback about inconsistencies and bloat, I recommend a phased approach:

### Phase 1: Extract Reference Sections into Separate Documents

Move these sections out of `index.html` into standalone markdown/HTML documents:

1. **`building-reference.md`** — Full building reference table (searchable). Currently Section 10 ("Full Building Reference"). This is purely reference data.

2. **`machine-logic.md`** — Machine Logic & Obsolescence Guide. Currently Section 4 ("Machine Logic & Obsolescence Guide"). This is advisory content that rarely changes.

3. **`gem-strategy.md`** — Gem values, processing chain, optimal products. Currently Section 9 ("Gem Strategy"). Static reference data.

4. **`byproduct-guide.md`** — Stone priority order, brick chains, sifter info. Currently Section 8 ("Byproduct Optimization"). Mostly static reference.

5. **`transport-guide.md`** — Unloader upgrades, splitters, layouts. Currently Section 7 ("Transport & Infrastructure"). Static reference.

6. **`product-comparison.md`** — Detailed product recipes and comparisons. Currently part of Section 3 ("End-Game: Ultimate Products"). The live comparison table stays in the main guide.

### Phase 2: Correct All Wiki Data

Update `index.html` with corrected values:

1. Fix all building prices (15 incorrect)
2. Fix all multipliers (Power Core ×2.5, Engine ×2.5, Laser ×1.75, Bolt +$5, Ring ×1.7, Prismatic ×1.15)
3. Fix material values (Dust $1, Brick $25, Concrete Brick $100, Cement $30)
4. Fix stone byproduct rate (0.5 stones per ore from Smelter)
5. Fix Ring Maker recipe (Gem + Coil, not Dust + Coil)
6. Fix gem prospector prices (Emerald $5K, Ruby $15K)
7. Fix Power Core Assembler price ($4.5M)

### Phase 3: Rewrite the Main Guide

With extracted reference content and corrected data, rewrite the core guide from scratch:

**Keep in main `index.html`:**
- Calculator section (ore selector + prestige toggles)
- Stage-by-Stage progression (rewritten with correct values)
- End-Game section with live comparison table
- Links to reference documents

**Rewrite priorities:**
1. Correct all `recalc()` formulas with wiki-verified values
2. Integrate prestige toggles into stage calculations (Duplicator, Transmuters, Nano Sifter should actually affect outputs)
3. Account for 1 stone per 2 ores in all byproduct calculations
4. Update stage cumulative costs with correct building prices
5. Recalculate all per-ore totals
6. Update comparison table with corrected multipliers
7. Simplify factory layout diagrams (or make them reflect corrected data)
8. Add medal progression info to prestige section
9. Add proper Duplicator/Transmuter calculation logic

### Phase 4: Verify and Test

1. Cross-reference every calculated value against manual wiki-based calculations
2. Test all prestige toggle combinations
3. Verify comparison table rankings with various ore values
4. Ensure internal consistency between HTML text and JS calculations

---

## Appendix: Ore Values (Verified ✅)

All 20 ore values in the guide match wiki data:

| Ore | Value | Status |
|-----|-------|--------|
| Tin | $10 | ✅ |
| Iron | $20 | ✅ |
| Lead | $30 | ✅ |
| Cobalt | $50 | ✅ |
| Aluminium | $65 | ✅ |
| Silver | $150 | ✅ |
| Uranium | $180 | ✅ |
| Vanadium | $240 | ✅ |
| Tungsten | $300 | ✅ |
| Gold | $350 | ✅ |
| Titanium | $400 | ✅ |
| Molybdenum | $600 | ✅ |
| Plutonium | $1,000 | ✅ |
| Palladium | $1,200 | ✅ |
| Mithril | $2,000 | ✅ |
| Thorium | $3,200 | ✅ |
| Iridium | $3,700 | ✅ |
| Adamantium | $4,500 | ✅ |
| Rhodium | $15,000 | ✅ |
| Unobtainium | $30,000 | ✅ |

---

## Appendix: Prestige ORE_TIERS Array (Verified ✅)

The Ore Upgrader tier list (15 tiers, Tin → Mithril) matches wiki data. ✅

---

*End of Review*
