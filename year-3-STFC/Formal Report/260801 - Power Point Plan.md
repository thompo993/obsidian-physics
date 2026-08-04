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





## Likely questions 

**On the acceptance threshold (slides 12–13)**

- Why is the threshold defined as "average detector P/V exceeds MuSR's" rather than a fixed absolute number? What happens to a stave where 90% of tiles pass but the average barely clears the line?
- The MuSR benchmark P/V (1.653±0.016) was measured "parasitically" — how confident are you that conditions during that parasitic measurement matched your bench-rig conditions?

**On the 210mm tile / resin story (slides 20–22)**

- This is your most detailed failure investigation, so expect the most digging here: was the "poor resin application" root-caused to a process step (technician, batch, timing), or could it recur unpredictably?
- You mention pooling fixed it but "did not fully recover performance" and cite "contamination between resin surfaces" — what's the mechanism there, and is it solved or just mitigated?
- Why did this defect only show up at 210mm and not at shorter lengths — is it purely handling/gravity during curing, or something else?

**On statistics generally**

- Several slides give a single tile-length distribution and one skew statement (e.g. "negatively skewed gaussian recovered") — will you be asked for the actual test statistic, or is "the shape looks skewed" sufficient at this stage?
- Your fractional error of ~6% is described as "really an estimate because the rig was different" — expect someone to ask whether this uncertainty was propagated into the pass/fail calls, or just used as context.

**On scope/next steps**

- Slide 23 mentions 43mm tiles now in production — is that a new geometry outside anything shown here, and do the same acceptance criteria apply?
- What's the actual plan for the 210mm tiles that still don't recover fully — are they excluded from the final build, or is further rework planned?
- "Develop absolute test of brightness" is listed as future work — do you have a proposed method already, or is this open?

**Likely quick clarifying question**

- Slide 18 mentions a new LHS/RHS correlation diagnostic — has this been validated on tiles with known defects, or is it applied for the first time in this dataset?

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
- sl