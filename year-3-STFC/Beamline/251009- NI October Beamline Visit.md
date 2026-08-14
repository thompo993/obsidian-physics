---
tags:
  - note
  - super-musr
created: 2026-08-13
---
# Links: 

# Notes:
## Day 1: 261009 
### Plan
- 90deg to beam
- Parallel to beam
- Voltage scan of all the tiles, low to high bias on the SiPM
- Discover root cause of increase in voltage causing higher counts
- Upturns in the distribution are the point of worry
- Not sharp spike at the end, this is just an effect of the binning of the events larger than the max LSB of the event
- Francesco believes it is not the SiPM
- Voltage scan will tell us if it is or isn't the SiPM
- Improve signal to noise may help resolve
- Effect may be as a result of shorter cables. (check urgently - 20m cables, no the issue)
- Francesco said scintillators are bad???? Because photopeak is lower, However geometry is unusual.
- Seeing how far past breakdown we are, different vbr given from the two different methods, variance and PHS method.
- Is the vbr is real from these measurements or is it a "fake" number
- Andrea plans to do sever tests - may have to help with that in the afternoon  - may be banned by dan
- Get HDMI cable from source room as we go past
##### Voltage Bais Scan - Ben and Lisa Objective - IF BEAM
- Change target PE from as low as you dare to the max overbias
- Do the scan in both positions, 90deg and parallel to beam
- Told by dan how to do this.
- vbr=51+/-1 
- Range to scan:
- Chug away with beam
##### Beam Procedure:
- Take key to unlock door,
- Unlock door
- Check door stop is working
- Extra step about lights etc - learn from dan
- Walk and change sample
- Press the search button
- Ensure no one is in there
- Shut door, lock door
- Place key back in place

#### Evidence of increase in voltage counts
![[fig-260813-beamline-ni-october-visit-1.jpg]]
This was later determined to be caused by the Line drivers [[251114 - Line Driver Testing]]. 

#### Example Experimental setup for the short stave
![[fig-260813-beamline-ni-october-visit-2.jpg]]

### Notes on runs 
#### HV SCAN Pulse height analysis Module B:A
- 3 25min scans, as statistics were poor for the 15 min ones
Notes:
- More photons per positron (from prelim data) for tiles that are closer to the interaction point (sample)
#### HV SCAN Pulse height analysis Module B:B
- 70mm slit width (basically fully open)
- No pictures taken due to time constraints, but can be assured that experimental setup was identical to that of PCB A
- Results of PCB a and B are in the google drive on your wrok email google drive, shared by lisa.
---
## Day 2: 251010
- Arrived to hot desk while one measurement was being taken, during the data.
- Scan with date around 10/10/25 - 9:30 may have nonsense data due to changed geometry.
- Andrea wrote script to optimize and automate the code
### Pump
- Check for "*" on left of panel, suggests it's not working
- Ensure bubbles are not stationary
- No kinks
- Should see a "+" or "-"
- No error messages should be seen, they flash

### Automation code - Manual:
- scan_pe.py is the name of the file there are some key params:
- DIGITIZER_IP: the ip of the specific digitizer you are using
- SETTINGS_FILE: The settings setup that are used for the automation
- PE_SCAN_VALUES: The value of photons per positron eg 1000 peak for PE of 20 would be a 50 photon peak, it repeats for all of the given PE
- ACQUISITION_TIME : The length the automation counts per iteration
- OUTPUT_DIR: output directory for data and pllots and data, timestamped and hard coded to the correct PE
```
# ============================================================================

# GLOBAL CONFIGURATION

# ============================================================================

# Digitizer IP address

DIGITIZER_IP = "130.246.54.1"

# Path to settings JSON file

SETTINGS_FILE = "scan_pe_protostave.json"

# Path to unified calibration JSON file

CALIBRATION_JSON = "calibration_module_c_10010.json"

# PE values to scan

PE_SCAN_VALUES = [30, 35, 25, 40, 20, 15, 50, 10, 5, 32, 28, 38, 22]

# Acquisition time for each PE value (seconds)

ACQUISITION_TIME = 45*60

# Output directory

OUTPUT_DIR = "run_scan_pe"
```


## Day 3: 251013 (i missed weekend)
- changed resistors to shift the PHS peak location

$$
\frac{(\frac{460}{33}+1)}{(\frac{470}{47}+1)} = 1.386
$$
We expect a approximately 40% change in peak location. 
![[fig-260806-onenote-transfer-line-driver-beamline-1.png]]
We are still experiencing upturn in the line driver chips.

## Day 4: 260814
- Crosstalk (em communication between two tracks in the PCB)
- This is making a negative signal, that can somethimes go past zero in the negative direction,
- Due to how binary works, this then takes you to the max value. "wrapping round effect"
- This is dans working hypothesis as of last night.
- Francesco is making adjustments to module BB
- There is a log of all time averaging plots in university OneDrive
- All the best code is in Lisa google drive (shared with work gmail google drive)
- Nothing too be done about upturn past 3500, key result of day
**Corrected Pre Transmission impedance to 33 Ohm from 22 Ohm**
- Likely cause for the reflection
- Still seeing crosstalk and the large negative cross talks.
![[fig-260814-beamline-ni-visit-evidence-of-reflection.png]]

**Cable Check**
- Moved back to the old digitiser, and have swapped cables, one 5m and one 10m.
- Should see less reflections and see if analogue pileup is a result of the cables
- Cables can be a band pass filter, which can hurt higher frequency signals
- So it seems like we can't saturate charge, unsure on how this is effecting the pileup in a differential sense.

**Horizontal_20m_Cables_testing_LD - Binary data, taken at 14/10/25 @15:00**
- A = 51 Ohms
- F=51 Ohms + rf(eedback)=470 Ohms
- All others have line drivers 33 Ohms

**Horizontal_20m_Cables_testing_LD - Binary data, taken at 14/10/25 @15:15**
- A = 0 Ohms
- F=51 Ohms + rf(eedback)=470 Ohms
- All others have line drivers 33 Ohms

**Horizontal_20m_Cables_testing_LD - Binary data, taken at 14/10/25 @15:35**
- A = 10 Ohms
- F=51 Ohms + rf(eedback)=470 Ohms
- All others have line drivers 33 Ohms
None of these yielded a successful solution to the upturn issue
Henceforth everything is returned to how it should be with correct 33 Ohm
Restoring RF

The cuase of the upturn is unknown at this point (future ben adding this in, we know it was the line drivers and this issue is resolved. )

### images of calbin and daq setup 

![[fig-260814-beamline-ni-visit-exp-geom.png]]
![[fig-260813-beamline-ni-october-visit-4.jpg]]


### Evening Run PtV 
 "loc_compact" represents the detectors has been rotated perpendicular to the beam, there is not much space around the detector. The detector is 90deg anticlockwise relative to the muon beam direction.
Peak to valley discrimination was not deemed very good, repeat of PE with increased slit width and increased PE --> 30
Offset was changed and done manually, as seemingly incorrect for the "dinner time run"
Andrea calibrations all done on the incorrect line drivers.
Do not trust PE as exact photons per LSB, but as more of a "dial/number"
V_br = 51.8 is the global standard for SiPM
we got some nice peak to valley values.
![[fig-260806-onenote-transfer-line-beamline-ni-visit-5.png]]