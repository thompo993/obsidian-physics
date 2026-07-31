---
tags:
  - note
  - super-musr
  - scintillating-tiles
created: 2026-07-27
---
# Links: 
[[260603 - Plan]]


**GET RID OF ALL TYPOS YOU  IDIOT**

- consider the fact that mis alignment in the longer tiles is worse because you are not just shortening the path length in one side, you are simultaneously increasing the path length in the other
#  Proof Read Table 
## Proofread #1

| Section                     | Proofread? |
| --------------------------- | ---------- |
| Abstract                    | yes        |
| Acknowledgements            | yes        |
| Introduction                | yes        |
| Theory                      | yes        |
| Experimental                | yes        |
| 30                          | yes        |
| 63                          | yes        |
| 105                         | yes        |
| 210                         | yes        |
| conclusions and future work | yes        |
| appendix                    | yes        |

## Proofread #2 
| Section                     | Proofread? |
| --------------------------- | ---------- |
| Abstract                    | yes        |
| Acknowledgements            | yes        |
| Introduction                | yes        |
| Theory                      | yes        |
| Experimental                | yes        |
| 30                          | yes        |
| 63                          |            |
| 105                         |            |
| 210                         |            |
| conclusions and future work |            |
| appendix                    |            |


# Previous paper and feedback
[[pdf-260727_Industrial_Placement_Report.pdf]]
![[pdf-260727_Industrial_Placement_Report.pdf]]
# Notes:

## Andrei Feedback
### priority
- Andre says it is a good start, structure was good and had technical details, main things to look for 
- better explanation of the physics of super MuSR, especially what field it measures, where and what for. you can give some graphs from papers with typical results and ideally correlate the proposed improvements with science 
- proofing of text too many inconsistencies, and even missing words
- less notebook style of writing, write more like a scientific report/paper
- ensure there is data to back everything up
- **ensure latex formatting is good and that there are no typos**
### Less relevant
- for future plans review what ahs been done on the epoxy and search for other examples of epoxy cracking **in this case look at creation of similar tiles and see if they discussed any issues**

## Claude cutting down plan 
#### Tier 1 — Cut Theory background wholesale (~950 words, lowest risk)

**1. §2.2 (µSR Fundamentals), the Larmor/asymmetry derivation — cut ~500 of 580 words.**  
From "_The muons magnetic moment (µ) is directly proportional..._" through "_...magnetic field strength within the sample can be determined [7]_" (eqs. 6–15), you derive Larmor precession from first principles and then the full forward/backward count-rate equations and calibration constant α. None of it is used again — §3–4 measure P/V ratio and integrated counts, never asymmetry or α. Cut to two sentences: spin precesses at the Larmor frequency in a local field (state eq. 9's result, skip the torque derivation), and positron emission is asymmetric relative to spin due to parity violation, which is why detectors are arranged in forward/backward banks. Eqs. 6–8 and 10–14 can go entirely.

**2. §2.3 (Types of µSR experiment) — cut ~200 of 258 words.**  
ZF/LF/TF/RF are each explained in full (compensation magnets, static vs. fluctuating fields, vortex lattices, hyperfine coupling), but your own bench test uses none of these beamline configurations. Compress to one or two sentences naming the techniques and what RF capability adds for Super MuSR; cut the mechanism explanations.

**3. §2.1 (Production of Muons), first half — cut ~165 of 192 words.**  
"_ISIS is a pulsed spallation source..._" through "_...produce neutrons via spallation [5] [6]_" walks through the RFQ, 70MeV linac, 800MeV synchrotron, and TS1/TS2 split — this is neutron-production detail from a different part of the facility, irrelevant to muon detector tiles. Compress to one sentence (e.g. "ISIS is a pulsed spallation source producing proton bunches at 50Hz [5,6]") and go straight to the carbon-target/pion/muon chain, which you do need.

**4. §2.4.1 (MuSR), PMT mechanism paragraph — cut ~85 of 105 words.**  
"_When light is incident on a PMT..._" through "_...producing a measurable current pulse [14]_" re-derives dynode-chain electron multiplication — standard textbook material, not specific to your tiles. Cut to one sentence: PMTs convert light to an amplified current pulse via a photocathode and dynode chain [14].

#### Tier 2 — De-duplicate repeated scaffolding in §3–5 (~490 words)

**5. §5 (Conclusions and Future Work) — restructure, cut ~185 of 340 words.**  
This section re-states all four failure rates already given in §4.1–4.4 almost verbatim, with no new synthesis, plus a broken sentence ("_After accounting for the refilled tiles, as this is no longer an issue going forward._"). Convert the four failure-rate sentences into a compact table (tile length / failure rate / cause identified), replace the prose with 2–3 sentences of actual synthesis (what this means for Super MuSR's overall readiness), and turn the future-work paragraph into a short bullet list (43mm testing, full-module beamline test Sept 2026, flood-field rig).

**6. Repeated t-test scaffolding — §4.1 (×2), §4.2, §4.3 — cut ~100 words.**  
Four near-identical sentences ("_A Student t-test... comparing... significance level of 0.05... p-value was X..._") re-explain the same method every time. Keep the fullest version at its first use (§4.1, resin comparison) and compress the other three to a parenthetical — e.g. "(t-test, p = 0.697, not significant)" instead of a full sentence.

**7. Repeated histogram/skew reasoning — §4.1, §4.2, §4.3, §4.4 — cut ~100 words.**  
"Tiles cluster near maximum achievable output; defects create the tail" is explained fully in §4.1, then re-derived at length in §4.2 (contrasting the positive skew), §4.3 (return to negative skew), and §4.4. Keep the full reasoning in §4.1 only; elsewhere, replace with a one-line back-reference, e.g. "As in §4.1, consistent with production clustering near maximum achievable output (Fig. 16b)."

**8. §3.1, benchmark-tile justification tangent — cut ~80 of 131 words.**  
"_Having a physical benchmark tile was used as it allows a consistent point of normalisation..._" through "_...PMT gain drift_" makes one point (a tile is a more stable reference than the PMT rig) across five overlapping clauses. Tighten to something like: "A physical tile was used as the benchmark rather than the detector setup, since tiles are less prone to damage than the PMTs (which can suffer photocathode damage, electronics faults, or gain drift). Backup tiles exist if the primary is damaged, at the cost of batch comparability." (~45 words)

**9. Duplicated acceptance-threshold definition — §4.2 and §4.3 — cut ~25 words.**  
"_the acceptance threshold is set as 2 standard deviations below the benchmark tile repeats_" (§4.2) and "_the acceptance threshold was set 2 standard deviations below the benchmark repeats_" (§4.3) say the same thing almost word-for-word. State it once, then just say "as before" the second time.

#### Tier 3 — Mechanical and line-level tightening (~165 words)

**10. "in order to" → "to" — 16 instances throughout — cut ~32 words.**  
Every instance loses nothing by dropping "in order" (e.g. "in order to determine" → "to determine"). Safe global find-and-replace.

**11. (Optional) §2.4.2, upgrade-benefits paragraph → bullet list — cut ~65 of 155 words.**  
"_Super MuSR is the upgrade that intends to replace..._" through "_...compared to the previous instrument_" lists four benefits (count rate, temporal resolution, field strength, RF capability) in prose. As a short bullet list (20× count rate; 6–10× temporal resolution; motivates oxide, vortex-lattice, and battery-material studies) it reads faster in less space.

**12. A couple more verbose-phrasing fixes, representative rather than exhaustive — cut ~40-50 words:**

- §2.1: "_µ+ are required for the Super MuSR and are produced at ISIS and are produced via the decay path_" → "µ+ for Super MuSR are produced at ISIS via the decay path" (drops the triple repeat of "are produced").
- §3.1: "_Longer tile lengths greatly increasing the range of path lengths, and hence amount of attenuation present in these tiles. This broadens the peaks, making the P/V ratio smaller._" → "Longer tiles have greater attenuation, broadening the PHS peaks and lowering the P/V ratio."
- Apply the same pattern anywhere you see one idea taking two clauses — not worth hunting exhaustively next to Tiers 1–2.

**13. Duplicate-word typos, ~1 word each but worth a global sweep:** "notes notes" (Tables 2–5 captions, same typo four times), "the the" (§3.1, §4.4), "tiles tiles" (§4.1), "a a result" → "As a result" (§4.2).

#### Running the numbers

|Tier|Focus|Est. words cut|
|---|---|---|
|1|Cut Theory background (§2.1–2.4.1)|~950|
|2|De-duplicate methodology in §3–5|~490|
|3|Mechanical/line-level tightening|~165|
|**Total identified**||**~1,605**|
|**Your target**||**~1,200**|

You've got roughly 400 words of margin. **Tier 1 alone gets you two-thirds of the way there with essentially no risk to your original methodology or results** — do that first, re-count, then pull from Tier 2 only as needed. If your placement is assessed partly on demonstrating theoretical grounding, keep more of §2.2's concept sentences than I've suggested and lean harder on Tier 2/3 instead — that trade-off is yours to make, not mine.

#### Also worth fixing before you submit (not word-count, but important)

- The "ADD EXTRA REASONS HERE" placeholder in §4.4, flagged above.
- Sentence fragments: Abstract — "_Tiles of lengths ranging from 30mm - 210mm._" (no verb; merge into the previous sentence); §3.2.2 — "_...giving a false positive result for tile failure without._" (dangling "without").
- Typos: "posotive" (§2.1) → positive; "thse" (§1) → these; "witht" (§3.2.2) → with; "aliment" (§3.1) → alignment; "front he" (§4.3) → from the; "Til.e" (Fig. A1 caption) → Tile; "63mmmm" (Abstract) → 63mm.

## Additional Sections to include 

### Declaration
#### What is it
The “Declaration” is an important part of your report. It should be on a blank page of its own, after the title page and before the Table of Contents. It should set out very clearly what YOUR contribution has been to the project (as distinct from your partner, supervisor, some helpful post-doc, technician or whoever else). It should take the form of a short paragraph which sets out what role you took in the project, and specifically which results you were responsible for. For dissertations, you should indicate the level of guidance you received from your supervisor in terms of the development of your work. An example declaration is attached to the end of this document 

### Example provided by UoB
Example Project: Measurement of the Fermi surface of transition metals Declaration We did not collect all of the data presented in this project. Specifically, we gathered the data on vanadium ourselves, but the section on Cr uses data was provided to us. The analysis used computer programs which we had to modify, sometimes quite substantially. The program called DataFit, we wrote ourselves from scratch, and another, FitData, was given to us and did not need changing at all. We had some assistance from a Ph.D. student (A.N. Other) both in collecting the data and beginning the data analysis. We also had some expert help from Mr. Smith in the electronics workshop in designing the electronic circuit for measuring the temperatures. Our supervisor helped us in the interpretation of our data, specifically suggesting the rigid band analysis. However, it was our idea to attempt the analysis presented in Chapter 3. In terms of how work was divided between my partner and myself, I wrote most of the computer code whilst my partner worked on deriving the equations and algorithms which I needed to code. We both played an equal role in data collection, analysis, and interpretation.

### Acknowledgements 
Whole team in the acknowledgements
who was involved technicians notes need to be credited 
this is in first person. 


## Additional feedback (self insert)
- why did we re lap tiles for the 30mm tiles 
- all /refs work (proofing)
- future work, smaller PMT rig, or switch to SiPMs, 
- ensure that the 210 and 105 scatter are acceptable
- add about better integrated counts error for the longer tiles
- [[260727 - Claude Proofread and suggestions]]
- add about tiles in 30mm not falling below threshold, but were low so investigated anway, e.g bc their 30mm and benchmark is super high
- **are tiles really failing, future work involves determining absolute failure rate, and if it will be used on the detector**
- mention the 2D analysis wasn't helpful in the 210mm tiles

### presentation 
- IN PRESENTATION EXPLAIN ERROR MORE, WHAT IF ANDRE ASKS ABOUT IT
- what are poisson statistics
- why do longer tiles get effected worse by the defects
	- scrates are the same size, as the cuase (human error)
	- remains the same, so for a dimmer and more sensitive tile, the same mistake will have a more significant effect. 
- temperature effects breakdown voltage of SiPMs what is the observed effect

## Error propagation update 
- I'm sure the code that Claude has written is good for estimating error, but for the sake of the report i am unsure of how to properly include it without plagiarizing. 
- So we are going to acknowledge that i couldn't estimate error on the integrated counts, and just used the standard deviation. surely the error cant be much on np.trapz
- as we do not know the exact function this partuclar experimental setup contains, we cannot approximate error for integrated counts 


## Procedure section 
Looking at the document, there are two distinct error sources being handled with two different strategies:

**1. Run-to-run error (timescale: minutes, within a single measurement session)**

This comes from the physical act of setting up a measurement — specifically the optical coupling and alignment of a tile to a PMT. It was isolated using a single 30mm tile as a test case:

- **Without disturbing the setup** (repeated PHS taken back-to-back, same grease/alignment): variation was **1.2%**. This is essentially the "floor" — electronic/statistical noise of the PMT + Picoscope readout itself.
- **With full disturbance** (removing tile, re-greasing, re-aligning, then re-measuring): variation rose to **6.2%**. This isolates the _human/mechanical_ contribution — how reproducibly the grease coupling and physical alignment can be redone each time.

Since this error can only ever _degrade_ the apparent light output (bad coupling never makes light output look artificially better), the paper's mitigation is to **take the highest-light-output run per tile** as the representative measurement — treating the rest as coupling losses rather than true tile variation.

**2. Long-term drift (timescale: weeks/across a measurement campaign)**

This is a separate, slower-acting error source — it isn't about how well any single tile is coupled, but about the _stability of the whole rig_ (PMT gain, HV supply, ambient temperature) drifting between measurement sessions taken days or weeks apart.

- This was quantified using the **benchmark tile**, which is re-measured at the start and end of every day's batch of measurements.
- Figure `peakfinder_stability` shows drift of **up to 9.8%** between these benchmark measurements across batches.
- Mitigation: every tile measurement in a batch is **normalised to the mean of that batch's benchmark measurements**, so the drift is calibrated out rather than treated as tile-to-tile difference.

**Summary of the distinction**

||Run-to-run error|Long-term drift|
|---|---|---|
|Timescale|Minutes (within a session)|Days–weeks (between sessions/batches)|
|Cause|Coupling/alignment reproducibility|PMT gain drift, HV drift, ambient temperature|
|Measured via|Repeated 30mm tile runs (with/without redoing coupling)|Repeated benchmark tile runs at start/end of each day|
|Magnitude|1.2% (setup unchanged) → 6.2% (setup redone)|up to 9.8%|
|Correction strategy|Take best (highest) run per tile|Normalise batch to its own benchmark mean|

One thing worth flagging: these two corrections are somewhat independent and applied at different stages — the "take the best run" correction handles the _within-session_ coupling variability for an individual tile, while the _batch normalisation_ handles the _across-session_ rig drift using the benchmark tile as a transfer standard. It might be worth stating explicitly in the write-up whether these two error contributions are assumed independent (and if you ever combine them into a total systematic uncertainty, e.g. by quadrature).

> It is important to distinguish between two independent sources of error present in this measurement campaign, acting on very different timescales.

The first is a short-timescale error, arising from the reproducibility of the optical coupling and physical alignment of a tile to its PMT, evaluated over a timescale of minutes. This was isolated using a single 30mm tile: when a series of PHS were taken without altering the optical coupling or alignment, the maximum variation between runs was 1.2\%, representing the intrinsic statistical/electronic floor of the measurement. When the coupling was instead removed, re-greased, and re-aligned between each measurement, this variation increased to 6.2\%, isolating the contribution from coupling and alignment reproducibility specifically. Since imperfect coupling can only ever suppress the apparent light output of a tile, never enhance it, this result is used purely to justify the selection of the highest light output run as the best representation of a given tile's true performance, rather than as a correction to be applied numerically.

The second is a long-timescale error, arising from drift in the experimental rig itself -- including PMT gain drift, HV power supply drift, and ambient temperature changes -- evaluated over a timescale of days to weeks across a measurement campaign. This was quantified using repeated measurements of the benchmark tile taken at the start and end of each day's batch of measurements, which showed variation of up to 9.8\% between batches (Figure \ref{fig:peakfinder_stability}). Unlike the short-timescale coupling error, this drift is corrected for directly, by normalising all tile measurements within a batch to the mean of that batch's own benchmark measurements.




Here are a few minimal-edit options, each targeting the same one or two sentences so you can mix and match. I've kept everything else in the extract untouched.

**Option A — small clause inserted into the existing sentence**

> These dominant sources of error can only degrade the light output, and since they act on the timescale of a single measurement rather than across the campaign, the highest light output run for each tile was selected to get the best representation of tile performance.

**Option B — split into two sentences, explicit "justify" language**

> These dominant sources of error can only degrade the light output. This justifies the selection of the highest light output run for each tile as the best representation of tile performance, rather than an average across runs.

**Option C — minimal, just adds "short-timescale" as a label**

> This short-timescale coupling and alignment error can only degrade the light output, therefore the highest light output run for each tile was selected to get the best representation of tile performance.

**Option D — ties it forward to the drift paragraph without adding much**

> These dominant sources of error can only degrade the light output, therefore the highest light output run for each tile was selected to get the best representation of tile performance (see below for the separate, longer-timescale drift correction applied via batch normalisation).

**Option E — tiny addition, just one word for clarity on "confirmed"**

> This was confirmed on a shorter timescale of minutes using a single 30mm tile.  
> (inserted in place of "This was confirmed by using a single 30mm tile.")

If you want, I can also do the same light-touch treatment for the drift paragraph so both sides use matching language (e.g. "long-timescale" mirroring whichever "short-timescale" phrasing you pick from above) — just say which option(s) you're leaning toward.

Here are two standalone sentences, one for each section, written to slot in with minimal disruption:

**For the optical coupling paragraph** (after "...variation between measurement increased to 6.2%."):

> This confirms that coupling and alignment reproducibility is a short-timescale error, acting within a single measurement session, which justifies selecting the highest light output run per tile rather than averaging across runs.

**For the calibration/stability paragraph** (after "...up to 9.8% variation between measurements of batches of the benchmark tile..."):

> This demonstrates a separate, long-timescale drift acting across the measurement campaign, which is instead corrected for directly through batch normalisation to the benchmark tile.