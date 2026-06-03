# Tags: 
[[Super MuSR]]
# Legacy Plan

## Discussion Points
- Tile testing setup
- Interesting theory, can explaining about design Philippos of the Tiles
- Experimental Procedure of testing tiles
- Integration time tests (small point)
- Tile 21 Benchmark tests
- Challenges posed by constantly changing what is being tested
- Rig design
- Bulk tile tests in photo refractometer
- Code written
- Calibrating dual PMT rigs
- Working elements of a PMT
- Data analysis
- Peak finding methodology and code
- Large data sets to analyse
- By august lots of further improvements to make
- Issues ran into during production of the tiles
- Stave Testing etc

## Tile Testing Structure - Interim Report Plan

### Overall

- Write out fully fleshed theory and then cut it down?!?!?!?

Introduction

- Purpose of the DSG at ISIS
- Purpose of the report
- Summary of work done at ISIS

### Theory

- Muons at ISIS
- How does MuSR work

- Larmor precession - flesh out this point
- Up until the positrons are being detected, then we can  pivot into positron detection

- Positron detection "some common methods of detecting these positions are . . ."

- PMTS
- SiPM's

- More discussion on how they work, show the discrete PHS distribution etc

- The Super MuSR upgrade

- Brief explanation that leads into the tiles
- Tiles design and purpose

- Eljen scintillating plastic with a  milled track for the WSF pre made as it arrives
- We bend the fibre, insert the fibre and then fill up the gaps with clear resin
- We then attach a stud to help hone the WSF to the SiPM's
- Using a Lapping Jig to smooth it down and make it flat

### Experimental

- Tile Testing setup (Dual PMT Rig)

- Bespoke Pico scope software
- Issues with different length Tiles
- Setting up and optimisation:

- Integration time
- Edge effects of the detector
- Location effects
- Repeatability tests

- Peak Finding Software

- Qualitative function

- Calibration and Gain Matching
- Procedure

- Settings used (maybe integration time studies)
- Baseline procedure, repeats etc
- Why the peak matters

- Explain tile 21 and the benchmark and what is the benchmark meaning physically

- Bulk modulus transparency, and over time tests

### Results

- Tiles initially were underperforming, found issue and then showed them improved
- Show that the improved tiles worked on beam successfully

### Discussion

- What is good

- Repeatability shown,
- Tiles proven on beam successfully

- What is bad

- Peak finding software needs improving
- Holding to ensure the tiles are in the same place on the PMT 
- Requirement to thermalise, change over time etc

### Conclusions

- Brief of current conclusions and what I can/will improve upon

- Error propagation and code improvements
- Concrete evidence from beam of tile tests

## Questions for Dan
Questions for Dan

- What is the key result of tile 21. we know its good, do we use the peak to valley ratio? Peak to valley used to determine a figure of merit, and kind of a signal to noise lower energy positrons we also don’t want to measure

Easier to think about it is that for neutrons you don’t want to measure gamma, muons you don’t want to measure low energy positrons as they carry the wrong information.

- How many tiles exactly on super musr, and how many detectors

960 tiles, 2x for the sipms

- Why do some muons lose polarisation between their production and being implanted in the sample. We are tuning for those ones, also a acceptance threshold of muons on the surface, plus interactions of the bunch, plus the thin films the muons interact with on the way there
- Can I use the tile 21 data and generalise this to the whole PMT? Or is it fair to only use counting stats (eg poission) so can I use Standard mean error on all data points? DONE ERROR PROPAGATION