---
tags:
  - note
  - super-musr
  - beamline
created: 2025-11-14
---
# Links: 

# Notes:
As mentioned in [[251009- NI October Beamline Visit]], there were issues regarding some upturn seen at the end of the LSB range on the line driver chips. Some tests were done with the picoquant laser to get a uniform source of light [[250805 - PicoQuant Laser Turn on Procedure]]


## with laser:
- Laser setup to have internal trigger on highest frequency
- Getting PHS with decent scan, 10m scans should suffice
- Unable to setup internal trigger using the Pico scope
![[fig-260806-onenote-transfer-line-driver-8.png]]
We could not get a suitable LSB range to see the upturn. Even increasing gain and therefore PE did not help. 

###  Swap in Line driver chips
Plan:
- Read NumPy binary traces to test the shape of the signals
- Detailed overnight PHS of a source over one of the channels
- Swap the line driver differential op amps.
14/11/25
- CHANGED UNIFIED CALIB FILE
- With the resistors with low values it can cause funny feedback loops and stuff, for current feedback ada4927

Obvious increase in the max LSB from changing gain of op amps:
- Changed unified Calib file offsets accordingly
- Two of the bias were too high, set to 58
- Reflections could be causing software to not work

![[fig-260806-onenote-transfer-line-driver-10.png]]