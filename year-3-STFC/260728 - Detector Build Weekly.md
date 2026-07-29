---
tags:
  - meeting
  - super-musr
created: 2026-07-28
---
# Links: 

# Agenda: 
- Francesco wants to know "how many tested tiles have been approved and am i on top of it"
- i am currently spot checking results 
- i want to speak with peter and rhea after the meeting or during to include specific aspects of why super MuSR matters, is there a "science case document"
- Refer to the minutes in [[260721 - Detector Build Weekly]] and answering all questions 
# Minutes:
## Determination of "what constitutes failure":
- currently we use a relative test, how good is tile a compared to tile b.  
- allows us to accept tiles for the final detector, does not allow us to reject tiles for the final detector
- While this is good for flagging tiles that need to be investigated, it is not good for discerning an absolute accept or reject threshold. 
- need to determine what this threshold is. 

## Do we need to order more tiles? 
- currently out of the tiles that have been tested, these are the current percentages of tiles that fall below the investigation threshold:

| Tile Length | % Below Acceptance Treshold           | Hypothesised Cause                                                                                          |
| ----------- | ------------------------------------- | ----------------------------------------------------------------------------------------------------------- |
| 30mm        | 1.4%                                  | Re-lapping and double handling (0 failures since tile ID 100)                                               |
| 63mm        | 13.9%                                 | No confirmed cause, tiles to be investigated further                                                        |
| 105mm       | 7.3%                                  | No confirmed cause, hypothesised poor optical coupling between WLSF and scintillator                        |
| Total       | 8.6%                                  | Double handling and WLSF-scintillator coupling are overall the most detrimental factors to tile performance |
- recall that this is a conservative estimate of the tiles failure rate. 
- 30mm ties, we think we will be okay very few failed after tile ID100 **don't need to order more**
- 60mm tiles, 13.9% seem to be a lot, but they are all within 10% of the benchmark, so are these really failures? will be determined by absolute accept/reject threshold **don't need to order more**
- 105mmmm tiles 7.3%, they are all within 5% of the benchmark mean, so these are **don't need to order more**
- 210mm tiles, 12/45 (26.7%) fall below 10%. we have recovered performance after tile 078. **should order more (approx 20?)**
![[fig-260728-tile-testing-210mm-tile-id-measured-on-260629.png]]


### Workshop discussion 
- Neil has glued all 30mm, 43mm, and 63mm tiles
- still no titanium screws for the detector barrels to be completed
- Neil has not got lids, or thermal paste (ordered in 200x100mm sheets)
- Neil needs a .step file or anything for the cut outs for the stave thermal cooling
### Cooling and deflection tests
- what is the progress on the deflection tests
	- how long do we expect it to take
	- can we squeeze this in before cooling tets 
- andy church and cooling tests
	- sleeve in development to prevent copper pipes from bending 
	- do they know barrels can be taken for tests
	- all parts for this ordered?