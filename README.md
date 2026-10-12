# Dead Sea DLE Pilot

Interactive 3D visualisation of the XtraLit direct-lithium-extraction pilot on Dead Sea
water, fed from an accumulation pond. Feed Li ≈ 18 ppm.


## Using the page

Press **Tour** in the bottom bar (or the T key) for a guided tour in 14 short steps: what the
3D view shows, the columns and the carousel, each process step with its pipe colour
(sorption, water wash, desorption, NaCl wash at pH 10), the valve battery, the analysers and the HMI — then the
controls: run / pause / speed, how to highlight one step, how to hide the labels or the panels,
and the camera. Each step rings the control it talks about or flies the camera to the
equipment and pins markers on it; the page stays fully usable while the tour is open.
← / → step through, Esc closes. A link can open the tour at a given step: `index.html?tour=5`.
**Help** (or ?) has the same on one page and a button into the tour.

## The skid

One skid, built like a factory ultrafiltration unit, carries the whole process side:

- **P-101** — the single brine pump, drawing from the accumulation pond.
- **Filtration train** — 50 µm mesh strainer → 10 µm cartridge → 1 µm cartridge → UF.
  UF concentrate and backwash go to the Spent Brine tank.
- **PT-101** after the UF and **NMR-in** — on-line lithium analyser on the feed.
- **Valve battery** — brine, water, acid and NaCl feed headers, spent / eluate / sulfate
  collect headers, and 7 actuated valves per column (56 in all) driven by the PLC.
  All valves sit in the battery at the headers, none on the column: brine → the
  column's brine line; water, acid or NaCl → its service line (the three feeds join
  after their valves); its outlet line → spent, eluate or sulfate.
- **Three pipes per column** — brine line and service line on top (they tee at the
  single top nozzle), one outlet line from the bottom nozzle back to the battery.
- **8 radial columns** (~20 L, 20 kg LMO) in two banks of four, each with a magnetic
  flowmeter FT-1…FT-8 on its outlet line (every step passes it).
- **P-201** (water wash), **P-301** (0.8 M H₂SO₄) and **P-401** (NaCl wash, pH 10).
- **Sample selector + NMR-out** — a sample line taps every column's outlet line in
  the battery and runs under the collect headers to the selector;
  NMR-out reads the outlet Li of the columns on sorption in turn.
- **EC-101 / pH-101** on the spent and sulfate collect lines, and the **PLC cabinet**.

Seven tanks stand off the skid: pond, fresh water, acid, NaCl (T-401), eluate, sulfate,
spent brine.

## Process

| Step | Medium | Direction | Valves (per column) | Control |
| --- | --- | --- | --- | --- |
| Sorption | Dead Sea brine, 9 m³/h | top → down (radial) | brine in, spent out | 1.5 h, or stop on breakthrough |
| Water wash | Fresh water, 9 m³/h | top → down | water in, spent out | EC to set-point |
| Desorption | 0.8 M H₂SO₄ | top → down, same way as sorption | acid in; eluate out (Fr-2/3) or sulfate out (Fr-1/4/5) | 5 timed fractions |
| NaCl wash | NaCl pH 10, 9 m³/h | top → down | NaCl in, sulfate out | pH back to ≈ 7 |

Every step enters through the column's top nozzle — brine by the brine line, water,
acid and NaCl by the service line — and leaves by the one outlet line at the bottom.

**NaCl wash.** After the acid the bed is reconditioned with NaCl at pH 10 instead of
plain water. The wash runs until the outlet pH (pH-101) is back to ≈ 7, so the
bromide-rich Dead Sea brine of the next sorption never meets an acidic bed, where
Mn(IV) would oxidise Br⁻ to Br₂. The effluent goes to the sulfate tank with the
non-product fractions. NaCl has its own header and valve per column because the NaCl
wash runs at the same time as other columns' water wash and desorption.

| Fraction | Li | Ca | Mg | Mn | To |
| --- | --- | --- | --- | --- | --- |
| Fr-1 | 44 | 846 | 1902 | — | sulfate |
| Fr-2 | 5230 | 455 | 1133 | 533 | eluate |
| Fr-3 | 1038 | 184 | 88 | 1434 | eluate |
| Fr-4 | 79 | 61 | 63 | 369 | sulfate |
| Fr-5 | 25 | 27 | 44 | 209 | sulfate |

(ppm, from the original Dead Sea pilot)

## Carousel

The eight columns run the same cycle shifted by one eighth of it: column N starts
(N−1)·P/8 after C1.

| | |
| --- | --- |
| Column cycle P | 2.40 h |
| Sorption | 1.50 h |
| Water wash · desorption · NaCl wash | 0.20 · 0.50 · 0.20 h (regeneration 0.90 h) |
| Shift between neighbours | P/8 = 0.30 h |
| Columns on sorption | 5 (fewer while a column is on hold) |
| P-101 brine | 45 m³/h nominal = 5 × 9 m³/h (36 with one column on hold) |
| P-201 water | one column at a time, 0–9 m³/h |
| P-301 acid | every column desorbing at the time, up to 2 at once |
| P-401 NaCl | one column at a time, 0–9 m³/h |
| Cycles per day | 10 |
| Brine per day | ~1080 m³ |

Brine flows without a break: some column is always on sorption, so P-101 never stops.

**Stop on breakthrough.** NMR-out watches the outlet of each sorbing column. When it
reaches 10 % of the feed Li (1.8 ppm), the PLC ends that column's sorption early: its valves
shut and it waits on **hold** until the end of its sorption slot. The schedule of
the following steps does not move.

## Sorption time

Sorption is timed to load the sorbent to 10 mg Li per g at 85 % recovery:

    t = m_sorbent x loading / (Q x C_feed x recovery)
      = 20 kg x 10 g/kg / (9 m³/h x 18 g/m³ x 0.85)
      = 1.45 h  (1.5 h)

The regeneration half of the cycle — water wash, the fractionated desorption and the
NaCl wash — takes a further 0.90 h, so a full cycle runs 2.40 h: **10 cycles per day**,
~8 kg Li₂CO₃ per cycle across the eight columns (~80 kg/day) on ~1080 m³/day of brine.
Eluate Li ≈ 6000 ppm, about 330 × the feed.

Derived from the Dead Sea pilot layout (igorkant/Dead-sea-pilot) and the Great Salt Lake
UF-skid pages (gsl-south-arm-skid).
