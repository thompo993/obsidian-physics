---
tags:
  - note
  - super-musr
  - scintillating-tiles
created: 2026-08-01
---
# Links: 
[[260603 - Plan]]

# Media 
 
![[pp-260803-presentation_with_erik_feedback.pdf]]
# Notes:
# V1
### presentation 
- IN PRESENTATION EXPLAIN ERROR MORE, WHAT IF ANDRE ASKS ABOUT IT
- what are poisson statistics
- why do longer tiles get effected worse by the defects
	- scrates are the same size, as the cuase (human error)
	- remains the same, so for a dimmer and more sensitive tile, the same mistake will have a more significant effect. 
- temperature effects breakdown voltage of SiPMs what is the observed effect on the traces

## Repeat measurements section 
- what is the experimental rig 
- what is the procedure, clean, optical coupling ratio of ISO etc 
- brief mention of gain matching 
- where sources of error come from
	> long timeframe errors 
	> short timeframe errors 
	> how we account for both of these 
	> why repeating benchmark measurements solves both of these issues 


### potential changes to structure 
collapse 105 and 63mm tiles into one, nothing clear showing issues between the tiles as message for both of them is the same
maybe get rid of 2D stud analysis sections?
cut theory section down if required






# V2 Erik Feedback
- way less bullet points data is overwhelming 
- better narrative and flow, a few things are not quite right.
- slide specific feedback is found in the above pdf, or in UoB OneDrive 
- **Much more to do, continue updating this feedback**


## V3 Erik Feedback 
- not mentioning all text on outline = good
- slide 9, mention why we are comparing them, Super MuSR is the replacement 
- slide 10, never said that SiPMs are light detectors 
- slide 11, don't point, describe where on the slides everything is, andre cannot see!
	- say where it is so that we can make it obvious the flow of the slide
	- CONSIDER ONLINE PEOPLE!!!
- slide 16: and later, the parastic measurements are not easily understood 
- it reads that the 210mm PHS spectrum is the parastic measurement image, images are being tested
- remove it for the stave picture only.  see how it looks 
- slide 20: took a long time to explain about the positive skew, try and make it shorter. expect to be asked about it
- slide 23, make the bullet points more clear, why is pooling good etc. but don't write too many words
- dont mention tile 78 specifically, just saw we saw the light output return that we are expecting. 


# Potential Questions 


**Presenter:** Ben Thompson

**How to use this:** Each section below has a short recap of what's on the slides, followed by likely questions. Questions marked **Already prepped** come from your own speaker notes — the answer you wrote is included so you can review it. Everything else is a question you haven't pre-written an answer for; only a one-line pointer is given so you can practice building the answer yourself out loud.

---

## Part 1 — Introduction

### About ISIS (Slide 4)

ISIS is a pulsed spallation neutron and muon source with 30+ instruments, ~1200 experiments and ~600 publications a year, covering advanced materials, chemistry/catalysis, life sciences, and environment/geology.

**Practice questions**

- What is a "pulsed" source, and how does that differ from a continuous source (e.g. PSI)?
- Where does ISIS sit relative to other major muon facilities worldwide (PSI, TRIUMF, J-PARC)?
- Why do materials scientists specifically care about muon techniques rather than just neutrons or X-rays? ==study's dynamics, better at measuring magnetic fields== 

### Production of Muons (Slides 5–6)

H⁻ ions are produced, accelerated through the RFQ and Linac, stripped to protons by a thin aluminium foil, accelerated to 84% of light speed in the 800 MeV synchrotron, and collided with a thin carbon target. This produces pions; positive pions decay via the weak interaction into muons emitted anti-parallel to their momentum, giving highly polarised "surface muons" that are guided to the sample.

**Practice questions**

- **Already prepped** _How do we know the initial muon spin, i.e. why are the muons polarised?_ Your answer: the polarisation comes from parity violation (a form of spatial-inversion-symmetry breaking). The neutrino has negative helicity, so its momentum is anti-parallel to its spin — the same must hold for the muon in the decay. Knowing the initial spin this way tells us the muons are polarised.
- **Already prepped** _How are the "surface" muons actually selected and directed to the sample?_ Your answer: velocity-selector magnets pull out muons that are at rest near the surface of the carbon target — these are the fully polarised ones — and direct them to the sample.
- Why is carbon specifically used as the production target?
- Why does the foil strip _electrons_ rather than something else happening at that stage — what's physically going on?
- What momentum/energy do surface muons typically have, and why does that matter for how deep they implant in a sample?

---

## Part 2 — MuSR Experiment Basics (Slides 7–10)

### The basic MuSR technique (Slide 8)

Polarised muons implant in the sample; the muon spin precesses around the local magnetic field at the Larmor frequency (giving the field magnitude), and muons decay preferentially in the direction of the spin at the moment of decay — so measuring the emitted positrons over time reveals the magnetic field distribution in the sample.

**Practice questions**

- **Already prepped** _Why does the positron energy/count distribution have the shape it does?_ Your answer: the muon decay is a three-body decay, so there's a range of energies the positron can take — that range of possible energies is what produces the distribution shape.
- What is the Larmor frequency actually telling you physically, and how do you get from "spin precession" to "field magnitude"?
- Why is 2.2 μs (the muon lifetime) the natural timescale that limits a MuSR experiment?
- What does "asymmetry" mean in this context, and how is it different from a raw positron count?

### MuSR vs Super MuSR (Slide 9)

MuSR: 64 detector elements, PMTs, plastic scintillator + Perspex light guide. Super MuSR: 960 detector elements across 32 staves of 32 elements each, SiPMs, 6–10× better temporal resolution, 20× more data collection — enabling a wider range of internal fields to be measured and faster/better battery-material research.

**Practice questions**

- Why move from PMTs to SiPMs specifically — what are the actual trade-offs (gain, magnetic-field immunity, dark counts, cost, temperature sensitivity)?
- Concretely, what does "6–10× temporal resolution" let you measure that you couldn't before?
- Are there any downsides introduced by the SiPM upgrade that you had to design around?

### Positron detection design (Slide 10)

Tiles: plastic scintillator + wavelength-shifting fibre (WLSF) that absorbs the scintillator's light, alignment studs, bonded with clear epoxy. Detection: pixelated Geiger-mode SiPMs, similar gain to a PMT, magnetically immune.

**Practice questions**

- Why use a WLSF at all instead of coupling the scintillator directly to the SiPM?
- What does "Geiger mode" mean, and why does that make the number of pixels triggered proportional to the number of photons?
- Why does magnetic immunity matter for this specific instrument (what field is present near the sample)?

---

## Part 3 — Tile Production (Slides 11–12)

Process: bend the WLSF (hot water bath, to avoid cracking), insert WLSF and studs into the scintillator, fix with degassed transparent epoxy, cure and set the resin, lap down protruding fibre/stud, wrap in ESR (Enhanced Specular Reflector) and aluminium tape. Finished tiles come in five lengths: 30, 43, 63, 105, 210 mm.

**Practice questions**

- Why does the resin need to be degassed before use?
- What does the ESR wrap physically do, and why does it need aluminium tape over it?
- Why five different tile lengths — what's the geometric constraint driving that (stave position, cryostat clearance)?
- What could go wrong at each production step, and which step turned out to matter most for the results you found later?

---

## Part 4 — Experimental Method (Slides 13–16)

### Measurement procedure (Slide 14)

Clean tile and PMTs with a 70:30 ISO/deionised-water mix, apply optical grease and align an Sr-90 source, then determine light output using either the integrated counts or the PHS (pulse height spectrum) peak.

**Practice questions**

- Why Sr-90 specifically as the calibration source?
- What does a pulse height spectrum actually represent, physically?
- Why does the cleaning step matter enough to standardise the solvent ratio?

### Calibration & error (Slide 15)

Two PMTs calibrated with a gain scan. Two error sources: optical coupling/alignment (short-term, run-to-run — handled via standard deviation across repeats) and long-term PMT drift (handled by normalising groups of measurements against a benchmark tile).

**Practice questions**

- What is a gain scan, mechanically, and why does it need repeating for each PMT?
- What physically causes long-term PMT drift (HV supply drift, photocathode damage, temperature)?
- Why is repeating benchmark-tile measurements a valid way to correct for that drift, rather than, say, a fixed calibration curve?

### Determining the acceptance threshold (Slide 16)

Requirement: average stave performance must exceed the peak-to-valley (P/V) ratio of the current MuSR instrument. MuSR P/V: 1.653 ± 0.016. Mean Super MuSR P/V: 2.715 ± 0.058. Mean 210 mm tile P/V: 1.556 ± 0.133 (below requirement on this metric alone).

**Practice questions**

- What does the peak-to-valley ratio actually measure about detector performance, and why is it the right metric to gate acceptance on?
- Why is it acceptable that the 210 mm tiles fall short on P/V specifically — what's the justification for still accepting them (broader valley / better separation on another metric, within error bars)?
- Why was the benchmark measurement done "parasitically" on MuSR rather than as a dedicated run — any implications for the data quality?

---

## Part 5 — Results by Tile Length (Slides 18–24)

### 30 mm tiles (Slides 18–19)

73 tested; 23.7% better light output overall than the benchmark; 1 tile below threshold. Investigated two candidate causes: relapping and resin type. Resin type showed no statistically significant effect; relapping (studs glued too long, then relapped/sheared more) is the likely cause of the underperformance.

**Practice questions**

- How was "no statistically significant result" for resin type actually established — what test, what sample size?
- Why would extra shearing force from relapping plausibly damage the tile's light output — what's the physical mechanism (microcracking, delamination)?

### 63 mm tiles (Slide 20)

36 tested; 5/36 flagged. A new LHS/RHS correlation-plot diagnostic can detect tile asymmetry. RT152 resin verified as an acceptable-brightness alternative and fixed a curing issue in longer tiles. No negative skew observed, attributed to an already-refined production process (tighter spread: 0.055 vs 0.100 SD for 30 mm vs 63 mm).

**Practice questions**

- What does the LHS/RHS correlation plot actually show when a tile is asymmetric, and why does that indicate a fault?
- Why did the original resin (used for the 105/210 mm tiles) deform in the oven, and why does a room-temperature-cure resin avoid that?
- Is a tighter standard deviation genuinely evidence of a "more refined process," or could it just reflect fewer tiles/less statistical power?

### 105 mm tiles (Slide 21)

41 tested; 3/41 flagged. 2.9% better performance than the benchmark repeats. No single fault identified; the negatively skewed distribution seen with the 30 mm tiles reappears here.

**Practice questions**

- **Already prepped** _If the flagged and unflagged tiles are all within ~5% of each other, why flag any of them at all — isn't that closer together than the 30 mm case?_ Your answer: this measurement uses integrated counts rather than the PHS peak value, so it isn't a direct like-for-like comparison with the earlier percentage figures.
- Why would longer tiles inherently be harder to manufacture consistently (light output, stud spacing, length of fibre run)?
- Why does a negative skew reappear here but not in the 63 mm tiles specifically?

### 210 mm tiles & resin pooling (Slides 22–24)

37 tested; 7/37 flagged (the largest proportion of any length). Tile IDs 36–40 were 32.1% worse than benchmark, traced to poor optical coupling between WLSF and scintillator. Root cause found on unwrapping: insufficient resin pooling along the top of some tiles vs. others. Refilling with more resin improved those tiles to 20.3% below benchmark (still worse, but recovered). Excluding the refilled tiles, only 3/37 remain below threshold.

**Practice questions**

- **Already prepped** _How do you know tiles that passed acceptance haven't also been underfilled with resin?_ Your answer: you don't — and it isn't useful to unwrap tiles that are already above the acceptance threshold just to check, even though they could in principle be better. This is also why a t-test isn't possible here: you can't cleanly separate tiles into "underfilled" vs. "properly filled" groups.
- Why doesn't refilling fully recover the lost performance (what does the resin-resin interface do differently to a single continuous fill)?
- Tile 078 is cited as an early success case for the pooled-application method — why is one example treated as meaningful evidence rather than an anecdote?
- Why is pooling less of a problem on the shorter tiles?

---

## Part 6 — Conclusions & Future Work (Slide 25)

Able to trace and mitigate sources of error; provided evidence for specific design choices (e.g. RT152 resin); 8.3% of all tiles flagged for investigation overall; the two dominant causes identified are relapping and poor WLSF–scintillator optical coupling. Future work: test 43 mm tiles, develop an absolute brightness test (current tests are pass/fail only), introduce flood-field and full-module beamline testing, and continue monitoring quality through to the final Super MuSR build.

**Practice questions**

- "8.3% flagged" — flagged doesn't mean rejected. What actually happens to a flagged tile, and does that number tell an audience anything about the final reject rate?
- What would an "absolute" brightness test look like, and why can't the current method provide one?
- What's the practical difference between flood-field testing and the tile-level testing you've described — what new failure modes would it catch that per-tile testing can't?
- If you had more time, what's the one thing in this QC pipeline you'd change first?
- What's the current status/timeline for Super MuSR coming online?

---

## Part 7 — Backup Slide Topics (Slides 28–30)

Keep these in your back pocket — they read as pre-prepared answers to questions you expect from a sharp audience member.

**Asymmetry equation (Slide 28)**

- Be ready to write out and explain each term of the asymmetry equation, and state in plain language what "asymmetry" is measuring.

**Why not 1024 detectors? (Slide 29)**

- Answer already on the slide: 64 detectors are removed to make room for the sample and cryostat to be lowered into the detector array, leaving 960. Be ready to explain _why_ that clearance is needed physically (cryostat insertion geometry) and to double-check you can state precisely which detectors are removed (e.g. is it symmetric around the beam axis?) in case you're pressed for detail.

**Why switch to integrated counts? (Slide 30)**

- Answer already on the slide: fitting a PHS peak finder to the 105 mm and 210 mm tiles was very difficult (broader/less well-defined peak for longer tiles), whereas integrated counts gives a robust "overall light output" figure regardless of peak shape.
- Likely follow-up: doesn't that make the integrated-counts tiles incomparable to the PHS-based ones? (This is the same point as the 105 mm "Already prepped" question above — good to link the two answers together.)

---

## Big-picture / defense-style questions

These tend to come up regardless of slide content — worth rehearsing even though nothing on the slides maps directly to them.

- What is the single most novel contribution of this work, in one sentence?
- What are the biggest limitations of your testing methodology, and how would you address them with more time or budget?
- How does your QC approach compare to what other muon facilities (PSI, TRIUMF, J-PARC) do for their detector production?
- Of all the failure modes you found, which one most changed your production process going forward?
- If a stave ends up with a few flagged-but-not-rejected tiles in it, what's the actual impact on Super MuSR's science output?
- What's the broader significance of this instrument upgrade for the muon-science user community, beyond battery research?

---

## Quick-reference numbers (for recall under pressure)

|Metric|Value|
|---|---|
|MuSR detector elements|64|
|Super MuSR detector elements|960 (32 staves × 32 elements)|
|Temporal resolution improvement|6–10×|
|Data collection improvement|20×|
|MuSR P/V ratio|1.653 ± 0.016|
|Mean Super MuSR P/V ratio|2.715 ± 0.058|
|Mean 210 mm tile P/V ratio|1.556 ± 0.133|
|Tile lengths produced|30, 43, 63, 105, 210 mm|
|30 mm tiles tested|73 (1 below threshold, 23.7% better overall)|
|63 mm tiles tested|36 (5 flagged)|
|105 mm tiles tested|41 (3 flagged, 2.9% better)|
|210 mm tiles tested|37 (7 flagged, 3 after excluding refilled)|
|210 mm underperformers before/after refill|32.1% worse → 20.3% worse than benchmark|
|Overall tiles flagged|8.3%|
# V3 Changes to make - Jeff Sarah, Dan
- ISIS is a neutron and muon source, mention both 
-  why carbon target? 
	- we need a pion decay chain 
	- musr sepcailises in muonium samples 