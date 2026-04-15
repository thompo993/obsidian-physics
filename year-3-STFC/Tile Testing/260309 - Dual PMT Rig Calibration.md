### Tags
[[scintillating tiles]]
[[Super MuSR]]



### Setup - Height Scan
- The purpose of this is due to the height changing from changing geometry in the box.
- Ideally, the 0.245 peak value will be recovered for tile 21, as the voltage has not changed.
- Therefore a fresh calibration will need to be carried out.
![[fig-260309-dual-pmt-rig-height-scan 1.png]]
### Setup - Voltage Scan
- Voltage scan to recalibrate and gain match PMTS.
- Height selected from the floor to the top of the source is 44.5cm
- Source aligned using alignment recital
- Grease coupled and then not touched for the rest of the scan to ensure variation is not due to optical coupling
- Error estimated using the error on the fit, and the SEM on a myriad of measurements on tile 21. 6% SEM from many measurements, taken from last report. This does not account for the fitting error.
![[fig-260309-dual-pmt-rig-gain-match.png]]
- This coarse scan is suggesting that the PMTs are gain matched at 760V.
- This is interesting as this is a much lower PHS peak- Giacamo says the PMT is not damaged and seems fine. The current etc. so photocathode has not been evaporated. This is promising as PMTs were exposed to moderate light at one point.
- The above data is stored on tile testing git. "gain match_260309" this will be useful for report.
- Fine scan will go ahead around 740 to 780.
- This was then verified using a 63mm tile to see if each channel was truly matched: 
![[fig-260309-dual-pmt-rig-63mm_tile_verification.png]]
