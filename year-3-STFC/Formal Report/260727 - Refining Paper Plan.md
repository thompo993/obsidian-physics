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


#  Proof Read Table 
## Proofread #1

| Section                     | Proofread? |
| --------------------------- | ---------- |
| Acknowledgements            |            |
| Introduction                |            |
| Theory                      |            |
| Experimental                |            |
| Discussion and analysis     |            |
| conclusions and future work |            |
| appendix                    |            |

## Proofread #2 
| Section                     | Proofread? |
| --------------------------- | ---------- |
| Acknowledgements            |            |
| Introduction                |            |
| Theory                      |            |
| Experimental                |            |
| Discussion and analysis     |            |
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
- removal of 43mm tiles
- why did we re lap tiles for the 30mm tiles 
- all /refs work (proofing)
- future work, smaller PMT rig, or switch to SiPMs, 
- what is a Geiger mode detector 
- IN PRESENTATION EXPLAIN ERROR MORE, WHAT IF ANDRE ASKS ABOUT IT
- what are poission statistics
- ensure that the 210 and 105 scatter are acceptable
- add about better integrated counts error for the longer tiles
- [[260727 - Claude Proofread and suggestions]]
- add about tiles in 30mm not falling below threshold, but were low so investigated anway, e.g bc their 30mm and benchmark is super high

## Error propagation update 
- I'm sure the code that Claude has written is good for estimating error, but for the sake of the report i am unsure of how to properly include it without plagiarizing. 
- So we are going to acknowledge that i couldn't estimate error on the integrated counts, and just used the standard deviation. surely the error cant be much on np.trapz
- as we do not know the exact function this partuclar experimental setup contains, we cannot approximate error for integrated counts 
