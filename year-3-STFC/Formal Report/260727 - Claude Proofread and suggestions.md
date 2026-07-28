---
tags:
  - note
  - super-musr
  - scintillating-tiles
created: 2026-07-27
---
# Links: 
[[260727 - Refining Paper Plan]]
# Proofreading & Review Notes — "Development and QC of Novel Scintillating Tiles for Super MuSR"

## 0. Verdict on the three points of previous feedback

| Feedback point                                                    | Status                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| ----------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Better physics explanation of Super MuSR (what field, where, why) | **Partially addressed.** Section 2.4.2 now explains _what_ improves (count rate, time resolution, field range) and gestures at _why it matters_ (oxides, vortex lattices, batteries, RF measurements), but it still doesn't explicitly say **what physical quantity is being measured** (the local magnetic field at the muon site, via the Larmor precession frequency) _at the detector-design level_, i.e. it doesn't reconnect back to Eq. 5 (ω = γμB_local) when explaining why faster timing/higher count rate matters. No literature graphs of typical μSR results were added — see §5 below. |
| Proofreading — inconsistencies and missing words                  | **Not yet fully done.** There are still frequent typos, duplicated words, missing words, and several broken/garbled sentences (some quite serious — see §2). This is the biggest gap between where the report is and where it needs to be.                                                                                                                                                                                                                                                                                                                                                           |
| Less "notebook style", more scientific-report style               | **Partially addressed.** Structure and section flow read like a proper report now, but capitalisation habits (Tile/Module/Stud capitalised inconsistently), comma-splice sentences, and casual phrases ("lots of practice") still give parts of it a lab-notebook feel.                                                                                                                                                                                                                                                                                                                              |
| Data backs up every claim                                         | **Mostly good**, but there are a few claims/numbers that don't quite line up with the data given — flagged in §1 as these are the highest priority to fix.                                                                                                                                                                                                                                                                                                                                                                                                                                           |

**Word count flag:** I ran the PDF through a text extractor and stripped figure captions, table headings, and equations. The main body (Introduction → Conclusions) comes out at roughly **5,500+ words**, well above the 2,500-word target you mentioned. My extraction isn't perfect (some words merge together in the parser), but the report reads as clearly over-length as it stands — worth checking with an accurate word count tool (e.g. Word's count, with captions/appendix/equations excluded) before submission.

---

## 1. Critical issues — data/logic consistency (fix these first)

1. **Appendix A.1, first line contradicts the whole of §4.1.**
    
    > "Although 43mm and 30mm tiles were not investigated within this report, and the benchmark was set in other work for the 30mm tiles, they are included for completeness"
    
    Section 4.1 is _entirely_ about investigating 30mm tiles (33+ tiles, two Student's t-tests, histograms, etc.). Only the **43mm** tiles were not investigated in this report (as stated in your own Conclusion: "these have had the fewest tiles tested"). This sentence needs rewriting so it doesn't claim 30mm tiles weren't investigated — probably you mean the 30mm _benchmark_ wasn't newly determined here (established in earlier work), which is a different claim from "not investigated."
    
2. **Abstract's resin recommendation has an overlapping boundary.**
    
    > "RAL71 resin is the most suitable choice for tiles ≤ 63mm, and RT152 resin for tiles ≥ 63mm."
    
    This puts 63mm tiles in both categories. From the body, RAL71 was used for 30mm and 63mm tiles, and RT152 was adopted for 105mm and 210mm tiles (mainly because RAL71 needed a higher cure temperature that warped the longer tiles, not purely because of light-output performance — worth reflecting that nuance in the abstract too, since "most suitable" implies a performance basis that your own t-test in §4.2 doesn't actually establish for RT152 vs RAL71 on 63mm tiles). Suggest: "RAL71 for tiles ≤63mm, RT152 for tiles ≥105mm" (or similar), and softening "most suitable" to reflect that the switch was also a processing/practicality decision.
    
3. **Table 4 (210mm production notes) has a duplicate tile ID.** Tile ID **36** appears twice with two different notes ("Resin a bit messy" / "Semi chilled resin, hair in resin"). Either this is a genuine duplicate entry that needs correcting to the right ID, or two separate tiles were mislabelled — worth double-checking against your raw data before submission, since a marker could easily flag this.
    
4. **Unmatched brackets** in two places change the meaning of the sentence:
    
    - §4.1: "...plotted on separate histograms (Figure 13b, which shows no clear difference..." — missing closing parenthesis.
    - §4.4: "...the 2D stud analysis (Figure A5 found that there was asymmetrical light output..." — missing closing parenthesis after "A5".
5. **§4.3 105mm tiles — mixed-up significance language.**
    
    > "The result was no statistically significant difference, with a p-value of 0.810 at the p≥0.05 level."
    
    "p≥0.05 level" isn't quite right terminology — you mean the significance/alpha level is 0.05, and your p-value (0.810) exceeds it. Elsewhere you correctly write "at the 0.05 significance level" — use that consistent phrasing here instead of "p≥0.05 level", which conflates the test statistic with the threshold.
    
6. **Appendix A.3.3 is incomplete.** The error propagation for integrated counts cuts off mid-thought:
    
    > "Once again there are two sources of error. The error om the integrated counts calculation is done"
    
    This stops before explaining how the integrated-counts error is actually calculated (no equation, no completion of the sentence — also "om" → "on"). Since integrated counts are your FoM for the 105mm/210mm tiles, this section needs finishing to satisfy "data backs up every claim."
    
7. **Tile ID reuse across lengths isn't flagged.** "Tile 120" appears in Table 1 (30mm, resin RAL71) and again in §4.2 text as a 63mm tile made with RT152. If tile IDs restart at 1 for each length (which seems to be the case), a one-line note early on ("Tile IDs are numbered independently within each tile length") would prevent a reader assuming these are the same physical tile.
    

---

## 2. Recurring, document-wide issues (search-and-fix across the whole file)

- **"effected" → "affected"** — used incorrectly at least twice (§3.1 "Optimal aliment... were not arriving perpendicular" region is a separate typo; the affect/effect mix-up appears in §4.4 "more greatly effected by defects" and elsewhere).
- **Missing apostrophes on plural/possessive nouns**, throughout: "technicians notes" → "technicians' notes", "production teams notes" (all four table captions) → "production team's notes", "benchmark tiles brightness" → "benchmark tile's brightness", "tiles studs" → "tile's studs", "muons magnetic moment" → "muon's magnetic moment", "instruments electromagnets" → "instrument's electromagnets".
- **"Savitky-Golay" → "Savitzky-Golay"** (misspelled twice; matches your own reference [19] "Savitzky A and Golay M").
- **"silicone carbide" → "silicon carbide"** (silicone and silicon are different materials — this is in the main text; the figure caption already has it right).
- **"Student t-test" → "Student's t-test"** (used several times without the possessive).
- **a/an errors**: "is a exponentially decaying curve", "a optically transparent resin", "a experimental setup", "a overall failure rate" — all need "an".
- **Inconsistent capitalisation of common nouns** — "Tile" vs "tile", "Module" vs "module", "Resin" vs "resin" are capitalised inconsistently throughout (e.g. "Each Tile is cleaned" vs "Each tile has a designated LHS"). Pick one convention (lower-case, since these aren't proper nouns) and apply it uniformly — this is one of the main things giving the "notebook" feel.
- **"Endeavour" programme naming** is inconsistent: "Endeavour programme" (intro, capital E), "endeavour program" (intro, lower-case + US spelling), "Endeavour programme" (again later). Pick one form (suggest "Endeavour Programme", matching ISIS's own branding) and use it throughout.
- **Duplicated words** (search for these exact strings and remove one instance): "the the" (×2), "a a" (×2), "as as", "that that", "this this", "within within", "tiles tiles".
- **Hyphenation of compound adjectives** — "peak to valley ratio" vs "peak-to-valley ratio (P/V)", "peak finding" → "peak-finding", "poor performing" → "poor-performing" (all four table captions), "light tight" → "light-tight", "gain matched" → "gain-matched", "solid state" → "solid-state", "general purpose" → "general-purpose", "time dependent" → "time-dependent".
- **Isotope notation**: "90Sr" and "Sr90" are both used for Strontium-90 — pick one (⁹⁰Sr or Sr-90) and standardise.

---

## 3. Section-by-section: clearest sentence-level errors

**Abstract**

- "novel scintillating **made if** scintillating plastic" → missing word + typo: "novel scintillating **tiles made of** scintillating plastic".
- "For the 63mm and 105mm, tiles, they performed" → remove stray comma: "For the 63mm and 105mm tiles, they performed".

**Introduction**

- "One of these instruments **is a part is** Super MuSR" → garbled; should be "One of these instruments is Super MuSR".
- "these are characteristic behaviours of the muon, and can be used to probe the **magnetic composition** of materials" → you probably mean magnetic _properties/structure_, not composition (composition implies chemical make-up).

**§2.2 μSR Fundamentals**

- "a mean lifetime τμ **−** 2.197μs" → should be "= 2.197μs" (the "−" looks like a garbled equals sign — check the source file, this may just be a PDF-export artefact rather than a real typo, but worth confirming it renders as "=").
- "µ+ **come to a rest**" → "come to rest" (no "a").
- "exerts a torque on the magnetic moment **on** the muon" → "...moment **of** the muon".
- "The resultant plot ... **is a** exponentially decaying curve" → "is **an** exponentially decaying curve".
- "...ionising radiation [11], **this light is then collected**" → comma splice; make it two sentences or use a semicolon.

**§2.3 Experimental Geometries**

- "...(TF) experiments **ZF measurements** utilise..." → missing full stop between "experiments." and "ZF measurements".
- "The muons depolarise over time known as muon relaxation, applications include..." → comma splice; "The muons depolarise over time in a process known as muon relaxation. Applications include..."
- "TF measurements are used in instrument calibration (Figure 3b) and investigating the vortex lattice state of superconductors **utilise TF geometries**" → redundant/garbled; "...and investigations of the vortex lattice state in superconductors also make use of TF geometries."

**§2.4.1 MuSR**

- "coupled to a Photomultiplier tube **(PMTs)**" → singular/plural mismatch, should be "(PMT)" or make "tube" plural.
- "PMTs convert light ... **into to** an amplified current pulse" → drop "to".
- "MuSR currently detects approximately 20% of **the all** emitted positrons" → "of all emitted positrons".

**§2.4.2 Super MuSR**

- "Two cylindrical barrels ... **each of which are made of** 16 staves made of 32 detector elements of varying length [16], each individual detector element has two..." → comma splice + subject/verb mismatch. Split into two sentences: "...each of which is made of 16 staves, themselves made of 32 detector elements of varying length [16]. Each individual detector element has two Silicon Photomultipliers (SiPMs)..."
- "**That are** extremely compact..." → sentence fragment starting with "That"; merge with the previous sentence about SiPM gain.

**§2.4.3 Tile Design**

- "before being **fixing** the WLSF to the scintillator with **a** optically transparent resin" → "before fixing the WLSF... with **an** optically transparent resin".
- "placed into a vacuum chamber **degassed** twice more" → missing word: "placed into a vacuum chamber **and** degassed twice more".
- "lapped **to the** using high grit micro-fine silicone carbide sandpaper" → "lapped **to the correct length** using high-grit, micro-fine silicon carbide sandpaper".
- "The final stage of assembly is the wrapping **state**" → "wrapping **stage**".
- "this involves placing an Excellent Specular Reflector (ESR) **is wrapped** around the tile" → redundant verb; "this involves wrapping an Excellent Specular Reflector (ESR) around the tile".

**§3.1 Acceptance Threshold**

- "more precise separation of **the the** noise floor" → duplicate word.
- "Optimal **aliment** of the 210mm tiles" → "alignment".
- "Longer tile **lengths greatly increases** the range of path lengths" → subject/verb agreement: "increase" (or "Longer tile length greatly increases...").
- "Having a physical benchmark tile allows a consistent point of normalisation, therefore **changes were to be made to the experimental setup can be corrected for**" → doesn't parse; suggest "...therefore any changes made to the experimental setup can be corrected for."
- "**Backup tiles that can be used**, but the ability to compare..." → sentence fragment; "Backup tiles can be used, but the ability to compare..."

**§3.2.2 Procedure**

- "a higher **the** PHS peak indicates" → delete stray "the".
- "for the longer lengths of **tiles(105mm** and 210mm)" → missing space/punctuation: "tiles (105mm and 210mm),".
- "integrated counts **was** calculated" → "were calculated" (counts is plural).

**§3.2.3 Calibration**

- "PMTs were **set too a** voltage of 760V" → "set **to** a voltage".
- "benchmark tiles **is** measured regularly ... across a standard day of measurements two runs are done at the start and end of **a day of measurements**" → subject/verb mismatch + redundant repetition of "day of measurements"; suggest "the benchmark tile is measured regularly...; across a standard day of measurements, two runs are done at the start and end of the day."
- "Each batch is normalised **to its to the** benchmark measurements" → duplicated/garbled; "normalised **to** the benchmark measurements".
- "Figure 10b demonstrates an example of batches of measurements **are** grouped" → missing "how": "...demonstrates an example of **how** batches of measurements are grouped".
- "The example shows **a of up to** 9.8%" → missing word: "shows **a variation of** up to 9.8%".

**§3.2.4 Stud Analysis**

- "Each event on the PHS is the **coincident** of pulses" → "coincidence".
- "measured by measuring the pulse height of a given event **is on** both the LHS and RHS PMTs" → garbled; "measured by comparing the pulse height of a given event **on** both the LHS and RHS PMTs".
- Figure 11 caption: "**that that** stud has a lower light output... **the stud LHS stud** was wobbly" → duplicated words; "...that stud has a lower light output; for this tile, on inspection, the LHS stud was found to be wobbly, suggesting damage."

**§4.1 30mm Tiles**

- "a key **criteria**" → "a key **criterion**".
- "Concern was raised for the longer tiles that would be **effected**" → "**affected**".
- "**4/5** of the poor performing tiles" → spell out ("Four of the five") — avoid fraction-slash notation and starting a sentence with a numeral.
- "**the the** peak of being in the 1.236-1.298 bin" → "the peak **being** in the 1.236–1.298 bin" (also duplicate "the the" a few lines earlier in this paragraph).
- "The asymmetric skew is expected **as as** most tiles" → duplicate word.

**§4.2 63mm Tiles**

- "Each of the tiles **have** a series of small defects" → "**has**" (subject is singular "each").
- "A t-test was conducted and **resulted and with** a p-value of 0.697" → "resulted **in** a p-value of 0.697".
- "**However this is a small sample size, a a result** the longer tiles" → "However, this is a small sample size; **as a** result, the longer tiles..."
- "compared to the 30mm **with** a standard deviation of 0.100" → "compared to the 30mm tiles, **which have** a standard deviation of 0.100".

**§4.3 105mm Tiles**

- "Figure A4 outlines the 2D stud analysis **all four** of the tiles" → missing "of": "...analysis **of** all four tiles".
- "resulting in **asymmetry in each of the tiles studs**" → "asymmetry in each of the **tiles' studs**".
- "Another feature of note is "Chilled Resin" **this refers to**..." → needs punctuation: "...is "chilled resin"; this refers to..." Also un-capitalise "chilled resin" (not a proper noun); typo "**technicas**" → "technicians".
- "so **cannot** be directly compared" → "so **it** cannot be directly compared" (missing subject).

**§4.4 210mm Tiles**

- "a severe drop in light output **for tiles a group of** four tiles" → duplicated/garbled; "...for **a group of** four tiles (36, 37, 38, and 39)".
- "**Despite tile performance being** 32.1% worse than **the the** benchmark mean." → sentence fragment + duplicate word; needs joining to the previous sentence.
- "guarantees **a the** resin gets into the cavity" → "guarantees **that** resin gets into the cavity".
- "**Furthermore the longer** appear to be more susceptible" → missing noun: "Furthermore, the longer **tiles** appear to be more susceptible".
- "**In addition to this this** top up will not help ... where resin has already **around** them" → duplicate word + missing verb: "In addition, this top-up will not help the gaps where resin has already **cured/set** around them".
- "Each **are** noted to have damage" → "Each **is** noted..."
- "**No clear indication ... were found**" → "was found" (subject "indication" is singular).
- "the remaining tile fell **within within** 20%" → duplicate word.
- "**This gives a overall failure rate** of 8.8%" → "an overall failure rate".

**§5 Conclusions**

- "since tile ID 100, **it was noted**..." → comma splice; make it two sentences.
- "**For all of these tiles that have fallen below the acceptance need to be investigated**" → doesn't parse; suggest "All tiles that have fallen below the acceptance threshold will need to be investigated further..."
- "**super MuSR**" (lower case) → capitalise to match "Super MuSR" everywhere else.
- "preparation for testing of complete detector modules ... **are** in progress" → "**is** in progress".
- "fallen **bellow** the acceptance threshold" → "below".
- "the main mode of failure is hypothesised **to due to**" → "hypothesised **to be due to**".
- "**Scintillating plastic**" → lower-case "scintillating plastic".

**Appendix**

- A.1: "...excluding the 63mm tile which can be found in **8b**" → "Figure 8b".
- A.2: "why the longer tiles had to have integrated counts used **a** FoM" → "used **as a** FoM".
- A.3.1: "**Np**" vs "**NV**" — inconsistent capitalisation of the subscript (should both be capital, matching Eq. 8).
- A.3.2: "the built **in** error function" → "built-**in**".
- A.3.3: incomplete — see Critical Issues §1 above.

---

## 4. Style notes (less "notebook", more report)

- Several sentences run two independent clauses together with just a comma ("comma splices") — this is the single most common issue and the main thing making it read informally. A good rule of thumb while doing a final pass: read each sentence and check whether it could stand alone as two sentences joined by "and"/"but"/"so" — if yes, and there's only a comma, it likely needs a semicolon, a period, or a conjunction added.
- A few informal turns of phrase worth tightening for a report register: "lots of practice" (§4.2) → "considerable experience"; "one choice" (§4.2, describing RT152) → "one candidate"; "a strong choice for resin" → "a strong candidate resin".
- Several tables' captions read as terse technician's-notebook shorthand ("Divot both ends, dull crackly finish..."). That's fine as a direct record of the workshop notes (and arguably strengthens the report by showing primary data), but it's worth a one-line caveat the first time one appears, e.g. "Notes are reproduced verbatim from the technicians' production log."

---

## 5. On the physics/graphs feedback specifically

What's there now (§2.2–2.4) explains muon precession and detector geometry reasonably well, but two things would directly answer the original comment:

1. **Explicitly tie "what field, where, for what" together in one place** — e.g., a sentence like: "μSR measures the local magnetic field _B__local at the muon stopping site (Eq. 5) by tracking the Larmor precession frequency ω in the decay-positron asymmetry (Eq. 7); Super MuSR's improved time resolution directly increases the maximum ω — and hence the maximum local field — that can be resolved before aliasing/loss of contrast sets in." This connects the "6-10× temporal resolution" number in §2.4.2 to an actual physical consequence rather than leaving it as an unexplained figure.
2. **A literature graph was requested and hasn't been added** — e.g. a typical asymmetry-vs-time spectrum from a published μSR paper (with a citation), or a plot showing how a higher time resolution resolves a higher-frequency precession that would otherwise be aliased. This is the one concrete piece of the original feedback that doesn't yet appear to have been actioned.

---

_This is not an exhaustive line edit — punctuation/comma issues follow similar patterns throughout and are worth a dedicated proofreading pass (or running through a grammar checker like Word's built-in one or Grammarly) once the structural/data points above are fixed._

